+++
date = '2026-05-04T22:51:01+09:00'
draft = true
title = 'ArchLinux安装教程（单盘双系统版）'
+++
{{< heatMapCard levelStandard="200,500,1000" >}}
## 前言
本教程为自用，意在于记录搭建、部署、设置及美化所需的所有代码，可能不适合所有人，仅为个人设置模板
### 必备
1. 已制作好的 ArchLinux 启动盘，使用 Refus 制作，注意选择分区类型为 GPT

2. 已压缩好一定大小分区的设备（至少50G+）
## 相关设置
### 安装系统
按序输入以下代码

```README.md
rfkill unblock wifi
iwctl
station wlan0 connect [WIFI名]  # 要输入密码
station list
exit
ping bing.com
lsblk

| Size | FS type | Mountpoint | Btrf |
| 1G(2G) | fat32 | /boot | / |
| 2G(8G) | linux-swap | / | / |
| 剩下所有 | btrfs | / | 两分区（@,/;@home,/home） |

archinstall

| 步骤 | 参考值（本设置以20260501版本为例，其他按需） |
| :---: | :---: |
| Mirrors | China |
| Disk | Manual(相关磁盘设置参考上表) |
| Swap | Disabled |
| Boot | Grub |
| Hostname、User account | 按需设置 |
| Profile | Desktop:KDE(第二个)  Graphics:Nvidia(open-source)  Greeter:SDDM |
| Applications | Audio:pipewire  Firewall:ufw  其余按需 |
| Pacman | color:on |
| Network | default backend |
| Additional | linux-lts-headers  btrfs-progs  os-prober  (↑上三必须 | 下面参考↓)
| / | base-devel  git  go  noto-fonts  noto-fonts-cjk  |
| / | noto-fonts-emoji  noto-fonts-extra  ttf-dejavu  ttf-liberation |
| Timezone | Asia/Tokyo |

install - yes
Reboot
```

### 连接网络
先看右下角有无wifi图标，有则直接连，无则输入对应代码

对应安装系统时的选项输入，**不要都输！**

| default | iwd |
| :---: | :---: |
| nmtui | iwctl |
| 连接wifi | 后续代码与初始代码一致，不再赘述 |

### 设置中文
按序输入以下代码

```README.md
sudo nano /etc/locale.conf
# 与 zh_CN 有关的都删掉 # 号，ctrl+x - y

sudo locale-gen
su
echo 'LANG=zh_CN.UTF-8' > /etc/locale.conf

# 设置选择简中
Reboot
```

### 多系统引导
按序输入以下代码

```README.md
cd /
sudo nano /etc/default/grub
# 删除最后一行的 # 号

su
lsblk # 找到win系统的EFI分区名称
mount /dev/sda1 /mnt
os-prober
grub-mkconfig -o /boot/grub/grub.cfg
# 显示完成或 done 表示成功，重启即可
```

## 各项配置
### 主题配置
按需配置

### 终端美化
输入以下代码

```README.md
sudo pacman -S zsh
chsh -s /bin/zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
nano ~/.zshrc
ZSH_THEME="agnoster"   # 找到主题设置行并修改
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)   # 修改插件引用配置

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

source ~/.zshrc   # 保存并应用更改
```

### GRUB 美化
**按需**输入以下代码

```README.md
cd ~
git clone https://github.com/13atm01/GRUB-Theme.git
cd 'Hoshimati Suisei'
sudo sh ./install.sh
sudo grub-mkconfig -o /boot/grub/grub.cfg
sudo reboot
```

### 安装 Makima-SDDM
**按需**输入以下代码

```README.md
# 注意：yay需要在普通用户下安装，以下步骤同
cd ~
git clone https://aur.archlinux.org/yay.git
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
cd yay
makepkg -si

# 安装Makima-sddm
sudo pacman -S sddm noto-fonts-cjk
yay -S otf-ipafont qt5-graphicaleffects qt5-quickcontrols2
sudo git clone https://github.com/Arnau029/Makima-SDDM.git
sudo mv Makima-SDDM /usr/share/sddm/themes
sudo nano /etc/sddm.conf
# 以下内容至sddm.conf内
[Theme]
#Current the name
Current=Makima-SDDM
```

Reboot后即可看到登录页面修改

### 显卡、声音设置
按需输入代码

```README.md
Nvidia显卡(TUxxx):
yay -S nvidia-settings nvidia-utils nvidia-open-dkms

声音:
先试已有的，都没用再根据型号下驱动
```

### 其他设置
按需设置