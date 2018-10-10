---
layout:  post
title:  Failed to start LSB:Bring up down networking
subtitle: Failed to start LSB:Bring up down networking 
date: 2018-10-10
author: ivo
catalog: true
header-img:
tags:
    - linux 
    - 网卡启动失败
---
「转载文章」

# Failed to start LSB: Bring up/down networking 

遇到这个错误好几次，所以总结了一下排解的几种方法。

错误记录及排查方法过程：

当我克隆出一台新的centos7的虚拟机的时候，修改了网卡配置文件启动时，报错  

<pre class="brush:bash;toolbar:false hljs coffeescript">[root@centos7 ~]# systemctl restart network
Job for network.service failed because the control process exited with error code. See "systemctl status network.service" and "journalctl -xe" for details.
[root@centos7 ~]#</pre>

按照提示输入systemctl status network.service查看

<pre class="brush:bash;toolbar:false hljs sql">[root@centos7 ~]# systemctl status network.service
● network.service - LSB: Bring up/down networking
   Loaded: loaded (/etc/rc.d/init.d/network; bad; vendor preset: disabled)
   Active: failed (Result: exit-code) since Mon 2017-03-13 23:24:37 CST; 16s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 2878 ExecStart=/etc/rc.d/init.d/network start (code=exited, status=1/FAILURE)

Mar 13 23:24:37 centos7 network[2878]: RTNETLINK answers: File exists
Mar 13 23:24:37 centos7 network[2878]: RTNETLINK answers: File exists
Mar 13 23:24:37 centos7 network[2878]: RTNETLINK answers: File exists
Mar 13 23:24:37 centos7 network[2878]: RTNETLINK answers: File exists
Mar 13 23:24:37 centos7 network[2878]: RTNETLINK answers: File exists
Mar 13 23:24:37 centos7 network[2878]: RTNETLINK answers: File exists
Mar 13 23:24:37 centos7 systemd[1]: network.service: control process exited, code=exited status=1
Mar 13 23:24:37 centos7 systemd[1]: Failed to start LSB: Bring up/down networking.
Mar 13 23:24:37 centos7 systemd[1]: Unit network.service entered failed state.
Mar 13 23:24:37 centos7 systemd[1]: network.service failed.</pre>

无奈去百度了一下，找到解决方法，说是centos7没有70-persistent-net.rules这个文件，所以新克隆的机器需要配置mac地址。

通过ip a命令查看mac地址是00:0c:29:0c:15:49 

<pre class="brush:bash;toolbar:false hljs perl">2: eno16777736: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 00:0c:29:0c:15:49 brd ff:ff:ff:ff:ff:ff</pre>

然后在配置文件中加入这一行（如果存在的话只修改就可以）  

<pre class="brush:bash;toolbar:false hljs ini">HWADDR=00:0c:29:0c:15:49</pre>

重启生效

<pre class="brush:bash;toolbar:false hljs coffeescript">[root@centos7 ~]# systemctl restart network.service
Job for network.service failed because the control process exited with error code. See "systemctl status network.service" and "journalctl -xe" for details.
[root@centos7 ~]#</pre>

发现依然有这个错误

查看启动日志  

<pre class="brush:bash;toolbar:false hljs sql">Mar 13 23:51:35 centos7 systemd: Starting LSB: Bring up/down networking...
Mar 13 23:51:35 centos7 network: Bringing up loopback interface:  Could not load file '/etc/sysconfig/network-scripts/ifcfg-lo'
Mar 13 23:51:35 centos7 network: Could not load file '/etc/sysconfig/network-scripts/ifcfg-lo'
Mar 13 23:51:35 centos7 network: Could not load file '/etc/sysconfig/network-scripts/ifcfg-lo'
Mar 13 23:51:35 centos7 network: Could not load file '/etc/sysconfig/network-scripts/ifcfg-lo'
Mar 13 23:51:35 centos7 network: [  OK  ]
Mar 13 23:51:36 centos7 network: Bringing up interface eth0:  Error: Connection activation failed: No suitable device found for this connection.
Mar 13 23:51:36 centos7 network: [FAILED]
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 network: RTNETLINK answers: File exists
Mar 13 23:51:36 centos7 systemd: network.service: control process exited, code=exited status=1
Mar 13 23:51:36 centos7 systemd: Failed to start LSB: Bring up/down networking.
Mar 13 23:51:36 centos7 systemd: Unit network.service entered failed state.
Mar 13 23:51:36 centos7 systemd: network.service failed.</pre>

发现无法加载/etc/sysconfig/network-scripts/ifcfg-lo文件

给NetworkManager-wait-online服务设置开机自启动  

<pre class="brush:bash;toolbar:false hljs bash">systemctl enable NetworkManager-wait-online.service</pre>

然后重启网卡 

<pre class="brush:bash;toolbar:false hljs coffeescript">[root@centos7 ~]# systemctl restart network
Job for network.service failed because the control process exited with error code. See "systemctl status network.service" and "journalctl -xe" for details.
[root@centos7 ~]#</pre>

查看日志

<pre class="brush:bash;toolbar:false hljs ruby">Could not load file '/etc/sysconfig/network-scripts/ifcfg-lo'</pre>

这个错误依然存在

<pre class="brush:bash;toolbar:false hljs bash">systemctl stop NetworkManager
systemctl disable NetworkManager</pre>

将 NetworkManager关闭，然后重启网卡，查看日志

<pre class="brush:bash;toolbar:false hljs perl">Mar 14 00:31:27 centos7 systemd: Starting LSB: Bring up/down networking...
Mar 14 00:31:27 centos7 network: Bringing up loopback interface:  [  OK  ]
Mar 14 00:31:28 centos7 network: Bringing up interface eth0:  ERROR    : [/etc/sysconfig/network-scripts/ifup-eth] Device eth0 does not seem to be present, delaying initialization.
Mar 14 00:31:28 centos7 /etc/sysconfig/network-scripts/ifup-eth: Device eth0 does not seem to be present, delaying initialization.
Mar 14 00:31:28 centos7 network: [FAILED]
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 network: RTNETLINK answers: File exists
Mar 14 00:31:28 centos7 systemd: network.service: control process exited, code=exited status=1
Mar 14 00:31:28 centos7 systemd: Failed to start LSB: Bring up/down networking.
Mar 14 00:31:28 centos7 systemd: Unit network.service entered failed state.
Mar 14 00:31:28 centos7 systemd: network.service failed.
Mar 14 00:33:42 centos7 dhclient[3813]: DHCPREQUEST on eno16777736 to 10.0.0.254 port 67 (xid=0x4d17f187)
Mar 14 00:33:42 centos7 dhclient[3813]: DHCPACK from 10.0.0.254 (xid=0x4d17f187)</pre>

那个错误已经不见了，但是重启网卡

<pre class="brush:bash;toolbar:false hljs coffeescript">[root@centos7 ~]# systemctl restart network
Job for network.service failed because the control process exited with error code. See "systemctl status network.service" and "journalctl -xe" for details.
[root@centos7 ~]#</pre>

依然是这个错误，然后我又重新百度，说虚拟机设置中的两个连接选项（已连接和启动时连接）没有选择也会报同样的错，但是我的已经连接，问题依然存在，到底是因为什么呢？再次查找方法。

结果这次认真看了日志报错后发现是说eth0这个文件找不到

<pre class="brush:bash;toolbar:false hljs ruby">Mar 14 00:36:39 centos7 network: Bringing up interface eth0:  ERROR    : [/etc/sysconfig/network-scripts/ifup-eth] Device eth0 does not seem to be present, delaying initialization.
Mar 14 00:36:39 centos7 /etc/sysconfig/network-scripts/ifup-eth: Device eth0 does not seem to be present, delaying initialization.</pre>

原来是之前做优化的时候将7的网卡名改成了eth0（众所周知7的网卡名是eno后面随机 一串数字），生成菜单时没有生效，那么在此生效一下  

注意网卡配置名是已经修改成eth0以后执行下面操作，一共修改的地方有三处，第一处网卡名：/etc/sysconfig/network-scripts/ifcfg-eth0 ，第二处配置文件里面：NAME=eth0 ，第三处也是配置文件里面：DEVICE=eth0

修改/etc/sysconfig/grub，添加net.ifnames=0 biosdevname=0

<pre class="brush:bash;toolbar:false hljs makefile">[root@centos7 ~]# cat  /etc/sysconfig/grub 
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=128M rd.lvm.lv=centos/root rhgb quiet net.ifnames=0 biosdevname=0"
GRUB_DISABLE_RECOVERY="true"
[root@centos7 ~]#</pre>

生成菜单

<pre class="brush:bash;toolbar:false hljs ruby">[root@centos7 ~]# grub2-mkconfig -o /boot/grub2/grub.cfg 
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-327.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-327.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-8058723e5e754d3aabc51842d9108e3b
Found initrd image: /boot/initramfs-0-rescue-8058723e5e754d3aabc51842d9108e3b.img
done
[root@centos7 ~]#</pre>

最后reboot重启  

最后登录后正常
>
> --  此内容引用自http://www.voidcn.com/article/p-dgasibma-bpo.html

