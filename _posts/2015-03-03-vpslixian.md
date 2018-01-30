---
layout: post
title: vps centos epel源 mldonkey离线下载
---
### 1.添加epel源

根据你的CentOS版本来选择正确的下载地址。

请注意EPEL的安装包是独立编译的，所以它可以安装在32位和64位系统中。

```
1.确认你的CentOS的版本
首先通过以下命令确认你的CentOS版本

$ cat /etc/redhat-release
CentOS release 6.4 (Final)

2.下载EPEL的rpm安装包
现在从上面的地址下载CentOS版本所对应的EPEL的版本

$ wget http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
3.安装EPEL
通过以下命令安装EPEL软件包

$ sudo rpm -ivh epel-release-6-8.noarch.rpm
或

$ sudo rpm -ivh epel-release*
4.检查EPEL源
安装好EPEL源后，用yum命令来检查是否添加到源列表

# yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
* base: mirrors.vonline.vn
* epel: buaya.klas.or.id
* extras: centos-hn.viettelidc.com.vn
* updates: mirrors.fibo.vn
repo id repo name status
base CentOS-6 - Base 6,381
epel Extra Packages for Enterprise Linux 6 - x86_64 10,023
extras CentOS-6 - Extras 13
nginx nginx repo 47
updates CentOS-6 - Updates 1,555
repolist: 18,019
EPEL已经在repo后列出，并且显示提供了上万个软件包，所以EPEL已经安装到你的CentOS了。
```
如果没有epel那行 编辑/etc/yum.repos.d/epel.repo  enable=0 修改成enbale=1来启动源


想暂停使用EPEL，在下面的文件中设置enabled=0即可.



### 2.安装mldonkey
```
1.安装mldonkey-server（如果找不到请安装epel源）

yum install mldonkey-server
2.生成配置文件

/etc/init.d/mldonkey start #必须启动一次才能生成/var/lib/mldonkey/downloads.ini配置文件
/etc/init.d/mldonkey stop #必须停止服务，才能修改上面的配置文件，否则重启服务后又会变回去
3.修改配置文件

vim /var/lib/mldonkey/downloads.ini #改此配置文件前请先停止服务，否则修改无效
找到allowed_ips，添加本机的外网IP（可通过ip138.com查询）
备注：由于我使用的长城宽带，IP一分钟变好几次，无法获得固定外网IP，所以我在downloads.ini中配置了0.0.0.0/0，(任意连接)之后添加用户名密码访问

4.启动服务

/etc/init.d/mldonkey start

5.iptables打开4080端口
编辑 vi  /etc/sysconfig/iptables  

-A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 4080 -j ACCEPT


添加4080端口

service iptables restart


```


### 3.配置密码

然后用浏览器打开http://IP:4080即可看到界面。

访问时提示：
SECURITY WARNING: user admin has an empty password, use command: useradd admin password
意思是MLDonkey有个默认用户admin密码为空需要设置一个秘密。（PS：删除了这个admin用户MLDonkey会启动不了）

为admin设置一个复杂的密码，在Webgui页面的命令栏输入：
useradd admin xxxxxx
点击后面的input按钮。

使用admin用户登陆，可以再添加一个自己的用户，比如：
useradd ivo 1989
添加一个zhang3用户，密码是1989。

### 4.下载方法

Web界面右上角有个长条（命令栏），后面有个按钮叫Input。在命令栏中填入地址（电驴地址，好像http地址也行，没试过），然后点Input就添加到下载列表了，BT的话就输入dllink /root/mulu/test.torrent，然后Input即可（或者将种子文件放到/var/lib/mldonkey/torrents/incoming目录下，这个目录是被自动扫描的，然后自动添加）。

下载后的文件默认存放为/var/lib/mldonkey/incoming/里面，下载的文件放在files文件夹，如果是下载的一个文件夹（bt类），就放在directories里面，当然，这些都是可以修改的。







