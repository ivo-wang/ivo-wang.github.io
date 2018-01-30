---
layout: post
title: debain使用私钥连接conoha主机以及恢复密码连接
---
### 运行环境 

- 主机：日本cp.conoha.jp 
- 主机系统：centos 5.9（32）
- 客户主机：debian testing jessie（32）

通过reverse dns lookup查看主机名与主机ip

```
ssh root@157.xxx
*直接提示*
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
lost connection
```

我擦咋回事？一查才发现原来cp.conoha.jp 主机默认只能通过私钥连接，然后后来再google发现几乎所有的教程都是再说用putty连接，没有说明linux下面咋说。后面再google之，终于找到了解决之道。

下面说具体方法，![](/images/3.png)
先下载下来自动生成的私钥xxx.key。修改改密钥的权限chmod 400 xxx.key

```
openssh 里面有工具一般都默认安装好的 
用ssh-add命令它的作用是 SSH代理相关程序，用来向SSH代理添加dsa　key
ssh-add xxx.key （这一步如果不成可key放到.ssh文件夹里面再去执行可能会用到root权限这点未测试）
ssh root@157.x.x.x 
不用输入密码直接就可以私钥连接了
```

### 下面再说一下如何不使用私钥连接直接还是使用用户名密码来登录


为了服务器的安全，像日本的名前vps，SSH的远程登录，就采取只能通过密钥key来登录服务器。密钥登录安全是安全，但是一旦密钥（key）没在身边的话，就痛苦了。要方便，不要安全，如何设置ssh通过密码登录呢？

- 1先登录后台，通过页面vnc登录上VPS

- 2 vim /etc/ssh/sshd_config 文件，进行如下设置：
root用户通过 SSH 登录：
找到 PermitRootLogin yes   把## 删除
设置为 
PermitRootLogin yes

- 3 文件浏览到最底部，启用密码登录：
找到 
PasswordAuthentication no 
改为 
PasswordAuthentication yes

- 4 重启sshd服务。然后就可以用putty等工具远程连接了。
service sshd restart
