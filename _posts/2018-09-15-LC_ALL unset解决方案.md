---
layout:  post
title:  LC_ALL unset解决方案
subtitle: LC_ALL unset解决方案 
date: 2018-09-15
author: ivo
catalog: true
header-img:
tags:
    - locales 
    - linux
---
「转载文章」
解决LC_ALL = (unset),LANGUAGE = (unset),

网上搜的方法，如

sudo locale-gen en_US en_US.UTF-8
dpkg-reconfigure locales
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

to the file /etc/environment:

LC_ALL=en_US.UTF-8
LANG=en_US.UTF-8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在terminal里面输入

locale 然后

export LANGUAGE=en_US.UTF-8

export LC_ALL=en_US.UTF-8

再 locale
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

通通无效，因此，阅读英文论坛，回帖中发现：

在 .bashrc 中

添加

export LC_ALL="en_US.UTF-8"
执行一下，即 . ~/.bashrc
