---
layout:  post
title:  mysql 忘记root密码
subtitle: mysql 忘记root密码 
date: 2018-09-12
author: ivo
catalog: true
header-img:
tags:
    - mysql 
    - root
---

又一次忘了root密码,充分验证那句话,「平时爱藏东西的人,最容易丢东西」.平常喜欢临时的设置一些稀奇古怪的密码,那么忘记是必然的.没办法再次各种搜索,记录下来全过程.

首先不管是编译安装的还是源安装的,先要做的第一步是找到my.cnf文件,也可能是my.ini(windows下面),找到以后编辑 在`[mysqld]` 这个选项下面去添加一行代码,`skip-grant-tables`,如果内有mysqld 手动的添加一个写上即可.

### 修改配置文件

```
vim /etc/mysql/my.cnf

[mysqlq]
skip-grant-tables
```
保存后退出

### 启动mysql服务
`sudo systemctl start mysqd`
如过没有报错那么启动正常

### 更改root密码
```
mysql 

mysql> use mysql; ##使用mysql数据库
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> update user set password=password("123456") where user="root";##更新密码
Query OK, 4 rows affected (0.00 sec)
Rows matched: 4  Changed: 4  Warnings: 0

mysql> flush privileges;##刷新权限
Query OK, 0 rows affected (0.00 sec)
```
### 收尾工作

`ps -ef`之后查找mysql相关的进程kill掉,也可以`killall mysqld`(这个方法不一定都有效,还是手动的查找靠谱),然后把配置文件里面的那个添加进去的句子注释掉(不注释掉谁都可以重置mysql的root密码了),之后再启动mysqld `sudo systemctl start mysqld` 就可以用新密码登录了.
