---
layout:  post
title:  Centos7 安装 VNC Server
subtitle: Centos7 安装 VNC Server 
date: 2018-10-10
author: ivo
catalog: true
header-img:
tags:
    - linux 
    - centos7
    - vnc
---
Centos7 安装 VNC Server

这篇教程将教会大家如何在 Centos7 的图形界面下去安装最新版的 VNC 服务端

VNC (Virtual Network Computing) 是一个 C/S 架构的软件系统，为用户提供远程的桌面环境
### 0x00 准备阶段
基本需要
CentOS 7 安装程序
RHEL 7 安装程序
### 0x01 在 Centos7 上面安装 VNC 服务端
Tigervnc-server 同过 Xvnc 服务端来启动 sessions。它可以运行在 Gnome 或 Kde等桌面环境

```
$ sudo yum install tigervnc-server
```

### 0x02 设置密码
后启动 VNC 需要设置一个六位的密码
如果需要连接的远程账户就是当前的用户，那么在终端里面直接输入
```
 vncpasswd
```
设置密码即可。如果是其他的账户，那么先 `su -账户` 再做密码设置即可。

### 0x03 配置systemd
配置 VNC 需要使用systemd来进行设置，这会涉及到后续的开机启动部分。下面的配置文件是一个模板。用root权限来进行操作添加

如你你的用户不在sudo的范围内，那么请直接使用root来操作。
```
sudo cp /lib/systemd/system/vncserver@.service  /etc/systemd/system/vncserver@:1.service
```
如果需要可以手动的更令这个配置文件

@符号后面的值1表示显示编号（端口5900 +显示）。此外，对于每个启动的VNC服务器，端口5900将增加1。
```0
sudo  vim /etc/systemd/system/vncserver@\:1.service  （没有这个文件的话可以自己手动添加）

[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target

[Service]
Type=forking
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
ExecStart=/sbin/runuser -l my_user -c "/usr/bin/vncserver %i -geometry 1280x1024"
PIDFile=/home/my_user/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

[Install]
WantedBy=multi-user.target
```
### 0x04 启动 VNC
使用systemd启动 VNC ，将 VNC 的进程添加到开机启动
```
sudo systemctl daemon-reload
sudo systemctl start vncserver@:1
sudo systemctl status vncserver@:1
sudo systemctl enable vncserver@:1
```

### 0x05 查看 VNC 的运行状态
要列出 VNC 的运行的端口可以使用`ss`命令来实现。因为我们只运行了一个 VNC 的服务端所以第一个运行的 VNC 的端口是 5901 使用的是TCP协议。

同样，必须使用root权限执行ss命令。如果您为不同的用户并行启动其他VNC实例，则第二个端口值为5902，第三个端口值为5903，依此类推。端口6000+用于允许X应用程序连接到VNC服务器。

```
sudo  ss -tulpn| grep vnc
Verify VNC Listening Ports
Verify VNC Listening Ports
```
0x06 防火墙配置
为了允许外部VNC客户端连接到CentOS中的VNC服务器，您需要确保允许正确的VNC开放端口通过防火墙。

如果只启动了一个VNC服务器实例，您只需打开第一个分配的VNC端口：5901 / TCP，方法是发出以下命令在运行时应用防火墙配置。
```
sudo firewall-cmd --add-port=5901/tcp
sudo firewall-cmd --add-port=5901/tcp --permanent
```


