---
layout:  post
title:  thinkpad x230 ubuntu 18.04 lts fingerprint
subtitle: thinkpad x230 ubuntu 18.04 lts fingerprint 
date: 2018-10-02
author: ivo
catalog: true
header-img:
tags:
    - linux 
    - x230
---
Packages for Fingerprint GUI (http://www.ullrich-online.cc/fingerprint/) for Ubuntu 14.04, 16.04, 17.10, 18.04 and any distribution based thereupon

Supported readers (run command lsusb to find out the ID of your reader)
============
     045e:00bb    08ff:1683    08ff:2660    08ff:268f    147e:2020
     045e:00bc    08ff:1684    08ff:2680    08ff:2691    147e:3001
     045e:00bd    08ff:1685    08ff:2681    08ff:2810    1c7a:0603
     045e:00ca    08ff:1686    08ff:2682    08ff:5501
     0483:2015    08ff:1687    08ff:2683    08ff:5731
     0483:2016    08ff:1688    08ff:2684    138a:0001
     04f3:0907    08ff:1689    08ff:2685    138a:0005
     05ba:0007    08ff:168a    08ff:2686    138a:0008
     05ba:0008    08ff:168b    08ff:2687    138a:0010
     05ba:000a    08ff:168c    08ff:2688    138a:0011
     061a:0110    08ff:168d    08ff:2689    138a:0017
     08ff:1600    08ff:168e    08ff:268a    138a:0018
     08ff:1660    08ff:168f    08ff:268b    138a:0050
     08ff:1680    08ff:2500    08ff:268c    147e:1000
     08ff:1681    08ff:2550    08ff:268d    147e:1001
     08ff:1682    08ff:2580    08ff:268e    147e:2016
and
     0483:2015    147e:1003    147e:3000
     0483:2016    147e:2015    147e:3001
     147e:1000    147e:2016    147e:5002
     147e:1001    147e:2020    147e:5003
     147e:1002

Installation
=========
0. First of all, if you have installed Fingerprint GUI manually before, get rid of it completely. Remove all binaries, shared libraries, any other files and undo all the changes you have made to your system config files (especially to files under /etc/pam.d/).

1. Add this PPA to your sources:

    sudo add-apt-repository ppa:fingerprint/fingerprint-gui
    sudo apt-get update

2. Install the packages:

    sudo apt-get install libbsapi policykit-1-fingerprint-gui fingerprint-gui

3. Log out of your session and log back in (we need the new session defaults to be picked up).

Setup
=====
After installation launch “Fingerprint GUI” and enrol your fingerprints.
That should be all you need to do!
Try locking your screen, logging out and in, sudo in terminal and running graphical apps requiring root privileges.

Uninstallation
===========
GNOME users: The package policykit-1-fingerprint-gui replaces GNOME's standard PolicyKit daemon (contained in package policykit-1-gnome). However, when you decide to uninstall Fingerprint GUI (and hence remove policykit-1-fingerprint-gui), policykit-1-gnome will not get automatically installed back, hence there won't be any PolicyKit daemon in the system, and APT will try to remove all the packages that depend on it. Therefore, if you want to remove Fingerprint GUI, make sure you install policykit-1-gnome back first.
   sudo apt-get install policykit-1-gnome
   sudo apt-get remove fingerprint-gui

KDE SC users: The above instructions for GNOME users apply to you as well, just replace “policykit-1-gnome” by “polkit-kde-1” throughout the text.

Note for KDE SC users
==================
Please note that Fingerprint GUI doesn't work with kdm and kscreensaver because of a bug in these applications (see https://bugs.kde.org/show_bug.cgi?id=105631).

Note for Lubuntu users
==================
lxdm does not properly support alternative authentication methods in the PAM stack. If you want to use fingerprint-gui with Lubuntu, you should install lightdm and lightdm-gtk-greeter.

Troubleshooting
=============
If something doesn't work as you expect, you can try one of the following steps:
 • In terminal run "sudo pam-auth-update". Make sure that the PAM profile called “Fingerprint authentication by Fingerprint GUI” is on top of the list and active. Otherwise, you may try resetting your PAM to system defaults with "sudo pam-auth-update --force".
 • Check /var/log/auth.log for any clues. Fingerprint GUI is set up to run in debugging mode by default.

Bugs & Contact
=============
If you think you have found a bug in the programme itself, the best way to report it is through the upstream forums at http://home.ullrich-online.cc/fingerprint/Forum/ .
For issues with the packaging contact the package maintainer: https://launchpad.net/~jurenka
