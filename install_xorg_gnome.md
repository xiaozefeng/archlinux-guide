# Archlinux 安装 gnome for xorg

## 编辑 /etc/pacman.conf

```bash
# 安装 32位的软件
vim /etc/pacman.conf
# 打开对 multilib 的注释
[multilib]
# 加入清华大学的镜像
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
# 导入 gpg
pacman -S archlinuxcn-keyring
```



## 显卡驱动

![](<https://github.com/xiaozefeng/archlinux-guide/raw/master/images/display_driver.png>)

```bash
# 这里看个人情况，我这里安装的时intel
pacman -S xf86-video-intel
# 如果使用笔记本还需要触摸板驱动
pacman -S xf86-input-synaptics
# 但是社区建议使用 mesa, 所以我也装上了
pacman -S mesa
# 支持32位的程序
pacman -S lib32-mesa
```

## 安装Xorg

Xorg是各种桌面环境的必备服务, 先安装它

```bash
pacman -S xorg
```

## 安装 gnome

```bash
pacman -S gnome
# gnome 配置工具包
pacman -S gnome-tweaks
```

## 安装GDM窗口管理器

```bash
pacman -S gdm
# 开机启动
systemctl enable gdm
```

## 安装字体

```bash
pacman -S wqy-zenhei wqy-micro
```

## 安装输入法 (有争议)

[gnome安装fcitx-sogoupinyin](<https://github.com/xiaozefeng/archlinux-guide/issues/9>)



## 安装 Docker

[安装Docker且加速](<https://github.com/xiaozefeng/archlinux-guide/issues/10>)

## 安装 Dropbox

[两种方式安装Dropbox](<https://github.com/xiaozefeng/archlinux-guide/issues/8>)