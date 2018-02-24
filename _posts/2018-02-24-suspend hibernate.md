---
layout:  post
title:  linux下suspend与hibernate的区别
subtitle: suspend hibernate 
date: 2018-02-24
author: ivo
catalog: true
header-img:
tags:
    - linux
    - 休眠
---
linux的休眠和待机,有时候不好区分.这里简单的解释一下之间的区别.
待机与休眠的区别，

待机（Suspend）是挂起到内存，需要保持对内存供电不能完全关闭电源。  
从外面看来，SUSPEND会黑掉屏幕，相当于屏幕保护，我的电源灯还在闪，而且可以通过键盘激活。

休眠（Hibernate）是挂起到硬盘，完全关闭电源。  
用的机器做实验，从Hibernate到关机（它自已关的）花了一点了时间，估计是它在做保存工作，最后电源灯灭点，不能激活。重启（按电源按钮)，启动过程和平时一样，开机时花了点时间（估计在做还原操作），但进去之后还是上面关机之前的session,应用程序都还在。
