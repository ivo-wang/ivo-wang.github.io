---
layout:  post
title:  重装win7后修复grub（clonezilla cmd修复）（win10，debian sid双系统）
subtitle: 重装win7后修复grub（clonezilla cmd修复）（win10，debian sid双系统） 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - grub
    - 
---
1.用clonezilla或是ubuntu的livecd进入。sudo su

2.运行终端，输入命令：

fdisk -l
找到linux所在的盘符，如我的时在dev/sda5下 一般是83标签的那个；

3.输入命令：

mount    /dev/sda5(数字是你的ubuntu所在的盘符)    /mnt
注意空格不能少；

4.输入命令：

grub-install    --root-directory=/mnt    /dev/sda
至此，Grub基本修复完毕；

4.关机重启，熟悉的引导界面回来了，进入ubuntu;

6.输入命令：

sudo update-grub
好了，一切OK！


本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
