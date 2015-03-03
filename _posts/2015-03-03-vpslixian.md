---
layout: post
title: centos 添加epel源
---

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












