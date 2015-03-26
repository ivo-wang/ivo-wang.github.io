---
title: Nginx 简单的负载均衡配置示例
author: huahua
layout: post
permalink: /?p=84
categories:
  - 未分类
---
<a href="http://www.zyan.cc/" target="_blank">www.zyan.cc</a> 和 blog.zyan.cc 域名均指向 Nginx 所在的服务器IP。

用户访问<a href="http://www.zyan.cc/" target="_blank">http://www.zyan.cc</a>，将其负载均衡到192.168.1.2:80、192.168.1.3:80、192.168.1.4:80、192.168.1.5:80四台服务器。

用户访问<a href="http://blog.zyan.cc/" target="_blank">http://blog.zyan.cc</a>，将其负载均衡到192.168.1.7服务器的8080、8081、8082端口。

以下为配置文件nginx.conf：  
<a name="entrymore"></a>

<div class="quote">
  <div class="quote-title">
    引用
  </div>
  
  <div class="quote-content">
    user  www www;worker_processes 10;</p> 
    
    <p>
      #error_log  logs/error.log;<br /> #error_log  logs/error.log  notice;<br /> #error_log  logs/error.log  info;
    </p>
    
    <p>
      #pid        logs/nginx.pid;
    </p>
    
    <p>
      #最大文件描述符<br /> worker_rlimit_nofile 51200;
    </p>
    
    <p>
      events<br /> {<br /> use epoll;
    </p>
    
    <p>
      worker_connections 51200;<br /> }
    </p>
    
    <p>
      http<br /> {<br /> include       conf/mime.types;<br /> default_type  application/octet-stream;
    </p>
    
    <p>
      keepalive_timeout 120;
    </p>
    
    <p>
      tcp_nodelay on;
    </p>
    
    <p>
      upstream  <a href="http://www.zyan.cc/" target="_blank">www.zyan.cc</a>  {<br /> server   192.168.1.2:80;<br /> server   192.168.1.3:80;<br /> server   192.168.1.4:80;<br /> server   192.168.1.5:80;<br /> }
    </p>
    
    <p>
      upstream  blog.zyan.cc  {<br /> server   192.168.1.7:8080;<br /> server   192.168.1.7:8081;<br /> server   192.168.1.7:8082;<br /> }
    </p>
    
    <p>
      server<br /> {<br /> listen  80;<br /> server_name  <a href="http://www.zyan.cc%3B/" target="_blank">www.zyan.cc;</a>
    </p>
    
    <p>
      location / {<br /> proxy_pass        <a href="http://www.zyan.cc%3B/" target="_blank">http://www.zyan.cc;</a><br /> proxy_set_header   Host             $host;<br /> proxy_set_header   X-Real-IP        $remote_addr;<br /> proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;<br /> }
    </p>
    
    <p>
      log_format  www_s135_com  &#8216;$remote_addr &#8211; $remote_user [$time_local] $request &#8216;<br /> &#8216;&#8221;$status&#8221; $body_bytes_sent &#8220;$http_referer&#8221; &#8216;<br /> &#8216;&#8221;$http_user_agent&#8221; &#8220;$http_x_forwarded_for&#8221;&#8216;;<br /> access_log  /data1/logs/www.log  www_s135_com;<br /> }
    </p>
    
    <p>
      server<br /> {<br /> listen  80;<br /> server_name  blog.zyan.cc;
    </p>
    
    <p>
      location / {<br /> proxy_pass        <a href="http://blog.zyan.cc%3B/" target="_blank">http://blog.zyan.cc;</a><br /> proxy_set_header   Host             $host;<br /> proxy_set_header   X-Real-IP        $remote_addr;<br /> proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;<br /> }
    </p>
    
    <p>
      log_format  blog_s135_com  &#8216;$remote_addr &#8211; $remote_user [$time_local] $request &#8216;<br /> &#8216;&#8221;$status&#8221; $body_bytes_sent &#8220;$http_referer&#8221; &#8216;<br /> &#8216;&#8221;$http_user_agent&#8221; &#8220;$http_x_forwarded_for&#8221;&#8216;;<br /> access_log  /data1/logs/blog.log  blog_s135_com;<br /> }<br /> }
    </p>
  </div>
</div>

附：Nginx 的安装方法可参照《<a href="http://zyan.cc/read.php/297.htm" target="_blank">Nginx 0.5.31 + PHP 5.2.4（FastCGI）搭建可承受3万以上并发连接数，胜过Apache 10倍的Web服务器</a>》文章的以下段落（仅做负载均衡，无需支持PHP的安装方法）：

二、安装PHP 5.2.4（FastCGI模式）  
4、创建www用户和组，以及其使用的目录：

三、安装Nginx 0.5.31  
1、安装Nginx所需的pcre库：  
2、安装Nginx  
3、创建Nginx日志目录  
5、启动Nginx