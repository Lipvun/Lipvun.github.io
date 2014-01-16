---
layout: post
title: 隐藏服务器程序版本号
category: Techs
tags: [nginx, varnish, php-fpm, apache2]
keywords: nginx, varnish, php-fpm, apache2
description: PPTP 隐藏服务器程序版本号nginx & varnish & php-fpm & apache2
---

闲来无事查了下本站HTTP头信息，发现nginx和php版本号都显示出来了。隐藏版本号的方法如下：

### 一、修改nginx配置文件/etc/nginx/nginx.conf

在http块中加入：

    server_tokens off;

### 二、修改/etc/php5/fpm/php.ini

    expose_php＝off

### 三、去掉部分varnish头信息

修改配置文件如：/etc/varnish/default.vcl

在sub vcl_deliver块下增加，以下未去除Age。

    remove resp.http.X-Varnish;
    remove resp.http.Via;

### 附：apache2去版本号

在apache2.conf中加入：

    ServerTokens Prod
    ServerSignature Off
