---
layout:  post
title:  移除ubuntu下面screenshot的声音效果
subtitle: 移除ubuntu下面screenshot的声音效果 
date: 2018-10-06
author: ivo
catalog: true
header-img:
tags:
    - linux 
    - gnome
---
经常会用到screenshot这个截屏的工具，但是一直有声音比较恼人，查了一下发现了2个方法记录下来

### 0x00 方法一
```
sudo mv /usr/share/sounds/freedesktop/stereo/camera-shutter.oga /usr/share/sounds/freedesktop/stereo/camera-shutter-disabled.oga

```
把声音文件更改名称，这样软件就找不到了。

### 0x01 方法二

```
1.Click on the sound icon at the upper right corner of the screen.
2.Now click on the sound setting.
3.Now click on sound effect 
Now mute sound from here.
4.You have done. No Screenshot sound from now onwards.
```
在gnome里面关闭了所有的声音特效。让音效处在静音的状态。
