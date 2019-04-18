---
layout:  post
title:  bypy python版命令行的百度云
subtitle: bypy python版命令行的百度云 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - bypy
    - 
---
百度网盘是一款云存储服务器，用户可以轻松把自己的文件上传到网盘上，并可以跨终端随时随地查看和分享。小编在本文提供的是Linux百度网盘的安装设置方法，对使用Linux系统的用户不妨来看看。

bypy - 百度云/百度网盘的Python客户端

下载地址：https://github.com/houtianze/bypy


这是一个百度云/百度网盘的Python客户端。主要的目的就是在Linux环境下（命令行）使用百度云盘的2TB的巨大空间。比如，你可以用在Raspberry Pi树莓派上。它提供文件列表、下载、上传、比较、向上同步、向下同步，等等。


全面支持Unicode / 中文。错误重试，递归上/下载，目录比较，哈希缓存。

界面是英文的，主要是因为这个是为了Raspberry Pi树莓派开发的。

第一次运行的时候要通过百度的网页进行授权（一次就好）

重要1 想要支持中文，你要把系统的区域编码（locale）设置为UTF-8。

重要2 你需要安装Python Requests 库. 在 Debian / Ubuntu / Raspbian 环境下，只需执行如下命令一次：

sudo pip install requests 或 easy_install requests
上手：
稳定版 直接sudo pip install bypy
显示使用帮助和所有命令（英文）:

bypy.py

更详细的了解某一个命令：

bypy.py help

显示在云盘（程序的）根目录下文件列表：

bypy.py list

用浏览器打开https://openapi.baidu.com.......这个网址-->点击授权--->复制授权码-->并粘入上面的提示中。

然后会在百度云的根目录里面出现一个文件夹
我的应用数据----bypy，这个就是同步的文件夹
下载里面的用后台下载 nohup bypy syncdown >/dev/null 2>&1 &

把当前目录同步到云盘：

bypy.py syncup

or

bypy.py upload

把云盘内容同步到本地来：

bypy.py syncdown

or

bypy.py downdir /

比较本地当前目录和云盘（程序的）根目录（这个很有用）：

bypy.py compare
还有一些其他命令 ...

哈希值的计算加入了缓存处理，使得第一次以后的计算速度有所提高。

运行时添加 -v 参数，程序会显示进度详情；添加 -d ，程序会显示一些调试信息。


本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
