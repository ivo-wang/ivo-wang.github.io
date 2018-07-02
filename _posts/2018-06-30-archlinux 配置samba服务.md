---
layout:  post
title:  archlinux 配置samba服务
subtitle: archlinux 配置samba服务 
date: 2018-06-30
author: ivo
catalog: true
header-img:
tags:
    - archlinux 
    - samba
---
最近看了好些的samba的文档，有的比较老，不适用于新版的samba，由于arch的激进所以最官方的还是去`wiki.samba.org`，文档太多我页看不过来。弄一些重要的简单说说。

### 配置文件
```
cat /etc/samba/smb.conf
[global]

    workgroup = workgroup

    server string = Samba Server Version %v

    log file = /var/log/samba/log.%m

    max log size = 50

    security = user

    passdb backend = tdbsam


[public]

   comment = Public Stuff

   path = /home/ivo/share

   public = yes

   writable = yes

   printable = no

   valid users = ivo,hy

```
### 解析
 Samba的配置文件为/etc/samba/smbsmb.conf.通过修改这个配置文件来完成我们的各种需求。打开这个配置文件，你会发现很多内容都用”#”或者”;”注视掉了
 主要有以上三个部分：[global], [homes], [printers]。

**[global]** 定义全局的配置，”workgroup”用来定义工作组，相信如果你安装过windows的系统，你会对这个workgroup不陌生。一般情况下，需要我们把这里的”MYGROUP”改成”WORKGROUP”（windows默认的工作组名字）。

security = user #这里指定samba的安全等级。关于安全等级有四种：

*   share：用户不需要账户及密码即可登录samba服务器
*   user：由提供服务的samba服务器负责检查账户及密码（默认）
*   server：检查账户及密码的工作由另一台windows或samba服务器负责
*   domain：指定windows域控制服务器来验证用户的账户及密码。

passdb backend = tdbsam # passdb backend （用户后台），samba有三种用户后台：smbpasswd, tdbsam和ldapsam。

smbpasswd：该方式是使用smb工具smbpasswd给系统用户（真实用户或者虚拟用户）设置一个Samba 密码，客户端就用此密码访问Samba资源。smbpasswd在/etc/samba中，有时需要手工创建该文件。

tdbsam：使用数据库文件创建用户数据库。数据库文件叫passdb.tdb，在/etc/samba中。passdb.tdb用户数据库可使用smbpasswd –a创建Samba用户，要创建的Samba用户必须先是系统用户。也可使用pdbedit创建Samba账户。

pdbedit参数很多，列出几个主要的：

-pdbedit –a username：新建Samba账户。 -pdbedit –x username：删除Samba账户。 -pdbedit –L：列出Samba用户列表，读取passdb.tdb数据库文件。 -pdbedit –Lv：列出Samba用户列表详细信息。 -pdbedit –c “[D]” –u username：暂停该Samba用户账号。 -pdbedit –c “[]” –u username：恢复该Samba用户账号。

ldapsam：基于LDAP账户管理方式验证用户。首先要建立LDAP服务，设置“passdb backend = ldapsam:ldap://LDAP Server” load printers 和 cups options 两个参数用来设置打印机相关。

除了这些参数外，还有几个参数需要你了解：

*   netbios name = MYSERVER # 设置出现在“网上邻居”中的主机名
*   hosts allow = 127\. 192.168.12\. 192.168.13\. # 用来设置允许的主机，如果在前面加”;”则表示允许所有主机
*   log file = /var/log/samba/%m.log #定义samba的日志，这里的%m是上面的netbios name
*   max log size = 50 # 指定日志的最大容量，单位是K

**[homes]** 该部分内容共享用户自己的家目录，也就是说，当用户登录到samba服务器上时实际上是进入到了该用户的家目录，用户登陆后，共享名不是homes而是用户自己的标识符，对于单纯的文件共享的环境来说，这部分可以注视掉。

**[printers]** 该部分内容设置打印机共享。
>
> --  此内容引用自http://wiki.jikexueyuan.com/project/linux/samba.html#7a6ac715aff30e4f32afd21d3ff65eaa

```
sudo useradd hy
sudo pdbedit -a hy
sudo pdbedit -L

```
