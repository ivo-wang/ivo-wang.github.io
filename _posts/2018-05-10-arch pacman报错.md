---
layout:  post
title: pacman error failed to commit transaction
subtitle: error failed to commit transaction 
date: 2018-05-10
author: ivo
catalog: true
header-img:
tags:
    - pacman
    - error
---
pacman -Syu 升级的时候遇到了错误 error: failed to commit transaction 
error: failed to commit transaction (conflicting files)
js52: /usr/lib/libmozjs-52.so.0 exists in filesystem
Errors occurred, no packages were upgraded.

随后查询了 pacman的文档 原文解释如下
> 发生了什么事: _pacman_ 检测到文件冲突，而且按照设计，_pacman_ 不会覆盖文件。这是设计功能，不是缺陷。
>
> 先用 (`pacman -Qo 文件的完整路径` 检查哪个软件包提供了文件。如果是其它软件包，请[报告问题](https://wiki.archlinux.org/index.php/Reporting_bug_guidelines "Reporting bug guidelines")。如果不是其它软件包提供，将已经存在的文件重命名并重新升级。如果一切顺利，可以删掉备份文件。
>
> 如果是通过 `make install` 等非 pacman 方式安装的软件，安装的文件不属于任何软件包！需要先手动删除这些文件，这样就可以正常安装软件了。[不属于任何软件包的文件列表](https://wiki.archlinux.org/index.php/Pacman_tips#Identify_files_not_owned_by_any_package "Pacman tips")一文中提供了查找这些文件的脚本。
>
> 每一个安装的软件包都会提供一个 `/var/lib/pacman/local/_$package-$version_/files` 文件，包含此软件包的元数据。如果文件损坏或者丢失，将会导致升级时出现`file exists in filesystem` 错误。此错误通常只会影响一个软件包，除了手动删除或移动所有的问题文件，可以作为特例使用`pacman -S --force $package`让 _pacman_ 强制覆盖这些文件。
>
> -- https://wiki.archlinux.org/index.php/Pacman_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#.E5.8D.87.E7.BA.A7.E6.97.B6.E9.81.87.E5.88.B0.E9.97.AE.E9.A2.98:_.22file_exists_in_filesystem.22.28conflicting_files.29.21


然后 我到/usr/lib/下查看libmozjs-52.so.0是一个ln的软链接，指向libmozjs-52.so,把/usr/lib/libmozjs-52.so.0重命名 /usr/lib/libmozjs-52.so.0.bak 然后再pacman -Syu 升级成功。查看的时候，发现新的/usr/lib/libmozjs-52.so.0是一个真实的文件不再是一个软链接，与以前是冲突的所以才报的这个错误。error: failed to commit transaction (conflicting files)
js52: /usr/lib/libmozjs-52.so.0 exists in filesystem
Errors occurred, no packages were upgraded.
。改一下名就好了
