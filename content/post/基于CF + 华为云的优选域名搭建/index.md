---
date: '2026-07-30T12:00:00+09:00'
title: '基于CF的优选域名搭建(其一：使用华为云)'
categories:
  - 
tags:
  - 教程
---
{{< heatMapCard levelStandard="200,500,2000" >}}
### 提前准备
1. Cloudflare账号 x1

2. 华为云国际版账号 x1

3. 域名 x1，注意不能是 xxx.cc.cd、xxx.ddns.ge等复合后缀产生的域名后缀
## 步骤
1. 在 Cloudflare 内新建一个worker，名称任意，自定义域绑定自己域名 `cf.example.com`

2. 点击【编辑代码】，将以下代码粘贴进去，点击【部署】，访问显示 OK 即完成

```worker.js
const PROXY_MODE = true;

const HOP_BY_HOP = new Set([
  'connection', 'keep-alive', 'proxy-authenticate', 'proxy-authorization',
  'te', 'trailers', 'transfer-encoding', 'upgrade',
  'cf-connecting-ip', 'cf-ray', 'cf-worker',
  'x-forwarded-for', 'x-real-ip'
]);

const DEFAULT_UA = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36';

const TIMEOUT_MS = 25000;
const MAX_RETRIES = 2;

const STATIC_TYPES = ['image/', 'font/', 'video/', 'audio/', 'text/css', 'application/font', 'application/javascript', 'application/octet-stream'];

const CORS_HEADERS = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Methods': 'GET,POST,PUT,DELETE,PATCH,OPTIONS,HEAD',
  'Access-Control-Allow-Headers': '*',
  'Access-Control-Max-Age': '86400',
};

export default {
  async fetch(request) {
    const url = new URL(request.url);
    const clientIP = request.headers.get('CF-Connecting-IP') || 'unknown';

    let targetPath = url.pathname;
    if (url.search) targetPath += url.search;

    if (!targetPath || targetPath === '/') {
      return new Response('OK', {
        headers: { 'Content-Type': 'text/plain; charset=utf-8', ...CORS_HEADERS }
      });
    }

    let targetUrl = targetPath.startsWith('/') ? targetPath.slice(1) : targetPath;
    if (!targetUrl.startsWith('http://') && !targetUrl.startsWith('https://')) {
      targetUrl = 'https://' + targetUrl;
    }

    const method = request.method;

    if (method === 'OPTIONS') {
      return new Response(null, { headers: CORS_HEADERS });
    }

    if (method === 'HEAD') {
      const resp = await proxyRequest(request, targetUrl, method, clientIP, url);
      if (!resp) return proxyError(504, 'Upstream timeout');
      return resp;
    }

    for (let attempt = 0; attempt <= MAX_RETRIES; attempt++) {
      const resp = await proxyRequest(request, targetUrl, method, clientIP, url);
      if (resp) return resp;
    }

    return proxyError(502, 'All upstream attempts failed');
  }
};

function proxyError(status, msg) {
  return new Response(msg, {
    status,
    headers: { 'Content-Type': 'text/plain; charset=utf-8', ...CORS_HEADERS }
  });
}

async function proxyRequest(request, targetUrl, method, clientIP, requestUrl) {
  const headers = new Headers(request.headers);

  for (const h of HOP_BY_HOP) headers.delete(h);

  if (!headers.has('User-Agent')) {
    headers.set('User-Agent', DEFAULT_UA);
  }

  headers.set('Accept-Encoding', 'gzip, deflate, br');

  const body = ['GET', 'HEAD'].includes(method) ? null : request.body;

  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), TIMEOUT_MS);

  try {
    const proxyReq = new Request(targetUrl, {
      method,
      headers,
      body,
      redirect: 'manual',
      cf: {
        minTlsVersion: '1.2',
        polish: 'off',
        mirage: false,
        apps: false,
      }
    });

    let resp = await fetch(proxyReq, { signal: controller.signal });

    const respHeaders = new Headers(resp.headers);

    for (const [k, v] of Object.entries(CORS_HEADERS)) {
      respHeaders.set(k, v);
    }
    respHeaders.set('X-Proxied-By', 'cf.107211.xyz');
    respHeaders.set('X-Edge-IP', clientIP);

    if (resp.status >= 300 && resp.status < 400 && resp.headers.has('location')) {
      const loc = resp.headers.get('location');
      respHeaders.set('location', loc.startsWith('http') ? '/' + loc : loc);
    }

    const ct = respHeaders.get('Content-Type') || '';
    const isStatic = STATIC_TYPES.some(t => ct.includes(t));
    const cacheMaxAge = isStatic ? '86400' : '0';

    respHeaders.set('Cache-Control', `public, max-age=${cacheMaxAge}`);

    if (isStatic && resp.status === 200) {
      const cached = new Response(resp.body, {
        status: resp.status,
        statusText: resp.statusText,
        headers: respHeaders,
        cf: { cacheTtl: 86400, cacheEverything: true }
      });
      return cached;
    }

    respHeaders.delete('Content-Disposition');

    return new Response(resp.body, {
      status: resp.status,
      statusText: resp.statusText,
      headers: respHeaders
    });
  } catch (err) {
    return null;
  } finally {
    clearTimeout(timeoutId);
  }
}
```

3. 在华为云国际版内添加该域名，并在 Cloudflare 内手动添加相关的 NS 记录

{{< details summary="为什么要用华为云的国际版？" >}}
省去实名认证、信息填写等一堆玩意，你又不备案和企业用，玩什么国内版？
{{< /details >}}

4. 先使用 `nslookup` 命令检查是否显示你的 worker 地址，并检查你的浏览器和网络是否使用了Cloudflare的服务，如 DOT、DOH、1.1.1.1等，将其修改为国内相关内容，若不检查将影响后续值

5. 使用 AI （本地可使用 opencode 等） 查找别人的优选域名或优选IP，不想找的话可前往[次元星域优选域名](https://cf.107211.xyz/)复制，挑选延迟低，dns 快且干净的优选域名

6. 按下表参考样式填写

| 域名 | 记录类型 | 线路类型 | TTL | 记录值 | 备注 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| cf.example.com | CNAME | 全网默认 | 300 | 你的worker地址 | / |
| cf.example.com | CNAME | 中国大陆 | 300 | 你找的优选域名 | 若找的是优选IP，此时【记录类型】为A，记录值为优选IP |
| *.cf.example.com | CNAME | 全网默认 | 300 | 你找的优选域名 | 同上 |

7. 稍等一两分钟，再次使用 `nslookup` 命令检查域名，若能顺利显示配置的内容则进行下一步，否则华为云内全部删掉，等待5分钟后重新配置

8. 使用 `ping cf.example.com` 测试延迟，或使用 itdog 等测速网站测速，若 ping 值小于70，再进行下一步

9. 使用 AI （本地可使用 opencode 等）测试域名是否可作为优选域名使用，让其全面测试并给出结论，担心结论不准确可使用多个AI测试，这种测试一般都很准确的

10. 若结论为“可作为优选域名使用”或其他类似语句，可视为自建优选域名完成；否则，返回检查