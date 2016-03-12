---
layout: post
title: 使用 Awstats 分析 Nginx 日志
tags: [notes]
---

###前言
[Awstats][1]是用Perl开发的，功能强大的服务器日志分析工具，但是默认只支持Apache和IIS，本文介绍下如何用其来分析Nginx日志，并且在Nginx下运行awstats。由于Nginx没有内置的Perl执行能力，我们采用Fastcgi来执行Perl。

###创建Fastcgi-fpm

确保机器上已经安装了[Perl][2]和cpan，输入`cpan`，然后安装`PerlFCGI`      
和`FCGI::ProcManager`两个包：

	$ cpan>install FCGI
	$ cpan>install FCGI::ProcManager

创建文件/usr/local/bin/cgiwrap-fcgi.pl，作为Fastcgi-fpm：

	$ wget -O /usr/local/bin/cgiwrap-fcgi.pl https://raw.github.com/gist/3675629/60391f70fe69b53e16831fe44d2bbed4a2026699/cgiwrap-fcgi.pl

赋予cgiwrap-fcgi.pl执行权限：

	$ chmod 755 /usr/local/bin/cgiwrap-fcgi.pl
	$ mkdir -p /var/run/nginx/


启动FPM，Nginx需要对socket有读写权限，否则会报502错误

	$ /usr/local/bin/cgiwrap-fcgi.pl > /dev/null 2>&1 &;
	$ chown -R www:www /var/run/nginx/cgiwrap-dispatch.sock

###Awstats安装配置

	$ cd /usr/local
	$ wget http://prdownloads.sourceforge.net/awstats/awstats-7.0.tar.gz
	$ tar -zxvf awstats-7.0.tar.gz
	$ mv awstats-7.0 awstats

执行`/usr/local/awstats/tools/awstats_configure.pl`，进行awstats配置。系统会提示选择日志格式种类，目前awstats默认只支持`Apache`和`IIS`，由于是Nginx，所以选择`none`；按要求输入自己的域名，假设为example.com，那么会在`/etc/awstats/`下面生成`/etc/awstats/awstats.example.com.conf`的配置文件。 

修改配置文件`/etc/awstats/awstats.example.com.conf`，添加如下内容：

	#指定日志文件位置，因为我们要在凌晨1点分析日志，所以要指定的是前一天的日志文件
	LogFile="/usr/local/nginx/logs/access_%YYYY-0%MM-0%DD-24.log"

	#日志格式，需要与Nginx中的日志格式一致
	LogFormat="%host - %time1 %methodurl %code %bytesd %refererquot %uaquot"

创建切割日志脚本/usr/local/nginx/sbin/logcron.sh：

	#!/bin/bash
	# 日志切割脚本
	mv /usr/local/nginx/logs/access.log /usr/local/nginx/logs/access_`date +%Y%m%d`.log
	kill -s SIGUSR1 `cat /usr/local/nginx/nginx.pid`

创建awstats日志更新脚本/usr/local/nginx/sbin/awstats_up.sh :

	#!/bin/bash
	# 更新awstats日志
	/usr/local/awstats/wwwroot/cgi-bin/awstats.pl -update -config=example.com

添加crontab任务，内容如下：

	#23:59切割nginx日志
	59 23 * * * /usr/local/nginx/sbin/logcron.sh
	
	# 1点开始生成awstats统计数据
	00 1 * * * /usr/local/nginx/sbin/awstats_up.sh

###配置Nginx

修改Nginx配置/usr/local/nginx/conf/nginx.conf，添加如下内容：

	#log格式，注意空格，这个格式必须与awstats中的LogFormat一样
	log_format  main     '$remote_addr - [$time_local] "$request" '
	'$status $body_bytes_sent "$http_referer" '
	'"$http_user_agent" $http_x_forwarded_for';
	
	access_log logs/access.log main;

	#awstats运行配置
	server {
	    listen 127.0.0.1:80;
	    server_name awstats.com;
	    access_log off;
	    error_log off;
		
	    root  /usr/local/awstats/wwwroot;
	    index index.html;

	    location ~ ^/cgi-bin/.*.cgi$ {
	        gzip off;
	        fastcgi_pass  unix:/var/run/nginx/cgiwrap-dispatch.sock;
	        fastcgi_index index.cgi;
	        include fastcgi_params;
	    }
	}

修改Fastcgi配置/usr/local/nginx/conf/fastcgi_params，添加如下内容：

	fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
	fastcgi_param  QUERY_STRING       $query_string;
	fastcgi_param  REQUEST_METHOD     $request_method;
	fastcgi_param  CONTENT_TYPE       $content_type;
	fastcgi_param  CONTENT_LENGTH     $content_length;

	fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
	fastcgi_param  REQUEST_URI        $request_uri;
	fastcgi_param  DOCUMENT_URI       $document_uri;
	fastcgi_param  DOCUMENT_ROOT      $document_root;
	fastcgi_param  SERVER_PROTOCOL    $server_protocol;

	fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
	fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;
	fastcgi_param  REMOTE_ADDR        $remote_addr;
	fastcgi_param  REMOTE_PORT        $remote_port;
	fastcgi_param  SERVER_ADDR        $server_addr;
	fastcgi_param  SERVER_PORT        $server_port;
	fastcgi_param  SERVER_NAME        $server_name;

	# PHP only, required if PHP was built with --enable-force-cgi-redirect
	fastcgi_param  REDIRECT_STATUS    200;

访问方式：http://example.com/cgi-bin/awstats.pl?config=example.com

###参考：

[http://wiki.nginx.org/NginxSimpleCGI](http://wiki.nginx.org/NginxSimpleCGI)     
[http://5ydycm.blog.51cto.com/115934/140029](http://5ydycm.blog.51cto.com/115934/140029)

[1]: http://www.awstats.org/ "awstats"
[2]: http://www.perl.com/ "Perl"