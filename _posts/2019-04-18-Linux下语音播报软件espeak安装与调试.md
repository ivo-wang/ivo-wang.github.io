---
layout:  post
title:  Linux下语音播报软件espeak安装与调试
subtitle: Linux下语音播报软件espeak安装与调试 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - 
    - 
---
apt-get install espeak
Full dictionary is not installed for ‘zh’
Can’t access (r) file ‘zh_rules’
apt-get install libpulse-dev
wget http://sourceforge.net/projects/e-guidedog/files/eSpeak-Chinese/1.47.11/espeak-1.47.11-source.tar.xz
tar xJvf espeak-1.47.11-source.tar.xzcd espeak-1.47.11-source/dictsource/
espeak --compile=zh
espeak -vzh '你好'


本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
