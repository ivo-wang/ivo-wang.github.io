---
layout:  post
title:  archlinux 更换grub背景
subtitle: archlinux 更换grub背景 
date: 2018-06-28
author: ivo
catalog: true
header-img:
tags:
    - grub2 
    - 背景
    - archlinux
---
默认的arch安装出来之后黑色背景太过于单调，现在需要更改它，查询了相关资料以后，更改成功记录一下。
### GRUB 启动图像搜索顺序

在 grub-2.02 中，对基于 Debian 的系统来说，它将按照以下顺序搜索启动背景：

1.  `/etc/default/grub` 里的 `GRUB_BACKGROUND` 行 
2.  在 `/boot/grub/` 里找到的第一个图像（如果发现多张，将以字母顺序排序）
>
> --  此内容引用自https://linux.cn/#toc_2

我是用1 更改了文件后
运行`grub-mkconfig -o /boot/grub/grub.cfg`重新生成grub，即可

