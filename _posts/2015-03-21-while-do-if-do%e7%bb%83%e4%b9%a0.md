---
title: while do if do练习
author: huahua
layout: post
permalink: /?p=69
categories:
  - 未分类
---
Shell script 練習(while do&#8230;done,for do&#8230;done)  
<1>mywhile.sh  
#由0開始+1到值變為10後停止  
#!/bin/bash  
myvar=0  
while [ $myvar -ne 10 ]  
do  
echo $myvar  
myvar=$(( $myvar + 1 ))  
done  
exit 0

執行方式:  
$./mywhile.sh 

修正版:  
a.  
#修正為手動輸入參數，判斷是否為數字0-9  
#~/bin/bash  
myvar=&#8221;$1&#8243;  
for (( i=0; i<=9; i=i+1 ))  
do  
if [ "$i" == "$myvar" ]; then  
while [ $myvar -ne 10 ]  
do  
echo $myvar  
myvar=$(( $myvar + 1 ))  
done  
fi  
done  
echo "Please input num 0-9"  
exit 0

執行方式:  
1.$./mywhile.sh 12515 出現錯誤訊息  
2.$./mywhile.sh abc 出現錯誤訊息  
3.$./mywhile.sh 5 正確執行，在畫面上秀出5,6,7,8,9 

b.  
#!/bin/bash  
[ "$#" != "1" ] &#038;&#038; echo "parameter too much" &#038;&#038; exit 1  
myvar="$1"

s=$(echo $1 |tr -d 0-9)  
[ -n "$s" ] &#038;&#038; echo "$myvar not number" &#038;&#038; exit 1

if [ $myvar -gt "9" -o $myvar -lt "0" ]; then  
echo "Please input 0-9"  
exit 0  
else  
while [ $myvar -ne 10 ]  
do  
echo $myvar  
myvar=$(( $myvar + 1 ))  
done  
fi  
exit 0

<2>myfor1.sh  
#秀出所給的所有參數  
#!/bin/bash  
for thing in &#8220;$@&#8221; #thing 表示變數  
do  
echo you typed $thing  
done  
exit 0

執行方式:  
1../myfor1.sh aaa bbb ccc 畫面依序秀出you typed aaa,you typed bbb,you typed ccc

進階練習:  
a.  
#將後面給的參數IP，交由ping來執行，然後在螢幕上秀出有無成功  
#!/bin/bash

for ip in &#8220;$@&#8221;  
do  
ping -c 2 $ip &#038;> /dev/null  
if [ &#8220;$?&#8221; != &#8220;0&#8221; ]; then  
echo ping $ip fail  
else  
echo ping $ip ok  
fi  
done  
exit 0

執行方式:  
1.$./myfor1.sh 192.168.120.1 168.95.1.1 192.168.1.1  
正確執行,依序秀出ping 192.168.120.1 ok,ping 168.95.1.1 ok,ping 192.168.1.1 fail  
b.  
#在畫面上秀出類似99乘法表  
#!/bin/bash  
#writer:Marcus Tsai 20100913  
read -p &#8220;請輸入第一個數字:&#8221; fn  
s=$(echo $fn |tr -d 0-9)  
[ -n &#8220;$s&#8221; ] &#038;&#038; echo &#8220;Error: $fn 不是數字&#8221; &#038;&#038; exit 1  
read -p &#8220;請輸入第二個數字:&#8221; sn  
s=$(echo $sn |tr -d 0-9)  
[ -n &#8220;$s&#8221; ] &#038;&#038; echo &#8220;Error: $sn 不是數字&#8221; &#038;&#038; exit 1

for (( i=1 ; i<=$fn ; i=i+1 ))  
do  
for (( j=1 ; j<=$sn ; j=j+1 ))  
do  
total=$(($i*$j))  
echo &#8220;$i X $j = $total&#8221;  
done  
done  
exit 0  
b修正版:  
#在畫面上將結果分開秀出，而不是都在同一列，並增加錯誤判斷  
#!/bin/bash  
#Author : marcus Tsai 20100913  
read -p &#8220;請輸入第一個數字1-9:&#8221; fn  
s=$(echo $fn |tr -d 0-9)  
[ -n &#8220;$s&#8221; ] &#038;&#038; echo &#8220;Error: $fn 不是數字&#8221; &#038;&#038; exit 1  
[ $fn -gt &#8220;10&#8221; -o $fn -lt &#8220;1&#8221; ] &#038;&#038; echo &#8220;Error: 請輸入1-9的數字&#8221; &#038;&#038; exit 1  
read -p &#8220;請輸入第二個數字1-9:&#8221; sn  
s=$(echo $sn |tr -d 0-9)  
[ -n &#8220;$s&#8221; ] &#038;&#038; echo &#8220;Error: $sn 不是數字&#8221; &#038;&#038; exit 1  
[ $sn -gt &#8220;10&#8221; -o $sn -lt &#8220;1&#8221; ] &#038;&#038; echo &#8220;Error: 請輸入1-9的數字&#8221; &#038;&#038; exit 1  
for (( i=1 ; i<=$sn ; i=i+1 ))  
do  
for (( j=1 ; j<=$fn ; j=j+1 ))  
do  
total=$(($i*$j))  
printf &#8220;%s\t &#8221; &#8220;echo $j X $i = $total&#8221;  
done  
echo -e &#8220;\n&#8221;  
done  
exit 0