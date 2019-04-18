---
layout:  post
title:  shell递归下载整站（修改版在9ishell基础上）
subtitle: shell递归下载整站（修改版在9ishell基础上） 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - shell
    - website
---
```
#!/bin/bash
# ----------------------------------------------------------------
# Filename: recursive_down_repo.sh
# Revision: 1.2
# Date: 2016-07-18
# Author: ivo
# Email: tjsytxt@126.com
# website: w-zh.ml
# Description: 递归下载网站文件,路径不乱码
# --------------------------------------------------------------

bashPath="http://ivof.tk/"
bashDownPath="/home/ivo/Videos/"
if [ ! -d $bashDownPath ]; then
mkdir -p $bashDownPath
fi
function recursive_down()
{
echo "当前网址路径:"$1
list=`curl $1 | grep -a href | awk -F '"' '{print $2}' | grep -v "^\."`
for l in $list
do
full=$1$l
#判断是否为目录,为目录则递归调用
#获得下载路径
downPath=`echo $full | sed "s#$bashPath#$bashDownPath#"`
if [[ $l =~ "/" ]]
then
#递归调用
recursive_down "$full"
else
#创建下载目录
if [ ! -d $downPath ]; then
printf $(echo -n $downPath | sed 's/\\/\\\\/g;s/\(%\)\([0-9a-fA-F][0-9a-fA-F]\)/\\x\2/g')"\n" >111
shot=`cat 111|sed s/[[:space:]]//g`
rm -rf 111
mkdir -p $shot
fi
#获取文件名
filename=$(basename "$full")
#判断是否存在文件,不存在即下载\
#变更urlencoding
printf $(echo -n $filename | sed 's/\\/\\\\/g;s/\(%\)\([0-9a-fA-F][0-9a-fA-F]\)/\\x\2/g')"\n" >111
filename=`cat 111|sed s/[[:space:]]//g`
rm -rf 111
#除重
if [ ! -f "$shot$filename" ];then
echo "正在下载$filename 到 $shot"
aria2c -c -x5 -s5 "$full" -o "$filename" -d "$shot"
fi
fi
done
}
recursive_down $bashPath
```


本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
