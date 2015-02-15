---
title: 'SSH远程会话管理工具 &#8211; screen使用教程'
author: huahua
layout: post
permalink: /?p=31
categories:
  - 未分类
---
常用的使用方法

用来解决编译安装程序时（比如安装lnmp）网络突然断开，或者其他情况导致不得不与远程SSH服务器链接断开的问题  
1.1 创建screen会话

可以先执行：screen -S lnmp ，screen就会创建一个名字为lnmp的会话。 VPS侦探 http://www.vpser.net/  
1.2 暂时离开，保留screen会话中的任务或程序

当需要临时离开时（会话中的程序不会关闭，仍在运行）可以用快捷键Ctrl+a d(即按住Ctrl，依次再按a,d)  
1.3 恢复screen会话

当回来时可以再执行执行：screen -r lnmp 即可恢复到离开前创建的lnmp会话的工作界面。如果忘记了，或者当时没有指定会话名，可以执行：screen -ls screen会列出当前存在的会话列表，如下图：

11791.lnmp即为刚才的screen创建的lnmp会话，目前已经暂时退出了lnmp会话，所以状态为Detached，当使用screen -r lnmp后状态就会变为Attached，11791是这个screen的会话的进程ID，恢复会话时也可以使用：screen -r 11791  
1.4 关闭screen的会话

执行：exit ，会提示：[screen is terminating]，表示已经成功退出screen会话。VPS侦探 http://www.vpser.net/  
2、远程演示

首先演示者先在服务器上执行 screen -S test 创建一个screen会话，观众可以链接到远程服务器上执行screen -x test 观众屏幕上就会出现和演示者同步。  
3、常用快捷键

Ctrl+a c ：在当前screen会话中创建窗口  
Ctrl+a w ：窗口列表  
Ctrl+a n ：下一个窗口  
Ctrl+a p ：上一个窗口