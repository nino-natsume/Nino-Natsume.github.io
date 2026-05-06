+++
date = '2026-05-05T22:00:00+09:00'
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

{{< details summary="完整代码" >}}
依此输入以下代码

rfkill unblock wifi

iwctl

station wlan0 connect [WIFI名，要删括号！！]  # 要输入密码

station list

exit

ping bing.com

lsblk     # 下表中 Size 为参考数值，根据实际大小修改，*斜体*的为安装时需设置的内容

| Size | FS type | Mountpoint | Btrf |
| :---: | :---: | :---: | :---: |
| 1G | fat32 | `*/boot*` | / |
| 2G | linux-swap | / | / |
| /(根目录，非空标识) | btrfs | / | `*两分区（@,/;@home,/home）*` |

archinstall

| 步骤 | 参考值（本设置以20260501版本为例，其他按需） |
| :---: | :---: |
| Mirrors | China |
| Disk | Manual(相关磁盘设置参考上表，记得分好区后先格式化再设置分区) |
| Swap | Disabled |
| Boot | Grub |
| Hostname、User account | 按需设置 |
| Profile | Desktop:KDE(第二个)  Graphics:Nvidia(open-source)  Greeter:SDDM |
| Applications | Audio:pipewire  Firewall:ufw   Additional fonts:全选  其他按需设置 |
| Pacman | color:on |
| Network | default backend |
| Additional | linux-lts-headers  btrfs-progs  os-prober  (↑上三必须  下面参考↓) |
| / | base-devel  git  go |
| / | **注意：若上方 Applications - Additional fonts 已经全选，则下方字体无需再勾选！！** |
| / | noto-fonts  noto-fonts-cjk  noto-fonts-emoji |
| / | noto-fonts-extra  ttf-dejavu  ttf-liberation |
| Timezone | Asia/Tokyo |

install - yes
Reboot
{{< /details >}}

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

# 完整代码
以下代码按需执行，可直接复制使用

```README.md
# 优先执行 yay 安装和 库安装；自动调整运行顺序，优先运行无需root权限的代码，再执行其他代码
cd ~
git clone https://aur.archlinux.org/yay.git
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
cd yay
makepkg -si

sudo pacman -S zsh linux-lts-headers btrfs-progs git go sddm os-prober base-devel
# 若安装系统时未勾选则需补充安装字体→ noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra ttf-dejavu ttf-liberation

#设置中文
sudo nano /etc/locale.conf     # 删除4个前缀为 zh_CN 的 # 号，保存并退出
sudo locale-gen
su
echo 'LANG=zh_CN.UTF-8' > /etc/locale.conf     # 设置内修改语言为 简体中文，若想省掉一次重启，可先不设置

# zsh美化
chsh -s /bin/zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo nano ~/.zshrc
ZSH_THEME="jonathan"   # 自定义主题
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)   # 修改插件引用配置

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
source ~/.zshrc     # 应用生效

# 安装 Makima-SDDM
cd ~
yay -S otf-ipafont qt5-graphicaleffects qt5-quickcontrols2
sudo git clone https://github.com/Arnau029/Makima-SDDM.git
sudo mv Makima-SDDM /usr/share/sddm/themes
sudo nano /etc/sddm.conf     # 以下内容写入该文件内
[Theme]
#Current the name
Current=Makima-SDDM

# 当当前设备至少有两个系统，且完成多系统引导时选装
cd ~
git clone https://github.com/13atm01/GRUB-Theme.git
cd ~/GRUB-Theme
cd 'Hoshimati Suisei'     # 不一定一定要这个主题，自己在仓库里挑
sudo sh ./install.sh
sudo grub-mkconfig -o /boot/grub/grub.cfg

# 自选安装
yay -S neofetch-git microsoft-edge-stable-bin

sudo reboot
```