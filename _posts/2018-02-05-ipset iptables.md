---
layout:  post
title: 使用ipset配合iptables使用,来进行转发
subtitle: ipset优化iptables转发
date: 2018-02-05
author: ivo
catalog: true
header-img:
tags:
    - iptables
    - ipset
---
iptables转发有一种思想是建一条链,然后做ip的排除,ip段里面的return非ip段里面的redirect 到对应的端口上
今天说另一种思想,类似autoproxy.用ipset建一个地址池方法如下

```
# ipset create myset-ip hash:ip
or

# ipset -N myset-ip iphash
Add any IP address that you'd like to block to the set.

# ipset add myset 1.1.1.1
# ipset -A myset 2.2.2.2```
```

关于ipset的用法最好查看arch wiki [https://wiki.archlinux.org/index.php/Ipset](https://wiki.archlinux.org/index.php/Ipset)非常的详细

下面是实例
```
ipset -N flyme iphash   #flyme池

iptables -t nat -A PREROUTING -p tcp -m set --match-set flyme dst -j REDIRECT --to-port 1080            #转发到1080端口

```
还要配合dnsmasq来使用 `server=/.yourdomain.com/8.8.8.8`和`ipset=/.yourdomain.com/flyme`

这样就可以将yourdomain通过8.8.8.8来获取dns最终通过1080端口出去
