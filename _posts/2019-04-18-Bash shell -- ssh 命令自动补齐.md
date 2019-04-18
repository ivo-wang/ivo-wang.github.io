---
layout:  post
title:  Bash shell -- ssh 命令自动补齐
subtitle: Bash shell -- ssh 命令自动补齐 
date: 2019-04-18
author: ivo
catalog: true
header-img:
tags:
    - bash
    - 
---
設定步驟如下述:

將此行加入 .bashrc 最後一行即可.
complete -W "$(echo $(grep -a '^ssh ' .bash_history | sort -u | sed 's/^ssh //'))" ssh

註: 上述取自此篇: bash autocomplete for SSH
登出再次登入即可. (或者直接 source .bashrc 亦可).
註: 上述寫法同理, 可以考慮將 .ssh/config 也加入此設定.




本内容同步更新在我的个人博客 「北极星」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
