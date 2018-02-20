---
layout:  post
title:  shell 下检测网络状态的脚本
subtitle: ping 检查网络
date: 2018-02-20
author: ivo
catalog: true
header-img:
tags:
    - linux
    - shell
---

```
#!/bin/sh
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
netstat=$(ping -c3 g.cn|grep transmitted |awk '{print $4}')
if((netstat==0))
#time out
then
        date >> network.down.log
else
#ok
        echo "network is ok"
fi
```
