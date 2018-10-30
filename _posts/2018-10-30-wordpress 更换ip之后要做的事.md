---
layout:  post
title:  wordpress 更换ip之后要做的事
subtitle: wordpress 更换ip之后要做的事 
date: 2018-10-30
author: ivo
catalog: true
header-img:
tags:
    - linux
    - wordpress
---
 网站更换ip后,wordpress不能正常访问 
 查看:wp-config.php中mysql相关设置,DB_HOST一般为localhost

登陆mysql: mysql -u root -p
加载数据库: use wordpress
更新站点url和主页

```
查看原来的链接是什么
mysql> use wordpress;

mysql> select * from wp_options where option_id=1;

mysql> update wp_options set option_value="http://xxxx" where option_id=1;

exit

service mysql restart
```

然后在wp里面进行设置，settings-常规-WordPress地址（URL）和站点地址（URL） 修改ip


