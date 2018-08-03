---
layout:  post
title:  ubuntu 安装ttf字体
subtitle: ubuntu 安装ttf字体 
date: 2018-08-03
author: ivo
catalog: true
header-img:
tags:
    - ubuntu 
    - ttf
---
切换到目录 `/usr/share/fonts/`或`.local/share/fonts` 下，新建文件夹，比如 yahei：

    cd /usr/share/fonts
    sudo mkdir yahei

将字体文件（*.ttf）拷贝到新建的目录：

    sudo cp [字体文件路径] /usr/share/fonts/yahei/

修改文件的权限：

    sudo chmod 644 /usr/share/fonts/yahei/*.ttf

开始安装步骤：

    sudo mkfontscale
    sudo mkfontdir
    sudo fc-cache -fv

> 命令大意：

1.  创建字体的 fonts.scalecale 文件，用来控制字体的旋转和缩放
2.  创建字体的 fonts.dir 文件，用来控制字体粗斜体产生
3.  建立字体缓存信息，也就是让系统跟字体进行深入的交流
`第一第二条好些时候可以不做`




如果 fc-cache 命令没有，可以根据提示用 `sudo apt-get install` 命令安装一个。

通过这种方式安装并不需要重启电脑，嗯，很好～
>
> --  此内容引用自http://www.cqamin.com/ubuntu-an-zhuang-wei-ruan-ya-hei/#html
