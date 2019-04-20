---
layout:  post
title:  linux下查看和添加PATH环境变量
subtitle: linux下查看和添加PATH环境变量 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - linux
    - 
---
**$PATH：决定了shell将到哪些目录中寻找命令或程序，PATH的值是一系列目录，当您运行一个程序时，Linux在这些目录下进行搜寻编译链接。**

**编辑 PATH 声明，其格式为：**

**PATH=$PATH:<PATH1>:<PATH2>:<PATH3>:------:<PATHN>**

**你可以自己加上指定的路径，中间用冒号隔开。环境变量更改后，在用户下次登陆时生效，如果想立刻生效，则可执行下面的语句：$source .bash_profile**

**可用 export命令查看PATH值。**

**单独查看PATH环境变量，可用： **echo $PATH****

**添加PATH环境变量，可用：**

**export PATH=路径:$PATH**

**查看命令：echo $PATH， 可判断是否添加PATH成功。**

**上述方法的PATH 在终端关闭后就会消失。所以还是建议通过编辑/etc/profile来改PATH，也可以改家目录下的.bashrc(即：~/.bashrc)。**

**第二种方法：**

**# vim/etc/profile**

**在文档最后，添加:**

**export PATH="目录:$PATH"**

**保存，退出，然后运行：**

**#source/etc/profile**

**不报错则成功。**
>
> --  此内容引用自http://blog.sciencenet.cn/

本内容同步更新在我的个人博客 「EZLOST」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
