---
layout:  post
title:  youtube-dl下载速度慢的解决办法
subtitle: youtube-dl下载速度慢的解决办法 
date: 2018-10-06
author: ivo
catalog: true
header-img:
tags:
    - linux
    - youtube-dl
---
在国内看了一个Python的教程，没想到被骗了。是个二道贩子重新压制的，自己加了片头片尾，然后噩梦来了，先18集遇到的是声音不匹配始终是差一点，难受的要死。后面开始就他妈的连声音也没有了，只有片头的几秒有声音。一点都不上心，后来照着片头一找，我去连网站都停了。然后找资源，一看youtube和youku上面都有，开始尝试下载下来，youku的没成功，youtube的倒是可以就是慢的要死。几经搜索找到了应对的方法。

###0x00 安装软件

```
sudo apt-get install youtube-dl aria2
```

###0x01 原理介绍
这里使用的是aria2的多线程方式来下载，还有一点就是在所有的youtube资源里面，速度最快的是画质为best的那个即22 看下面的介绍。

youtube-dl用法

查看使用帮助

    youtube-dl -h

一些常用的参数：

    youtube-dl --list-extractors  #查看支持网站列表
    youtube-dl -U  #程序升级
    youtube-dl --get-format URL #获取视频格式
    youtube-dl -F URL #获取所有格式（目前仅支持YouTube），例如：
    youtube-dl -F https://www.youtube.com/watch?v=nCfrfCzaB2A
    [youtube] Setting language
    [youtube] n-BXNXvTvV4: Downloading video webpage
    [youtube] n-BXNXvTvV4: Downloading video info webpage
    [youtube] n-BXNXvTvV4: Extracting video information
    格式编号
    format code extension resolution  note
    171         webm      audio only  DASH audio , audio@ 48k (worst)
    140         m4a       audio only  DASH audio , audio@128k
    160         mp4       144p        DASH video , video only
    242         webm      240p        DASH video , video only
    133         mp4       240p        DASH video , video only
    243         webm      360p        DASH video , video only
    134         mp4       360p        DASH video , video only
    244         webm      480p        DASH video , video only
    135         mp4       480p        DASH video , video only
    247         webm      720p        DASH video , video only
    136         mp4       720p        DASH video , video only
    248         webm      1080p       DASH video , video only
    137         mp4       1080p       DASH video , video only
    271         webm      1440p       DASH video , video only
    264         mp4       1440p       DASH video , video only
    272         webm      2160p       DASH video , video only
    138         mp4       2160p       DASH video , video only
    100         webm      360p        3D
    82          mp4       360p        3D
    84          mp4       720p        3D
    17          3gp       176x144
    36          3gp       320x240
    5           flv       400x240
    43          webm      640x360
    18          mp4       640x360
    22          mp4       1280x720    (best)

    如果你想下真正的最高画质需要分别下上面的138和140，然后用视频软件合成。
    下载普通的视频只需要youtube-dl https://www.youtube.com/watch?v=nCfrfCzaB2A 默认下载下来的格式为webm
    youtube-dl -f format URL #下载指定格式的视频，这里以下载1080p原画质量的视频格式为例:
    youtube-dl -f 137 http://www.youtube.com/watch?v=n-BXNXvTvV4


    所以要下载最快的就得用下面这样的例子记住这个`22`
    youtube-dl -f 22 http://www.youtube.com/watch?v=n-BXNXvTvV4

下面说说`aria2多线程`

```
youtube-dl    https://www.youtube.com/***   --external-downloader aria2c --external-downloader-args "-x 16  -k 1M"

参数说明
--external-downloader aria2c //调用外部下载工具
--external-downloader-args //外部下载工具指定参数
-x 16 //启用aria2 16个线程，最多就支持16线程
-K 1M //指定块的大小
```

### 总结
综上，最快的方法就是用下面的这个命令（地址部分替换成自己的）

```
 youtube-dl -f 22 https://www.youtube.com/playlist\?list\=PLFx-dS8yeeGPPQAdpV6hiAqgi4Qzohcmk --external-downloader aria2c --external-downloader-args "-x 16  -k 1M"

```
如果使用了socks5的代理还可以加上代理

```
 youtube-dl -f 22 --proxy 'socks5://127.0.0.1:1080'  https://www.youtube.com/playlist\?list\=PLFx-dS8yeeGPPQAdpV6hiAqgi4Qzohcmk --external-downloader aria2c --external-downloader-args "-x 16  -k 1M"

```

