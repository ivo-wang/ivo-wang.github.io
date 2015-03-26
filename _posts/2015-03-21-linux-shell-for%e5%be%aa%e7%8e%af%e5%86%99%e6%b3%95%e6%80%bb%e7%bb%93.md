---
title: Linux Shell for循环写法总结
layout: post
categories:
  - 未分类
---
关于shell中的for循环用法很多，一直想总结一下，今天网上看到上一篇关于for循环用法的总结，感觉很全面，所以就转过来研究研究，嘿嘿&#8230;

1、 for((i=1;i<=10;i++));do echo $(expr $i \* 4);done  
2、在shell中常用的是 for i in $(seq 10)  
3、for i in \`ls\`

4、for i in ${arr[@]}  
5、for i in $* ; do  
6、for File in /proc/sys/net/ipv4/conf/*/accept_redirects; do  
7、for i in f1 f2 f3 ;do  
8、for i in *.txt  
9、for i in $(ls *.txt)  
for in语句与\` \`和$( )合用，利用\` \`或$( )的将多行合为一行的缺陷，实际是合为一个字符串数组

============ -_- ==============for num in $(seq 1 100)  
10、LIST=&#8221;rootfs usr data data2&#8243;  
for d in $LIST; do  
用for in语句自动对字符串按空格遍历的特性，对多个目录遍历  
11、for i in {1..10}  
12、for i in stringchar {1..10}  
13、awk &#8216;BEGIN{for(i=1; i<=10; i++) print i}&#8217;

注意：AWK中的for循环写法和C语言一样的

===============================================================

01.#/bin/bash

02.# author: 周海汉

03.# date :2010.3.25

04.# blog.csdn.net/ablo_zhou

05.arr=(&#8220;a&#8221; &#8220;b&#8221; &#8220;c&#8221;)  
06.echo &#8220;arr is (${arr[@]})&#8221;  
07.echo &#8220;item in array:&#8221;  
08.for i in ${arr[@]}  
09.do  
10. echo &#8220;$i&#8221;  
11.done  
12.echo &#8220;参数,\$*表示脚本输入的所有参数：&#8221;  
13.for i in $* ; do  
14.echo $i  
15.done  
16.echo  
17.echo &#8216;处理文件 /proc/sys/net/ipv4/conf/*/accept_redirects：&#8217;  
18.for File in /proc/sys/net/ipv4/conf/*/accept_redirects; do  
19.echo $File  
20.done  
21.echo &#8220;直接指定循环内容&#8221;  
22.for i in f1 f2 f3 ;do  
23.echo $i  
24.done  
25.echo  
26.echo &#8220;C 语法for 循环:&#8221;  
27.for (( i=0; i<10; i++)); do  
28.echo $i  
29.done

&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;

shell中for循环用法  
shell语法好麻烦的，一个循环都弄了一会 ，找了几个不同的方法来实现输出1－100间可以被3整除的数  
1.用(())  
#!/bin/bash  
clear

for((i=1;i<100;i++))  
for  
do  
if((i%3==0))  
then  
echo $i  
continue  
fi  
done  
2.使用\`seq 100\`  
#!/bin/bash  
clear

for i in \`seq 100\`  
do  
if((i%3==0))  
then  
echo $i  
continue  
fi  
done  
3.使用while  
#!/bin/bash  
clear

i=1  
while(($i<100))  
do  
if(($i%3==0))  
then  
echo $i  
fi  
i=$(($i+1))  
done

&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;  
在shell用for循环做数字递增的时候发现问题，特列出shell下for循环的几种方法:

1.

for i in \`seq 1 1000000\`;do

echo $i

done

用seq 1 10000000做递增，之前用这种方法的时候没遇到问题，因为之前的i根本就没用到百万(1000000),因为项目需要我这个数字远大于百万，发现用seq 数值到 1000000时转换为1e+06,根本无法作为数字进行其他运算,或者将$i有效、正确的取用，遂求其他方法解决，如下

2.

for((i=1;i<10000000;i++));do

echo $i

done

3.

i=1

while(($i<10000000));do

echo $i

i=\`expr $i + 1\`

done

因为本方法调用expr故运行速度会比第1，第2种慢不少不过可稍作改进，将i=\`expr $i + 1\`改为i=$(($i+1))即可稍作速度的提升,不过具体得看相应shell环境是否支持

4.

for i in {1..10000000;do

echo $i

done

其实选用哪种方法具体还是得由相应的shell环境的支持，达到预期的效果，再考虑速度方面的问题。