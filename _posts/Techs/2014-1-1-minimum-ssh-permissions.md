---
layout: post
title: 最小ssh权限
category: Techs
tags: [ssh,]
keywords: ssh
description: 最小ssh权限
---

创建只能作 TCP 转发的 SSH 账号

通过下面这个命令，就可以创建一个只允许 TCP 转发而不允许登陆（交互）的 SSH 账号。

    useradd -s /bin/false username

重置密码

    passwd username

创建允许更改密码以及 TCP 转发但不能登陆的 SSH 账号

更人性化一些，你可能需要允许你的朋友们自己去更改一个便于他们自己记忆的密码。这个时候我们可以让 /usr/bin/passwd 作为他们的 Shell ，这样他们就可以自己修改密码了。

1.编辑/etc/shells，并加入：

    /usr/bin/passwd

2.通过以下命令创建 SSH 账号：

    useradd -s /usr/bin/passwd username

