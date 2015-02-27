---
layout: post
title: git +github+markdown+jekyll  建立个人博客
---



先声明，这玩意及其复杂没有超级好的耐心就不用弄了活活折磨死你，要具备的基本条件如下

- git的基础知识 [github基础知识链接](http://git-scm.com/book/zh/v1)至少看完前几章
- jekyll基础知识[jekyll中文教程](http://git-scm.com/book/zh/v1)这个也是使劲的看吧
- html语言与css和js，如果不懂基本可以放弃了这东西不是一天两天就会了的，不懂也可以硬上不过会磨光你的耐心，你连别人的模板都改不了
- markdown语言linux好用的是remarkable,它和github用的是一样的链接地址[http://remarkableapp.net/](http://remarkableapp.net/)。这里再给出几个好用的linux上面的地址[http://www.linuxbsdos.com/2014/10/05/the-best-markdown-editors-for-linux/](http://www.linuxbsdos.com/2014/10/05/the-best-markdown-editors-for-linux/)。还有一个国内的中文不错的markdown的在线编辑器[https://www.zybuluo.com/mdeditor](https://www.zybuluo.com/mdeditor)
- 还有就是安装各种运行环境了，我用的是debian testing jessies linux遇到了各种各样的问题，win上面会更多。相信我。。。。。。。。。

#git环境及配置
检查SSH keys的设置

首先我们需要检查你电脑上现有的ssh key：

```
$ cd ~/.ssh 检查本机的ssh密钥

如果提示：No such file or directory 说明你是第一次使用git。
生成新的SSH Key：

$ ssh-keygen -t rsa -C "邮件地址@youremail.com"
例
$ ssh-keygen -t rsa -C "wxtbeyond@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>

注意1: 此处的邮箱地址，你可以输入自己的邮箱地址；注意2: 此处的「-C」的是大写的「C」

然后系统会要你输入密码：

Enter passphrase (empty for no passphrase):<输入加密串>
Enter same passphrase again:<再次输入加密串>

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

注意：输入密码的时候没有*字样的，你直接输入就可以了。
```
看到一个字符画就ok了

添加SSH Key到GitHub

在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。

```
    windows下1、打开本地C:\Documents and Settings\Administrator.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。
  linux下面在./ssh/里面cat id_rsa.pub
    2、登陆github系统。点击右上角的 Account Settings—->SSH Public keys —-> add another public keys

    3、把你本地生成的密钥内容复制到里面（key文本框中）， 点击 add key 就ok了
```

测试

可以输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：

```
$ ssh -T git@github.com

如果是下面的反馈：

The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?

不要紧张，输入yes就好，然后会看到：

Hi cnfeat! You've successfully authenticated, but GitHub does not provide shell access.
```

设置用户信息

现在你已经可以通过SSH链接到GitHub了，还有一些个人信息需要完善的。

Git会根据用户的名字和邮箱来记录提交。GitHub也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的，名字必须是你的真名，而不是GitHub的昵称。

```
$ git config --global user.name "cnfeat"//用户名
$ git config --global user.email  "cnfeat@gmail.com"//填写自己的邮箱
例
$ git config --global user.name "ivo-wang"
$ git config --global user.email  "wxtbeyond@gmail.com"
SSH Key配置成功

```




#安装各种运行环境

``` 
sudo apt-get install ruby        （说明最好安装的是最新版，源码安装，这里testing的源里面够新我才没有从源码安装）
sudo apt-get install rubygems
sudo apt-get install ruby-dev （这个一定要装要不安装jekyll会报错）

安装js runtime：（没有这个启动会报错提示没有js的运行环境）
$ sudo apt-get install nodejs 
$ gem install execjs


http://ruby.taobao.org/      (ruby  taobao源不用这个在天朝基本上装不上的)
如何使用？
$ gem sources --remove http://rubygems.org/ 
$ gem sources -a http://ruby.taobao.org/ 
$ gem sources -l *** CURRENT SOURCES *** 
http://ruby.taobao.org 
# 请确保只有 ruby.taobao.org 
```
使用gem安装Jekyll
使用命令gem install jekyll
安装完成后使用jekyll -v查看一下是否安装成功了


推荐文章


上面的如果不懂得话看了也是白看，这东西本来就是不是给新手准备的，即使是程序员也要鼓捣一会的，小白真的不要尝试了。。。。。。。。。。。。。
http://yansu.org/2014/02/12/how-to-deploy-a-blog-on-github-by-jekyll.html
http://yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/
