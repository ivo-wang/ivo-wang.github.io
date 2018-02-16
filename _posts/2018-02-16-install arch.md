---
layout:  post
title:  根据官方文档安装archlinux 基础篇
subtitle: 安装uefi启动 gpt分区的archlinunx
date: 2018-02-16
author: ivo
catalog: true
header-img:
tags:
    - linux
    - archlinux
---
简单介绍一下，这里使用的是arch的官方文档的安装顺序进行的安装，这里介绍的是安装出来不带图形界面的版本的archlinux，下面会尽量的详细去记录整个的安装过程。

### 0x01 准备过程
- archlinux的livecd光盘，这个去archlinux官网去下载[https://www.archlinux.org/download/](https://www.archlinux.org/download/)
- 把iso刻录进光盘;用dd命令或其他方式烧录iso到u盘中
- 设置主机bios 从u盘或是光盘启动
- 设置bios里面的uefi选项为启用
- 有线网络，尽量用有线。无线我的笔记本就不好用，以后会更新解决办法
- 一部手机或ＩＰＡＤ用来查看文档用，我是整机安装的没有使用虚拟机
### 0x02 安装过程

