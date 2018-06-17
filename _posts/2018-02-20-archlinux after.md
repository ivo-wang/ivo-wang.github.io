---
layout:  post
title:  archlinux安装完成的后续工作
subtitle: arch的各种基本软件安装
date: 2018-02-20
author: ivo
catalog: true
header-img:
tags:
    - archlinux
    - linux
---
archlinux 安装完成以后,就需要对这个系统进行更多的基本的软件安装,下面列举了一些常用的软件.
- xarchiver 是Xfce4 默认的压缩文档管理器。`sudo pacman -S xarchiver p7zip`
- 为了能够支持samba,访问共享时，在 thunar 路径栏中输入 smb://ip 即可.`sudo pacman -S gvfs-smb`
- 百度网盘导出的crx插件 地址.`https://github.com/acgotaku/BaiduExporter`
- aria2建议的配置文件`https://raw.githubusercontent.com/acgotaku/BaiduExporter/master/aria2c/aria2.conf`
- 查看基本的显示信息`sudo pacman -s screenfetch`
- 字体`sudo pacman -S adobe-source-han-sans-cn-fonts ttf-dejavu` `yaourt dejavu`
powerline
- 自动挂载 `sudo pacman -S gvfs gvfs-mtp ntfs-3g` 
- 媒体 `yaourt kodi`
- 主题icon `yaourt numix`
- 主题 `sudo pacman -S arc-gtk-theme `
- 文本 `yaourt wps-office`
- 解压  sudo pacman -S unarchiver
- virtualbox虚拟机 `sudo pacman -S virtualbox-host-modules-arch virtualbox `,之后再装扩展插件.这里因为没有使用自己编译的内核所以要用virtualbox-host-modules-arch这个包,其他的扩展同理.
- pdf viewer `sudo pacman -S evince`
- curl,jq 用来更新壁纸用 `sudo pacman -S curl jq`
