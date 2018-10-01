---
layout:  post
title:  新装debian服务器报错 'ldconfig' not found in PATH or not executable
subtitle: 新装debian服务器报错 'ldconfig' not found in PATH or not executable 
date: 2018-10-01
author: ivo
catalog: true
header-img:
tags:
    - linux 
    - apt
---
十一了，大量的失误要处理，这里没有写错就是失误。为了解决acl新装了一台debian，装完就出了问题了，记录一下解决的过程。开始的时候apt换源成功了，update成功，install的时候报错了。

```
apt-listchanges: Reading changelogs...
Extracting templates from packages: 100%
Preconfiguring packages ...
dpkg: warning: 'ldconfig' not found in PATH or not executable
dpkg: warning: 'start-stop-daemon' not found in PATH or not executable
dpkg: error: 2 expected programs not found in PATH or not executable
Note: root's PATH should usually contain /usr/local/sbin, /usr/sbin and /sbin
E: Sub-process /usr/bin/dpkg returned an error code (2)
```

最终的解决方法是在root下的.bashrc 加入一个环境变量source一下

```
vim /root/.bashrc
export PATH=/sbin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin

source /root/.bashrc
```
