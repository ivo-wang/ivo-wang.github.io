---
title: VPS侦探 Linux VPS上配置Nginx反向代理
author: huahua
layout: post
permalink: /?p=13
categories:
  - 未分类
---
Nginx是一款高性能的HTTP和反向代理服务器。VPS侦探以前已经多次介绍过Nginx的HTTP应用，比如lnmp一键安装包。下面要说的是Nginx的反向代理功能。

反向代理是什么？

反向代理指以代理服务器来接受Internet上的连接请求，然后将请求转发给内部(或其他)网络上的服务器，并将从服务器上得到的结果返回给Internet上请求连接的客户端。

实现方法：

比如我想在VPS上建一个t.vpser.net的域名用来反向代理访问twitter，首先在域名注册商那里的域名管理上为域名t.vpser.net添加A记录到VPS的IP上，再在VPS上修改Nginx的配置文件，添加如下：

server  
{  
listen 80;  
server_name t.vpser.net;

location / {  
proxy_pass http://twitter.com/;  
proxy_redirect off;  
proxy\_set\_header X-Real-IP $remote_addr;  
proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;  
}  
}

添加好后，先执行：/usr/local/nginx/sbin/nginx -t 检查配置是否正常，如果显示：the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok configuration file /usr/local/nginx/conf/nginx.conf test is successful 则正常，否则按错误提示修改配置。

再执行 kill -HUP \`cat /usr/local/nginx/logs/nginx.pid\` 使配置生效，域名解析生效后就可以通过t.vpser.net 访问twitter了。