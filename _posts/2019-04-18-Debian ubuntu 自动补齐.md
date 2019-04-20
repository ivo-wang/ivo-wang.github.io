---
layout:  post
title:  Debian ubuntu 自动补齐
subtitle: Debian ubuntu 自动补齐 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - debian
    - 
---
**1、安装bash-completion**

apt-get install bash-completion

**2、编辑~/.bashrc 文件**

添加如下内容：

if [ -f /etc/bash_completion ]; then  
. /etc/bash_completion  
fi

**3、使其生效**

退出SSH，重新登录。

apt-get install build-e  然后TAB一下，自动补齐了吧。



本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
