---
layout: post
title: Samba 配置与使用
tags: [notes]
---

###介绍

[Samba][1]可以让我们在windows中访问linux系统中的文件，如果用来调试linux虚拟机中的代码会非常的方便。

###安装

以下是CentOS中Samba的安装方式：

	yum install samba
	/etc/init.d/samba start


添加用户到Samba中,这个用户就可以用来访问Samba中的文件：

	groupadd sambagroup
	useradd -g sambagroup sambauser1
	passwd sambauser1 #shell 登录用
	smbpasswd -a sambauser1 #samba登录用
	smbpasswd sambauser1   #修改samba用户sambauser1的密码

###配置

Samba配置文件为`/etc/samba/smb.conf`用户验证访问模式：

	[global]
	workgroup = Ubuntu
	netbios name = UbuntuServer
	server string = Linux Samba Server TestServer
	security = user
	encrypt passwords = yes
	smb passwd file = /etc/samba/smbpasswd
	display charset = cp936
	unix charset = cp936
	dos  charset = cp936

	[noshare]
	path = /home/sambauser1
	writeable = yes   
	browseable = yes
	create mask =  0664
	directory mask = 0775
	valid users = sambauser1 #允许访问的用户，多个以','分割

匿名访问方式：

	[global]
	workgroup = Ubuntu
	netbios name = UbuntuServer
	server string = Linux Samba Server TestServer
	security = share
	display charset = cp936
	unix charset = cp936
	dos  charset = cp936

	[share]
	path = /share/smb
	writeable = yes
	browseable = yes
	create mask =  0664
	directory mask = 0775
	guest ok = yes #允许匿名访问
	public = yes

###问题

最近在配置samba，windows连接的时候出现如下问题：__samba不允许一个用户使用一个以上用户名与一个服务器或共享资源的多重连接__，google查询后，找到解决方法：

	net use * /del /y
 事实上这个不是samba的限制，是Windows的限制。始终要用public＝yes的话，上面的方法都不能有效解决，因为：在打开存在public＝yes的samba服务器时，如果首先点击了有public＝yes的共享资源的时候，widows会用默认的用户名去连接服务器，一般就是windows的登录名（可以在服务器端查看到的），这时候，再去点击没有public＝yes的共享资源，由于使用了user级别，服务器就会要求验证，这时，之前的默认登录已经存在，就出现了楼主的故障了。即使注销连接后如果没有采用正确的顺序访问共享资源，还是会陷入这个泥潭中。因此，最好办法就是不用public＝yes，给公共帐号建立一个共用的账户并公示出来。这样处理，其实权限更清晰一些。

###Selinux

在配置好samba后，有时候还会出现无法访问的情况，这种情况下需要关闭Selinux：

修改 `/etc/selinux/config` 修改 `SELINUX=enforcing` 为 `SELINUX=disabled`，然后重启服务器。


参考：

[http://www.linuxsir.org/main/?q=node/158](http://www.linuxsir.org/main/?q=node/158)     
[http://blogold.chinaunix.net/u/19637/showart_491257.html](http://blogold.chinaunix.net/u/19637/showart_491257.html)   


[1]: http://www.samba.org/ "samba"
