---
layout: post
title: Varnish获取并显示真实IP地址
category: Techs
tags: [Varnish]
keywords: Varnish
description: Varnish获取并显示真实IP地址
---

本站使用了varnish缓存,nginx作为Http服务器。搭建好wordpress后，速度加快了很多。但查看评论是发现，所有评论的IP均为nginx服务器地址无法记录客户端的真实IP.
解决方法

### 第一步：修改varnish前端配置文件:/etc/varnish/default.vcl

在sub vcl_recv块下增加

remove req.http.X-real-ip;
set req.http.X-real-ip = client.ip;
set req.http.X-Forwarded-For = client.ip;

### 第二步：修改nginx后端配置文件，/etc/nginx/nginx.conf

在http块下增加:

```
set_real_ip_from nginx_proxy_ip/24; #nginx后端IP
set_real_ip_from nginx_proxy_ip;
real_ip_header X-Real-IP;
```

如：

```
set_real_ip_from 192.168.10.0/24;
set_real_ip_from 192.168.10.6;
real_ip_header X-Real-IP;
```

### 第三步：测试是否成功

用PHP脚本显示IP地址，如你在客户端用浏览器打开服务器上的些PHP显示出你客户端的IP表示设置成功，内容如下：

``` php
<?php
echo $_SERVER['REMOTE_ADDR'];
```

### 附：apache2 LogFormat配置

在/etc/apache2/apache2.conf中加入

    LogFormat “%{X-Forwarded-For}i %l %u %t \”%r\” %>s %b \”%{Referer}i\” \”%{User-Agent}i\”" varnishcombined

在VirtualHost文件中修改LogFormat

    CustomLog /var/www/apache2/access.log varnishcombined
