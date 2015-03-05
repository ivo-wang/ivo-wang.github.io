---
layout: post
title: 怎样从VPS搬家
---

用VPS的同鞋们，搬家VPS，备份VPS数据，是必修的功课，用过一段是境国外VPS的应该都懂的，在这，我就不说其中的原因了。进入我的今天的主题吧。

无论是搬家VPS还是备份VPS,主要就是备份网站的文件和数据。

先说一下备份网站的数据，由于网站数据文件可能很多，拉文件夹下来，是很不方便，也是很慢的，所以，我们先把网站的目录打一下包

```
[root@www ~]# cd /home/wwwroot //进入相应的目录
[root@www www]# tar zcvf web_root.tar.gz web_root //使用tar打包且压缩web_root文件夹，压缩后的文件名为：web_root.tar.gz

```
打包后，可以下载到本地电脑上，也可以直接传到其它VPS上

下载到本机就不说了，传到其它VPS上可以用以下命令

```
[root@www www]# scp -P 22 web_root.tar.gz root@your_vps_ip:/data //这个命令 -P 22 是指定vps SSH的端口，root@your_vps_ip,是帐号和你VPS的ip,回车后，会提示输入密码。输入确定后，文件就会传到你新的VPS的/data目录上
```
然后在新的VPS上用命令解压文件就行了

```
[root@www www]# tar -zxvf web_root.tar.gz
```

网站文件就搬完了，下面我们说一下搬备份数据库

```
方法一：使用PHPmyadmin备份数据库

这个就不说了，都是界面操作，导出并下载以本地，然后再上传到新VPS，再用PHPmyadmin导入就行了

方法二：使用mysqldump定时自动备份数据库

mysqldump -u用户名 -p密码 数据库名 > xxx.sql //导出数据库为sql文件
然后用传网站文件的方法，把SQL文件传到新VPS，如查文件太大的话，还可以用tar命令压缩一下再传输。

mysql -u你新建的用户名 -p用户名密码 你刚才新建的数据库名 < xxx.sql //导入到新的VPS数据库
```
