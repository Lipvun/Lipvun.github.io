---
layout: post
title: PPTP VPN连接时出现619错误
category: Techs
tags: [pptp, vpn]
keywords: pptp, vpn
description: PPTP VPN连接时出现619错误
---



不能建立VPN连接，出现619错误，是由于相关设备对GRE协议（通用路由封装协议）和TCP 1723端口支持的问题引起的。

1.请检查您自己机器及家庭网关上的防火墙设置是否支持GRE协议（通用路由封装协 议）及开放对外访问TCP 1723端口；或先将防火墙关闭进行测试，保证您的机器可以 使用GRE协议及TCP 1723端口对外进行访问；家庭网关是否支持GRE，应核实家庭网关 用户手册；重启家庭网关设备可以帮助解决家庭网关设备的临时性故障；

2.向您的ISP查询是否开放GRE协议及对外访问的TCP 1723端口；

服务器端日志:

```
 pptpd[3476]: GRE: read(fd=6,buffer=8058640,len=8196) from PTY failed: status = -1 error = Input/output error, usually caused by unexpected termination of pppd, check option syntax and pppd logs
 pptpd[3476]: CTRL: PTY read or GRE write failed (pty,gre)=(6,7)
 pptpd[3476]: CTRL: Reaping child PPP[3477]
 pptpd[3476]: CTRL: Client IPADDRESS control connection finished
 CRON[3479]: pam_unix(cron:session): session opened for user root by (uid=0)
 CRON[3479]: pam_unix(cron:session): session closed for user root
 pptpd[3502]: MGR: Maximum of 100 connections reduced to 91, not enough IP addresses given
 pptpd[3503]: MGR: Manager process started
 pptpd[3503]: MGR: Maximum of 91 connections available
 sshd[3413]: Received disconnect from IPADDRESS: 0: 
 sshd[3413]: pam_unix(sshd:session): session closed for user root
 pptpd[3504]: CTRL: Client IPADDRESS control connection started
 pptpd[3504]: CTRL: Starting call (launching pppd, opening GRE)
 pppd[3505]: Plugin /usr/lib/pptpd/pptpd-logwtmp.so loaded.
 pppd[3505]: pppd 2.4.5 started by root, uid 0
 pppd[3505]: Using interface ppp0
 pppd[3505]: Connect: ppp0 <--> /dev/pts/1
 pptpd[3504]: GRE: Bad checksum from pppd.
 pppd[3505]: LCP: timeout sending Config-Requests 
 pppd[3505]: Connection terminated.
 pppd[3505]: Modem hangup
 pppd[3505]: Exit.
 pptpd[3504]: GRE: read(fd=6,buffer=8058640,len=8196) from PTY failed: status = -1 error = Input/output error, usually caused by unexpected termination of pppd, check option syntax and pppd logs
 pptpd[3504]: CTRL: PTY read or GRE write failed (pty,gre)=(6,7)
 pptpd[3504]: CTRL: Reaping child PPP[3505]
 pptpd[3504]: CTRL: Client IPADDRESS control connection finished
```

1.Make sure you paste the most up-to-date password without mistake;

2.If you have firewall software installed, make sure TCP port 1723 is opened;

3.Check wireless router firewall settings, make sure PPTP VPN Pass Through is enabled;

4.Check your local ISP that if the GRE protocol on port 47 is opened;


