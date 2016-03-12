---
layout: post
title: Keepalived 配置与使用
tags: [notes]
---

###介绍

[Keepalived]是一个基于VRRP协议来实现的WEB服务高可用方案，可以利用其来避免单点故障。一个WEB服务至少会有2台服务器运行Keepalived，一台为主服务器（MASTER），一台为备份服务器（BACKUP），但是对外表现为一个虚拟IP，主服务器会发送特定的消息给备份服务器，当备份服务器收不到这个消息的时候，即主服务器宕机的时候，备份服务器就会接管虚拟IP，继续提供服务，从而保证了高可用性。

	 	+---------VIP(192.168.0.7)----------+
		|                                   |
	    |                                   |
	server(MASTER) <----keepalived----> server(BACKUP)
	(192.168.0.1)                       (192.168.0.2)


###VRRP

在[VRRP]协议中，有两组重要的概念：VRRP路由器和虚拟路由器，主控路由器和备份路由器。 VRRP路由器是指运行VRRP的路由器，是物理实体，虚拟路由器是指VRRP协议创建的，是逻辑概念。一组VRRP路由器协同工作，共同构成一台虚拟路由器。该虚拟路由器对外表现为一个具有唯一固定IP地址和MAC地址的逻辑路由器。处于同一个VRRP组中的路由器具有两种互斥的角色：主控路由器和备份路由器，一个VRRP组中有且只有一台处于主控角色的路由器，可以有一个或者多个处于备份角色的路由器。VRRP协议使用选择策略从路由器组中选出一台作为主控，负责ARP相应和转发IP数据包，组中的其它路由器作为备份的角色处于待命状态。当由于某种原因主控路由器发生故障时，备份路由器能在几秒钟的时延后升级为主路由器。由于此切换非常迅速而且不用改变IP地址和MAC地址，故对终端使用者系统是透明的。

###安装

编译安装：

	$ wget http://www.keepalived.org/software/keepalived-1.2.2.tar.gz</a>
	$ tar -zxvf keepalived-1.2.2.tar.gz
	$ cd keepalived-1.2.2
	$ ./configure --prefix=/usr/local/keepalived
	$ make && make install
	
拷贝需要的文件：

	$ cp /usr/local/keepalived/etc/rc.d/init.d/keepalived /etc/init.d/keepalived
	$ cp /usr/local/keepalived/sbin/keepalived /usr/sbin/
	$ cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
	$ mkdir -p /etc/keepalived/
	$ cp /usr/local/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf 

`/etc/keepalived/keepalived.conf`是默认配置文件

###配置

master:

	global_defs {
	   notification_email {
	 	  user@example.com
	   }
	   
	   notification_email_from mail@example.org
	   smtp_server 192.168.200.1
	   smtp_connect_timeout 30
	   router_id LVS_DEVEL
	}

	vrrp_instance VI_1 {
	    state MASTER #标示状态为MASTER
	    interface eth0
	    virtual_router_id 51
	    priority 101   #MASTER权重要高于BACKUP
	    advert_int 1
	    mcast_src_ip 192.168.2.115 #vrrp实体服务器的IP
		
	    authentication {
	        auth_type PASS #主从服务器验证方式
	        auth_pass 1111
	    }

	    #VIP
	    virtual_ipaddress {
	        192.168.2.233 #虚拟IP
	    }
	}

backup:

	global_defs {
	   notification_email {
		   user@example.com
	   }

	   notification_email_from mail@example.org
	   smtp_server 192.168.200.1
	   smtp_connect_timeout 30
	   router_id LVS_DEVEL
	}

	vrrp_instance VI_1 {

	    state BACKUP #状态为BACKUP
	    interface eth0
	    virtual_router_id 51
	    priority 100  #权重要低于MASTER
	    advert_int 1
	    mcast_src_ip 192.168.2.227 #vrrp实体服务器的IP

	    authentication {
	        auth_type PASS
	        auth_pass 1111
	    }

	    #VIP
	    virtual_ipaddress {
	        192.168.2.233 #虚拟IP
	    }
	}

###使用

	$ /etc/init.d/keepalived start | restart | stop

当启动了keepalived之后，通过`ifconfig`是看不到VIP的，但是通过`ip a`命令是可以看到的。 当MASTER宕机，BACKUP升级为MASTER，这些VRRP_Instance状态的切换都可以在`/var/log/message`中进行记录。

###参考：

[http://www.keepalived.org/](http://www.keepalived.org/)     
[http://www.chinaunix.net/jh/30/284898.html](http://www.chinaunix.net/jh/30/284898.html)     

[Keepalived]: http://www.keepalived.org/ "Keepalived"
[VRRP]: http://en.wikipedia.org/wiki/Virtual_Router_Redundancy_Protocol "Virtual Router Redundancy Protocol"