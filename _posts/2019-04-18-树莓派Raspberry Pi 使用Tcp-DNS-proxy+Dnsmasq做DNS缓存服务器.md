---
layout:  post
title:  树莓派Raspberry Pi 使用Tcp-DNS-proxy+Dnsmasq做DNS缓存服务器
subtitle: 树莓派Raspberry Pi 使用Tcp-DNS-proxy+Dnsmasq做DNS缓存服务器 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - 
    - 
---
### 背景

刚刚做个个无线路由器，为了发挥树莓派的功能，决定做个DNS缓存服务器。

这里我们要用到两个软件：**Tcp-DNS-proxy+Dnsmasq**

**Tcp-DNS-proxy**：用来解决**DNS 污染**防止**中间人攻击**。当然网上多用DNSCrypt

**Dnsmasq**：缓存DNS，加快解析速度。

[DNSCrypt 的工作原理是什么？为什么能防止 DNS 污染？](http://www.zhihu.com/question/24253866/answer/29291568)


### Tcp-DNS-proxy

先下载[https://github.com/henices/Tcp-DNS-proxy](https://github.com/henices/Tcp-DNS-proxy)。

它是用Python语言写的，所以首先你要安装Python2.7

但是软件运行依赖**gevent >= 1.0.1;python-daemon >= 1.5.5**，so,我们要安装**pip or easy_install**

     wget https://bootstrap.pypa.io/ez_setup.py -O - | python

安装好后，sh [install.sh](http://install.sh/) 就可以 自动安装以上两个依赖的软件。

为了配合dnsmasq缓存，我们这里要改下默认端口53改为35535。

    if __name__ == "__main__":

        parser = argparse.ArgumentParser(description='TCP DNS Proxy')
        parser.add_argument('-f', dest='config_json', type=argparse.FileType('r'),
                required=True, help='Json config file')
        args = parser.parse_args()

        cfg = json.load(args.config_json)
        if not cfg.has_key("host"):
            cfg["host"] = "0.0.0.0"
        if not cfg.has_key("port"):
            cfg["port"] = 35535

**更新：**　新版更换端口直接在tcpdns.json里修改．

###  测试

运行 tcp-dns-porxy

    [root@linsirpi tcpdns]# python tcpdns.py -f tcpdns.json
    TCP DNS Proxy, https://github.com/henices/Tcp-DNS-proxy
    DNS Servers:
    8.8.8.8:53
    8.8.4.4:53
    156.154.70.1:53
    156.154.71.1:53
    208.67.222.222:53
    208.67.220.220:53
    74.207.247.4:53
    209.244.0.3:53
    8.26.56.26:53

修改 vi /etc/resolve.conf

    nameserver 127.0.0.1

dig (如果没有，pacman -S dnsutils)

    [root@linsirpi ~]# dig -p 35535 sina.com

    ; <<>> DiG 9.9.2-P2 <<>> -p 35535 sina.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34678
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;sina.com.      IN  A

    ;; ANSWER SECTION:
    sina.com.   30  IN  A 12.130.132.30

    ;; Query time: 81 msec
    ;; SERVER: 127.0.0.1#35535(127.0.0.1)
    ;; WHEN: Sat Aug 23 02:53:37 2014
    ;; MSG SIZE  rcvd: 53

    >> Query Timeout: 20.000000
    >> Enable Cache: True
    >> Now you can set dns server to localhost

OK,能正常获取。

* * *

### Dnsmasq

vi /etc/dnsmasq.conf 修改以下：

    no-resolv 
    no-poll 
    server=127.0.0.1#35535 
    cache-size=10000    

重启dnsmasq

    systemctl restart dnsmasq

测试

    [root@linsirpi ~]# dig -p 53 sina.com.cn

    ; <<>> DiG 9.9.2-P2 <<>> -p 53 sina.com.cn
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 33464
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 512
    ;; QUESTION SECTION:
    ;sina.com.cn.     IN  A

    ;; ANSWER SECTION:
    sina.com.cn.    32  IN  A 202.108.33.60

    ;; Query time: 167 msec
    ;; SERVER: 127.0.0.1#53(127.0.0.1)
    ;; WHEN: Sat Aug 23 03:05:37 2014
    ;; MSG SIZE  rcvd: 56

第二次查询的时候：

    sina.com.cn.    20  IN  A 202.108.33.60

    ;; Query time: 2 msec
    ;; SERVER: 127.0.0.1#53(127.0.0.1)
    ;; WHEN: Sat Aug 23 03:05:59 2014
    ;; MSG SIZE  rcvd: 56

只用了2ms!!缓存OK !

* * *

### 开机启动

> 不同的Linux发行版有不同的自启动机制，如RedHat有 /etc/rc.local 文件，在里面写上要执行的命令就可以开机执行。 Arch Linux 采用的是守护进程的机制（daemon）。 在Arch Linux中, 守护进程是用systemd管理的.

Archlinux 是没有rc.local服务的，所以我们先新建个rc.local.service:  

1.  创建服务文件 /usr/lib/systemd/system/rc-local.service
```
    [Unit] 
    Description=/etc/rc.local Compatibility
    ConditionPathExists=/etc/rc.local
    [Service]
    Type=forking
    ExecStart=/etc/rc.local start
    TimeoutSec=0
    StandardOutput=tty
    RemainAfterExit=yes
    SysVStartPriority=99
    [Install]  
    WantedBy=multi-user.target
```
2.添加软链接
```
    cd /etc/systemd/system/multi-user.target.wants

    ln -s /usr/lib/systemd/system/rc-local.service rc-local.service
```
3.脚本写入/etc/rc.local
```
    #!/bin/bash

    python2 /root/tcpdns/tcpdns.py -f tcpdns.json >/dev/null 2<&1 &
```
添加可执行权限
```
   chmod +x /etc/rc.local
```
4.设置开机启动
```
        systemctl enable rc-local.service
        systemctl start  rc-local.service
        systemctl enable dnsmasq.service
```
Okay,enjoy it！！

让解析更智能,请猛击这里:[Dnsmasq:让DNS智能查询](https://linsir.org/post/let-dns-query-smartly-with-dnsmasq)

* * *

###   更新

**2015.06.24** 最近升级tcp-dns发现评论里的安装python-daemon后还是会提示：

    ImportError: cannot import name Daemon

查看包，发现确实没有Daemon

    >>> import daemon
    >>> dir(daemon)
    ['DaemonContext', '__builtins__', '__doc__', '__file__', '__name__', '__package__', '__path__', 'absolute_import', 'daemon', 'unicode_literals']
    >>> 

放狗搜，找到解决办法：

**下载https://github.com/serverdensity/python-daemon/blob/master/daemon.py 到tcp-dns目录下即可。**

另外由于墙的很厉害，在tcp-dns.json里把**speed_test改为true**，不然总会提示timed out，程序会终止。

* * *

参考：

1.  [Linux 下加密 DNS 和缓存 DNS 技术](http://www.aguidetoshanghai.com/programnote/dnscrypt-and-dnsmasq/)
2.  [树莓派开机自启动程序(Arch Linux 版本)](http://www.linuxidc.com/Linux/2013-05/84748.htm)
>
> --  此内容引用自https://linsir.org/post/Raspberry_Pi_Tcp_Dnsmasq_cache_server


本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
