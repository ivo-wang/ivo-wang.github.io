---
layout: post
title: centos 部署shadowsocks
---
###**1手动安装**
```
sudo apt-get install python-pip
pip install shadowsocks
ssserver -p PORT -k YOUR_PASSWORD -m rc4-md5 -d start 
```
###**2使用一键脚本**

本脚本适用环境：
系统支持：CentOS 6.x 32或64位
内存要求：≥64M
日期：2015年01月21日

关于本脚本：
一键安装 Python 版 shadowsocks 的最新版，同时安装了 Python 包管理工具 pip。

默认配置：
服务器端口：8989
客户端端口：1080
密码：自己设定（如不设定，默认为teddysun.com）
备注：脚本默认创建单用户配置文件，如需配置多用户，安装完毕后参照下面的教程 sample 手动修改配置文件后重启即可。

客户端下载：
http://sourceforge.net/projects/shadowsocksgui/files/dist/

使用方法：
使用root用户登录，运行以下命令：

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

chmod +x shadowsocks.sh

./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

安装完成后，脚本提示如下：

```
Congratulations, shadowsocks install completed!
Your Server IP:your_server_ip
Your Server Port:8989
Your Password:your_password
Your Local IP:127.0.0.1
Your Local Port:1080
Your Encryption Method:aes-256-cfb

Welcome to visit:http://teddysun.com/342.html
Enjoy it!
```

卸载方法：
使用root用户登录，运行以下命令：

./shadowsocks.sh uninstall

单用户配置文件sample（2015年01月21日修正）：
配置文件路径：/etc/shadowsocks.json

```
{
    "server":"your_server_ip",
    "server_port":8989,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"yourpassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

多用户多端口配置文件 sample（2015年01月21日修正）：
配置文件路径：/etc/shadowsocks.json

```
{
    "server":"your_server_ip",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password":{
         "8989":"password0",
         "9001":"password1",
         "9002":"password2",
         "9003":"password3",
         "9004":"password4"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

使用命令（2015年01月21日修正）：
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status




更多版本 shadowsocks 安装：
[CentOS 下 shadowsocks-nodejs 一键安装脚本](http://teddysun.com/355.html)
[CentOS 下 shadowsocks-libev 一键安装脚本](http://teddysun.com/357.html)
[Debian 下 shadowsocks-libev 一键安装脚本](http://teddysun.com/358.html)
[CentOS 下 shadowsocks-go 一键安装脚本](http://teddysun.com/392.html)

参考链接：
http://teddysun.com/339.html







###3.特别说明
1、已安装旧版本的 shadowsocks 需要升级的话，直接运行
``` pip install -U shadowsocks```
即可，升级完成后需重启。
2、关于 CentOS 的默认 iptables 防火墙规则 icmp-host-prohibited ，如果安装之后发现已经启动 shadowsocks，本地客户端却不能连接上，请检查 iptables 是不是有如下的一条规则：

```
REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited
```

运行命令：

```
/etc/init.d/iptables status 
```

如果有这条规则的话，则添加的 8989 端口需手动更改一下，放到这条规则的上一行。编辑 /etc/sysconfig/iptables 文件，将：

```
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8989 -j ACCEPT

放在：

-A INPUT -j REJECT --reject-with icmp-host-prohibited

的前面。最终效果如下：

-A INPUT -p tcp -m state --state NEW -m tcp --dport 8989 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited

编辑完后，重启 iptables 防火墙。命令：/etc/init.d/iptables restart
```

##4.设置开机启动

- 1.添加一个后台程序
vim /etc/rc.local
在里面加入这一句：
nohup /usr/bin/ssserver -c /etc/shadowsocks.json > /dev/null 2>&1 &
- 2.如果你实在不想添加到开机启动
那么也可以在每次重启vps以后，手动执行上面这句命令，也会启动后台应用。
或者更加简洁的这样执行
ssserver -c /etc/shadowsocks.json &
也同样可行
这句里面/etc/shadowsocks.json就表示你的ss配置文件位置


##5.使用

```
apt-get install python-pip

pip install shadowsocks
和服务端一样的安装
写配置文件 /etc/shadowsocks.json
{
"server":"服务器地址",
    "server_port":服务器端口,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"密码",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

连接时候用sslocal -c /etc/shadowsocks.json

浏览器开启127.0.0.1 1080 socks5即可





