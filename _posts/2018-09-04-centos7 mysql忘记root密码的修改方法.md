---
layout:  post
title:  centos7 mysql忘记root密码的修改方法
subtitle: centos7 mysql忘记root密码的修改方法 
date: 2018-09-04
author: ivo
catalog: true
header-img:
tags:
    - 
    - 
---

1．首先确认服务器出于安全的状态，也就是没有人能够任意地连接MySQL数据库。

因为在重新设置MySQL的root密码的期间，MySQL数据库完全出于没有密码保护的状态下，其他的用户也可以任意地登录和修改MySQL的信息。可以采用将MySQL对外的端口封闭，并且停止Apache以及所有的用户进程的方法实现服务器的准安全状态。最安全的状态是到服务器的Console上面操作，并且拔掉网线。

2．修改MySQL的登录设置：


`vim /etc/my.cnf`

在[mysqld]的段中加上一句：skip-grant-tables

例如：


`[mysqld]`

`datadir=/var/lib/mysql`

`socket=/var/lib/mysql/mysql.sock`

`skip-``grant``-tables`

保存并且退出vi。

3．重新启动mysqld


`service mysqld restart`

`Stopping MySQL: [ OK ]`

`Starting MySQL: [ OK ]`

4．登录并修改MySQL的root密码

`mysql -uroot -p` 直接回车密码是空


`Welcome` `to` `the MySQL monitor. Commands` `end` `with` `;` `or` `\g.`

`Your MySQL` `connection` `id` `is` `3` `to` `server version: 3.23.56`

`Type ‘help;``' or ‘\h'` `for` `help. Type ‘\c``' to clear the buffer.`

`mysql> USE mysql ;`

`Database changed`

`mysql> UPDATE user SET Password = password ( '``new-``password``' ) WHERE User = '``root' ;`

`Query OK, 0` `rows` `affected (0.00 sec)`

`Rows` `matched: 2 Changed: 0 Warnings: 0`

`mysql> flush` `privileges` `;`

`Query OK, 0` `rows` `affected (0.01 sec)`

`mysql> quit`

5．将MySQL的登录设置修改回来

`vim /etc/my.cnf`

将刚才在[mysqld]的段中加上的skip-grant-tables删除

保存并且退出vim

6．重新启动mysqld

`service mysqld restart`

`Stopping MySQL: [ OK ]`

`Starting MySQL: [ OK ]`
>
> --  此内容引用自https://www.cnblogs.com/phper12580/p/8022658.html#cnblogs_post_body
