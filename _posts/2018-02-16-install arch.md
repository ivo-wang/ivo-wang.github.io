---
layout:  post
title:  根据官方文档安装archlinux 基础篇
subtitle: 安装uefi启动 gpt分区的archlinunx
date: 2018-02-16
author: ivo
catalog: true
header-img:
tags:
    - linux
    - archlinux
---
简单介绍一下，这里使用的是arch的官方文档的安装顺序进行的安装，这里介绍的是安装出来不带图形界面的版本的archlinux，下面会尽量的详细去记录整个的安装过程。

### 0x01 准备过程
- archlinux的livecd光盘，这个去archlinux官网去下载[https://www.archlinux.org/download/](https://www.archlinux.org/download/)
- 把iso刻录进光盘;用dd命令或其他方式烧录iso到u盘中
####　刻录及烧录
　- 刻录使用刻录大师，选iso镜像，千万不要选择成数据盘这样没用
　- linux 下建议用dd命令来操作　dd if=./archlinux.iso of=/dev/sdx #x代表ｕ盘。windows下建议使用rufus来烧录，操作非常的方便成功率也高。
- 设置主机bios 从u盘或是光盘启动
- 设置bios里面的uefi选项为启用
- 有线网络，尽量用有线。无线我的笔记本就不好用，以后会更新解决办法
- 一部手机或ＩＰＡＤ用来查看文档用，我是整机安装的没有使用虚拟机

### 0x02 安装准备过程
- 查看键盘布局`loadkeys layout` 默认为美式键盘映射 一般不用改
- 验证uefi `ls /sys/firmware/efi/efivars` 不存在的话启动的是bios方式，需要对启用主板的uefi功能。如果没有就算了。
- 检查网是不是连上了 `ping -c 3 archlinux.org` 若发现网络不通,利用 `systemctl stop dhcpcd@`,`TAB` 停用 `dhcpcd` 进程。这里如果就有问题，请前往arch官网查看网络相关的配置。出师不利（我用的wifi，非常的不好用就是连不上，最后重新用的有线，折腾到最后也基本上玩明白无线了，以后会讲，篇幅略大）
- 更新系统时间 `timedatectl set-ntp true`
- 下面要对硬盘重新的分区 因为用的gpt所以我用的gdisk工具，如果是用mbr直接用disk就好了。说几个gdisk常用的选项 o 新建分区表gpt格式的;n新建分区;p 列出现有分区;d删除分区;w保存。我用的硬盘分区如下 128g固态，500m esp分区，4g swap，其余给/;500g 机械，给/home
 具体过程如下。首先用`lsblk`来查看具体的分区大小来判断哪个是机械硬盘，哪个是固态硬盘。我的机械是`/dev/sda`，固态`/dev/sdb`. 现在开始分区，`gdisk /dev/sdb`

