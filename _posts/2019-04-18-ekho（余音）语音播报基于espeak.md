---
layout:  post
title:  ekho（余音）语音播报基于espeak
subtitle: ekho（余音）语音播报基于espeak 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - ekho
    - 
---
```
wget http://sourceforge.net/projects/e-guidedog/files/Ekho/6.4/ekho-6.4.2.tar.xz
tar xf  ekho-6.4.2.tar.xz
cd ekho-6.4.2
$ sudo apt-get install libsndfile1-dev libpulse-dev libncurses5-dev libestools2.1-dev festival-dev libmp3lame-dev
$ tar xJvf ekho-xxx.tar.xz
$ cd ekho-xxx
mkdir /usr/local/ekho
$ ./configure --enable-festival --prefix=/usr/local/ekho
$ make
这里如果是树莓派改make为make CXXFLAGS=-DNO_SSE
$ sudo make install
$ ekho "hello 123"
添加环境变量
vim /etc/profile
在文件最后加入
export EKHO_DATA_PATH="/usr/local/ekho/share/ekho-data$EKHO_DATA_PATH"
详解
如果没有make install，需要在当前目录运行./ekho，或者把环境变量EKHO_DATA_PATH指向ekho-data目录
source /etc/profile
```
---

首先，在树莓派里下载中文语音合成软件Ekho的Linux版。
然后，根据安装说明进行安装。关键的一句是make CXXFLAGS=-DNO_SSE
再给树莓派插上耳机线。这只是暂时测试，应该寻找音量更大的方案。
把树莓派音量调到最大（这个时候不要把耳机带上，否则音量会过大）
amixer -D pulse sset Master 100%
运行测试命令：
./ekho "hello 123"
这个时候应该可以听到耳机发出微弱的声音。 


本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
