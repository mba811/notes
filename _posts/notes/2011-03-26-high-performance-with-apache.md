---
layout: post
title: Apache 性能优化
tags: [notes]
---

###前言

[Apache]是世界上使用最广泛的WEB服务器，根据[PageSpeed]的规则，我们可以从[KeepAlive]，[浏览器缓存][http-cache]，[Gzip]等方面对其进行些调整，从而提高网站性能。

###开启Keep-Alive

开启Keep-Alive后，可以保证浏览器和服务器之间的连接持久存在，这样如果同一个域名请求很多资源的情况下可以省去建立连接的时间和资源消耗。所以对于静态服务器来说，由于一个域名会请求N多资源，比较宜开启Keep-Alive，但是对于动态服务器，不宜开启Keep-Alive，因为这样会造成很多的空闲进程，浪费内存空间。 配置：

	KeepAlive On #开启KeepAlive
	KeepAliveTimeout 5 #保持连接5秒


###HTTP缓存设置

当直接在浏览器中输入一个URL，或者点击一个链接的时候，那么浏览器缓存就会起作用，如果缓存没有过期，那么浏览器会从本地读取资源，不会发起HTTP请求，如果缓存过期，那么浏览器会发起新的浏览器请求。按`ctrl+F5`，浏览器会清空本地缓存，重新请求资源。      
`Expires`是HTTP/1.0的缓存头， `Cache-Control: max-age`是HTTP/1.1是用来进行HTTP缓存的头。     
Expires指定了资源过期的绝对时间，GMT格式，Cache-Control: max-age指定了资源过期的相对时间，单位是秒。      
在支持HTTP/1.1的浏览器上，如果发送两个头，那么Cache-Control: max-age会覆盖掉Expires；       
在支持HTTP/1.0的浏览器上，即使发送了两个头，但是只有Expires会起作用，所以为了兼容老的浏览器，还是要同时发送这两个头。       

设置HTTP缓存，需要安装expires_module，其会发送Expires和Cache-Control: max-age两个HTTP头。配置如下：

	<IfModule expires_module>
		ExpiresActive On
		ExpiresByType application/x-javascript  "access plus 30 days"
		ExpiresByType text/css  "access plus 30 days"
		ExpiresByType image/gif  "access plus 30 days"
		ExpiresByType image/jpeg  "access plus 30 days"
		ExpiresByType image/png  "access plus 30 days"
	</IfModule>


`ExpiresByType application/x-javascript "access plus 30 days"`表示对js资源设置`Expires`和`Cache-Control: max-age`头，其中`Expires`的值是以客户端访问资源的时间为基准的后30天，`Cache-Control: max-age`的值是3600x24x30秒。

`ExpiresByType application/x-javascript "modification plus 30 days"`和上面效果一样，只是`Expires`的时间是以资源最后修改的时间作为计算的基准。

###开启Gzip压缩，并设置vary头

Gzip会对文本资源进行压缩，一般能节省40%的大小，二进制内容不需要开启Gzip压缩，因为这些文件是已经压缩过的，如果再进行Gzip压缩反而会增加其大小。静态资源一般都会在代理服务器上进行缓存，而有的浏览器支持Gzip，但是也有不支持Gzip的老旧浏览器，所以需要设置`Vary: Accept-Encoding` 头，这个头告诉代理缓存服务器要对资源缓存两份，一份压缩过的，一份没有压缩过，然后根据浏览器发送的`Accept-Encoding`头来返回压缩或者不压缩的内容。设置Gzip压缩，需要安装`deflate_module`。 配置如下：

	<IfModule deflate_module>
	
		#对js,html,xml,css,普通文本开启Gzip压缩
		AddOutputFilterByType DEFLATE application/x-javascript text/html text/plain text/xml text/css
		
	</IfModule>

###关掉ETag

`Last-Modified`与`ETag`是同样的功能，都是用来标识一个资源是否更改过，Last-Modified的值是资源的时间戳，如果按`F5`或者刷新按钮则`If-Modified-Since`头会带着时间戳发送到服务器，如果服务器上资源的最后修改时间<=这个时间，那么返回`304 Not Modified`，否则返回`200 OK` 以及新的资源；ETag的值是通过资源的信息（一般为inode，大小，时间戳）而计算出来的一个字符串，如果按F5或者刷新按钮则`If-None-Match`头会带着这个值发送到服务器，服务器用这个值来和当前资源的值进行比对，如果相等，则返回`304 Not Modified`，否则返回`200 OK` 以及新的资源。默认情况下Apache对静态资源会发送Last-Modified和ETage，但是由于ETage的计算会耗费服务器的CPU资源，所以选择关掉，只开启Last-Modified。 配置：

	FileETag None
	Header unset ETag

###参考：

[http://lamp.linux.gov.cn/Apache/ApacheMenu/mod/mod_expires.html](http://lamp.linux.gov.cn/Apache/ApacheMenu/mod/mod_expires.html)

[PageSpeed]: https://developers.google.com/speed/docs/best-practices/rules_intro "PageSpeed"
[KeepAlive]: http://en.wikipedia.org/wiki/Keepalive "KeepAlive"
[Apache]: http://httpd.apache.org/ "Apache"
[http-cache]: http://en.wikipedia.org/wiki/HTTP_cache "HTTP cache"
[Gzip]: http://en.wikipedia.org/wiki/Gzip "Gzip"