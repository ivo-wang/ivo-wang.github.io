---
layout:  post
title:  ubuntu 使用snap软件
subtitle: ubuntu 使用snap软件 
date: 2018-08-30
author: ivo
catalog: true
header-img:
tags:
    - snap 
    - ubuntu
---
最近在ubuntu 18.04 lts上面一直再用freeplane,直到它一直各种问题,从一开始的无法正常启动,到点击后就卡死java占用100%,openjdk异常.最近一直在研究docker然后就想到了snap已经默认集成在了新版的ubuntu 18.04 lts 里面,然后简单搜索了一下找到了一个snap的store.安装非常简单,现在长时间使用也没有任何异常.

### 如何使用
到[https://snapcraft.io/](https://snapcraft.io/)点击store,然后search 你需要的软件包

然后在结果里面会出现一个绿色的install按钮,点开后就是安装的命令.有一点需要注意的是,有些软件在snap仓库里面和在其他的源里面的命令行启动的名字是不一样的,这点需要注意.
