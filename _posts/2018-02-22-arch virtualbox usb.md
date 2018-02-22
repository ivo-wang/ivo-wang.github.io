---
layout:  post
title:  archlinux virtualbox usb设备支持
subtitle: 
date: 2018-02-22
author: ivo
catalog: true
header-img:
tags:
    - archlinux
    - virtualbox
---
刚安装完的archlinux 里面的virtualbox,安装好宿主的扩展与虚拟机系统里面的扩展以后,usb设备是不能够使用的.比如u盘.virtualbox 不能用u盘,这时候.将需要运行 VirtualBox 的用户名添加到 vboxusers 用户组，USB 设备才能被访问。具体命令如下
```	
usermod -a -G  vboxusers `whoami`
```
