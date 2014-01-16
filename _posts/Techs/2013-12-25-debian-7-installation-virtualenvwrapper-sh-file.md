---
layout: post
title: Debian 7下安装无Virtualenvwrapper.sh文件
category: Techs
tags: [Virtualenvwrapper,]
keywords: Virtualenvwrapper
description: Debian 7下安装无Virtualenvwrapper.sh文件
---

Debian 7在使用

    apt-get install virtualenvwrapper

Debian 7下如使用apt-get 安装virtualenvwrapper后的下一步由：

    source /usr/local/bin/virtualenvwrapper.sh.

改成：

    source /etc/bash_completion.d/virtualenvwrapper
