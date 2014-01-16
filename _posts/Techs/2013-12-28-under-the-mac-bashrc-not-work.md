---
layout: post
title: mac 下.bashrc不起作用
category: Techs
tags: [mac, bashrc]
keywords: mac, bashrc
description: mac 下.bashrc不起作用
---

在mac下`～/.bashrc`里面的一些设置，新开一个终端都要`source .bashrc`才起作用。

unix下当shell是login shell，.bash_profile才会加载，而bashrc正好相反。

真正的区别是在linux下，当用户登录到一个图形界面，然后打开一个终端terminal，那些shell是non-login shell。

然而，在OS X登录的时候，并没有运行着一个shell，所以，在运行Terminal.app的时候，其实那是一个login shell。
解决办法：

新建了 `.bash_profile`加载来`.bashrc`

```
if [ "${BASH-no}" != "no" ]; then
    [ -r ~/.bashrc ] && . ~/.bashrc
fi
```

加入mysql和mysqladmin的别名：

```
#mysql
alias mysql='/usr/local/mysql/bin/mysql'
alias mysqladmin='/usr/local/mysql/bin/mysqladmin'
```
