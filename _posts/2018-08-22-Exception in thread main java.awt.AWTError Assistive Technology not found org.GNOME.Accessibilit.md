---
layout:  post
title: Exception in thread main java.awt.AWTError Assistive Technology not found org.GNOME.Accessibilit
subtitle: freeplane 无法启动
date: 2018-08-22
author: ivo
catalog: true
header-img:
tags:
    - ubuntu 18.04 lts 
    - freeplane
---
ubuntu 18.04 lts 中freeplane 在使用的时候打开没有任何的反应.在命令行里面打开freeplane以后,看到了上面的报错`Exception in thread "main" java.awt.AWTError: Assistive Technology not found: org.GNOME.Accessibilit`,经查询后发现是openjdk的设置有了问题,变一下就可以了

我发现我的机器默认安装了2个版本的openjdk,一个openjdk8 一个openjdk11,所以我需要更改两个配置文件.

```
/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/accessibility.properties

/usr/lib/jvm/java-11-openjdk-amd64/jre/lib/accessibility.properties

都注释掉这个

# assistive_technologies=org.GNOME.Accessibility.AtkWrapper
```
