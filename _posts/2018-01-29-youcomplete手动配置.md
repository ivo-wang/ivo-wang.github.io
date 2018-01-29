---
layout:  post
title:	vim 安装youcompleteme
date:	2018-01-29
author:	ivo
catalog: ture
tags:
    - vim
    - youcompleteme
---
### vim 安装youcompleteme
- 简介：全手动安装，
简单方式在ubuntu gnome上无效在deepin 15.04上可以。
确保你的Vim 7.4.1578以上 且支持 Python 2 or Python 3 .使用`vim --version`检查是否支持。
### 简单方式：
```
sudo apt-get install vim
sudo apt-get install vim-addon-manager 
vam install youcompleteme
sudo vam install youcompleteme
```

### 全手动方式
#### 0x01. 编译安装vim
- 安装编译vim用的基本软件
```
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev \
libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev \
python3-dev ruby-dev lua5.1 liblua5.1-dev libperl-dev git
```
- 卸载现有的vim
```
sudo apt-get remove vim vim-runtime gvim
sudo apt-get autoremove
sudo apt-get install -f
```
- 编译安装vim

>Note: If you are using Python, your config directory might have a machine-specific name (e.g. `config-3.5m-x86_64-linux-gnu`). Check in /usr/lib/python[2/3/3.5] to find yours, and change the `python-config-dir` and/or `python3-config-dir` arguments accordingly.
Note for Ubuntu 14.04 (Trusty) users: You can only use Python 2 or Python 3. If you try to compile vim with both `python-config-dir` and `python3-config-dir`, it will give you an error `YouCompleteMe unavailable: requires Vim compiled with Python (2.6+ or 3.3+) support.`
Add/remove the flags below to fit your setup. For example, you can leave out `enable-luainterp` if you don't plan on writing any Lua.
Also, if you're not using vim 8.0, make sure to set the VIMRUNTIMEDIR variable correctly below (for instance, with vim 8.0a, use /usr/share/vim/vim80a). Keep in mind that some vim installations are located directly inside /usr/share/vim; adjust to fit your system:
```
cd ~
git clone https://github.com/vim/vim.git
cd vim
./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-pythoninterp=yes \
            --with-python-config-dir=/usr/lib/python2.7/config \
            --enable-python3interp=yes \
            --with-python3-config-dir=/usr/lib/python3.5/config \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 \
            --enable-cscope \
            --prefix=/usr/local
make VIMRUNTIMEDIR=/usr/local/share/vim/vim80
```
使用 `checkinstall`可以简单的卸载vim.

```
    sudo apt-get install checkinstall
    cd ~/vim
    sudo checkinstall
```

 用makeinstall安装
```
    cd ~/vim
    sudo make install
```

用 `update-alternatives`设置vim为默认的编辑器.

```
    sudo update-alternatives --install /usr/bin/editor editor /usr/bin/vim 1
    sudo update-alternatives --set editor /usr/bin/vim
    sudo update-alternatives --install /usr/bin/vi vi /usr/bin/vim 1
    sudo update-alternatives --set vi /usr/bin/vim
```
进入vim以后 输入：version 查看是否有支持python2与python3

#### 0x02.安装Vundle（vim的插件管理器，YouCompleteMe 简称YCM是其中一个插件）
前提是必须保证之前的vim已经安装完成了
使用git clone来复刻过来到本地

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

拷贝下面的内容到～下的.vimrc 这个隐藏文件中

```
set nocompatible              " be iMproved, required
filetype off                  " required
set backspace=indent,eol,start


" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" install youcompleteme
Plugin 'Valloric/YouCompleteMe'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

造好这个配置文件以后就可以使用vundle来安装了
打开vim 然后输入`:PluginInstall` 或者也可以使用命令行来安装`vim +PluginInstall +qall`

- 安装基本的编译安装用的以来软件

```
sudo apt-get install build-essential cmake python-dev python3-dev 
```
- 编译一下让ycm支持对应的语言
支持c语言
```
cd ~/.vim/bundle/YouCompleteMe
./install.py --clang-completer
Compiling YCM without semantic support for C-family languages:
```
去除c的支持
```
cd ~/.vim/bundle/YouCompleteMe
./install.py
```
除了c语言外，还支持 c#、go、typescript、javascript、rust等 最间单的方式支持所有的语言使用 ./install.py --all 以上具体的单一遇见的支持可以参见一下引用的资料

>C# support: install Mono and add --omnisharp-completer when calling ./install.py.
>Go support: install Go and add --gocode-completer when calling ./install.py.
>TypeScript support: install Node.js and npm then install the TypeScript SDK with npm install -g typescript.
>JavaScript support: install Node.js and npm and add --tern-completer when calling ./install.py.
>Rust support: install Rust and add --racer-completer when calling ./install.py.


####以上操作以后即可使用ycm了
