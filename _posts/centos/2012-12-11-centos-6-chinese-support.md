---
layout: post
title: 虚拟机安装 CentOS 6，设置中文
tags: [centos]
---

虚拟机安装centos6后，发现是英文的，连个输入法都没有。。。面板就不用说了，折腾了一晚，终于搞定  
使用命令行，要确定能上网  

*  1.切换到root  
`su root`  
输入root密码即可

*  2.用yum安装中文  
`yum groupinstall “chinese support”`

*  3.修改配置文件  
`vi /etc/sysconfig/i18n`  
打开后，把`en_US`改为`zh_CN`

*  4.重启系统  
`reboot`

  

