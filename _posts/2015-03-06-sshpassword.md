---
layout: post
title: ssh免密码登录
---

如果需要“machineA机器的nameA账号”建立到“machineB机器的nameB账号”的ssh信任关系，达到无需输密码即可登陆的目的，那么我一般是这样做的：

```
1 将machineA机器的/home/nameA/.ssh/id_rsa.pub文件的内容拷贝出来

2 登陆到machineB机器的/home/nameB/.ssh中，如果不存在则创建authorized_keys文件，
将第1步中的内容追加到文件尾部。

3 检查authorized_keys文件的权限，确保其group/other位没有w权限

4 登陆到machineA机器，测试ssh信任关系是否建好
```
【五分钟学会ssh-copy-id】

为nameA账号建立自己的公钥私钥，建立好之后，会在/home/nameA/.ssh里多出id_rsa（私钥）和id_rsa.pub（公钥）两个文件：

```
[nameA@machineA]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/nameA/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/nameA/.ssh/id_rsa.
Your public key has been saved in /home/nameA/.ssh/id_rsa.pub.
The key fingerprint is:
bb:3b:14:be:5d:45:ab:72:27:ec:93:21:c6:a3:7d:77 nameA@machineA
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|                 |
|        .   .    |
|       .X. o .   |
|       .o.  +    |
|       .*+.o     |
|       +++C+o E  |
|      . +C++ .   |
+-----------------+
好了，准备工作就绪，我们开始建立信任关系：
```

```
[nameA@machineA]$ ssh-copy-id nameB@machineB
ssh: connect to host machineB port 22: Connection refused
```
悲剧，新的错误提示又来了，原来我们的B机器的sshd的服务端口不是22，而是22000，但是ssh-copy-id命令却不知道这个信息。这可如何是好。

我们试试加个-p参数设置下端口：

```
[nameA@machineA]$ ssh-copy-id nameB@machineB -p 22000
ssh: connect to host machineB port 22: Connection refused
还是不好使，-p参数完全没有被ssh-copy-id命令识别。
如果你man ssh-copy-id就可以看到它根本就没有这个选项的。

好吧，不卖关子了，其实解决办法一点也不复杂，只是用了一个小技巧，那就是：

[nameA@machineA]ssh-copy-id "-p 22000 nameB@machineB"
nameB@machineB's password:
[nameB@machineB]
大功告成，终于可以无密码登陆了：

[nameA@machineA]$ ssh nameB@machineB -p 22000
[nameB@machineB]$
其实ssh-copy-id是一个普普通通的脚本文件：
```
