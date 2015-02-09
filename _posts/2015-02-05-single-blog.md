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
$ gem sources --remove https://rubygems.org/ 
$ gem sources -a https://ruby.taobao.org/ 
$ gem sources -l *** CURRENT SOURCES *** 
https://ruby.taobao.org 
# 请确保只有 ruby.taobao.org 
```
使用gem安装Jekyll
使用命令gem install jekyll
安装完成后使用jekyll -v查看一下是否安装成功了


推荐文章


上面的如果不懂得话看了也是白看，这东西本来就是不是给新手准备的，即使是程序员也要鼓捣一会的，小白真的不要尝试了。。。。。。。。。。。。。
http://yansu.org/2014/02/12/how-to-deploy-a-blog-on-github-by-jekyll.html
http://yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/
