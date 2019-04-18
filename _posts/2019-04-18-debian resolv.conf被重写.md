---
layout:  post
title:  debian resolv.conf被重写
subtitle: debian resolv.conf被重写 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - debian
    - 
---
vim /etc/dhcp/dhclient.conf
去掉注释
prepend domain-name-servers 127.0.0.1;
这里可以写2个

或者

-------------------------------------------------------------------------------------------
修改/etc/network/interfaces这个文件，配置IP、网关什么的，其实这里面还有个参数可以配置，那就是DNS了，对应的参数名为dns-nameservers，这里设置的优先级比resolv.conf高，也就是网络会从这里读取DNS配置，如果没配置才去看resolv.conf里面的设置，因此在这里面配置DNS更简单。


# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback
auto eth0

iface eth0 inet static
address 192.168.1.151
netmask 255.255.255.0
gateway 192.168.1.2

dns-nameservers 202.38.64.1



本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
