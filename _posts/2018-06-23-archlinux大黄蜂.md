---
layout:  post
title:  archlinux大黄蜂 bumblebee
subtitle: archlinux大黄蜂 
date: 2018-06-23
author: ivo
catalog: true
header-img:
tags:
    - archlinxu 
    - bumblebee
---

首先安装这几个包，nvidia的配置不要去设置。
```
sudo pacman -S bumblebee mesa nvidia xf86-video-intel
# 必装的
sudo pacman -S mesa-demos
# 3d
sudo pacman -S nvidia-settings
# nvidia设置，没啥用
sudo pacman -S bbswitch
# 大黄蜂的自动电源管理
sudo pacman -S xorg-xrandr
# xrandr,用来查看分辨率用
```
```
sudo gpasswd -a <user> bumblebee
# 安装,并添加用户到bumblebee用户组
systemctl enable bumblebeed.service
# 启用bumblebee，然后重启继续
# bumblebee的作用是禁用nvidia独立显卡,
# 需要使用独显时，使用”optirun 程序名“开启nvidia来运行需要加速的程序。
optirun glxspheres64
optirun glxspheres32
# 运行glxspheres64测试程序，optirun用于使用独显运行程序
# 测试(x64或x32)，会打开一个动画窗口
sudo pacman -S bbswitch
# Bumblebee会自动检测bbswitch，可以自动关闭N卡
lspci | grep VGA
# 查看独显示状态，(rev ff)表示关闭，否则为打开状态
# 运行glxspheres64时，则不为(rev ff)
# 附：安装cuda等，一条命令就搞定，很是方便
sudo pacman -S cuda cudnn
# 安装CUDA, CUDNN，安装在/opt/cuda，里面有samples可以运行
sudo pacman -S python-tensorflow-cuda
# 安装tensorflow
# 注意：同样会安装numpy等包，如果已经用pip安装，需要先删了，不然会提示文件冲突
```

调整分辨率的时候用xrandr看一下对应分辨率支持的刷新率，kde桌面设置的时候要在高级里面手动选择
部分引用自`http://yhhx.tech/2017/06/11/%E7%AC%94%E8%AE%B0/Arch-Linux%E5%AE%89%E8%A3%85%E8%AE%B0%E5%BD%95/`
