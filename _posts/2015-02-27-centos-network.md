---

layout: post
title: centos mini安装或是base server安装后网络不能用

---
installed CentOS 6.4 from a "minimal" ISO.


安装后ping测试


connect: Network is unreachable.

网络服务不可用

```
using vi to edit /etc/sysconfig/network-scripts/ifcfg-eth0, 注释掉2行

#NM_CONTROLLED="yes"
#ONBOOT="no"

保存，之后重启网络服务

/etc/init.d/network restart

Everything works.
```

