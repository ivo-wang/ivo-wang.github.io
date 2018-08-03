---
layout:  post
title:  virtualbox虚拟机中启用usb3.0的支持
subtitle: virtualbox win7 开启usb 3.0 
date: 2018-02-07
author: ivo
catalog: true
header-img:
tags:
    - virtualbox
    - usb3.0
---
首先确保前提条件,virtualbox 5.06以上.然后我用的是intel的处理器.我的环境是在ubuntu下来虚拟32位win7.我参考的是[http://www.cnblogs.com/yagzh2000/p/6003165.html](http://www.cnblogs.com/yagzh2000/p/6003165.html)他用的64位win7虚拟32位win7.

### 0x00 前提条件

刚安装完的archlinux 或ubuntu里面的virtualbox,安装好宿主的扩展与虚拟机系统里面的扩展以后,usb设备是不能够使用的.比如u盘.virtualbox 不能用u盘,这时候.
将需要运行 VirtualBox 的用户名添加到 vboxusers 用户组，USB 设备才能被访问。具体命令如下
```
1、添加usbfs用户组（装完成后会有vboxusers和vboxsf 两个用户组）

    sudo groupadd usbfs

2、将你的Linux常用用户(这里假定用户名是ivo)添加到vboxusers、usbfs这个两个组中   

    sudo adduser ivo vboxusers
    sudo adduser ivo usbfs
```


然后设置下面的

### 0x01 安装virtualbox extension pack
- 打开https://www.virtualbox.org/wiki/Downloads
- 点击All supported platforms 下载
- 安装VirtualBox Extension Pack 

### 0x02 设6003165置
- 在virtualbox没有开启系统时，点击“设置”->"USB设备"->选择“启用USB控制器”->选择"USB3.0
- 设置-系统设置-chipest piix3
- 设置-系统设置-extend features-enable I/O APIC
此时启动主机,有部分主机可以使用3.0了
不能使用的的继续往下看
- 去下载[https://downloadcenter.intel.com/download/21129/USB-3-0-Driver-Intel-USB-3-0-eXtensible-Host-Controller-Driver-for-Intel-7-Series-C216-Chipset-Famil](https://downloadcenter.intel.com/download/21129/USB-3-0-Driver-Intel-USB-3-0-eXtensible-Host-Controller-Driver-for-Intel-7-Series-C216-Chipset-Family) intel3.0的芯片组驱动.安装一下
- 现在再去virtualbox选择'设备'->'USB'选择要连接的u盘（前机打勾）即可，此时你会发现在virtual中已显示了usb盘符!
