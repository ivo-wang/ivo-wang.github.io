---
layout:  post
title:  Linux Shell远程执行命令（命令行与脚本方式）及aria2多线程同步脚本
subtitle: Linux Shell远程执行命令（命令行与脚本方式）及aria2多线程同步脚本 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - linux
    - 
---
### shell远程执行：

　　经常需要远程到其他节点上执行一些shell命令，如果分别ssh到每台主机上再去执行很麻烦，因此能有个集中管理的方式就好了。一下介绍两种shell命令远程执行的方法。

### 前提条件：

配置ssh免密码登陆

### 对于简单的命令：

如果是简单执行几个命令，则：
```
ssh user@remoteNode "cd /home ; ls"
```

基本能完成常用的对于远程节点的管理了，几个注意的点：

1.  双引号，必须有。如果不加双引号，第二个ls命令在本地执行
2.  分号，两个命令之间用分号隔开

### 对于脚本的方式：

有些远程执行的命令内容较多，单一命令无法完成，考虑脚本方式实现：
```
#!/bin/bash
ssh user@remoteNode > /dev/null 2>&1 << eeooff
cd /home
touch abcdefg.txt
exit
eeooff
echo done!
```
远程执行的内容在“<< eeooff ” 至“ eeooff ”之间，在远程机器上的操作就位于其中，注意的点：

1.  << eeooff，ssh后直到遇到eeooff这样的内容结束，eeooff可以随便修改成其他形式。
2.  重定向目的在于不显示远程的输出了
3.  在结束前，加exit退出远程节点

利用aria2多线程同步远端ftp文件夹的实例

aria2 -i 指定url文件
```
#!/bin/bash

#获取远端目录  
ssh root@45.62.118.157 -p 29006 > /dev/null 2>&1 << eeooff  
tree -if /var/ftp > /var/ftp/1.tt  
exit  
eeooff  
aria2c ftp://45.62.118.157/1.tt

#格式修剪

sed ‘s/\/var\/ftp/ftp:\/\/45.62.118.157/g’ 1.tt > 2.tt && sed ‘/^$/d’ 2.tt |grep ftp > 3.tt  
rm -rf ./1.tt ./2.tt

aria2c –conf-path=/home/ivo/aria2/aria2.conf -i ./3.tt  
rm 3.tt -rf
```


本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
