---
layout:  post
title:  shell写jeklly的title脚本
subtitle: 快速编写markdown博客 
date: 2018-02-01
author: ivo
catalog: true
header-img:
tags:
    - shell
---
写博客总写头,太麻烦,于是编写了下面的这个脚本,有需要的拿走吧.具体代码如下
保存成一个文件,比如我写成的是pl放到/usr/local/bin下面给pl添加执行权限
就可以直接在终端里面使用pl来写md了.
输入pl后会要求输入简洁的关键字或是博客的名字,然后回车,在vim中就可以直接编辑了

[pl的源码](/code/pl)

<pre>
#!/bin/bash
workdir=~/Documents/post/
cudate=`date +%Y-%m-%d`
read -p"Please input simple title:" name
touch $workdir$cudate-"$name".md
cat >$workdir$cudate-"$name.md"<<EOF
---
layout:  post
title:  $name
subtitle: $name 
date: $cudate
author: ivo
catalog: true
header-img:
tags:
    - 
    - 
---
EOF

vim $workdir$cudate-"$name".md

</pre>
