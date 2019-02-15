---
layout:  post
title:  Centos7 安装支持 Snap
subtitle: Centos7 安装支持 Snap 
date: 2019-02-15
author: ivo
catalog: true
header-img:
tags:
    -  Centos7
    -  Snap
---
### 0x00 前言
有好多时候需要搭建一个 Demo 为同事或其他人演示一些应用程序，如果在服务器上新建一套比如 Owncoud Nextcloud 之类的软件，对于有些有软件洁癖的人来说是一个痛苦，用完了还得删除，不弄的话又没有好的演示环境。这时候用 Snap 和 Docker 就再合适不过了，今天我在这里为大家演示的是在 Centos7 上面添加 Snap 的安装环境来演示。Snap 官放默认的支持发型版本有以下这些，Centos 不在其中
*   [Arch Linux](https://docs.snapcraft.io/t/installing-snap-on-arch-linux/6758)*   [Debian](https://docs.snapcraft.io/t/installing-snap-on-debian/6742)*   [Deepin](https://docs.snapcraft.io/t/installing-snap-on-deepin/6791)*   [Elementary OS](https://docs.snapcraft.io/t/installing-snap-on-elementary-os/6768)*   [Fedora](https://docs.snapcraft.io/t/installing-snap-on-fedora/6755)*   [GalliumOS](https://docs.snapcraft.io/t/installing-snap-on-galliumos/6801)*   [KDE Neon](https://docs.snapcraft.io/t/installing-snap-on-kde-neon/6792)*   [Kubuntu](https://docs.snapcraft.io/t/install-snap-on-kubuntu/9967)*   [Linux Mint](https://docs.snapcraft.io/t/installing-snap-on-linux-mint/6765)*   [Lubuntu](https://docs.snapcraft.io/t/installing-snap-on-lubuntu/9965)*   [Manjaro Linux](https://docs.snapcraft.io/t/installing-snap-on-manjaro-linux/6807)*   [openSUSE](https://docs.snapcraft.io/t/installing-snap-on-opensuse/8375)*   [Parrot Security OS](https://docs.snapcraft.io/t/installing-snap-on-parrot-security-os/6810)*   [Raspbian](https://docs.snapcraft.io/t/installing-snap-on-raspbian/6754)*   [Solus](https://docs.snapcraft.io/t/installing-snap-on-solus/6803)*   [Ubuntu](https://docs.snapcraft.io/t/installing-snap-on-ubuntu/6740)*   [Xubuntu](https://docs.snapcraft.io/t/installing-snap-on-xubuntu/9963)*   [Zorin OS](https://docs.snapcraft.io/t/installing-snap-on-zorin-os/6804)
>
> --  此内容引用自https://docs.snapcraft.io/installing-snapd/6735


### 0x01 安装
> 安装 yum 插件和源

yum install yum-plugin-copr epel-release

yum copr enable ngompa/snapcore-el7

>过程需要输入 2 次 y，最后有 copr done 字样
[root@ecs-e084-0039 ~]# yum copr enable ngompa/snapcore-el7
Loaded plugins: copr, fastestmirror
You are about to enable a Copr repository. Please note that this
repository is not part of the main Fedora distribution, and quality
may vary.
The Fedora Project does not exercise any power over the contents of
this repository beyond the rules outlined in the Copr FAQ at
<https://fedorahosted.org/copr/wiki/UserDocs#WhatIcanbuildinCopr>, and
packages are not held to any quality or securty level.
Please do not file bug reports about these packages in Fedora
Bugzilla. In case of problems, contact the owner of this repository.
Do you want to continue? [y/N]: y
You are about to enable a Copr repository. Please note that this
repository is not part of the main Fedora distribution, and quality
may vary.
The Fedora Project does not exercise any power over the contents of
this repository beyond the rules outlined in the Copr FAQ at
<https://fedorahosted.org/copr/wiki/UserDocs#WhatIcanbuildinCopr>, and
packages are not held to any quality or securty level.
Please do not file bug reports about these packages in Fedora
Bugzilla. In case of problems, contact the owner of this repository.
Do you want to continue? [y/N]: y
copr done




>安装 snapd

yum install snapd

>systemd 启用及开机启动 snapd

systemctl start snapd

systemctl enable  snapd

### 0x02 试用
此时就可以使用了

sudo snap install nextcloud

