---
layout:  post
title:  Deepin linux 操作系统提高 WiFi 速度
subtitle: Deepin linux 操作系统提高 WiFi 速度 
date: 2019-04-01
author: ivo
catalog: true
header-img:
tags:
    - linux
    - wifi
---

### 0x01 问题
之前一直没有注意过原来 deepin 的 WiFi 速率一直是 54 Mbs ，直到有一天一个同事说无线网卡的速率特别慢，我才注意到这个现象开始还以为是无线网卡的问题，没太在意直到后来这个速度已经严重的影响了我的正常使用。

### 0x02 解决
解决这个问题也很容易只需要更改一个配置文件即可

```
sudo vim /etc/modprobe.d/iwlwifi.conf 
更改 11n_disable=1 为 11n_disable=8

options iwlwifi 11n_disable=8 bt_coex_active=0 power_save=0 swcrypto=1
```

验证
```
iwconfig 
wwp0s20u10  no wireless extensions.

wlp4s0    IEEE 802.11  ESSID:"cashway_113_5g"  
          Mode:Managed  Frequency:5.745 GHz  Access Point: DC:FE:18:0A:F1:05   
          Bit Rate=866.7 Mb/s   Tx-Power=22 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          Link Quality=65/70  Signal level=-45 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:97   Missed beacon:0

lo        no wireless extensions.

enp0s25   no wireless extensions.
```


本内容同步更新在我的个人博客 「我们都是害虫」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
