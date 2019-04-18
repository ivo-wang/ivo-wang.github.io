---
layout:  post
title:  ubuntu samba配置
subtitle: ubuntu samba配置 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - linux
    - samba
---
 

首先apt-get insatll samba

之后需要创建一个用户名 useradd 999

在之后 smbpasswd -a 999（此处的用户名需要是操作系统里面存在过的，威这个用户重建密码）

然后 在配置文件里面添加下面hdd的部分。即可针对单一用的来共享文件，所有用户的共享需要参考以前的samba的帖子。

最后需要使用 sudo service smbd restart 来生效添加的配置。

 

具体选项参考
```

[hdd]
#共享文件夾的名字，隨意就行，無所謂
comment = File
#將要共享的文件夾
path = /home/ivo/111
#public = yes
#guest ok = yes
# security = user
browseable = yes
user = 999
```
「 转载文章 」http://blog.csdn.net/i_chips/article/details/19191957


本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
