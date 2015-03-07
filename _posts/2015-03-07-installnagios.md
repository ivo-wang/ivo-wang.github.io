---
layout: post
title: centos部署nagios
---

###准备工作
安装基本环境

```
yum -y install httpd gcc glibc glibc-common gd gd-devel php php-mysql mysql mysql-devel mysql-server
添加用户组  
  109  groupadd nagcmd
  110  useradd -G nagcmd nagios
  111  usermod -a -G nagcmd apache
  下载最新的源码包nagios与nagios-plus
wget http://nagios-plugins.org/download/nagios-plugins-2.0.3.tar.gz
wget http://jaist.dl.sourceforge.net/project/nagios/nagios-4.x/nagios-4.1.0/nagios-4.1.0rc1.tar.gz
```
###安装

```

  125  cd nagios-4.1.0rc1
  126  ./configure --with-command-group=nagcmd --enable-event-broker --sysconfdir=/etc/nagios
  
  具体的configure选项可以 ./configure --help | less 查看有很多
  --sysconfdir定义配置文件所在的文件夹位置
  
  127  make all
  128  make install
  129  make install-init
  130  make install-commandmode
  131  make install-config
  132  make install-webconf

  
  make install-webconf（开启web端）
  
  142  htpasswd -c /etc/nagios/htpasswd.users nagiosadmin
  
  
  
    cd /etc/httpd/conf.d/
  144  ls
  145  less nagios.conf  web端配置文件
  
  
  
  146  service httpd start              
  147  chkconfig --add nagios
  148  chkconfig nagios on
  149  service nagios start
  
  
  
  
  
  152  cd nagios-plugins-2.0.3

  154  ./configure --with-nagios-user=nagios --with-nagios-group=nagios
  158  make
  159  make install
  
  
  160   setenforce 0           关闭selinux
  166  service iptables status
  167  iptables -I INPUT -p tcp --dport 80 -j ACCEPT 
  168  iptables -I INPUT -p tcp --dport 22 -j ACCEPT 
  169  /etc/init.d/iptables save
  170  /etc/init.d/iptables restart
  
  service nagios restart
  service httpd restart
```
浏览器打开192.168.1.113/nagios 输入用户名 nagiosadmin 密码 xxx即可登录
