---
layout:  post
title:  How to enable finger printer with Thinkpad X230
subtitle: How to enable finger printer with Thinkpad X230 
date: 2018-10-02
author: ivo
catalog: true
header-img:
tags:
    - linux 
    - fingerprint
---
> I am a newbie with Ubuntu and Linux, and I just switched from Windows 8\. I have installed 12.04 on my new Lenovo Thinkpad x230\. It seems everything else works perfectly except the finger printer. I have been using Finger Printer for a long time, and I really rely it on for my Lastpass application. But I don't know how to enable or install the driver of the finger printer. Can anybody help me with this?

> Thanks in advance.
>
> --  此内容引用自https://askubuntu.com/questions/197336/how-to-enable-finger-printer-with-thinkpad-x230



For 13.04 and newer
For the X220/X230:
(ubuntu 18.04 Lts is work)
```
sudo apt-get update  
sudo apt-get install libpam-fprintd libfprint0 fprint-demo fprintd  
```
Then run this command to configure pam:
```
sudo pam-auth-update  
```
Finally enroll your finger with:

```
fprintd-enroll
```
