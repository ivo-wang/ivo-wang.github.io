---
layout: post
title: 利用Iptables设置防火墙规则屏蔽IP
---
服务器上有两个用户被DDOS攻击，虽然他们已经更换了服务器，但为了安全起见，还是屏蔽了一下攻击源IP，这里介绍一下如何利用Iptables屏蔽IP。

有位用户总是把CPU跑满了，起初怀疑是他程序导致的，但是在检查之后发现程序也没异常。

后来，我查看了一下他的网站访问日志，发现有一个IP188.138.33.41请求非常多，使用netstat -an命令查看发现该IP出现的频率也最高，用防火墙屏蔽后CPU就正常了。

这里以Centos系统为例介绍如何利用防火墙规则**屏蔽IP**，指令如下：
**
iptables -I INPUT -s 188.138.33.41 -j DROP**

如果要屏蔽这个**IP段**，则是：
**
iptables -I INPUT -s 188.138.33.0/24 -j DROP**

表示屏蔽IP188.138.33.1到188.138.33.254

还可以**屏蔽整个IP段**：
**
iptables -I INPUT -s 188.0.0.0/8 -j DROP**

表示屏蔽IP188.0.0.1到188.255.255.254

如果要**解封一个IP**，则为：
**
iptables -D INPUT -s 188.138.33.41 -j DROP**

设置之后，还要记得保存：

service iptabels save

重启防火墙：

service iptables restart

以上我以IP188.138.33.41为例，大家可根据实际需求替换IP
