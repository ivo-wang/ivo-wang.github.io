---
layout:  post
title:  修改WordPress中附件上传大小限制
subtitle: 修改WordPress中附件上传大小限制 
date: 2018-04-16
author: ivo
catalog: true
header-img:
tags:
    - wordpress 
    - 
---
#### 写作背景

使用wordpress时候，上传照片的时候发现附件大于2M不能进行上传。
机器配置 centos 7.4
nginx 1.12.2
mysql 5.56
#### 查看PHP配置文件php.ini路径

首先在网站根目录下建一个info.php文件，文件内容如下：
```
<?php

echo
phpinfo();

?>
```


然后在浏览器中访问这个文件，例如：http://localhost/info.php 
这一步的目的是：查看本服务器上的php.ini所在位置 + 查看默认附件大小的信息
修改php.ini，但是前提是一定要修改正确位置的php.ini，不然纵使php.ini改了千万遍也不会有效果的
因为如果不是购买空间而是自己搭建的话，可能由于存在多个php.ini而没有修改正确位置的php.ini
路径如下图：
我们查看到了php.ini的位置是：/etc/php.ini

#### 修改附件上传大小

使用vim编辑该文件
搜索：memory_limit、post_max_size、upload_max_filesize、max_execution_time、max_input_time
一般默认的设置值为：
memory_limit=128M　　　 //相当于单个脚本可调用内存大小
post_max_size=8M　　　　 //上传文件大小上限
upload_max_filesize=2M　 //默认上传文件大小，这个就是2M的限制！
max_execution_time=30　　//最大执行时间，页面等待时间
max_input_time=60　　　　//最大输入时间？具体意义不明确，就是上传时间相关

然后将其改为自己需要的值，例如：
memory_limit=128M
post_max_size=15M
upload_max_filesize=10M　//这样就改为可以传10M以下的文件了
max_execution_time=60
max_input_time=60

重启nginx及php服务使得设置才能生效！
systemctl restart nginx
systemctl restart php-fpm

