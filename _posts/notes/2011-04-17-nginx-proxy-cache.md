---
layout: post
title: 利用 Proxy Cache 使 Nginx 对静态资源进行缓存
tags: [notes]
---

###前言

[Nginx]是高性能的HTTP服务器，通过[Proxy Cache]可以使其对静态资源进行缓存。其原理就是把静态资源按照一定的规则存在本地硬盘，并且会在内存中缓存常用的资源，从而加快静态资源的响应。

###配置Proxy Cache

以下为nginx配置片段：

	proxy_temp_path   /usr/local/nginx/proxy_temp_dir 1 2;

	#keys_zone=cache1:100m 表示这个zone名称为cache1，分配的内存大小为100MB
	#/usr/local/nginx/proxy_cache_dir/cache1 表示cache1这个zone的文件要存放的目录
	#levels=1:2 表示缓存目录的第一级目录是1个字符，第二级目录是2个字符，即/usr/local/nginx/proxy_cache_dir/cache1/a/1b这种形式
	#inactive=1d 表示这个zone中的缓存文件如果在1天内都没有被访问，那么文件会被cache manager进程删除掉
	#max_size=10g 表示这个zone的硬盘容量为10GB

	proxy_cache_path  /usr/local/nginx/proxy_cache_dir/cache1  levels=1:2 keys_zone=cache1:100m inactive=1d max_size=10g;

	server {
	    listen 80;
	    server_name *.example.com;
		
	    #在日志格式中加入$upstream_cache_status
	    log_format format1 '$remote_addr - $remote_user [$time_local]  '
			'"$request" $status $body_bytes_sent '
			'"$http_referer" "$http_user_agent" $upstream_cache_status';
			
	    access_log log/access.log fomat1;

	    #$upstream_cache_status表示资源缓存的状态，有HIT MISS EXPIRED三种状态
	    add_header X-Cache $upstream_cache_status;
	    location ~ .(jpg|png|gif|css|js)$ {
	        proxy_pass http://127.0.0.1:81;
	        
			#设置资源缓存的zone
	        proxy_cache cache1;

	        #设置缓存的key
	        proxy_cache_key $host$uri$is_args$args;

	        #设置状态码为200和304的响应可以进行缓存，并且缓存时间为10分钟
	        proxy_cache_valid 200 304 10m;

	        expires 30d;
	    }
	}

###安装Purge模块

Purge模块被用来清除缓存

	$ wget http://labs.frickle.com/files/ngx_cache_purge-1.2.tar.gz
	$ tar -zxvf ngx_cache_purge-1.2.tar.gz
 
查看编译参数

	$ /usr/local/nginx/sbin/nginx -V 

在原有的编译参数后面加上`--add-module=/usr/local/ngx_cache_purge-1.2`

	$ ./configure --user=www --group=www --prefix=/usr/local/nginx \
	--with-http_stub_status_module --with-http_ssl_module \
	--with-http_realip_module --add-module=/usr/local/ngx_cache_purge-1.2
	$ make && make install

退出nginx，并重新启动

	$ /usr/local/nginx/sbin/nginx -s quit
	$ /usr/local/nginx/sbin/nginx

###配置Purge

以下是nginx中的Purge配置片段

	location ~ /purge(/.*) {
	    #允许的IP
	    allow 127.0.0.1;
	    deny all;
	    proxy_cache_purge cache1 $host$1$is_args$args;
	}

###清除缓存

使用方式：
	
	$ wget http://example.com/purge/uri

其中`uri`为静态资源的URI，如果缓存的资源的URL为
`http://example.com/js/jquery.js`，那么访问
`http://example.com/purge/js/jquery.js`则会清除缓存。

###命中率

保存如下代码为hit_rate.sh：

	#!/bin/bash
	# author: Jeremy Wei <shuimuqingshu@gmail.com>
	# proxy_cache hit rate

	if [ $1x != x ] then
	    if [ -e $1 ] then
	        HIT=`cat $1 | grep HIT | wc -l`
	        ALL=`cat $1 | wc -l`
	        Hit_rate=`echo "scale=2;($HIT/$ALL)*100" | bc`
	        echo "Hit rate=$Hit_rate%"
	    else
	        echo "$1 not exsist!"
	    fi
	else
	    echo "usage: ./hit_rate.sh file_path"
	fi

使用方式

	$ ./hit_rate.sh /usr/local/nginx/log/access.log

###参考：

[http://wiki.nginx.org/HttpProxyModule](http://wiki.nginx.org/HttpProxyModule)

[Nginx]: http://nginx.org/ "Nginx"
[Proxy Cache]: http://wiki.nginx.org/HttpProxyModule#proxy_cache "proxy_cache"
