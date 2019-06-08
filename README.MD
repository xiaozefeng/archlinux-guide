## Archlinux 安装指南

## 下载镜像文件

https://www.archlinux.org/download/

打开链接找到 China , 下面有很多国内的镜像

## 烧录启动盘

所谓的 "启动盘烧录" 其实就是将 Archlinux 系统写进 U盘。

如果你是 mac 电脑，可以参考我写的这篇文章去烧制

[OSX 下如何烧制系统启动盘](<https://github.com/xiaozefeng/archlinux-guide/issues/16>)

如果你是Linux 操作系统，可以参考这一下这一篇文章

[Linux 下烧制系统启动盘](<https://github.com/xiaozefeng/archlinux-guide/issues/17>)

## 从U盘启动

不同型号的电脑如何设置从U盘启动， 百度一下

如果是UEFI启动方式的话可能还需要设置 boot security 选项为 为 false

我用来装archlinux 的电脑是 Tinkpad S2 启动键进入 uefi 是通过 F12



## 网上设置 

如果你使用虚拟机安装可以跳过这一步

首先要保证你电脑要联网，因为安装系统需要从互联网获取数据

### 有线联网 (路由器)

插上网线， U盘启动会自动启动 dhcpch服务， 所以会自动联网, 需要拨号上网的自行找资料

如果插上了网线没有网络，可以手动启动 dhcpcd 服务

```bash
systemctl start dhcpcd.service
```



### 无线联网

```bash
# wifi-menu 会打开一个弹出框，让你选择wifi
# 选择wifi 输入密码 (如果有的话)
wifi-menu 
```



**测试是否联网**

```bash
ping www.sina.com.cn
```

```
PING spool.grid.sinaedge.com (111.62.64.152) 56(84) bytes of data.
64 bytes from 111.62.64.152 (111.62.64.152): icmp_seq=1 ttl=55 time=42.1 ms
64 bytes from 111.62.64.152 (111.62.64.152): icmp_seq=2 ttl=55 time=39.9 ms
64 bytes from 111.62.64.152 (111.62.64.152): icmp_seq=3 ttl=55 time=39.6 ms
```



## 磁盘分区

### 磁盘分区策略

请参照 [磁盘分区策略](<https://github.com/xiaozefeng/archlinux-guide/issues/4>)

### 检查启动模式  BIOS 或者 UEFI

```bash
efivar -l
```

执行命令后如果刷了满满的一屏，说明电脑时 UEFI启动，如果返回一行错误信息，说明电脑由传统的BIOS模式启动。

由于我的笔记本是 UEFI启动的方式，所以以下内容时针对UEFI模式启动的机器。

对于BIOS模式启动的机器有几点不同

请参照  [安装archlinux BIOS 启动 和 UEFI 启动有哪些不同 ?](<https://github.com/xiaozefeng/archlinux-guide/issues/18>)

### 设置分区格式

```bash
# 查看磁盘 fdisk 或者 lsblk
lsblk 
# 设置 分区格式为 gpt ，一般都是 sda 如果有你有多块盘，可能有 sdb, sdc 
parted /dev/sda  # 进入交互命令行
mktable # create  a new disklabel 
# 提示你 disk label type 
gpt
# 执行完毕
quit # 退出
```

[查看lsblk命令](https://wangchujiang.com/linux-command/c/lsblk.html)

[查看fdisk命令](https://wangchujiang.com/linux-command/c/fdisk.html)

```bash
# 使用 fdisk 或者 cfdisk 分区， 后者对新手更友好
cfdisk  /dev/sda
# 设置完成后 write
quit # 选中 quit 退出
```

![](<https://github.com/xiaozefeng/archlinux-guide/raw/master/images/partition.png>)

打印一下目前的磁盘状态

![](<https://raw.githubusercontent.com/xiaozefeng/archlinux-guide/master/images/disk_status.png>)

### 格式化磁盘

```bash
# 格式化根目录
mkfs.ext4 /dev/sda1
# 格式化 home 
mkfs.ext4 /dev/sda2
# 格式化引导分区
mkfs.vfat /dev/sda3
# 格式化交换分区
mkswap /dev/sda4
swapon /dev/sda4 # 打开交换分区
```

### 挂载 mount

```bash
# 先从跟分区开发挂载
mount /dev/sda1 /mnt
# 挂载 home 分区
mkdir /mnt/home
mount /dev/sda2 /mnt/home
# 挂载 uefi 分区
mkdir -p /mnt/boot/EFI	
mount /dev/sda3 /mnt/boot/EFI
```



## 安装系统

## 镜像设置 (加速下载)

```bash
vim /etc/pacman.d/mirrorlist
# 在文件顶部加入
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
# 保存后 执行同步
pacman -Syy
```

### 安装系统

```bash
pacstrap /mnt base base-devel
```

### 生成 fstab 文件

> fstab是一个关于文件系统的文件。在开机时就这个文件告诉引导程序文件系统的挂载情况

```bash
genfstab -U -p /mnt >> /mnt/etc/fstab
# 验证一下是否成功， 检查 /mnt/etc/fstab 文件
cat /mnt/etc/fstab
```

### 进入系统继续操作

目前一直是在U盘上操作，切换到操系统环境上操作

```bash
arch-chroot /mnt
```

### 设置时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# 设置时间
hwclock --systohc # set the RTC from the system time
```

### 设置编码

```bash
vim /etc/locale.gen
# 如果没有装vim 
pacman -S vim
# 搜索 en_US
# 解除 en_US.UTF-8 UTF-8 注释
locale-gen # 执行这条命令去生成 locale
vim /etc/locale.conf
# 写入
LANG=en_US.UTF-8
LANG=zh_CN.UTF-8
```

### 设置hostname

```bash
echo archlinux > /etc/hostname
# 这里的archlinux 你可以替换成别的名字
```



### 给root重置密码

```bash
passwd 
```



### 安装引导程序

```bash
pacman -S grub efibootmgr
```

### 将引导信息写入磁盘的引导分区

```bash
grub-install --recheck /dev/sda
# 生成引导配置文件
grub-mkconfig -o /boot/grub/grub.cfg
```



### 添加用户

```bash
useradd mikefeng -m -g wheel -G audio,video,storage
# 修改密码
passwd mikefeng
# 设置可以提升root权限
vim /etc/sudoers
# 找到 wheel ALL=(ALL) ALL 后，取消这一行的注释， 保存退出
```

### dhcp服务开机启动

```bash
systemctl enable dhcpcd.service
```

### 卸载根分区

```bash
exit 
umount -R /mnt
reboot
```

**一个基本的archlinux 系统就安装完成了**

**拔掉U盘**