---
layout:  post
title:  ubuntu 安装nginx的依赖问题
subtitle: ubuntu 安装nginx的依赖问题 
date: 2018-09-02
author: ivo
catalog: true
header-img:
tags:
    - ubuntu 
    - nginx
---

ubuntu在安装nginx的时候安装依赖的名称与centos不大一样所以这里做一下记录,磁盘文章内容为转载记录使用的,原文地址见最下方


**首先使用dpkg命令查看自己需要的软件是否安装。**

**例如查看zlib是否安装：**

    dpkg -l | grep zlib

**解决依赖包openssl安装，命令：**

    sudo apt-get install openssl libssl-dev

**解决依赖包pcre安装，命令：**

    sudo apt-get install libpcre3 libpcre3-dev

**解决依赖包zlib安装，命令：**

    sudo apt-get install zlib1g-dev
>
> --  此内容引用自https://blog.csdn.net/z920954494/article/details/52132125#article_content
