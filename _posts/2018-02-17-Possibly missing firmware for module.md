---
layout:  post
title:  Possibly missing firmware for module aic94xx wd719x
subtitle: archlinux 更新linux或mkinitcpio -p linux 时候报错
date: 2018-02-17
author: ivo
catalog: true
header-img:
tags:
    - archlinux
    - 
---
报错 Possibly missing firmware for module: aic94xx .Possibly missing firmware for module: wd719x

```
git clone https://aur.archlinux.org/aic94xx-firmware.git
cd aic94xx-firmware
makepkg -sri
```

```
git clone https://aur.archlinux.org/wd719x-firmware.git
cd wd719x-firmware
makepkg -sri
```

完事`mkinitcpio -p linux` 在做一遍
