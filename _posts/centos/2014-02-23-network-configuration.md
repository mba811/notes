---
layout: post
title: CentOS 基础网络配置路由和默认网关
tags: [centos]
---

**一、查看路由**

在命令行中输入

`netstat` `-r`

或者

`route`

命令即可查看系统中正在起作用的路由

**二、增加路由**

（1）在命令行中输入

`route add -net 192.168.0.0``/16` `gw 10.1.1.254`

这种方法系统重启后就失效了，在 rc.local 里面用添加此行； 

（2）在 /etc/sysconfig/network文件中设置系统默认网关

`GATEWAY=192.168.0.1`

（3）针对单个网卡 /etc/sysconfig/network-scripts/ifcfg-eth0设置各自不同的默认网关

`GATEWAY=192.168.0.1`

（4）针对单个网卡设置多个路由网络/etc/sysconfig/network-scripts/eth0.route

`ADDRESS0=192.168.0.0`

` ``NETMASK0=255.255.0.0`

` ``GATEWAY0=10.1.1.254`

` ``ADDRESS1=172.16.0.0`

` ``NETMASK1=255.240.0.0`

` ``GATEWAY1=10.1.1.254`

**针对文件的修改的生效，**

方法（2）需要重启系统网络服务

`service network restart`

方法（3）（4）重启单个网卡即可

`ifdown eth0 && ifup eth0`
