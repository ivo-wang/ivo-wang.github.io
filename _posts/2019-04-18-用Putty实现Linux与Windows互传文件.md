---
layout:  post
title:  用Putty实现Linux与Windows互传文件
subtitle: 用Putty实现Linux与Windows互传文件 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - 
    - 
---
一般Linux与Windows大都使用FTP或者wget之类的工具来传输文件，Linux与Linux之间互传文件则使用scp工具。
scp（secure copy）确实是个好东西，用于在Linux下进行远程拷贝文件的命令，和它类似的命令有cp，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的:

本地上传文件至服务器:
scp 本地文件名 远程用户名@远程IP地址:路径/新文件名;
例:scp AA.zip test@200.100.0.1:www/AA_new.zip;

从远程服务器下载文件至本地:
scp 远程用户名@远程IP地址:路径/新文件名 本地文件名;
例:scp test@200.100.0.1:www/AA.zip AA_new.zip;

这对于linux与linux之间互传是非常方便的。

如果从一台Windows机器要传输数据到一台仅开SSH服务的Linux服务器时，pscp就要发挥威力了。

PSCP和SCP功能相同，是putty的一个附加程序，一般在putty的目录下可以找到。pscp.exe只有一个文件，(将pscp.exe放到C:WINDOWSsystem32下就能直接在命令行下使用pscp命令了）。语法与scp相同，下面是几个有用的options。

-p 拷贝文件的时候保留源文件建立的时间。
-q 执行文件拷贝时，不显示任何提示消息。
-r 拷贝整个目录
-v 拷贝文件时，显示提示信息。

pscp [options] source [source...] [user@]host:target

例如我要将windows上的一个zip包通过SSH服务传输到Linux服务器上可以这样做：
D:PROGRA~1Putty>pscp -pw mypasswd "E:TDDOWNLOADDiscuz!_6.0.0_SC_GBK.zip" Holmesian@192.168.128.128:.
Discuz!_6.0.0_SC_GBK.zip | 3711 kB | 1855.6 kB/s | ETA: 00:00:00 | 100%

相应的从Linux服务器上下载文件只需要将目标和源反过来即可。


本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
