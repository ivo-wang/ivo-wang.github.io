---
layout:  post
title:  linux远程传文件scp和sftp详解
subtitle: linux远程传文件scp和sftp详解 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - 
    - 
---
一般情况，本地网络跟远程网络进行数据交抱，或者数据迁移，常用的有三种方法，一是ftp,二是wget /fetch 三是，rsync 大型数据迁移用rysync，其次用fetch/wget ，最次是ftp，最慢是ftp.
这几天，在忙数据迁移时，用到ssh的scp方法来迁移数据。速度与效果都很好。特别是现在许多服务器为了安全，都会改 ssh默认的22端口，改成一个特殊的端口。所以。在scp或者sftp时，就要指定通过什么端口来迁移。现在，特记下这个特殊端口来scp的命令。
```
scp -P port user@serverip:/home/user/filename /home/user/filename
```
以上端口大写P 为参数，port 端口 user 为ssh user serverip 为远程服务器ip或者域名 ,/home/user/filename 为远程服务器的文件名 /home/user/filename 为本地服务服务器的文件名。该命令的作用就是将远程的filename复制到本地对应的目录下面。
scp 的作用真的很巨大，详细用法，可以man scp 或者 scp –help ，下面为附上一篇 scp 使用详解。。。
```
linux 的 scp 命令 可以 在 linux 之间复制 文件 和 目录；
==================
scp 命令
==================
scp 可以在 2个 linux 主机间复制文件；
命令基本格式：
scp [可选参数] file_source file_target
======
从 本地 复制到 远程
======
* 复制文件：
* 命令格式：
scp local_file remote_username@remote_ip:remote_folder
或者
scp local_file remote_username@remote_ip:remote_file
或者
scp local_file remote_ip:remote_folder
或者
scp local_file remote_ip:remote_file
```
第1,2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名；
第3,4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名；
* 例程：
```
scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music
scp /home/space/music/1.mp3 root@www.cumt.edu.cn:/home/root/others/music/002.mp3
scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music
scp /home/space/music/1.mp3 www.cumt.edu.cn:/home/root/others/music/002.mp3
```
* 复制目录：
* 命令格式：
```
scp -r local_folder remote_username@remote_ip:remote_folder
或者
scp -r local_folder remote_ip:remote_folder
```
第1个指定了用户名，命令执行后需要再输入密码；
第2个没有指定用户名，命令执行后需要输入用户名和密码；
* 例程：

```
scp -r /home/space/music/ root@www.cumt.edu.cn:/home/root/others/
scp -r /home/space/music/ www.cumt.edu.cn:/home/root/others/

上面 命令 将 本地 music 目录 复制 到 远程 others 目录下，即复制后有 远程 有 ../others/music/ 目录
scp -r /home/space/music/.* www.cumt.edu.cn:/home/root/others/musc/
拷贝目录,-r是将目录下的目录递归拷贝。".*"是将隐藏文件也拷贝过去。需要先在远端创建好相应的目录。
```
从 远程 复制到 本地
```
从 远程 复制到 本地，只要将 从 本地 复制到 远程 的命令 的 后2个参数 调换顺序 即可；
例如：
scp root@www.cumt.edu.cn:/home/root/others/music /home/space/music/i.mp3
scp -r www.cumt.edu.cn:/home/root/others/ /home/space/music/
scp的优点是使用简单，缺点是无法列出远端目录和改变目录。复杂一点的用法是用sftp。
```
sftp：
sftp -o port=60066 user@serverip:/home/user/
其中-o port选项指定非缺省的ssh端口


本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
