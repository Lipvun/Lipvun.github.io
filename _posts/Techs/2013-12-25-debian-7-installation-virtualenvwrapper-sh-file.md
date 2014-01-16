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

安装python虚拟环境时,无法

    source /usr/local/bin/virtualenvwrapper.sh.

### 分析：

未用find命令，因文件太多

```
# which virtualenvwrapper.sh
virtualenvwrapper.sh not found
# which virtualenvwrapper
virtualenvwrapper not found
# dpkg-query -L virtualenvwrapper | grep 'virtualenvwrapper\.sh'
# dpkg-query -L virtualenvwrapper | grep 'virtualenvwrapper'
/etc/bash_completion.d/virtualenvwrapper
…
```

或在`http://packages.debian.org/zh-cn/wheezy/all/virtualenvwrapper/filelist`中找到`virtualenvwrapper`包中的文件无`virtualenvwrapper.sh`

由此可见Debian 7中virtualenvwrapper.sh可能已改名并放在其它路径。

### 证据：

查看文档

```
# cat /usr/share/doc/virtualenvwrapper/README.Debian
In contrast to the information in
/usr/share/doc/virtualenvwrapper/en/html/index.html this package installs
virtualenvwrapper.sh as /etc/bash_completion.d/virtualenvwrapper.

Virtualenvwrapper is enabled if you install the package bash-completion and
enable bash completion support in /etc/bash.bashrc or your ~/.bashrc.

If you only want to use virtualenvwrapper you may just add

    source /etc/bash_completion.d/virtualenvwrapper

to your ~/.bashrc.

```

### 结论：

Debian 7下如使用apt-get 安装virtualenvwrapper后的下一步由：

    source /usr/local/bin/virtualenvwrapper.sh.

改成：

    source /etc/bash_completion.d/virtualenvwrapper
