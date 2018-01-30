---
layout: post
title: linux系统下nginx/mysql/php启动、停止、重启命令
---



/usr/local/nginx/sbin/nginx
/etc/init.d/mysql start
/usr/local/php/sbin/php-fpm start

#nginx命令 
 
start： 

/usr/local/nginx/sbin/nginx 

stop： 

/usr/local/nginx/sbin/nginx -s stop 

reload： 

/usr/local/nginx/sbin/nginx -s reload 

#******************************************** 

#php-fpm(fast-cgi)命令 
 
start： 

/usr/local/php/sbin/php-fpm 

stop： 

/bin/ps -ef | grep 'php-fpm' | grep -v grep | cut -c 9-15 | xargs kill -9 


Linux：PHP 5.3.3 以上版本的php-fpm的重启

INT, TERM：立刻终止
QUIT：平滑终止
USR1：重新打开日志文件
USR2：平滑重载所有worker进程并重新载入配置和二进制模块

示例：
1）php-fpm 关闭：
kill -INT `cat /usr/local/php/var/run/php-fpm.pid`

2）php-fpm 重启：
kill -USR2 `cat /usr/local/php/var/run/php-fpm.pid`

查看php-fpm进程数：
ps aux | grep -c php-fpm

#******************************************** 

#mysql命令 

start： 

/etc/init.d/mysqld start 

stop： 

/etc/init.d/mysqld stop 

restart： 

/etc/init.d/mysqld restart 

#******************************************** 
