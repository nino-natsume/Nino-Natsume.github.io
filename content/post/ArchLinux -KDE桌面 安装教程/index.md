+++
date = '2026-05-05T22:00:00+09:00'
title = 'ArchLinux - KDE桌面 安装教程'
+++
{{< heatMapCard levelStandard="200,500,1000" >}}
## 前言
本教程为自用，意在于记录搭建、部署、设置及美化所需的所有代码，可能不适合所有人，仅为个人设置模板
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
| Profile | Desktop:KDE  Graphics:Nvidia  Greeter:SDDM |
| Applications | Audio:pipewire  Firewall:ufw   Additional fonts:全选 |
| Pacman | color:on |
| Network | default backend |
| Additional | linux-lts-headers  btrfs-progs  os-prober |
| / | ↑上三非必须  下面供参考↓  |
| / | 建议安装 → base-devel  git  go |
| / | **注意：若上方 Applications - Additional fonts 已经全选，则下方字体无需再勾选！！** |
| / | noto-fonts  noto-fonts-cjk  noto-fonts-emoji noto-fonts-extra  ttf-dejavu |
| Timezone | Asia/Shanhai(Tokyo) |

install

Reboot
{{< /details >}}

# 完整代码
以下代码按需部分执行

```README.md
The is supported for ArchLinux **KDE** Desktop!

# 优先安装 yay，以下代码建议都打，不然安装很慢 
cd ~
git clone https://aur.archlinux.org/yay.git
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
cd yay
makepkg -si

# 第二行的五个字体必须安装，否则乱码
sudo pacman -S zsh linux-lts-headers btrfs-progs os-prober base-devel git go sddm
noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra ttf-dejavu

# zsh的安装与美化
chsh -s /bin/zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

sudo nano ~/.zshrc
ZSH_THEME="jonathan"   # 自定义主题
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)   # 修改插件引用配置
source ~/.zshrc     # 应用生效

# 安装 Makima-SDDM
cd ~
yay -S otf-ipafont qt5-graphicaleffects qt5-quickcontrols2
sudo git clone https://github.com/Arnau029/Makima-SDDM.git
sudo mv Makima-SDDM /usr/share/sddm/themes
sudo nano /etc/sddm.conf     # 以下内容写入该文件内

[Theme]
# Current the name
Current=Makima-SDDM

# 安装 GRUB 主题
cd ~     # 切换为普通用户
git clone https://github.com/13atm01/GRUB-Theme.git
cd GRUB-Theme
cd "Hoshimati Suisei"
sudo bash ./install.sh
sudo grub-mkconfig -o /boot/grub/grub.cfg

# 当当前设备至少有两个系统（win+arch），且需要多系统引导时选装
cd /
sudo nano /etc/default/grub     # 删除最后一行的 # 号
su
mount /dev/sda1 /mnt     # sda1为win系统的EFI分区，不清楚需 lsblk查看
os-prober
grub-mkconfig -o /boot/grub/grub.cfg

sudo reboot
```