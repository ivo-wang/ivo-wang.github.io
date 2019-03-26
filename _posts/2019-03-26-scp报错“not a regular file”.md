---
layout:  post
title:  scp报错“not a regular file”
subtitle: scp报错“not a regular file” 
date: 2019-03-26
author: ivo
catalog: true
header-img:
tags:
    - scp
    - linux
---

### 0x00 简介

今天需要和服务器之间传资料。犹豫了0.1秒以后永了scp。没有使用rsync。最主要原因还是因为我服务器用的特殊端口没有使用常规的22，用rsync的话要用-e参数”ssh root@45.62.118.157 -p 29999″,好像是这样记不清了还得用别的参数现在就记者一个 av，本着玩不明白就不玩的传统。用了scp，scp还出了一个bug，ssh能用密钥连接scp不行。后来重启了机器好了。然后就出现了一个报错

### 0x01 系统环境

远程主机 ： Centos 7 64 bit （vps openvz架构）  
本地主机： Kali linux 64bit （移动硬盘系统）

### 0x02 问题现象

scp -P -r 29999 root@45.62.118.157:~ .  
先是报错-r 有问题（后来发现写的没错scp抽风）  
然后改成了 scp -P 29999 root@45.62.118.157:~ .  
这样的话一直让我提示输入密码，我用的密钥登录没有密码，疑惑。

重启本地机器  
再次输入  
scp -P 29999 root@45.62.118.157:~ .  
报错scp: /root: not a regular file

### 0x03 解决

冷静了一下，这个报错说/root不是一个目录。我还是为/root不让scp复制。  
重新添加参数  
scp -P -r 29999 root@45.62.118.157:~ .  
ok没问题了，缺的就是这个-r  
我第一遍输入的是对的，出问题的是scp。。。。


本内容同步更新在我的个人博客 「我们都是害虫」 [https://ezlost.com](https://ezlost.com) 搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
