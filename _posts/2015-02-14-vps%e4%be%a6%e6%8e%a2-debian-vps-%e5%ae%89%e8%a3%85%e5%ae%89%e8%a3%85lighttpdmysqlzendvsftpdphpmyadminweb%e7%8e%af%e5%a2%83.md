---
title: Debian VPS 安装安装(Lighttpd+MySQL+Zend+vsftpd+PHPMyAdmin)Web环境

layout: post

---
安装前更新下Debian服务器.

使用命令

apt-get update

输入命令

apt-get install mysql-server mysql-client

中途出现 “Do you want to continue [Y/n]?”

输入 y 回车继续安装

2.1.1 查看MYSQL是否工作.

输入命令

netstat -tap | grep mysql

2.1.2 设置MySql 管理员（root）密码

命令

mysqladmin -u root password 你的密码  
如果想把root 密码设置为 anqun 则这样书写  
mysqladmin -u root password 1234

2.1.3 使用ROOT帐户登陆MYSQL.

命令

mysql -u root –p

提示输入密码.回车即可成功登陆MYSQL.

MySQL安装成功后我们开始安装WWW服务器Lighttpd

2.2.0 安装Lighttpd

安装命令

apt-get install lighttpd

这时打开你的独立IP地址 就可以看见欢迎页面了.安装成功！

lighttpd默认网页文件夹

/var/www

lighttpd配置文件

/etc/lighttpd/lighttpd.conf

OK Lighttpd 既然安装成功后 我们就安装PHP让 Lighttpd支持PHP!

2.2.1 安装PHP.

命令

apt-get install php5-cgi

2.2.2 让Lighttpd支持PHP

我们要修改2个文件

/etc/php5/cgi/php.ini  
/etc/lighttpd/lighttpd.conf

修改方法通常使用VI指令  
先找到/etc/php5/cgi/php.ini

打开他转到edit编辑状态.

在最后一行插入

cgi.fix_pathinfo = 1

注:在此代码后，一定要再空出一行！

然后保存

接着用同样的方法编辑/etc/lighttpd/lighttpd.conf

[&#8230;]  
server.modules = (  
&#8220;mod_access&#8221;,  
&#8220;mod_alias&#8221;,  
&#8220;mod_accesslog&#8221;,  
&#8220;mod_fastcgi&#8221;,  
\# &#8220;mod_rewrite&#8221;,  
\# &#8220;mod_redirect&#8221;,  
\# &#8220;mod_status&#8221;,  
\# &#8220;mod_evhost&#8221;,  
\# &#8220;mod_compress&#8221;,  
\# &#8220;mod_usertrack&#8221;,  
\# &#8220;mod_rrdtool&#8221;,  
\# &#8220;mod_webdav&#8221;,  
\# &#8220;mod_expire&#8221;,  
\# &#8220;mod\_flv\_streaming&#8221;,  
\# &#8220;mod_evasive&#8221;  
)  
[&#8230;]

增加代码:

&#8220;mod_fastcgi&#8221;,

注意逗号.

接着在文件最后增加

fastcgi.server = ( &#8220;.php&#8221; => ((  
&#8220;bin-path&#8221; => &#8220;/usr/bin/php5-cgi&#8221;,  
&#8220;socket&#8221; => &#8220;/tmp/php.socket&#8221;  
)))

保存即可完成Lighttpd 与PHP的关联.

重新启动lighttpd

/etc/init.d/lighttpd restart

2.2.3 测试PHP.

我们使用iProber探针测试环境.

同样使用HyperVM控制面板&#8221;File Manager上传iProber.php到 /var/www

iProber探针下载 http://soft.vpser.net/prober/iProber.zip

然后我们打开 http://你的独立IP/iProber.php

看到页面代表PHP运行成功&#8230;&#8230;&#8230;.

2.3 让PHP支持MYSQL.

很激动吧！PHP装好了吧&#8230;.

不过突然发现探针上面

出现MYSQL不支持！！！！

啊！原来还有一步没有做呢！那好吧~让我们的php与mysql相处吧！

获得MySQL在PHP中的支持，我们可以安装php5 &#8211; MySQL的方案。这是一个很好的主意，需要安装一些其他php5模块，以及允许他们为您的应用程序。您可以搜索那些可用的php5模块使用这个命令：

apt-cache search php5

这时我们看到了php5-mysql这个模块，但是为了保证服务器对大部分程序的支援，我们还需要安装其他需要的模块，如php5-curl php5-gd php-pear等等。我们使用命令：

apt-get install php-pear php5-curl php5-dev php5-gd php5-idn php5-imagick php5-imap php5-mcrypt php5-memcache php5-mhash php5-ming php5-mysql php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-suhosin php5-tidy php5-xmlrpc php5-xsl

注：php5-suhosin模块就是前面提到的安全防护系统，它有两种安装方式，这里是用模块安装的方法相当于给PHP5进行打补丁。后面我会提到用第三方扩展的形式进行安装的。

重启我们的Lighttpd

/etc/init.d/lighttpd restart

现在再打开探针地址，是不是连接成功了？

3.0 安装Zend

安装Zend，我们需要登录到Zend的官方网站下载，下载需要用户名，我们可以注册一个，当然这是免费的，然后下载对应版本，然后上传到服务器Root用户目录/root

安装过程如下：

wget http://downloads.zend.com/optimizer/3.3.3/ZendOptimizer-3.3.3-linux-glibc23-i386.tar.gz  
tar -zxf ZendOptimizer-3.3.3-linux-glibc23-i386.tar.gz  
cd ZendOptimizer-3.3.3-linux-glibc23-i386  
sh install.sh

安装的时候会给你看相关说明，并且会让你同意其条款，这些我们不管，我们需要注意的是后面它会让你输入目前php.ini所在目录，这里我们输入/etc/php5/cgi，然后会问你是否使用Apache Web Server，因为我们选择了Lighttpd，所以这里我们选否，然后安装结束，系统会告诉你之前的php.ini已经备份为php.ini.bak，新的php.ini文件所在目录为/usr/local/Zend/etc/，以后如果要修改php的设置，就需要进入/usr/local/Zend/etc/这个目录修改php.ini。

然后输入：

cd ..  
rm -rf ZendOptimizer-3.3.3-linux-glibc23-i386.tar.gz ZendOptimizer-3.3.3-linux-glibc23-i386

删除之前的安装文件

4.0 安装suhosin

前面我们说过suhosin有两种方法，上面那个是以补丁包的方式进行安装，这里我们将它以第三方扩展的形式进行安装。安装过程如下：

wget http://download.suhosin.org/suhosin-0.9.27.tgz  
tar -zxf suhosin-0.9.27.tgz  
cd suhosin-0.9.27  
phpize  
./configure  
make&#038;&#038;make install  
cd ..  
rm -rf suhosin-0.9.27.tgz suhosin-0.9.27

然后我们再在/usr/local/Zend/etc/php.ini文件中添加

[Suhosin]  
extension=suhosin.so

suhosin的默认配置已经足够满足大部分人的需求了，如果需要增强设置，可以在php.ini文件中添加相应的值。详情请登录http://www.hardened-php.net/suhosin/#using_suhosin

5.0 安装Ftp服务端软件

作为服务器，必不可少需要安装FTP服务器端软件。Linux下有很多出色的Ftpd软件，我们这里选用vsftpd，短小精悍，足够满足我们的需求。使用命令：

apt-get install vsftpd

安装完成之后，默认的配置文件所在为/etc/vsftpd.conf

启动 vsftpd：

/etc/init.d/vsftpd start

以下命令建立FTP帐户及相应目录：

useradd FTP帐户 -d /home/你要的目录名 -s /bin/nologin

比如我要在 123目录建立一个 456用户，那命令应该是：

useradd 456 -d /home/123 -s /bin/nologin

-s /bin/nologin则表示禁止SSH登陆，去掉则可以使用SSH

使用“passwd 用户名”设定用户密码

passwd 用户名

会出现密码提示，输入密码即可

6.0 安装phpmyadmin

apt-get install phpmyadmin

安装好之后，如果出现在/usr/share/目录，就输入以下

mv /usr/share/phpmyadmin /var/www/phpmyadmin

移动个目录

安装成功后，可以直接登陆

http://你的独立IP/phpmyadmin