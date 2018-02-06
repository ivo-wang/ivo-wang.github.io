---
layout:  post
title: centos6.x 升级python 2.66到2.7.5
subtitle: python update 以及yum修复
date: 2018-02-06
author: ivo
catalog: true
header-img:
tags:
    - python
    - centos
---
Centos6系统默认安装的Python版本是2.6，但是目前好多程序支持2.7及以上版本，故这里介绍如何从2.6升级到2.7，以及过程中遇到的问题
*原文出自[https://www.jianshu.com/p/b0c124cd8aa9](https://www.jianshu.com/p/b0c124cd8aa9)更改了其中一些错误*
---

### 环境

```
[root@i-cnh16bcn opt]# cat /etc/redhat-release
CentOS release 6.8 (Final)

[root@i-cnh16bcn opt]# python -V
Python 2.6.6
```

### 安装Python2.7

#### 下载源码包

```
wget http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tar.bz2
```

#### 解压并且安装

```
tar -zxvf Python-2.7.5.tar.gz
cd Python-2.7.5
./configure --prefix=/usr/local/python2.7.5 --enable-shared
make && make install
```

**注意这里的`--enable-shared` 强烈建议加上该参数，否则可能执行`import module`的时候报错不能正常导入**

#### 升级

```
mv /usr/bin/python /usr/bin/python2.66                  #千万不要删除后面要用到,先处理老的在处理新的
ln -s /usr/local/python2.7.5/bin/python /usr/bin/python
```

#### 验证

```
[root@i-cnh16bcn opt]# python -V
Python 2.7.5
```

### 遇到问题

---

**问题1**

```css
python: error while loading shared libraries: libpython2.7.so.1.0: cannot open shared object file: No such file or directory
```

**解决之道**

Python2.7 使用`--enable-shared` 之后在 /usr/local/python2.7.5/lib下面生成共享文件，需要把这个加入到相关的搜索配置中去

```bash
vim /etc/ld.so.conf
/usr/local/python2.7.5/lib

## 然后执行下面的命令使其生效
/sbin/ldconfig
/sbin/ldconfig -V
```

---

**问题2**

```python
[root@i-cnh16bcn opt]# pip -V
Traceback (most recent call last):
  File "/usr/bin/pip", line 7, in <module>
    from pip import main
ImportError: No module named pip
```

**解决之道**

`/usr/bin/pip`已经存在但是还是提示没有`pip`模块，所以怀疑还是升级版本导致，需要安装最新版

```python
cd /tmp
wget https://bootstrap.pypa.io/get-pip.py

## 用新版Python安装
/usr/local/python2.7.5/bin/python /tmp/get-pip.py

## 新安装的pip
[root@i-cnh16bcn opt]# ls -l /usr/local/python2.7.5/bin/pip*
-rwxr-xr-x 1 root root 205 Apr 29 22:10 /usr/local/python2.7.5/bin/pip
-rwxr-xr-x 1 root root 205 Apr 29 22:10 /usr/local/python2.7.5/bin/pip2
-rwxr-xr-x 1 root root 205 Apr 29 22:10 /usr/local/python2.7.5/bin/pip2.7

## 验证新安装的pip
[root@i-cnh16bcn opt]# /usr/local/python2.7.5/bin/pip -V
pip 9.0.1 from /usr/local/python2.7.5/lib/python2.7/site-packages (python 2.7)
```


---

**问题3**

```python
Traceback (most recent call last):
File "/usr/bin/pip", line 5, in <module>
from pkg_resources import load_entry_point
ImportError: No module named pkg_resources
```

**解决之道**

还是因为新版Python对应的`pip`问题，需要把系统默认的换成最新的

```python
mv /usr/bin/pip /usr/bin/pip.old
ln -s /usr/local/python2.7.5/bin/pip /usr/bin/pip
```

---

### 附加

因为YUM使用的是Python2.6，升级之后避免YUM不能使用应该做如下操作

```bash
vim /usr/bin/yum
#!/usr/bin/python
将上面的更改为下面的样子即可
#!/usr/bin/python2.66
```
