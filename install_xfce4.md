# Arch 安装 xfce4

## xorg 所有桌面环境都依赖于它

```bash
pacman -S xorg xorg-xinit
```

## xfce4 

```bash
pacman -S xfce4
pacman -S xfce4-goodies # 一些xfce工具包
```



## lightdm 登录管理器

```bash
pacman -S lightdm
pacman -S lightdm-gtk-greeter
# 开机启动
systemctl enable lightdm

```

### 网络

```bash
pacmsn -S iw wpa_supplicant networkmanager
# 开启启动
systemctl enable NetworkManager
```



## 驱动相关

### 显卡驱动

根据自己的需要安装

![](<https://raw.githubusercontent.com/xiaozefeng/archlinux-guide/master/images/display_driver.png>)

我是intel 的机器

```bash
pacman -S xf86-video-intel mesa lib32-mesa
```



### Intel 微码

```bash
pacman -S intel-ucode
```





## 生成用户目录

```bash
pacman -S xdg-user-dirs
```



## 重启

**开启启动使用默认pane**





## 配置pacman ArchlinuxCN 镜像

```bash
vim /ect/pacman.conf
# 取消对 [multiple]的注释
# 添加清华大学的源
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
pacman -Syy
# 导入 gpg key
pacman -S archlinuxcn-keyring 
```



## 安装AUR helper

```bash
pacman -S yaourt yay
```

## ZSH  & Oh-My-Zsh

```bash
sudo pacman -S zsh
sudo pacman -S git
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh
chmod +x install.sh
./install.sh
```



## ZSH 自动补全

#### `zsh-autosuggestions`自动补全插件

1. 下载插件到 oh-my-zsh 的 plugins 目录下

```bash
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

2. 配置 zsh-autosuggestions

```bash
vim ~/.zshrc
# 修改这行 添加 zsh-autosuggestions
plugins=(git zsh-autosuggestions)
```

3. 生效配置

```bash
source ~/.zhsrc
```



## 常用软件

```bahs
pacman -S google-chrome netease-cloud-music
```



# 声音调节

```bash
pacman -S pulseaudio pulseaudio-alsa alsa-utils alsa-oss 
# 混音器安装
pacman -S pavucontrol
```

### 字体

```bash
pacman -S wqy-zenhei wqy-microhei  noto-fonts
```



## 输入法

```bash
pacman -S fcitx-qt4 fcitx-configtool fcitx-sogoupinyin
# 配置
vim ~/.xprofile
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
export LC_CTYPE=en_US.UTF-8

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=”@im=fcitx”
```





## 配置桌面

### 主题安装

```bash
sudo pacman -S arc-icon-theme papirus-icon-theme numix-icon-theme-git
sudo pacman -S arc-gtk-theme materia-gtk-theme numix-gtk-theme
```



### 其他

#### 弹出菜单

xfce4-popup-windowmenu 可以在键盘中设置对应的快捷键

设置 dm-tool 到对应的命令 



