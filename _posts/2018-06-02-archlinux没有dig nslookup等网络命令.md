---
layout:  post
title:  archlinux没有dig nslookup等网络命令
subtitle: archlinux没有dig nslookup等网络命令 
date: 2018-06-02
author: ivo
catalog: true
header-img:
tags:
    - 网络
    - dig
    - nslookup
    - archlinux
---
### archlinux安装后没有ifconfig命令

问：很多和网络有关的命令都没有，ifconfig,route ,nslookup这些都没有，变量没设置错误，用root也找不到，这是什么原因呢？

答：以前net-tools属于base组，装base时自动就装上了，现在哪个组都不属于了，这些工具需要单独安装。其中ifconfig、route在net-tools包中，nslookup、dig在dnsutils包中，ftp、telnet等在inetutils包中，ip命令在iproute2包中。
```
pacman -S net-tools dnsutils inetutils iproute2
```
> -- https://blog.csdn.net/zhyh1986/article/details/39892611#article_content
