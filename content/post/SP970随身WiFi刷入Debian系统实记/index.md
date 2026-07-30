---
date: '2026-07-30T13:00:00+09:00'
title: 'SP970随身WiFi刷入Debian系统'
categories:
  - 
tags:
  - 教程
---
{{< heatMapCard levelStandard="200,500,2000" >}}
### 必备
1. SP970板子的随身WiFi x1

2. 对应的刷机包 x1
## 步骤
1. 下载[刷机包](https://1812799543.share.123pan.cn/123pan/BVBKVv-RXHHd?pwd=0721#) 至本地位置，解压备用

2. 将随身WiFi插入电脑，打开【设备管理器】，查看是否显示设备为 `Android Devices`，查找教程，使设备更新为 `远程 NDIS 兼容设备`

3. 进入刷机包文件夹，地址栏输入cmd运行 `adb devices`，查看设备是否在线

| 左 | 右 |
| :---: | :---: |
| List of devices attached | / |
| 0123456789 | device |

4. 退出cmd，打开刷机包内的 `一键刷入工具.bat`，选择想要的频率版本序号，之后傻瓜操作，根据提示按回车键即可，等待提示完成后，插拔随身WiFi，等待完全开机后通过SSH连接设备，之后跟Debian内的操作一致，配置网络、更新国内源、安装什么你想装的，故不再赘述

5. 可能出现安装Docker后安装应用提示无法创建endpoint，建议抛弃Docker，使用源码编译方式