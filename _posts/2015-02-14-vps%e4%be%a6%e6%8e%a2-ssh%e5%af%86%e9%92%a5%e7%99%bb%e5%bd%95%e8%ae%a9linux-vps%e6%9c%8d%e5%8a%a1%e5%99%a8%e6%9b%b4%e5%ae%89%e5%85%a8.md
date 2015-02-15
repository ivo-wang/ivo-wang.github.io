---
title: VPS侦探 SSH密钥登录让Linux VPS/服务器更安全

layout: post

---
Linux SSH登录有两种：

1、使用密码验证登录

通常VPS或服务器开通后都是直接提供IP和root密码，使用这种方式就是通过密码方式登录。如果密码不够强壮，而且没有安装DenyHosts之类的防止SSH密码破解的软件，那么系统安全将存在很大的隐患。

2、使用密钥验证登录

基于密钥的安全验证必须为用户自己创建一对密钥，并把共有的密钥放在需要访问的服务器上。当需要连接到SSH服务器上时，客户端软件就会向服务器发出请求，请求使用客户端的密钥进行安全验证。服务器收到请求之后，先在该用户的根目录下寻找共有密钥，然后把它和发送过来的公有密钥进行比较。如果两个密钥一致，服务器就用公有的密钥加密“质询”，并把它发送给客户端软件（putty,xshell等）。客户端收到质询之后，就可以用本地的私人密钥解密再把它发送给服务器，这种方式是相当安全的。  
一、生成密钥

因为puttygen生成的密钥有问题可能会出现：“Server refused our key”，最好使用XShell生成密钥或者在远程Linux VPS/服务器生成密钥。  
1、在Linux远程服务器生成密钥：  
登录远程Linux VPS/服务器，执行：  
root@vpser:~# ssh-keygen -t rsa //先运行这个命令  
Generating public/private rsa key pair.  
Enter file in which to save the key (/root/.ssh/id_rsa): //直接回车  
Created directory &#8216;/root/.ssh&#8217;.  
Enter passphrase (empty for no passphrase): //输入密钥密码  
Enter same passphrase again: //重复密钥密码  
Your identification has been saved in /root/.ssh/id_rsa. //提示公钥和私钥已经存放在/root/.ssh/目录下  
Your public key has been saved in /root/.ssh/id_rsa.pub.  
The key fingerprint is:  
15:23:a1:41:90:10:05:29:4c:d6:c0:11:61:13:23:dd root@vpser.net  
The key&#8217;s randomart image is:  
+&#8211;[ RSA 2048]&#8212;-+  
|=&#038;@Bo+o o.o |  
|=o=.E o . o |  
| . . . |  
| . |  
| S |  
| |  
| |  
| |  
| |  
+&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;+  
root@vpser:~#  
将/root/.ssh/下面的id\_rsa和id\_rsd.pub妥善保存。  
2、使用XShell生成密钥

Xshell是一款Windows下面功能强大的SSH客户端，能够按分类保存N多会话、支持Tab、支持多密钥管理等等，管理比较多的VPS/服务器使用XShell算是比较方便的，推荐使用。

下载XShell，安装，运行XShell，点击菜单：Tool ->User Key Generation Wizard，出现如下提示：

点击Save as file将密钥保存为id_rsa.pub。  
二、将密钥添加到远程Linux服务器

1、用winscp，将id\_rsa.pub文件上传到/root/.ssh/下面（如果没有则创建此目录），并重命名为：authorized\_keys（如果是在Linux服务器上生成的密钥直接执行：mv /root/.ssh/id\_rsa.pub /root/.ssh/authorized\_keys），再执行：chmod 600 /root/.ssh/authorized_keys 修改权限。

2、修改/etc/ssh/sshd_config 文件，将RSAAuthentication 和 PubkeyAuthentication 后面的值都改成yes ，保存。

3、重启sshd服务，Debian/Ubuntu执行/etc/init.d/ssh restart ；CentOS执行：/etc/init.d/sshd restart。  
三、客户端测试使用密钥登录  
1、使用putty登录

putty使用的私钥文件和Linux服务器或XShell的私钥格式不同，如果使用putty的话，需要将Linux主机上生成的id\_rsa文件下载的本地。运行putty压缩包里面的puttygen.exe，选择Conversions->Import key选择私钥文件id\_rsa，输入密钥文件的密码，会出现如下界面：

点击“Save Private Key”，将私钥保存为id_rsa.ppk

运行putty，在Host Name填写：root@主机名或ip

如果设置了密钥密码，出现：Passphrase for key &#8220;imported-openssh-key&#8221;时输入密钥密码。

如果设置没问题就会登录成功，出现用户提示符。  
2、XShell登录

运行XShell，选择菜单File->New，按如下提示填写：

打开创建好的Session

如果设置没问题就会登录成功，出现用户提示符。  
3、Linux客户端登录测试

在Linux客户端执行：chmod 600 /root/id\_rsa 再执行：ssh root@www.vpser.net -i /root/id\_rsa /root/id_rsa为私钥文件，第一次链接可能会提示确认，输入yes即可，再按提示输入密钥密码，没有问题就会出现用户提示符。  
四、修改远程Linux服务器sshd服务配置  
1、修改/etc/ssh/sshd_config 文件

将PasswordAuthentication yes 修改成 PasswordAuthentication no  
2、重启sshd服务

Debian/Ubuntu执行/etc/init.d/ssh restart ；CentOS执行：/etc/init.d/sshd restart。

ok，设置完成。

再提醒一下一定要保存好Putty私钥文件id\_rsa.ppk或Linux服务器下载下来的id\_rsa私钥文件。