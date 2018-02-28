---
layout:  post
title:  arch桌面及美化
subtitle: archlinux xfce桌面及美化
date: 2018-02-16
author: ivo
catalog: true
header-img:
tags:
    - linux
    - archlinux
    - desktop
---
好了，接上一篇。上一篇安装完我们就有了一个基本的archlinux，没有桌面环境，工作起来不是很方便。这里介绍安装配置常用的软件及优化桌面。

### 基本配置
- 安装zsh `pacman -S zsh git wget` `sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"` 这时候因为zsh不匹配`*` 所以还要做一条设置添加到`~/.zshrc`添加在最后`setopt no_nomatch` 然后进行source .zshrc命令
- 创建新用户ivo `useradd -m -G wheel ivo ` `-m`：在创建时同时在`/home`目录下创建一个与用户名同名的文件夹，这个目录就是你的**家目录**。`-G wheel`：`-G`代表把用户加入一个组。
> 只要家目录不变，你重装系统后只需要重新安装一下软件包（它们一般不存放在家目录），然后所有的配置都会从家目录中读取，完全不用重新设置软件着。你可以在家目录不变的情况下更换你的发行版而不用重新配置你的环境。切换用户后所有的设置会从新的用户的家目录中读取，将不同用户的资料与软件设置等完全隔离。有些著名的配置文件比如vim的配置文件~/.vimrc，只要根据自己的使用习惯配置一次， 在另一个Linux系统下（例如你的服务器）把这个文件复制到家目录下，就可以完全恢复你的配置。引用自[https://www.viseator.com/2017/05/19/arch_setup/](https://www.viseator.com/2017/05/19/arch_setup/)

- sudo 安装`pacman -S sudo`，配置`visudo`，取消注释`%wheel ALL=(ALL)ALL`。 
- 配置普通用户的用户密码 `passwd ivo`，重启`reboot`以后连同sudo都生效，下次进入就不用root了，用ivo。

### 图形化
- 先装显卡驱动，我用的intel的集成显卡,所以安装`sudo pacman -S xf86-video-intel`就好了.独立显卡现在再没装,不玩steam用处不大,虽然我有gtx950m.这里有详细的解释我就不多说了可以选择中文,在左侧.[https://wiki.archlinux.org/index.php/Xorg#Driver_installation](https://wiki.archlinux.org/index.php/Xorg#Driver_installation)
- xrog基础的图形服务.安装`pacman -S xorg-server xorg-xinit`即可. 
- 安装xfce4 桌面 `pacman -S xfce4 xfce4-goodies networkmanager network-manager-applet` networkmanager、network-manager-applet 是网络管理器后者是桌面的一个显示,开机自启 networkmanager `systemctl enable NetworkManager`
- 安装桌面管理器 sddm `sudo pacman -S sddm`.它用来后面可以选择桌面环境用比如现在装了xfce4,想体验kde,启动的时候就要用它来选择桌面环境. 设置sddm开机启动`sudo systemctl enable sddm`
> sudo systemctl start   服务名 （启动一项服务）
sudo systemctl stop    服务名 （停止一项服务）
sudo systemctl enable  服务名 （开机启动一项服务）
sudo systemctl disable 服务名 （取消开机启动一项服务）

- 这里还要禁用ntctl `sudo systemctl disable netctl` 它和NetworkManager有冲突.
- reboot 就可以使用图形界面了.下面是解决问题的时间
###　问题解决
- 打开终端以后大写字母是重叠的．这个是因为字体的原因造成的，需要安装对应的字体然后再终端里面重新选择．`sudo pacman -S adobe-source-han-sans-cn-fonts ttf-dejavu`之后再终端设置dejavu的mono字体即可.还有一点可以设置的是application-setting-settings manager-Appearance-fonts-default font 选择成source han sans cn
- ssh-keygen 无法使用,因为没有安装openssh `pacman -S openssh`
- github上面自己的项目 `git cone` 下来init输入了用户名密码后面push时候还要密码,这个git clone 的方式不对 不能用https开头的.用git@github.com开头的这个,然后用ssh-keygen生成的公钥,添加到github自己的账户里面.
- 触摸板点击没有效果,只能实体按键.设置 setting-settings manager-mouse and touchpad-touchpad-general-勾选tap touchpad to click.这里如果不好使,装去触摸板的程序`pacman -S xf86-input-synaptics`
- 没有locate,装`sudo pacman -S mlocate` `sudo updatedb`
- yaourt.安装这个后面有着非常大的便利  `cd tmp` `git clone https://aur.archlinux.org/package-query.git`,这是依赖,进入目录`cd package-query`,`makepkg -si`,一路回车,速度较慢各种编译.然后`git clone https://aur.archlinux.org/yaourt.git`,`cd yaourt`,'makepkg -si'即可安装完成.

```
git clone https://aur.archlinux.org/package-query.git
cd package-query
makepkg -si
cd ..
git clone https://aur.archlinux.org/yaourt.git
cd yaourt
makepkg -si
cd ..
```
```
Yaourt是archlinux方便使用的关键部件之一，但没有被整合到系统安装中的工具。建议在装完系统重启之后，更新完pacman和基本系统之后，就安装这个工具。
最简单安装Yaourt的方式是添加Yaourt源至您的 /etc/pacman.conf，在文件最后加入:

[archlinuxcn]
#The Chinese Arch Linux communities packages.
SigLevel = Optional TrustAll
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
然后

pacman -Syu yaourt

```


- 安装`chrome` 官方源没有 要用 `yaourt`来安装 `yaourt -S google-chrome`这时必须不能用root账户来操作.
- 输入法 `pacman -S fcitx-im fcitx-configtool`.在/~下创建一个.xprofile的文件,内容如下,cat是查看的命令只保留 export部分即可

```
 cat .xprofile 
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

```
- 自动挂载移动硬盘和手机，读写 NTFS 盘`pacman -S gvfs gvfs-mtp ntfs-3g`

