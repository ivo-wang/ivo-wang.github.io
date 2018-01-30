---
title: 手动清除或刷新Linux的SWAP分区
layout: post
categories:
  - 未分类
---
XEN等架构的VPS都有SWAP（交换分区）。物理内存接近饱和时，系统会自动将不常用的内存文件转储到SWAP中，但SWAP使用率达30%的时候对系统性能可能有一定影响。  
对于较大物理内存的VPS或服务器，或根据自己服务器的情况，可以考虑手动关闭或刷新SWAP分区。

一、SWAP开关：

1、关闭SWAP

一般用于大物理内存的服务器

swapoff -a

在SSH中执行以上命令，则可以关闭SWAP分区。

swap-1.jpg

2、开启SWAP

swapon -a

在SSH中执行以上命令，则可以开启SWAP分区。

二、刷新SWAP

当SWAP占用率高达30%，对系统性能可能会有一定影响，所以在适当情况下，我们可以执行上述的两个命令刷新一次SWAP（将SWAP里的数据转储回内存，并清空SWAP里的数据）

swapoff -a &#038;&#038; swapon -a

在SSH中执行上述命令，即可达到相应目的。

其实，刷新SWAP原理就是把swap关闭后再重启。