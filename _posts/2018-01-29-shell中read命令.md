---
layout: post
title: shell中read的用法
date:	2018-01-29
author:	ivo
catalog: true
tags:
    - shell
    - read
---
read命令接收标准输入（键盘）的输入，或其他文件描述符的输入（后面在说）。得到输入后，read命令将数据放入一个标准变量中。下面是 read命令的最简单形式::  

```  
#!/bin/bash  
echo -n "Enter your name:"   //参数-n的作用是不换行，echo默认是换行  
read  name                   //从键盘输入  
echo "hello $name,welcome to my program"     //显示信息  
exit 0                       //退出shell程序。  
```  
由于read命令提供了-p参数，允许在read命令行中直接指定一个提示。  
所以上面的脚本可以简写成下面的脚本:
```
#!/bin/bash  
read -p "Enter your name:" name  
echo "hello $name, welcome to my program"  
exit 0  
```  
在上面read后面的变量只有name一个，也可以有多个，这时如果输入多个数据，则第一个数据给第一个变量，第二个数据给第二个变量，如果输入数 据个数过多，则最后所有的值都给第一个变量。如果太少输入不会结束。  
