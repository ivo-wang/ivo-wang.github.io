---
layout:  post
title:  xfce4自动更新bing壁纸脚本
subtitle: xfce4 wallpapper auto download
date: 2018-02-20
author: ivo
catalog: true
header-img:
tags:
    - archlinux
    - xfce4
    - wallpapper
---
需要使用curl,wget,和jq,代码在这里

```
#!/bin/bash
#sleep 15
current_date=`date +%Y%m%d`
#downpath
downpath=/home/ivo/Pictures/BingPics/cn.bing.com/az/hprichbg/rb/
if [ ! -d "$downpath" ];then
	mkdir -p $downpath
fi

cd $downpath
netstat=$(ping -c3 g.cn|grep transmitted |awk '{print $4}')
if [ $netstat == 0 ]
then
	echo "/home/ivo/script/wallpapper.sh"|at "now +5 minutes"
	#tmptime=ls |grep
		elif [ `ls|grep $current_date|wc -l` == 0 ]
		then
		# use http://www.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=en-US get base_url
		base_url=`curl -s 'http://cn.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=zh-CN'|jq .images[0].url|awk '{print $1}'|sed 's/"//g'`
		#use_url=`echo $base_url |awk '{print $1}'|sed 's/"//g'`
		head_url="http://cn.bing.com"
		original_url=$head_url$base_url
		#get Pictures_name
		Pictures_name=`echo $original_url|awk -F\/ '{print $NF}'`
		#download
		wget -c $original_url
		fs=_
		mv $Pictures_name $current_date$fs$Pictures_name
		# Set the GNOME3 wallpaper
		xfconf-query -c  xfce4-desktop -p /backdrop/screen0/monitor0/workspace0/last-image -s  $downpath$current_date$fs$Pictures_name
fi

```
