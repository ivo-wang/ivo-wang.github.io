---
layout:  post
title:  centos install mysql5.7
subtitle: centos install mysql5.7 
date: 2019-04-17
author: ivo
catalog: true
header-img:
tags:
    - centos 7
    - mysql 5
---
### 简介

[MySQL](https://www.mysql.com/)是一个开源的数据库，它也是LEMP的一部分[LEMP](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7) (Linux, Nginx, MySQL/MariaDB, PHP/Python/Perl)

CentOS 7默认使用mariadb来代替传统的masql，mariadb是从mysql商业化以后fork出来的一个开发版本. 如果你运行`yum install mysql`在 CentOS 7, 它实际上安装的是mariadb. 下面这些是 MySQL 与 MariaDB的区别, [MariaDB will generally work seamlessly in place of MySQL](https://mariadb.com/kb/en/mariadb/mariadb-vs-mysql-compatibility/), so unless you have a specific use-case for MySQL, see the [How To Install MariaDB on Centos 7](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-centos-7) guide.

下面将教你在 CentOS 7服务器上面安装 MySQL 5.7版本

## 准备

## 步骤 1

如果是必须安装mysql而不是mariadb。你需要到mysql的开发站去找对应的版本 [the MySQL community Yum Repository](https://dev.mysql.com/downloads/repo/yum/)

在浏览器中打开下面的地址：

    https://dev.mysql.com/downloads/repo/yum/

根据下图来操作，寻找对应的版本

![Screencapture highlighting current yum repo name](https://assets.digitalocean.com/articles/mysql-centos7/repo-name-small.png)

    wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

下载好了rpm包以后，需要做一个md5校验，这样安全一些

    md5sum mysql57-community-release-el7-9.noarch.rpm

    Output1a29601dc380ef2c7bc25e2a0e25d31e mysql57-community-release-el7-9.noarch.rpm

对比计算出来的md5码与网站上提供的md5码，看是否有区别。

![Screencapture highlighting md5dsum](https://assets.digitalocean.com/articles/mysql-centos7/md5-sum-small.png)

确认了 md5 一致，rpm包没有被篡改以后安装

    sudo rpm -ivh mysql57-community-release-el7-9.noarch.rpm

这会添加2个新的 MySQL yum 源, 我们现在可以安装 MySQL 服务了:

    sudo yum install mysql-server

按 `y`健，确认 GPG key

## 步骤 2 — 启动 MySQL

启动守护进程:

    sudo systemctl start mysqld

`systemctl` 不显示状态，需要用下面的命令来查看

    sudo systemctl status mysqld

如果 MySQL 启动成功 `Active: active (running)` 你将看下类似下面的提示:

    Dec 01 19:02:20 centos-512mb-sfo2-02 systemd[1]: Started MySQL Server.

**Note:** MySQL 在操作系统里面是自动启动状态，用这个命令可以中止自动启动`sudo systemctl disable mysqld`

在安装mysql以后, MySQL会创建一个临时密码给root用户.存储在`mysqld.log`中，用下面的命令来查看:

    sudo grep 'temporary password' /var/log/mysqld.log

    Output2016-12-01T00:22:31.416107Z 1 [Note] A temporary password is generated for root@localhost: mqRfBU_3Xk>r

你需要尽快更改这个密码。新密码由12个字符组成。至少有一个大写字母，一个小写字母，一个数字和一个特殊字符。

## 步骤3 配置mysql

MySQL 包含一个安全脚本，用于更改远程root登录名和示例用户等安全性较低的默认选项。

用下面这个命令运行.

    sudo mysql_secure_installation

这会提示你输入默认的root密码。

    OutputThe existing password for the user account root has expired. Please set a new password. New password:

输入新密码

您将收到关于新密码强度的反馈，然后系统会立即提示您再次更改。 如果你非常自信也可以用 `No`:

    OutputEstimated strength of the password: 100
    Change the password for root ? (Press y|Y for Yes, any other key for No) :

在我们拒绝提示再次更改密码后，我们将按`Y`，然后按ENTER键以解决所有后续问题，以便删除匿名用户，禁止远程root登录，删除测试数据库并访问它，以及 重新加载特权表。

## 步骤 4 — 测试 MySQL

我们用 `mysqladmin` 工具可以连接测试, 也可以使用mysql 客户端来连接 **root** (`-u root`),输入密码(`-p`), 返回版本信息

    mysqladmin -u root -p version

下面是回显

    mysqladmin Ver 8.42 Distrib 5.7.16, for Linux on x86_64
    Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved. Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners. Server version 5.7.16
    Protocol version 10
    Connection Localhost via UNIX socket
    UNIX socket /var/lib/mysql/mysql.sock
    Uptime: 2 min 17 sec Threads: 1 Questions: 6 Slow queries: 0 Opens: 107 Flush tables: 1 Open tables: 100 Queries per second avg: 0.043


本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
