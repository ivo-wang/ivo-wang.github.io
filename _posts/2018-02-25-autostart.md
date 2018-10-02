---
layout: post
title: systemd制作开机启动
subtitle: autostart
date: 2018-02-25
author: ivo
catalog: true
header-img:
tags:
    - systemd
    - 开机启动
    - autostart
---
Arch linux Systemd自启动脚本,systemd与以往的方式不同需要用服务来实现,所以要先有一个服务,再服务里面加入需要开机启动的脚本.

在写启动脚本之前，需要写一个service：

1：首先切换用户到root，然后进入/etc/systemd/system目录
创建一个.service文件，名称顺便取，本例中为AutoExec.service

设置权限：chmod 644 AutoExec.service

添加以下内容到AutoExec.service:

```
[Unit]
Description=AutoExec
[Service]
ExecStart=/root/AutoExec.sh
[Install]
WantedBy=multi-user.target
```

上面的ExecStart=/root/AutoExec.sh表明开机会执行/root/AutoExec.sh，那么命令写在这里面就行了，当然，文件是可以任意指定的，改为/etc/rc.local之类的都没问题。
连接到multi-user.target.wants
ln -s AutoExec.service multi-user.target.wants/
 （注：这一步是为了让这个服务开机自启动，效果和这句是等价的）

systemctl enable AutoExec.service
(推荐直接使用systemctl来实现service自启)

创建/root/AutoExec.sh
向里面写入想要开机启动的命令，并设置权限：

chmod 744 /root/AutoExec.sh
（文件路径和文件名可随便改，但一定要添加可执行权限）

以上就设置完了，systemctl start AutoExec.service试试吧，就能看到AutoExec.sh里的命令执行了，每次开机也是自动执行的。
> 部分引用自[http://lckiss.com/?p=1623](http://lckiss.com/?p=1623)
