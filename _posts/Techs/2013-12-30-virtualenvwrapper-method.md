---
layout: post
title: Virtualenvwrapper使用方法
category: Techs
tags: [Virtualenvwrapper,]
keywords: Virtualenvwrapper
description: Virtualenvwrapper使用方法
---


基本设置：

设置$WORKON_HOME，在~/.bashrc下加入：

    export WORKON_HOME=~/Envs
    source /etc/bash_completion.d/virtualenvwrapper

创建环境：

    $ mkvirtualenv [-i package] [-r requirements_file] [virtualenv options] ENVNAME

列出所有环境

    $ lsvirtualenv [-b] [-l] [-h]

删除环境

    $ rmvirtualenv ENVNAME

复制环境

    $ cpvirtualenv ENVNAME TARGETENVNAME

启动环境

    $ workon [environment_name]

离开环境

    $ deactivate
