---
title: Linux下使用proxychains将SSH tunnel作为全局代理使用。
layout: post
---
安装

sudo apt-get install proxychains
配置

sudo vi /etc/proxychains.conf 
选择dynamic\_chain，而不是random\_chain和strict\_chain。即注释掉strict\_chain和random_chain那两行。

在最后的[ProxyList]下面添加： socks5 127.0.0.1 7070（假设SSH tunnel的本地端口是7070）。

使用方法

proxychains <程序名> 
即可让程序使用代理。

例如：

sudo proxychains apt-get update 
报错

ERROR: ld.so: object libproxychains.so.3 from LD_PRELOAD cannot be preloaded: ignored. 
解决： 修改 /usr/bin/proxychains 中的

export LD_PRELOAD=libproxychains.so.3 
为：

export LD_PRELOAD=/usr/lib/libproxychains.so.3
