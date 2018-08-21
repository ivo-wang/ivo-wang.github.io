---
layout:  post
title:  zsh 不支*通配符解决方案,zsh:no matches found
subtitle: zsh 不支*通配符解决方案,zsh:no matches found 
date: 2018-08-21
author: ivo
catalog: true
header-img:
tags:
    - zsh 
    - 通配符
    - no mathches
---
在ubuntu下删除libreoffice的时候,使用了`apt-get purge libreoffice-*`,之后提示了` zsh: no matches found: libreoffice-*`

这个是由于zsh的配置文件造成的,解决的方法是在.zshrc中加入一行配置项
`setopt no_nomatch`

然后进行 `source .zshrc`
