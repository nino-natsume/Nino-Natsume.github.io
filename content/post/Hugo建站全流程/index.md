---
date: '2026-02-28T11:00:00+09:00'
title: 'Hugo建站全流程'
weight: 1
categories:
  - 次元星域
tags:
  - 次元星域
  - Hugo
  - 教程
---
{{< heatMapCard levelStandard="200,500,8000" >}}
# 前言
因为不想专门开一个blog来写这个教程，所以统一用这篇文章来讲，尽量写详细到位点，确保你尽可能地成功搭建出来

本篇使用Hugo框架 + hugo-theme-reimu主题编写，从创建、配置再到部署，用一篇文章分点讲清楚，其他主题需参考该主题的官方文档设置

本篇需要一定的操作能力，要有耐心完成全篇操作，一个精美网站的制作不是一下子就能搞定的

这篇总计8920字，字数多少文章顶部的热力图有标，每一步操作，每一个重点细节都是我一点点实践出的，仔细操作，耐心配置，如果不改任何东西，10分钟内都可搭建出来

## **文章理解与配置说明：**
为了便于你理解本篇内容，墙裂建议阅读本说明，知道下方教程中对应哪个文件夹：

| 文件夹、文件或路径 | 含义 |
| :----: | :----: |
| /blog | 初始文件夹 |
| /blog/blog | 博客目录，若自定义请自改，如/blog/0721 |
| /blog/blog/xxx | 博客目录下对应路径的文件或文件夹 |
| /theme/hugo-theme-reimu/xxx | 主题文件夹内对应路径的文件或文件夹 |

## 事前说明：

1. 本人不是主题作者，仅为个人部署过程讲解；

2. 本篇将全以文字的形式讲解，如有问题请在下方评论区留言或联系邮箱info@natsume.xin

（下面为教程正文）
# Hugo个人blog全流程搭建教程
## 1.环境搭建
### 1.1 Git下载
1. 前往[Git官网](https://git-scm.com/)，下载安装程序

2. 一直点下一步，默认安装即可，若不懂其意思和作用，尽量不要修改选项！！
### 1.2 Hugo下载
1. 前往[Hugo Github](https://github.com/gohugoio/hugo/tags)官方页面，选择第一个最新版本，下载后解压

2. 倒数第三个就是Windows下载的最新版本，带withdeploy的好用
### 1.3 下载主题
1. 前往[hugo-theme-reimu](https://github.com/D-Sketon/hugo-theme-reimu/tags)官方页面，同样选择第一个最新版本

2. 下载Source code后解压，删除文件名后的版本号，仅保留 `hugo-theme-reimu`，编辑好文件夹名后备用
## 2.部署博客
### 2.1 创建博客
1. 进入 /blog，在文件夹的地址栏输入cmd，呼出命令行

2. 输入 `hugo new site blog` 创建 hugo博客主文件夹 blog

3. 继续输入 cd blog，并将上一级文件夹中的 hugo.exe复制进blog中，此时，/blog 文件夹的文件路径应为：
```blog/
blog/
|- blog/
|  |-archetypes/
|  |-assets/
|  |-content/
|  |-data/
|  |-i18n/
|  |-layouts/
|  |-static/
|  |-themes/
|  |-hugo.toml
|- hugo.exe
|- LICENSE
|- README.md
```

4. 输入 hugo server -D 启动博客，出现 http://localhost:1313 后，ctrl + 鼠标单击 地址进入，此时，浏览器显示文字"Page Not Found"，表示博客服务启动成功

5. 键盘按 ctrl + c退出，否则后续一操作或一保存浏览器都会弹出来
### 2.2 初始配置
1. 在/blog/blog内创建config文件夹，在config文件夹内创建_default文件夹，当前路径应为：/blog/blog/config/_default/

2. 将步骤1.3备用的主题文件夹复制或拖入至/blog/blog/theme中，当前路径应为：/blog/blog/theme/hugo-theme-reimu/

3. 将 theme/hugo-theme-reimu/config/_default/params.yml复制到/blog/blog/config/_default内

4. 将 theme/hugo-theme-reimu/data内所有文件复制到/blog/blog/data中

5. 将 theme/hugo-theme-reimu/_example内的所有文件复制到/blog/blog/content中，若无文件夹请自行创建

6. 将 theme/hugo-theme-reimu/assets内的所有文件复制到/blog/blog/assets中

7. 将 theme/hugo-theme-reimu/layouts内的所有文件复制到/blog/blog/layouts中

8. 将 theme/hugo-theme-reimu/static内的所有文件复制到/blog/blog/static中

9. 复制以下代码至 /blog/blog/hugo.toml 内

                                                                                                        点击👇展开
```hugo.toml
baseURL = 'https://example.com/xxx'
theme = 'hugo-theme-reimu'
defaultContentLanguage = 'zh-CN'
enableEmoji = 'true'
[languages]
[languages.zh-CN]
languageCode = 'zh-CN'
languageName = '简体中文'
weight = 1
hasCJKLanguage = 'true'
title = 'xxxxxxxx'
author = 'xxxxxxxx'

[languages.en]
languageCode = 'en'
languageName = 'English'
weight = 2
hasCJKLanguage = 'true'
title = 'xxxxxxxx'
author = 'xxxxxxxx'

[languages.ja]
languageCode = 'ja'
languageName = '日本語'
weight = 3
hasCJKLanguage = 'true'
title = 'xxx'                                      # 自己编辑
author = 'xxx'                                     # 自己编辑

[markup.highlight]
guessSyntax = 'true'
noClasses = 'false'

[markup.goldmark.extensions.passthrough]
enable = 'true'
delimiters.block = [["\\[", "\\]"], ["$$", "$$"]]
delimiters.inline = [["\\(", "\\)"], ["$", "$"]]

[outputs]
home = ["Algolia", "HTML", "RSS"]

[outputFormats.Algolia]
baseName = "algolia"
isPlainText = 'true'
mediaType = "application/json"
notAlternative = 'true'

[markup]
  [markup.tableOfContents]
    endLevel = 4
    ordered = 'true'
    startLevel = 1
```

10. 在/blog/blog的地址栏输入cmd，呼出命令行，输入 `hugo server -D` 启动博客，出现地址后，ctrl + 鼠标单击进入，此时，浏览器应该就能顺利显示主题示例界面，此时博客搭建完成

## 3.主题设置（可选阅读）
### 3.1 多语言i18n配置
1. 打开 /blog/blog/config/_default/params.yml，自定义配置 name和 url，修改完后保存

2. 打开 /blog/blog/i18n 文件夹内对应语言的 .yml文件，开启几个就把对应部分都改了

[假设我设置了上方的hugo.toml内的三种语言：简体中文、English、日本語，接下来我要为这三种语言配置i18n]

以下为示例代码：

```params.yml
# menu部分
menu:
  - name: home
    url: ""
  - name: archives
    url: "archives"
  - name: about
    url: "about"
  - name: link
    url: "link"
```
```en.yml
# 仅显示与 menu有关的代码
home: Home
about: About
link: Link
archives: Archives
```
```zh-CN.yml
# 仅显示与 menu有关的代码
home: 首页
about: 关于
link: 更多
archives: 归档
```
```ja.yml
# 仅显示与 menu有关的代码
home: ホーム
about: プロフィール
link: リンク
archives: アーカイブ
```

参考上方代码修改后可尝试运行，可以看到能正常实现多语言功能

### 3.2 核心设置
本节将讲解该主题的核心配置文件params.yml，将代码一段一段分析功能，助你顺利完成配置

进入 /blog/blog/config/_default/params.yml后，请按以下说明进行自定义设置

代码篇幅非常长，请仔细阅读代码后再做操作，中英文注释很详细，想准确配置一定要看！！

                                                                                                        点击👇展开

```params.yml
menu:                                  # 此部分设置菜单文字及图标，前面已有教程说明如何配置此板块
  - name: home
    url: ""
    icon: 'xxx'                       # 可前往 https://github.com/D-Sketon/hexo-theme-reimu/issues/30 选择想要的图标，注意应选iconfont列而非fontawesome列
  - name: archives
    url: "sort"
    icon: 'xxx'
  - name: about
    url: "about"
    icon: 'xxx'
  - name: link
    url: "link"
    icon: 'xxx'

mainSections: ["post"]                 # 此栏为时间的显示格式，可不改
yearFormat: "2006"
monthFormat: "2006-01"
dateFormat: "2006-01-02"
timeFormat: "2006-01-02 15:04:05"

author: xxx                            #设置作者，此栏显示在主页头像下方
email: xxx@example.com                 #设置邮箱，此栏不显示
description: "xxxxxxxxxxxxxxxxxxx"     #设置描述，此栏显示在主页作者下方
# subtitle: "少女祈祷中..."              #设置副标题，若下方启用则此处保持注释状态
subtitle:
  typing:
    enable: true                       #启动打字动画，此栏显示在标题下方
    strings:                           #若启用需删除 - 前的#，下方为该动画的配置
      # - 句子1
      # - 句子2
      # - 句子3
      # - 句子4
    typeSpeed: 100              # 打字速度（毫秒/字符） 
    backSpeed: 50               # 删除速度（毫秒/字符）
    backDelay: 2000             # 打完一句话后，等待时间
    startDelay: 300             # 页面加载后，等待时间
    loop: true                  # 是否循环播放
    shuffle: true               # 是否随机输出句子
    showCursor: true            # 是否显示闪烁的字符
    cursorChar: "|"             # 字符
    smartBackspace: true        # 智能退格，只删除不同的部分
  text: "xxxxxxxxxxxxx"         # 当 typing.enable = false 时使用

banner: "https://example.com"   # 或 banner: "/image/banner.png"，若为图片API 使用默认语句，本地图片则使用本栏语句

banner_srcset:                  #设置本地背景图，该设置可让背景图在以下图片中随机显示
  enable: false
  srcset:
    - src: "images/banner-600w.webp"
      media: "(max-width: 479px)"
    - src: "images/banner-800w.webp"
      media: "(max-width: 799px)"
    - src: "images/banner.webp"
      media: "(min-width: 800px)"

avatar: "avatar.png"             #设置头像，此栏显示在主页头像处

# Control the display of the post cover
# If not set, the banner image will be displayed by default
# Its priority is lower than the cover in the Front-matter
cover: # https://example.com / false / rgb(255,117,117)

# Control the display of the post toc
# Its priority is lower than the toc in the Front-matter
toc: true

# Open Graph
open_graph:
  enable: true
  options:
    #twitter_card: <twitter:card>
    #twitter_id: <twitter:creator>
    #twitter_site: <twitter:site>
    #twitter_image: <twitter:image>
    #google_plus: <g+:profile_link>
    #fb_admins: <fb:admin_id>
    #fb_app_id: <fb:app_id>

# Content
excerpt_link: Read More

# Footer copyright
# Inject code snippet right in the footer copyright
# Make sure your code snippet is safeHTML
copyright:
# Need help choosing? Please see...
# https://creativecommons.org/choose/
# https://choosealicense.com/
# copyright: |-
#   <div style="flex-direction:column;align-items: center;"><a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a>
#   All website licensed under <a href="https://creativecommons.org/licenses/by/4.0/" target="_blank">CC BY 4.0</a></div>

footer:                            #设置页尾，此栏显示在主页最下方
  since: 2025 # 2020-current year
  powered: true
  count: true
  busuanzi: false
  icon:
    url: "../images/taichi.png"     # 若不设置则为默认图片
    rotate: true
    mask: true # whether to use the images as a mask

sidebar:                            #设置侧边栏，此栏显示在主页侧边栏处
  position: left
  menu: true  # 侧边栏菜单
  article:
    show_common: true               # 侧边栏最新文章栏处

social:                            #设置社交方式，若启用需删除#
  # email: xxx:xxx@example.com
  # github: https://github.com/xx
  # google: https://plus.google.com/yourname
  # twitter: https://twitter.com/yourname
  # facebook: https://www.facebook.com/yourname
  # instagram: https://www.instagram.com/yourname
  # linkedin: https://www.linkedin.com/in/yourname
  # pinterest: https://www.pinterest.com/yourname
  # youtube: https://www.youtube.com/channel/yourname
  # vimeo: https://vimeo.com/yourname
  # flickr: https://www.flickr.com/photos/yourname
  # dribbble: https://dribbble.com/yourname
  # behance: https://www.behance.net/yourname
  # bilibili: https://space.bilibili.com/xx
  # weibo: https://weibo.com/yourname
  # zhihu: https://www.zhihu.com/people/yourname
  # reddit: https://www.reddit.com/user/yourname
  # tumblr: https://yourname.tumblr.com
  # medium: https://medium.com/@yourname
  # deviantart: https://yourname.deviantart.com
  # stackoverflow: https://stackoverflow.com/users/yourname
  # keybase: https://keybase.io/yourname
  # telegram: https://t.me/xx
  # discord: https://discordapp.com/users/yourname
  # steam: https://steamcommunity.com/id/yourname

widgets:                            #设置，此栏显示在侧边栏描述下方，此栏尽量不要改
  - category
  - tag
  - tagcloud
  - recent_posts

# Widget behavior                   #设置，设置上方栏目行为
category_limits: 10
tag_limits: 10
recent_posts_limits: 5
tagcloud_limits: 20

# Archive behavior
only_show_capsule_in_index: false # If you have hugo amounts of tags and categories, you can set this to true to only show the capsule in the index page for better performance
uppercase_capsule: true # If you want to keep taxonomic capsule unchanged, please set this to false

show_update_time: false # If you want to show the update time, please set this to true

# Summary content
summary:
  enable: false
  style: 'subtitle' # 'subtitle' or 'blockquote'

# RSS output
rss:
  limit: 10             # The number of recent articles to be output, write -1 to output all
  showFullContent: false # output full content or description
  showCopyright: false   # If true, add copyright to the end of article.

# Sort order configuration
# Available values: default, date, date-reverse, weight, weight-reverse
# default: hugo's default sort order for page collections, see https://gohugo.io/quick-reference/page-collections/#sort
# date: sorted by date in ascending order (oldest first)
# date-reverse: sorted by date in descending order (newest first)
# weight: sorted by weight in ascending order
# weight-reverse: sorted by weight in descending order
sort_order:
  taxonomy:
    category: date-reverse # controlled by categories_weight
    tag: date-reverse # controlled by tags_weight
  archive: date-reverse # controlled by weight
  home: default # controlled by weight

paginate:                            #设置每页文章数，将xx替换为数值即可，注意保留后方空格，否则报错
  archive: xx # Number of posts per page in archive and taxonomy pages
  post: xx # Number of posts per page in home and other list pages

########################################
# CSS
########################################

layout:
  max_width: 1350px # the max width of the main content area

triangle_badge:                          # 设置页面右上角三角标
  enable: false
  icon: github # same as the icon in the social config
  link: https://github.com/D-Sketon/hugo-theme-reimu

anchor_icon: # default use '#' icon, you can use a hexadecimal representation of fontawesome or icon_font, like 'f0c1', you can also use false to hide anchor icon

reimu_cursor:                            #设置鼠标图标，若自定义请自行准备图片，建议像素32 X 32
  enable: true
  cursor:
    default: ../images/cursor/default.png  # this path is relative to the css/main.css, so it needs to go up one level to reach the images folder
    pointer: ../images/cursor/pointer.png
    text: ../images/cursor/text.png

icon_font: 4552607_4k4bc36ef96            #设置菜单图标，此栏勿改

# Font loading strategy
# Custom Font -> Google Fonts -> Local FallBack Font

# Custom Font
custom_font:                            #设置字体，若自定义请自行准备字体css文件
  enable: false
  article:
    # - css: https://fontsapi.zeoseven.com/292/main/result.css # font css
    #   name: LXGW WenKai # font name
  code:

# https://fonts.google.com/
# Google Fonts, higher priority than local_font
font:                                   #设置字体，选用google字体，若自定义请自行准备字体文件
  enable: true
  article:
    - Mulish
    - Noto Serif SC
  code:
    # - Ubuntu Mono
    # - Source Code Pro
    # - JetBrains Mono

# Local FallBack Font
local_font:                            #设置字体，选用本地字体文件，若自定义请自行准备字体文件
  article:
    - "-apple-system"
    - PingFang SC
    - Microsoft YaHei
    - sans-serif
  code:
    - Menlo
    - Monaco
    - Consolas
    - monospace

dark_mode:                            #设置明暗主题，下方enable自选
  # true means that the dark mode is enabled by default
  # false means that the dark mode is disabled by default
  # auto means that the dark mode is automatically switched according to the system settings
  enable: auto # true | false | auto

########################################
# Analytics
########################################

baidu_analytics: false                 #设置分析
google_analytics: false
clarity: false

########################################
# Markdown Display
########################################

# mermaid url https://github.com/mermaid-js/mermaid
mermaid:
  zoom: true # whether to enable zoom for mermaid diagrams

code_block:                            #设置代码块启用，用于写文章
  # whether to expand the code block by default
  # true means expand all code blocks by default
  # false means collapse all code blocks by default
  # number means collapse the code block by default when the number of lines exceeds the specified value
  expand: true

# Clipboard configuration
clipboard:                            #设置粘贴板显示效果
  success: 
    en: Copy successfully (*^▽^*)
    zh-CN: 复制成功 (*^▽^*)
    zh-TW: 複製成功 (*^▽^*)
    ja: コピー成功 (*^▽^*)
  fail: 
    en: Copy failed (ﾟ⊿ﾟ)ﾂ
    zh-CN: 复制失败 (ﾟ⊿ﾟ)ﾂ
    zh-TW: 複製失敗 (ﾟ⊿ﾟ)ﾂ
    ja: コピー失敗 (ﾟ⊿ﾟ)ﾂ
  copyright:
    enable: false
    count: 50 # The number of characters when the copyright is displayed
    license_type: by-nc-sa # https://creativecommons.org/licenses

math:                                 #设置启用数学公式
  katex:
    enable: true
  mathjax:
    enable: false
    options: # see https://docs.mathjax.org/en/latest/web/configuration.html
    # we need to put the configuration in an array, because hugo will automatically convert the key to lowercase
      [
        {
          tex:
            {
              tags: "ams",
              useLabelIds: true,
              inlineMath: [["$", "$"], ['\\(', '\\)']],
              displayMath: [["$$", "$$"], ['\\[', '\\]']],
              processEscapes: true,
              processEnvironments: true,
              autoload: { color: [], colorv2: ["color"] },
              packages: { "[+]": ["noerrors"] },
            },
          options:
            {
              skipHtmlTags:
                ["script", "noscript", "style", "textarea", "pre", "code"],
              ignoreHtmlClass: "tex2jax_ignore",
              processHtmlClass: "tex2jax_process",
            },
          loader: { load: ["input/asciimath", "[tex]/noerrors"] },
        },
      ]

########################################
# 下方为评论系统的配置，请仔细阅读代码后配置
########################################

comment:                      # comment system title
  title:
    en: Leave a comment
    zh-CN: 说些什么吧！
    zh-TW: 說些什麼吧！
    ja: コメントを残す
  default: waline             # default comment system, when you enable multiple comment systems

# valine comment system. https://valine.js.org
valine:
  enable: false # 启用填 true
  appId: # 填写 app id
  appKey: # 填写 app key
  pageSize: 10 # comment list page size
  avatar: mp # gravatar style https://valine.js.org/#/avatar
  # lang: zh-cn # deprecated, use html.lang instead
  placeholder: Just go go # valine comment input placeholder(like: Please leave your footprints )
  guest_info: nick,mail,link #valine comment header info
  recordIP: true # whether to record the IP address of the commenters
  highlight: true # whether to highlight the code blocks
  visitor: false # whether to display the number of visitors
  serverURLs: # leancloud server url

# https://waline.js.org/
waline:
  enable: false
  serverURL:
  # lang: zh-CN # deprecated, use html.lang instead
  locale: {} # https://waline.js.org/guide/features/i18n.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%AF%AD%E8%A8%80
  emoji:
    - https://unpkg.com/@waline/emojis@1.2.0/weibo
    - https://unpkg.com/@waline/emojis@1.2.0/alus
    - https://unpkg.com/@waline/emojis@1.2.0/bilibili
    - https://unpkg.com/@waline/emojis@1.2.0/qq
    - https://unpkg.com/@waline/emojis@1.2.0/tieba
    - https://unpkg.com/@waline/emojis@1.2.0/tw-emoji
  meta:
    - nick
    - mail
    - link
  requiredMeta:
    - nick
    - mail
  wordLimit: 0
  pageSize: 10
  pageview: true

# https://twikoo.js.org
twikoo:
  enable: true
  envId: https://xxx.vercel.app # 腾讯云环境填 envId；Vercel 环境填地址（https://xxx.vercel.app）
  region:

# https://github.com/gitalk/gitalk/blob/master/readme-cn.md
gitalk:
  enable: false
  clientID:
  clientSecret:
  repo:
  owner:
  admin:
  md5: false

# https://giscus.app/zh-CN
giscus:
  enable: false
  repo:
  repoId:
  category:
  categoryId:
  mapping: mapping
  strict: 0
  reactionsEnabled: 1
  emitMetadata: 0
  inputPosition: bottom
  # commentTheme: preferred_color_scheme invalid
  # lang: zh-CN # deprecated, use html.lang instead

# https://disqus.com
disqus:
  enable: false
  shortname: # your disqus shortname
  count: true

# https://utteranc.es
utterances:
  enable: false
  repo: owner/repo # Change this to "Your GitHub Username/The Repository Name" used for storing blog comments
  issue_term: title
  theme: auto # auto means to automatically adapt to dark and light themes, you can also use specific themes like github-light, github-dark, preferred-color-scheme, etc.

########################################
# Search
########################################

algolia_search:
  enable: true
  appID: xxx
  apiKey: xxxxxxxxxxxxxxxxxxxxxxxxx
  indexName: 'xxx'
  hits:
    per_page: 20

preloader:                            #设置预加载界面
  enable: true
  text: 
    zh-CN: 少女祈祷中...
    zh-TW: 少女祈禱中...
    en: Loading...
    ja: 少女祈祷中...
  icon: #  '/images/taichi.png' 或图片API 'https://xxxxx.com'
  rotate: true # 旋转图标

# see https://github.com/D-Sketon/aos.js
animation:                            #设置整体动画效果
  enable: true
  options:
    header:
      title: slide-up
      subTitle: slide-down
    home:
      post: fade-up
      widget: fade-up
      sidebar: fade-up
    article:
      whole: fade-up
      date: zoom-in
      category: zoom-in
      tag: zoom-in
      comment: zoom-in
      reading: zoom-in
      nav: fade-up
    archive:
      whole: fade-up
      tag: zoom-in
      category: zoom-in
      section: fade-up
      nav: fade-up

# see https://github.com/D-Sketon/mouse-firework
firework:                            #设置点击时的烟花效果
  enable: true
  disable_on_mobile: false
  options:
    excludeElements: ["a", "button"]
    particles:
      - shape: circle
        move: ["emit"]
        easing: easeOutExpo
        colors: ["var(--red-1)", "var(--red-2)", "var(--red-3)", "var(--red-4)"]
        number: 20
        duration: [1200, 1800]
        shapeOptions:
          radius: [16, 32]
          alpha: [0.3, 0.5]
      - shape: circle
        move: ["diffuse"]
        easing: easeOutExpo
        colors: ["var(--red-0)"]
        number: 1
        duration: [1200, 1800]
        shapeOptions:
          radius: 20
          alpha: [0.2, 0.5]
          lineWidth: 6

article_copyright:                            #设置版权信息，显示在文章末尾
  enable: true
  content:
    author: true
    link: true
    title: true
    date: true
    updated: false
    license: true
    license_type: by-nc-sa # https://creativecommons.org/licenses

# Back To Top
top:                                          #设置 返回顶部 按钮配置
  enable: true
  position: right # left or right
  icon:
    url: "../images/2.png" # 配置图标
    rotate: true
    mask: true # whether to use the images as a mask

outdate:                                      #设置 文章最后更新于xxx天前，可能不适用 效果
  enable: false
  daysAgo: 180 # The number of days after which the article is considered outdated
  message:
    en: This article was last updated on {time}. Please note that the content may no longer be applicable.
    zh-CN: 本文最后更新于 {time}，请注意文中内容可能已不适用。
    zh-TW: 本文最後更新於 {time}，請注意文中內容可能已不適用。
    ja: この記事は最終更新日：{time}。記載内容が現在有効でない可能性がありますのでご注意ください。

# ICP 备案
icp:
  icpnumber: # ICP备案号
  beian: # 网安备案号
  recordcode: # 网安备案链接中的recordcode参数

# 萌国 ICP 备案
moe_icp:
  icpnumber: # 萌国ICP备案号

# 赞助
sponsor:
  enable: false
  tip: 
    zh-CN: 请作者喝杯咖啡吧
    zh-TW: 請作者喝杯咖啡吧
    en: Buy me a coffee
    ja: コーヒーを買ってください
    pt-BR: Compre-me um café
  icon:
    url: "../images/3.png" # this path is relative to the css/main.css, so it needs to go up one level to reach the images folder
    rotate: true
    mask: true # whether to use the images as a mask
  qr:
    # - name: 支付宝
    #   src: "sponsor/alipay.jpg"
    # - name: 微信
    #   src: "sponsor/wechat.png"

# Share
share:
   - facebook
   - twitter
   - linkedin
   - reddit
   - weibo
   - qq
   - weixin

# 设置目录卡片
home_categories:
  enable: true
  content:
    - categories: '' # string (single-layer category) or array (multi-layer category)
      cover: 'https://xxx.com' # 目录图片，空则随机图片
    - categories:
      cover:

########################################
# Experimental features
########################################

# 代码注入器
injector:
  head_begin: # Inject code snippet right after <head>
  head_end: # Inject code snippet right before </head>
  body_begin: # Inject code snippet right after <body>
  body_end: # Inject code snippet right before </body>
  sidebar_begin: # Inject code snippet right after <aside>
  sidebar_end: # Inject code snippet right before </aside>

# Experimental, may have a lot of bugs, open with caution!
pjax:                                 #设置加速网页显示速度
  enable: true

# Experimental
# https://github.com/GoogleChromeLabs/quicklink
quicklink:                            #设置提前加载链接，利于跳转更快
  enable: true
  # The `requestIdleCallback` timeout, in milliseconds.
  timeout: 3000
  # Whether or not the URLs within the options.el container should be treated as high priority.
  # When true, quicklink will attempt to use the fetch() API if supported (rather than link[rel=prefetch]).
  priority: true
  # Determine if a URL should be prefetched.
  # Only support string
  ignores: []

# Experimental
service_worker:
  enable: false

#下方live2d和live2d_widgets只能开后者，前者无反应
# Experimental
live2d:
  enable: false
  position: right # left or right

# Experimental
live2d_widgets:
  enable: true
  position: right # left or right

# https://github.com/CodeByZach/pace
pace:
  enable: true

# 音乐播放器，现为完全开启状态，禁用则将aplayer或meting任一设置为false即可
player:
  disable_on_mobile: true
  position: before_sidebar # before_sidebar | after_sidebar | after_widget
  # if you enable meting, you must enable aplayer first
  aplayer:
    # 下方配置请前往 Aplayer官方中文文档： https://aplayer.js.org/#/zh-Hans/?id=%E5%8F%82%E6%95%B0 查看
    enable: true
    options:
      audio: []
      fixed: true
      autoplay:
      loop:
      order:
      preload:
      volume:
      mutex:
      listFolded:
      lrcType:
  meting:
    # https://github.com/metowolf/MetingJS
    enable: true
    meting_api: # custom api
    options:
      id: xxxxx
      server: netease
      type: playlist
      auto: true

########################################
# pangu.js
########################################

pangu:
  # more information: https://github.com/vinta/pangu.js 
  enable: false  # enable pangu.js to add space between Chinese and English

########################################
# Theme
########################################

# experimental, may have a lot of bugs, open with caution!
# A dynamic color generation tool based on Google's Material You design guidelines, capable of extracting primary colors from any image and generating complete light and dark color schemes.
material_theme:
  # more information: https://github.com/2061360308/material-theme
  # notice: when you enable this feature, all the covers will be automatically added "crossorigin" attribute to support the dynamic color scheme
  # so make sure your custom covers will not be blocked by the browser's CORS policy
  enable: false # enable material_theme to generate dynamic color schemes based on the banner image

internal_theme:                            #设置主题强调色
  light:
    --red-0: '#ffb7c5'
    --red-1: '#ffb7c2'
    --red-2: '#ff7c7c'
    --red-3: '#ffafaf'
    --red-4: '#ffd0d0'
    --red-5: '#ffecec'
    --red-5-5: '#fff3f3'
    --red-6: '#fff7f7'
    --color-red-6-shadow: 'rgba(255, 78, 78, 0.6)'
    --color-red-3-shadow: 'rgba(255, 78, 78, 0.3)'

    --highlight-nav: "#f5f5f5"
    --highlight-scrollbar: "#d6d6d6"
    --highlight-background: "#fdfdfd"
    --highlight-selection: "#e9e9e988"
    --highlight-foreground: "#24292e"
    --highlight-comment: "#7d7d7d"
    --highlight-red: "#d73a49"
    --highlight-orange: "#e36209"
    --highlight-yellow: "#cb911d"
    --highlight-green: "#22863a"
    --highlight-aqua: "#005cc5"
    --highlight-blue: "#032f62"
    --highlight-purple: "#6f42c1"
    --highlight-deletion: "#b31d28"
    --highlight-deletion-bg: "#ffeef0"
    --highlight-addition: "#22863a"
    --highlight-addition-bg: "#f0fff4"
  dark:
    --red-4: 'rgba(255, 208, 208, 0.5)'
    --red-5: 'rgba(255,228,228,0.15)'
    --red-5-5: 'rgba(255,236,236,0.05)'
    --red-6: 'rgba(255, 243, 243, 0.2)'

    --highlight-nav: "#222830"
    --highlight-scrollbar: "#454d59"
    --highlight-background: "#1e2027"
    --highlight-selection: "#51515155"
    --highlight-foreground: "#c9d1d9"
    --highlight-comment: "#8b949e"
    --highlight-red: "#ff7b72"
    --highlight-orange: "#ffa657"
    --highlight-yellow: "#ffcc66"
    --highlight-green: "#7ee787"
    --highlight-aqua: "#a5d6ff"
    --highlight-blue: "#79c0ff"
    --highlight-purple: "#d2a8ff"
    --highlight-deletion: "#ffa198"
    --highlight-deletion-bg: "#490202"
    --highlight-addition: "#7ee787"
    --highlight-addition-bg: "#04260f"

```
### 3.3 文章页写法
1. 在 /blog/blog 的地址栏呼出命令行，输入 `hugo new content post/blog1/index.md` ，或在 /blog/blog/content 文件夹内新建文件夹为 blog1，在 blog1文件夹内新建文件 index.md，打开文件，若需使用短代码功能请见下方 4.3节

2. 请根据需要分别复制以下代码，**两段代码用途不一样，按需选择**

```文章示例1.md
# 这个是纯文章的格式
+++
data = '2026-01-01T09:00:00+09:00'
weight = 1                                     # 这个是文章置顶的格式
+++
内容：巴拉巴拉巴拉巴拉巴拉巴拉巴拉巴拉
```

```文章示例2.md
# 这个是含设置标签或目录的格式
---
data: '2026-01-01T09:00:00+09:00'
weight: 1                                     # 这个是文章置顶的格式
tags:
  - 标签1
  - 标签2
categories:
  - 目录1
  - 目录2                                     # 需在 params.yml 配置第二个目录名，否则无法显示
---
内容：巴拉巴拉巴拉巴拉巴拉巴拉巴拉巴拉
```

### 3.4 短代码的使用
有些时候想让你的网页功能更多时，可能会需要使用更丰富的代码，但是，Hugo框架限制了某些地方，但为了使用者的需要，还留了一个后门：短代码

若需使用对应代码，请自行删除如 !{ 间、两行间或 {{ 间的空格，确保代码可运行

在编写时已确认代码可行，若不行则自行调整代码空格即可
#### heatMapCard 文章热力图
该卡片仅能显示在      **文章页**     的开头，主页或其他位置均无法显示！！

若想查看热力图显示效果，请返回文章开头处！

若想在文章页使用该卡片，参考下方代码，如：
```文章页.md
{ {< heatMapCard levelStandard="?" >} }
{{< heatMapCard levelStandard="?" >}}
```
#### 友链卡片
若content内有friend.md文件，参考下方代码，如：

```文章页.md
{ {< friendsLink >} }
{{< friendsLink >}}
```
#### tagRoulette 标签轮盘
该卡片仅能显示在文章页

若想使用该卡片，参考下方代码，自定义修改tags和icon

用法：点击icon后会使用随机显示效果随机显示一个tag

如：
```文章页.md
{ {< tagRoulette tags="标签1,标签2,标签3" icon="🎲" >} }
{{< tagRoulette tags="标签1,标签2,标签3" icon="🎲" >}}
```
#### alertBlockquote 块引用
该卡片仅能显示在文章页

若想在文章页使用该卡片，参考下方代码，如：
```文章页.md
{ {< alertBlockquote type="0721" >}}
Ciallo～(∠・ω< )⌒★
{ {</alertBlockquote>} }

{{< alertBlockquote type="0721" >}}
Ciallo～(∠・ω< )⌒★
{{</alertBlockquote>}}

或可使用类似模块：note、tips、important、warning等，其他模块类似，如：

{ {< alertBlockquote type="note" >} }
绫地宁宁 is coming~
{ {< /alertBlockquote  >} }

{{< alertBlockquote type="note" >}}
绫地宁宁 is coming~
{{< /alertBlockquote  >}}
```
#### link 链接卡片
该卡片仅能显示在文章页

若想在文章页使用该卡片，参考下方代码，如：
```文章页.md
{ {< link title="0721" link="https://ciallo.cc" cover="auto" escape="true" >} }
{{< link title="0721" link="https://ciallo.cc" cover="auto" escape="true" >}}或

{ {< link title="0721" path="https://ciallo.cc" cover="auto" escape="true" >} }
{{< link title="0721" path="https://ciallo.cc" cover="auto" escape="true" >}}
```
#### gallery 照片墙
该卡片仅能显示在文章页

若想在文章页使用该卡片，参考下方代码，如：
```文章页.md
{ {< gallery >} }

! [图1] (https://xxx.com/xxx.webp)

! [图2] (https://xxx.com/xxx.webp)
...
{ {</gallery>} }
{{< gallery >}}
![图1](https://tc.alcy.cc/tc/20260121/2c785c4a015e858d9d470d51ca702a1a.webp)
![图2](https://tc.alcy.cc/tc/20260121/6c4e58cf6f6ce1cc144f834ba98d22fa.webp)
...
{{</gallery>}}
```
#### grid 网格布局
该卡片仅能显示在文章页

若想在文章页使用该卡片，参考下方代码，如：
```文章页.md
{ {< grid width=300 col=3 >} }

<!-- cell -->
07211

<!-- cell -->
07212

<!-- cell -->
07213

{ {< /grid >} }


展示效果：

{{< grid width=300 col=3 >}}
<!-- cell -->
07211
<!-- cell -->
07212
<!-- cell -->
07213
{{< /grid >}}
```
#### details 折叠面板
该卡片仅能显示在文章页

若想在文章页使用该卡片，参考下方代码，如：
```文章页.md
{ {< details summary="0721" >} }

Ciallo～(∠・ω< )⌒★

{ {< /details >} }
{{< details summary="0721" >}}
Ciallo～(∠・ω< )⌒★
{{< /details >}}
```

## 4.博客部署
### 4.1 标准部署（**首次必须使用该方式**）
**下方步骤的每一点都要做对，否则都会部署失败！！**

{{< alertBlockquote type="warning" >}}
此处需将example更改为你自己的username，不要傻乎乎地把我给的example粘贴上去，如某人的Github主页是 `https://github.com/ciallo` ，此处的example应为ciallo，TA的仓库名应为 ciallo.github.io，后面有关仓库名的部分也是如此

还有一件事~

**请确保你的username应全为小写，若出现大写或数字开头等情况都会无法使用！！！**
{{< /alertBlockquote  >}}

1. 登录你的Github账号，在 https://github.com/example?tab=repositories 界面新建一个仓库，仓库名为 `example.github.io`

{{< details summary="还在犹豫仓库名填什么？" >}}
如果你后续都不打算使用自己的域名的话可以加后缀，**但是！！！**

如果你仓库名没有使用过 `example.github.io`，那么你博客 **一定**要用这个名

不管你是否想好了其他仓库名，不然后续部署极大概率出现样式没渲染或页面显示空白等情况
{{< /details >}}

2. 打开 /blog/blog/hugo.toml ，修改 baseurl为 `https://example.github.io/`，保存

3. 删除 /blog/blog/public 文件夹，在 /blog/blog文件夹地址栏呼出的命令行处输入 `hugo -D`以生成新的 public文件夹

{{< details summary="为什么要这么做？" >}}
**删除缓存，提高网站运行速度**
{{< /details >}}

之后再依此输入以下代码：

```
cd public                    # 若手动进 public文件夹则不用输这行
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/example/blog.git         # 记得改
git push -u origin main                                              # 若失败则把 -u 改为 -f
```

4. 待显示100%进度后，刷新github界面，此时能看到正在运行的仓库，点击 Settings，点击 Pages，在 Branch栏选择 main，之后点击 Save

5. 稍等几分钟后，刷新，在本页面可看到地址，如 `Your site is live at https://example.github.io/` ，即表示部署成功

{{< details summary="救命锦囊" >}}
如果进入博客后出现无渲染等情况，删除仓库，同时打开 显示隐藏的项目，删除 /blog/blog/.git文件夹，重新按此部分操作即可
{{< /details >}}

### 4.2 自动部署（**已完成初次部署，后续需更新代码**）
1. 在 /blog/blog 中新建文件 .gitignore，打开文件，将以下内容复制到文件中后保存

```.gitignore
public
resources
.hugo_build.lock
hugo.exe
```

2. 在 /blog/blog 内创建 .github文件夹，在 .github文件夹内创建 workflows文件夹，在 workflows文件夹内新建文件 hugo_deploy.yaml，将以下代码复制到文件中后保存

```hugo_deploy.yaml
name: deploy

# 代码提交到main分支时触发github action
on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
        - name: Checkout
          uses: actions/checkout@v4
          with:
              fetch-depth: 0

        - name: Setup Hugo
          uses: peaceiris/actions-hugo@v3
          with:
              hugo-version: "latest"
              extended: true

        - name: Build Web
          run: hugo -D

        - name: Deploy Web
          uses: peaceiris/actions-gh-pages@v4
          with:
              PERSONAL_TOKEN: ${{ secrets.TOKEN }}
              EXTERNAL_REPOSITORY: [你的github名/你的仓库名]                         # 记得改，不然运行不了
              PUBLISH_BRANCH: main
              PUBLISH_DIR: ./public
              commit_message: auto deploy

```

3. 在仓库界面点击头像，点击 Settings，下滑至**侧边栏**的最底下，点击 Developer settings，点击 Personal access tokens，点击显示的 Token(classic)，点击 Generate new token，点击Generate new token(classic)

4. 验证用户，可选输入 Note，Expiration 选择 no expiration，Select scopes勾选 repo的全部和 workflow，下滑到最后点击Generate token，**非常重要**，生成的密钥一定要复制保存，只显示一次！！！

5. 返回创建的仓库页面，点击Settings，下滑，在侧边栏找到 Security内的 Secret and variables，点击Actions，

6. 在 Repository secrets处点击 New repository secret，按下方模板填写，填写完成后点击 Add secret

| Name* | Secret* |
| :---: | :---: |
| TOKEN | ghp_xxxxxxxxxxxxxxxxxxxxxx |

上方的 `ghp_xxxxxxxxxxxxxxxxx`为刚刚第4步生成的密钥粘贴在这

7. 删除 /blog/blog/public 文件夹，在 /blog/blog文件夹地址栏呼出的命令行处输入 `hugo -D`以生成新的 public文件夹

8. 在 /blog/blog文件夹的地址栏输入cmd呼出命令行，根据具体情况分别输入以下代码（ **看清楚适用的情况，爆红了自己检查**），为确保上传成功，请常开代理

```
#######################
以下代码用于首次使用自动部署时
#######################
git init
git add .
git commit -m "update"
git branch -M main
git remote add origin https://github.com/example/blog.git         # 记得改
git push -u origin main         # 若失败则输入 `git push -f origin main`

#######################
以下代码用于已完成第一次自动部署时
#######################
git add .
git commit -m "update"
git push                        # 若失败则输入 `git push -u origin main` 或 `git push -f origin main` 
```

9. 返回 Github界面，点击 Action，当看到当前两个工作流（网页显示workflow）成功运行，显示地址时即可查看博客

{{< details summary="想使用自定义域名？" >}}
如果想使用其他域名，请参考以下配置，以Cloudflare和华为云为例，其他域名服务商的配置可能有所不同

配置域名方法：
1. 在Cloudflare处完成主域名解析，比如在 Cloudflare内解析了一个主域名如 `example.com`，在执行以下操作前决定好博客要用什么域名，比如 `blog.example.com`，注意，此处应为二级域名，且仅能为二级域名，其他级别的域名都会绑不上华为云或后续绑好后无法访问博客

2. 打开你的梯子且 **一定要开全局！！**，否则注册的是国区而非国际区，之后打开[华为云](https://www.huaweicloud.com/intl/zh-cn/)官网，注册账号，用邮箱注册即可，中途如果需要你做登录验证的点选"我已知悉"后点"暂不开启"，需要你绑定手机号的一律跳过！！
3. 进入控制台后，在 我的资源 栏点击 公网域名 ，添加域名为刚第 1 步决定好的域名，添加成功后按下面模板分别在 Cloudflare和华为云内配置

```
Cloudflare配置：

| 类型 | 名称 | 名称服务器 | TTL |
| :---: | :---: | :---: | :---: |
| NS | blog | ns1.huaweicloud-dns.org | 自动 |
| NS | blog | ns1.huaweicloud-dns.net | 自动 |
| NS | blog | ns1.huaweicloud-dns.com | 自动 |
| NS | blog | ns1.huaweicloud-dns.cn | 自动 |

华为云配置需点击 添加记录集，填写下面信息后确定：

| 记录类型 | 主机记录 | 线路类型 | TTL（秒）| 记录值 |
| :---: | :---: | :---: | :---: | :---: |
| CNAME | 不填 | 全网默认 | 300 | Github仓库名 |

```

4. 在 Github 的 Setting - Pages页面，下滑找到 Custom domain，输入域名，点击 Save，再是**一定要可勾选** "Enforce HTTPS"，等待生效即可。若点了 Save，Enforce HTTPS为灰色的不可勾选状态就是有地方配置错了，返回去检查.
5. 这样配置完成后，访问域名 `https://blog.example.com`即可进入博客啦~
{{< /details >}}