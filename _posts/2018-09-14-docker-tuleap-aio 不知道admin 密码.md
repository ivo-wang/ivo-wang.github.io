---
layout:  post
title:  docker-tuleap-aio 不知道admin 密码
subtitle: docker-tuleap-aio 不知道admin 密码 
date: 2018-09-14
author: ivo
catalog: true
header-img:
tags:
    - docker  
    - docker-tuleap-aio
---
近期一直在折腾 docker使用各种各样的官方的docker images,然后就遇到了各种各样的奇葩情况,这个算一个.官方的东西有的会写明了admin的密码一般是admin,123,password之类的弱密码,还有一类就是需要了解这个docker项目再去搞密码,比如这个docker-tuleap-aio.

### 获取方法
`docker exec -ti [这里写容器的CONTAINER id] cat /data/root/.tuleap_passwd`

正式写的时候没有中括号,下面的是一个实例

```
 docker exec -ti aaced587a829 cat /data/root/.tuleap_passwd

```
