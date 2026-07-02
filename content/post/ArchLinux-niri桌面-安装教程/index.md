+++
date = '2026-07-02T22:00:00+09:00'
title = 'ArchLinux Niri桌面 安装教程'
+++
{{< heatMapCard levelStandard="200,500,1000" >}}
## 前言
1、本教程使用于 niri 桌面环境，请注意检查

2、本教程为自用，意在于记录搭建、部署、设置及美化所需的所有代码，可能不适合所有人，仅为个人设置模板
### 必备
1. 已制作好的 ArchLinux 启动盘，使用 Refus 制作，注意选择分区类型为 GPT

2. 已压缩好一定大小分区的设备（至少50G+）
## 相关设置
### 安装系统
依此输入以下代码或进行对应操作

{{< details summary="代码" >}}
rfkill unblock wifi

iwctl

station wlan0 connect [WIFI名，要删括号 →]  # 要输入密码

station list

exit

ping bing.com

lsblk     # 下表中 Size 为参考数值，根据实际大小修改，*斜体*的为安装时需设置的内容

| Size | FS type | Mountpoint | Btrf |
| :---: | :---: | :---: | :---: |
| 1G | fat32 | `*/boot*` | / |
| 2G | linux-swap | / | / |
| /(根目录，非空标识) | btrfs | / | `*两分区（@,/ ; @home,/home）*` |

archinstall

| 步骤 | 参考值（本设置以20260701版本为例，其他按需） |
| :---: | :---: |
| Locale | zh-CN.UTF-8 |
| Mirrors | China |
| Disk | Manual(相关磁盘设置参考上表，记得分好区后先格式化再设置分区) |
| Swap | Disabled |
| Boot | Grub |
| Hostname、User account | 按需设置 |
| Profile | Desktop:niri;polkit  Graphics:Nvidia  Greeter:SDDM |
| Applications | Audio:pipewire  Firewall:ufw   Additional fonts:全选 |
| Pacman | color:on |
| Network | iwd backend |
| Additional | base-devel  git  go  nano |
| Timezone | Asia/Shanhai(Tokyo) |

install

Reboot
{{< /details >}}

## 基础操作
部分快捷键操作，完整的自己上网搜，其中 MOD 键为键盘的 win 键，安装完成后，部分快捷键使用主题配置的快捷键

| 操作 | 快捷键 |
| :---: | :---: |
| 打开终端 | MOD + T |
| 呼出软件菜单 | MOD + D |
| 复制 | Ctrl + Shift + C |
| 粘贴 | Ctrl + Shift + V |

# 完整代码
本教程与KDE环境教程代码稍有不同，想自己一点点设置就找上期，不想设置就输入以下代码，使用现成主题

```README.md
# 依旧优先安装 yay 
cd ~
git clone https://aur.archlinux.org/yay.git
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
cd yay
makepkg -si

# 添加 archlinuxcn 源
sudo nano /etc/pacman.conf               # 文件底部添加代码

[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

sudo pacman -Sy archlinuxcn-keyring

-----------------------------------------------------------------------
# 【非必须】安装 Makima-SDDM 和 GRUB-Theme，此部分选装
cd ~
yay -S otf-ipafont qt5-graphicaleffects qt5-quickcontrols2

git clone https://gh-proxy.com/github.com/13atm01/GRUB-Theme.git && git clone https://gh-proxy.com/github.com/Arnau029/Makima-SDDM.git && sudo mv Makima-SDDM /usr/share/sddm/themes

cd GRUB-Theme && cd "Hoshimati Suisei"

sudo bash ./install.sh && sudo grub-mkconfig -o /boot/grub/grub.cfg

sudo nano /etc/sddm.conf     # 以下内容写入该文件内

[Theme]
# Current the name
Current=Makima-SDDM

-----------------------------------------------------------------------
# 安装代理软件，协议不同安装的软件不同，如 clash-verge、flclash 等
yay -S clash-verge-rev                    # 安装完成后自行设置，并开启 TUN 模式

# 打开好软件后就不要关了，后续直至完成都将呼不出菜单了
yay -R fuzzel

# 运行以下代码
sudo pacman -Syu && git clone https://gh-proxy.com/github.com/SHORiN-KiWATA/shorin-arch-setup.git && cd shorin-arch-setup && sudo bash istall.sh

# 中途需操作，看[教程](https://www.bilibili.com/video/BV1Q12tBEE8e/?)操作，或按下方文字操作
# 大致需操作几步，期间请确保梯子没掉且速度良好

| 步骤 | 操作 |
| :---: | :---: |
| 桌面选择 | shorin niri |
| 第二步 | 直接回车 |
| 提示是否更换镜像？ | 输入 n ，回车 |
| 等待 | 等待 |
| flat开头的 | 直接回车 |
| 是否安装软件？ | 按需，n:全部按序安装 ; y:选哪个安哪个 |
| 等待 | 直至完成 |
```

# 结语
安装完成后重启，输入密码即可