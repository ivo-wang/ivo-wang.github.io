---
layout:  post
title:  archlinux 修复uefi引导启动
subtitle: archlinux 修复uefi引导启动 
date: 2018-05-29
author: ivo
catalog: true
header-img:
tags:
    - arch 
    - uefi
    - grub
---
由于最近用kali，装进了u盘里面。所以出现了一些问题，尤其是在启动时候。之前主机设置的bios为uefi only 更改了dualboot以后，kali的u盘是能启动了，宿主机器又崩溃了，只能在启动的时候选择boot from file 手动的去找efi文件去启动。然后查帖子综合出来了下面的方案。
> https://antergos.com/wiki/de/miscellaneous/how-to-fix-grub-with-efi-boot/
> http://ezlost.tk/2018/02/16/install-arch/

### 做livecd
我用的是arch的，就用的arch的安装盘
- linux 下建议用dd命令来操作dd if=./archlinux.iso of=/dev/sdx #x代表ｕ盘。
- windows下建议使用rufus来烧录，操作非常的方便成功率也高。

### 启动livecd
进去以后根据我以前的安装的记录和现有的真实情况 fdisk -l查看具体的分区信息。找出来sdb1是以前uefi存储的位置，就是sdb1 500m的esp分区，sdb3是根分区，所以用下面的操作

```
mount /dev/sdb3 /mnt
mount /dev/sdb1 /mnt/boot/efi
```

### 修复
进入 chroot `arch-chroot /mnt` 
>配置`grub-install --recheck /dev/sdb` 如果提示error:cannot find EFI directory，说明找不到EFI文件夹的位置，还需要加上--efi-directory参数指明安装位置`grub-install --recheck /dev/sdb --efi-directory=/boot`
完成后 输入 exit 退出chroot  reboot重启即可
