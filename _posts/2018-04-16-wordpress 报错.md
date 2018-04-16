---
layout:  post
title:  WordPress上传图片出现HTTP错误的解决方法
subtitle: wordpress 报错 
date: 2018-04-16
author: ivo
catalog: true
header-img:
tags:
    - wordpress
    - 
<section class="xpend_copyright">
    文链接：https://www.wordpressmatrix.com/faq/221/fix-wordpess-upload-image-error/

    文章作者：[灵风子](https://www.wordpressmatrix.com/author/tonvin)

    <time datetime="2017-07-23 05:39:35">发表日期：2017年07月23日</time>

    <time datetime="2017-11-27 11:01:52">更新日期：2017年11月27日</time>
</section>

WordPress网站后台，撰写新文章，添加媒体，上传文件时出现“HTTP错误”；

我使用的是Centos7.4 操作系统，WEB服务器是Nginx 1.12.2

查看Nginx错误日志


Shell

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 14px !important; line-height: 20px !important; z-index: 0; opacity: 0; overflow: hidden;">tail -f /var/log/nginx/error.log</textarea>

col 1 | col 2                           
----- | --------------------------------
1     | tail -f /var/log/nginx/error.log

发现错误信息 client intended to send too large body

此种错误在web前端通常显示为413 Request Entity Too Large

此时编辑Nginx配置文件


<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 14px !important; line-height: 20px !important; z-index: 0; opacity: 0; overflow: hidden;">vim /etc/nginx/nginx.conf</textarea>

col 1 | col 2                    
----- | -------------------------
1     | vim /etc/nginx/nginx.conf

在http{………….}配置中添加下面一行，设定上传文件大小不超过5M


YAML

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 14px !important; line-height: 20px !important; z-index: 0; opacity: 0; overflow: hidden;">client_max_body_size 5M;</textarea>

col 1 | col 2                   
----- | ------------------------
1     | client_max_body_size 5M;

然后重载nginx配置


<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 14px !important; line-height: 20px !important; z-index: 0; opacity: 0; overflow: hidden;">service nginx reload</textarea>

col 1 | col 2               
----- | --------------------
1     | systemctl restart Nginx

再次执行上传图片操作，未出现错误。

