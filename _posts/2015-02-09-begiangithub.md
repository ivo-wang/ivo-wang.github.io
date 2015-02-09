---
layout: post
title: 配置和使用Github
---
以下教程主要参考beiyuu的《使用Github Pages建独立博客》写成。
配置SSH keys

我们如何让本地git项目与远程的github建立联系呢？用SSH keys。
检查SSH keys的设置

首先我们需要检查你电脑上现有的ssh key：

	$ cd ~/.ssh 检查本机的ssh密钥	

如果提示：No such file or directory 说明你是第一次使用git。
生成新的SSH Key：
```
$ ssh-keygen -t rsa -C "邮件地址@youremail.com"  
Generating public/private rsa key pair.  
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
```
注意1: 此处的邮箱地址，你可以输入自己的邮箱地址；注意2: 此处的「-C」的是大写的「C」

然后系统会要你输入密码：
```
Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>
```
在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

注意：输入密码的时候没有*字样的，你直接输入就可以了。

最后看到这样的界面，就成功设置ssh key了：

添加SSH Key到GitHub

在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。

    1、打开本地C:\Documents and Settings\Administrator.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。

    2、登陆github系统。点击右上角的 Account Settings—->SSH Public keys —-> add another public keys

    3、把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了

测试

可以输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：

$ ssh -T git@github.com

如果是下面的反馈：

The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?

不要紧张，输入yes就好，然后会看到：

Hi cnfeat! You've successfully authenticated, but GitHub does not provide shell access.
