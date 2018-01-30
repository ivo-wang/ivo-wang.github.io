---
layout: post
title: CentOS 6 vps上搭建Wordpress开源博客平台
---
用LNMP的一键安装包，安装Nginx、MySQL、PHP、phpMyAdmin。然后安装WordPress

### 首先安装LNMP

#### 先安装scrrent

```
   yum install screen

安装完成之后在终端下运行如下命令

   screen -S lnmp    （这个命令能保证过程中ssh不会断线）
```

#### 然后安装lnmp

lnmp是linu下的(Nginx、MySQL、PHP、phpMyAdmin)一键安装Shell脚本

   wget -c http://soft.vpser.net/lnmp/lnmp1.1-full.tar.gz && tar zxf lnmp1.1-full.tar.gz && cd lnmp1.1-full && ./centos.sh

在终端运行上面的命令之后开始安装lnmp，等待安装完成即可(具体安装完成时间视你的网速以及服务器硬件配置)

详细的LNMP安装教程可以去[lnmp官网](http://lnmp.org/install.html)查看.

安装完成之后在浏览器里面输入你的IP地址即可打开默认的lnmp界面啦;

```
1.设置MySQL的root密码，直接回车会默认密码root

2.需要确认是否启用MySQL InnoDB，如果不确定是否启用可以输入 y ，这个可以单独在MySQL文件里关闭，输入 y 表示启用，输入 n 表示不启用。输入 y 或 n 后回车进入下一步

3.选择php版本，可以选择 PHP 5.3.28 或 PHP 5.2.17，如果需要安装PHP 5.3.28的话输入 y ，如果需要安装PHP 5.2.17 输入 n，（一般选高版本的这里按y）

4.选择MySQL 版本 5.1.73、5.5.37或MariaDB 5.5.37，如果需要安装MySQL 5.5.37的话输入 y ，如果需要安装MySQL 5.1.73 输入n，如果需要安装MariaDB 5.5.37的话输入 md，输入完成后回车，完成选择。（这里我选的y）

5.输入完成后回车，完成选择。
提示"Press any key to start..."，按回车键确认开始安装。

LNMP脚本就会自动安装编译Nginx、MySQL、PHP、phpMyAdmin、Zend Optimizer这几个软件。

安装时间可能会几十分钟到几个小时不等，主要是机器的配置网速等原因会造成影响

```

安装完成
如果显示如下界面：![](/images/4.png)
Nginx、MySQL、PHP都是running，80和3306端口都存在，说明已经安装成功。
接下来按添加虚拟主机教程，添加虚拟主机，通过sftp或ftp服务器上传网站，将域名解析到VPS或服务器的IP上，解析生效即可使用。


如果显示 ![](/images/5.png)则失败了


### 添加虚主机

添加虚主机也就是在VPS上给你的wordpress添加一下文件目录，设置一下域名什么的;

#### 运行vhost.sh

   cd /root

    ./ vhost.sh

在终端里面执行上面的命令，即可开始添加虚拟主机;

#### 域名设置

   =========================================================================

    Add Virtual Host for LNMP V1.0  ,  Written by Licess 

    =========================================================================

    LNMP is a tool to auto-compile & install Nginx+MySQL+PHP on Linux 

    This script is a tool to add virtual host for nginx 

    For more information please visit http://www.lnmp.org/

    =========================================================================

    Please input domain:

    (Default domain: www.lnmp.org):www.w-zh.ml w-zh.ml

    ===========================

我这里输入了2个域名;
www.w-zh.ml和w-zh.ml是2个不同的域名

#### 是否还要添加域名

    domain=www.w-zh.ml w-zh.ml

    ===========================

    Do you want to add more domain name? (y/n)

如果需要就添加，不需要就直接输入n即可


#### 接下来设置网站目录

   Please input the directory for the domain:www.w-zh.ml w-zh.ml :

    (Default directory: /home/wwwroot/www.w-zh.ml w-zh.ml):

一般默认直接回车即可，要修改也可以，需要绝对路径。

#### 是否开启伪静态

    ===========================

    Allow Rewrite rule? (y/n)

    ===========================

一般都是要的，所以输入y后回车

#### 下面选择伪静态类型

    Please input the rewrite of programme :

    wordpress,discuz,typecho,sablog,dabr rewrite was exist.

    (Default rewrite: other):wordpress

默认有discuz、discuzx、wordpress、sablog、emlog、dabr、phpwind、wp2(二级目录wp伪静态)、dedecms、drupal、ecshop、shopex可选，主机输入即可。

#### 是否开启log功能

   ===========================

    Allow access_log? (y/n)

    ===========================

这个一般没啥用输入n后回车

#### 开始安装

    Press any key to start create virtul host...

出现按任意键提示后敲回车开始安装，等待安装完成。

### 安装WordPress

先切换到网站目录下

   cd /home/wwwroot/

然后看看你的'www.w-zh.ml'文件夹是否存在.

#### 下载WorPress

   wget https://cn.wordpress.org/wordpress-4.1-zh_CN.zip

运行wget下载最版本的WordPress

#### 运行unzip解压

   unzip wordpress-4.1-zh_CN.zip

#### 拷贝Wordpress到你的网站目录下

   cp -R wordpress/* /home/wwwroot/www.w-zh.ml/

将wordpress目录下的所有文件拷贝到www.w-zh.ml中

#### 设置目录权限

由于wordpress在安装的时候以及在安装插件主题和自升级的需要可写权限。所以我要对特定目录设权限。

```
chmod -R 777 wp-admin/

chmod -R 777 wp-content/

chmod -R 777 wp-includes/

chmod -R 777 wp-config-sample.php

chmod -R 777 readme.html
```
#### 创建数据库

在安装lnmp之后我们就已经可以通过pvs主机IP打开默认网站，通过上面的phpmyadmin这个连接 
用户名是root
密码是之前设置的mysql数据库密码

完事后可以进入图形界面的数据库管理程序，phpmyadmin
点击数据库标签
创建一个名字叫wordpress的数据库（只要创建这样一个空的数据库就行了）
#### 关于域名解析
1.去自己的域名商那里修改dns为dnspod的（我的用国内的dnspod解析）
2.去dnspod面板添加需要解析的w-zh.ml 添加a记录（里面填写自己的vps主机的ip地址）
#### 安装wordpress
现在直接打开w-zh.ml就可开始安装了，按照提示输数据库名，账号密码之后即可开始安装。

到此在VPS上用lnmp搭wordpress就完成了。

### 注意事项

#### 1,安装主题需要FTP账号密码

修改网站目录下的wp-config.php文件，添加如下内容

    define("FS_METHOD","direct");

    define("FS_CHMOD_DIR",0777);

    define("FS_CHMOD_FILE",0777);  

保存之后，在wordpress刷新即可。

#### 2,wordpress后台主题不显示，仅显示默认使用的主题(还有一直显示刷新翻译都使用下面的解决方式)

这是由于lnmp默认禁用了一些php的函数导致的，

    修改/usr/local/php/etc/php.ini

    查找disable_functions下删除scandir

然后重启php-fpm即可

    service php-fpm restart

 问题及解决

绑定域名后，在浏览器中输入域名，无法解析到主机目录。

解决方法 ：绑定域名后需要重启lnmp ， 执行命令 /root/lnmp restart 重启lnmp服务重新登录后成功

#### 3.重启后只能用代理访问的问题
没错，你的防火墙iptables的问题。80端口没开................
```

sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
或是
#/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
然后保存：
#/etc/init.d/iptables save

 
查看打开的端口：
# /etc/init.d/iptables status

```
