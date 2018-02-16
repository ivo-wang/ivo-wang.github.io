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
  - 刻录使用刻录大师，选iso镜像，`千万不要选择成数据盘`这样没用
  - linux 下建议用`dd命令`来操作`dd if=./archlinux.iso of=/dev/sdx` #`x代表ｕ盘`。windows下建议使用`rufus`来烧录，操作非常的方便成功率也高。

- 设置主机`bios` 从u盘或是光盘启动
- 设置bios里面的uefi选项为启用
- 有线网络，尽量用有线。无线我的笔记本就不好用，以后会更新解决办法
- 一部手机或ＩＰＡＤ用来查看文档用，我是整机安装的没有使用虚拟机

### 0x02 安装准备过程
- 查看键盘布局`loadkeys layout` 默认为美式键盘映射 一般不用改
  - 将 _layout_ 转换为您的键盘布局，如`fr`，`uk`，`dvorak`或`be-latin1`。[这里](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements "wikipedia:ISO 3166-1 alpha-2")有国家的二位字母编码表。使用命令 `ls /usr/share/kbd/keymaps/**/*.map.gz` 列出所有可用的键盘布局。[Console fonts](https://wiki.archlinux.org/index.php/Console_fonts "Console fonts") 位于 `/usr/share/kbd/consolefonts/`, 设置方式请参考 [setfont(8)](https://jlk.fjfi.cvut.cz/arch/manpages/man/setfont.8).
- 验证uefi `ls /sys/firmware/efi/efivars` 不存在的话启动的是bios方式，需要对启用主板的uefi功能。如果没有就算了。
- 检查网是不是连上了 `ping -c 3 archlinux.org` 
  - 连上的话是有返回的
  - 若发现网络不通,利用 `systemctl stop dhcpcd@`,`TAB` 停用 `dhcpcd` 进程。这里如果就有问题，请前往arch官网查看网络相关的配置。出师不利(我用的wifi，非常的不好用就是连不上，最后重新用的有线，折腾到最后也基本上玩明白无线了，以后会讲，篇幅略大)  
    - 无线的方式连接
- 更新系统时间 `timedatectl set-ntp true`
- 下面要对硬盘重新的分区 因为用的gpt所以我用的gdisk工具，如果是用mbr直接用disk就好了。说几个gdisk常用的选项 o 新建分区表gpt格式的;n新建分区;p 列出现有分区;d删除分区;w保存。我用的硬盘分区如下 128g固态，500m esp分区，4g swap，其余给/;500g 机械，给/home
 具体过程如下。首先用`lsblk`来查看具体的分区大小来判断哪个是机械硬盘，哪个是固态硬盘。我的机械是`/dev/sda`，固态`/dev/sdb`. 现在开始分区，`gdisk /dev/sdb`
  - 首先 p一下看看有哪些已经做好的分区，然后一个一个的d删除，o一下新建一个gpt分区表，之后n，创建新的分区，编号默认，起始默认，+500M这是esp分区，8300 linux默认即可，这就是sdb1 500m的esp分区。这里说一下有的教程会让更改83xx的号，没用，后面都得重新格式化，所以没有意义。继续，n，number不管，起始默认，+4G，8300。再继续，n，number不管，起始默认，结束默认，8300默认，这里要保存一下，w。完成。然后将机械硬盘整体格式化为`sda1 500g`
  - 此时分区为 `esp /dev/sdb1 500m`，`swap /dev/sdb2 4g`，`根分区 / /dev/sdb3`，`/home /dev/sda1 500g`。分别格式化操作。`mkfs.fat -F32 /dev/sdb1`,`mkswap /dev/sdb2`,`mkfs.ext4 /dev/sdb3`,`mkfs.ext4 /dev/sda1`
- 挂载 `mount /dev/sdb3 /mnt` 创建三个文件夹boot，boot/efi，home。`cd /mnt` `mkdir -p boot/efi` `mkdir home`.继续挂载 `mount /dev/sdb1 /mnt/boot/efi` `mount /dev/sda1 /mnt/home`.启用swap `swapon /dev/sdb2`。后面会用到genfstab，它会自动检测挂载的文件系统和 swap 分区。

### 0x03 安装
- 选择镜像源，编辑 `/etc/pacman.d/mirrorlist`因为我们在中国把china的放几个就好啦，注释要不要无所谓，最终这个文件里面只要有几个中国的地址就好来，比如163 ustc 和清华的
```
## China
Server = http://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = http://mirrors.xjtu.edu.cn/archlinux/$repo/os/$arch
Server = http://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```
这个mirrorlist非常的重要一定要做对了，后面也将通过 pacstrap 被复制并保存在到系统中。
- 安装基本系统。因为要使用AUR所以还要装一个`base-devel`。如果不用可以不装这个。输入命令`pacstrap -i /mnt base base-devel` 。使用 `-i` 选项时会在实际安装前进行确认。

### 0x04 配置系统
- fstab 这个用命令去生成不用手动的敲，`genfstab -U /mnt >> /mnt/etc/fstab`，在这样做完以后最好去看一下/mnt/etc/fstsb有没有内容，内容正确不正确。

- chroot [Chroot](https://en.wikipedia.org/wiki/Chroot "wikipedia:Chroot") 就是变更当前进程及其子进程的可见根路径。变更后，程序无法访问可见根目录外文件和命令。这个目录叫作 _chroot jail_。输入命令 `arch-chroot /mnt`这样根目录就变成了/mnt了。以下是需要的操作
  - 设置时区上海的时区 `ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`  - 设置时间标准为 UTC，并调整时间漂移`hwclock --systohc --utc`
  - Locale 本地化的程序与库若要本地化文本，都依赖 Locale, 后者明确规定地域、货币、时区日期的格式、字符排列方式和其他本地化标准等等.在下面两个文件设置：`locale.gen` 与 `locale.conf`.`/etc/locale.gen`是一个仅包含注释文档的文本文件。指定您需要的本地化类型，只需移除对应行前面的注释符号（`＃`）即可，建议选择帶`UTF-8`的項
  ```
# nano /etc/locale.gen
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
zh_TW.UTF-8 UTF-8
  ```
  - 执行`locale-gen`以生成locale信息。`/etc/locale.gen` 生成指定的本地化文件，每次 [glibc](https://www.archlinux.org/packages/?name=glibc) 更新之后也会运行 `locale-gen`。
  - 创建 `locale.conf` 将系统 locale 设置为`en_US.UTF-8`，系统的 Log 就会用英文显示，这样更容易问题的判断和处理。输入命令 ` echo LANG=en_US.UTF-8 > /etc/locale.conf`不推荐将这里设置中文，tty容易乱码。
  - 主机名 要设置`hostname`，将其添加到 `/etc/hostname`, `myhostname` 是需要的主机名：`echo myhostname > /etc/hostname`。最好也添加到hosts的`127.0.1.1`一份。
  ```
/etc/hosts
127.0.0.1	localhost.localdomain	localhost
::1		localhost.localdomain	localhost
127.0.1.1	myhostname.localdomain	myhostname
  ```
  - 网络配置 如果使用有线网的话，令dhcp服务开机启动:`systemctl enable dhcpcd.service`.如果使用无线网络的话，要安装这几个包，否则重启之后无法连接无线网络`pacman -S iw wpa_supplicant dialog networkmanager`.
  > 如果退出chroot之后，进入arch，使用Wifi，那么就可以禁用dhcp，让networkmanager自启。如果是有线就暂时不进行下面的操作。使用networkmanager的原因是和Gnome3结合的很好，并且可以管理有线和wifi。
  > 对新安装的系统，需要再次设置网络。具体请参考 [Network configuration (简体中文)](https://wiki.archlinux.org/index.php/Network_configuration_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87) "Network configuration (简体中文)") 。对于 [无线网络配置](https://wiki.archlinux.org/index.php/Wireless_network_configuration_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87) "Wireless network configuration (简体中文)")，[安装](https://wiki.archlinux.org/index.php/%E5%AE%89%E8%A3%85 "安装") 软件包 [iw](https://www.archlinux.org/packages/?name=iw), [wpa_supplicant](https://www.archlinux.org/packages/?name=wpa_supplicant)，[dialog](https://www.archlinux.org/packages/?name=dialog) 以及需要的 [固件软件包](https://wiki.archlinux.org/index.php/Wireless#Installing_driver.2Ffirmware "Wireless").
  - initramfs 创建一个初始 RAM disk：`mkinitcpio -p linux`
  - root密码 用`passwd`来更改
  - grub引导.先安装所需要的程序`pacman -S  grub efibootmgr intel-ucode` 然后配置`grub-install --recheck /dev/sdb` 如果提示error:cannot find EFI directory，说明找不到EFI文件夹的位置，还需要加上--efi-directory参数指明安装位置`grub-install --recheck /dev/sdb --efi-directory=/boot`
> 没有错误则说明安装成功。安装完毕之后还需要生成一个grub配置文件。这一步会探测系统上已经安装的系统并写入到配置文件中。但是由于在安装介质环境中，此时Windows系统可能会探测不到。等到一会重启真正进入Arch环境之后，还需要重新执行一下这个命令，这样就会正常地探测到所有系统了。
运行`grub-mkconfig -o /boot/grub/grub.cfg`即将完成了。

> 处理器厂商会发布 microcode 以增强系统稳定性和解决安全问题。Microcode 可以通过 BIOS 更新，Linux 内核也支持启动时应用新的 Microcode。没有这些更新，可能会遇到一些很难查的的死机或崩溃问题。建议所有 Intel 用户使用新的微代码。Intel Haswell 和 Broadwell 处理器家族的用户请务必使用最新的微代码。

### 重启
输入 `exit` 或按 `Ctrl+D` 退出 chroot 环境。
可选用 `umount -R /mnt` 手动卸载被挂载的分区：这有助于发现任何“繁忙”的分区，并通过  [fuser(1)](https://jlk.fjfi.cvut.cz/arch/manpages/man/fuser.1) 查找原因。
最后，通过执行 `reboot` 重启系统：_systemd_ 将自动卸载仍然挂载的任何分区。不要忘记移除安装介质，然后使用root帐户登录到新系统。

至于图形界面及美化，将在下一篇中介绍

