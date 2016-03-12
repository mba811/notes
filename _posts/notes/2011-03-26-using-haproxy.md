---
layout: post
title: HAProxy 配置
tags: [notes]
---

###前言

[Haproxy][1]是一个负载均衡服务器，能够提供4层，7层代理，并能支持上万级别的连接，你可以直接在WEB服务器前端加上它，而不影响应用的访问，完全透明。

###安装

	$ wget http://haproxy.1wt.eu/download/1.4/src/haproxy-1.4.8.tar.gz
	$ tar -zxvf haproxy-1.4.8.tar.gz
	$ cd haproxy-1.4.8
	$ ./configure --prefix=/path/to/haproxy
	$ make && make install

###配置

首先要添加haproxy:haproxy用户：

	$ groupadd haproxy
	$ useradd -g haproxy haproxy
	
查看uid和gid

	$ sudo cat /etc/passwd |grep haproxy 

编辑haproxy.cfg，添加如下内容：

	global
	    log 127.0.0.1   local3
	    maxconn 4096            #最大连接数
	    chroot /path/to/haproxy #安装目录
	    uid 535  #用户haproxy
	    gid 520  #组haproxy
	    daemon   #守护进程运行
	    nbproc 1 #进程数量
	    pidfile logs/haproxy.pid

	defaults

	   log     127.0.0.1       local3
	   mode    http       #layer 7代理
	   option  httplog
	   option  httpclose
	   option  dontlognull
	   option  forwardfor
	   retries 2
	   maxconn 2000
	   balance roundrobin
	   stats   uri     /haproxy-stats
	   contimeout      5000
	   clitimeout      50000
	   srvtimeout      50000

	frontend http-in
	
        bind *:80 #监听地址
        default_backend pool1

	backend pool1

        option  httpchk GET /test.php #用来做健康检查
        stats refresh 2
        server server1 192.168.1.1:82 weight 3 maxconn 32 check #check表示对这个server进行健康检查
        server server2 192.168.1.2:82 weight 3 maxconn 32 check
		
查看后端server状态： http://example.com/haproxy-stats

启动

	$ sudo ./sbin/haproxy -f haproxy.cfg

重启

	$ sudo ./sbin/haproxy -f haproxy.cfg -st `cat logs/haproxy.pid`


###日志问题

有童鞋说日志怎么也写不进去，我也遇到了这个问题，在这里分享下。 编辑`/etc/syslog.conf`文件，添加：

	local3.*	/var/log/haproxy.log


编辑`/etc/sysconfig/syslog`文件，把

	SYSLOGD_OPTIONS="-m 0"

改成

	SYSLOGD_OPTIONS="-r -m 0" #enables logging from remote machines

重启syslogd:

	/etc/init.d/syslog restart 

通过`tail`应该可以看到日志输出了：
	
	tail -f -n 30 /var/log/haproxy.log 


###参考：

[http://blog.chenlb.com/2009/06/install-haproxy-and-configure-load-balance.html](http://blog.chenlb.com/2009/06/install-haproxy-and-configure-load-balance.html)    
[http://haproxy.1wt.eu/download/1.4/doc/configuration.txt](http://haproxy.1wt.eu/download/1.4/doc/configuration.txt)   


[1]: http://haproxy.1wt.eu/