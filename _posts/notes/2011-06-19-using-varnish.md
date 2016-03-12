---
layout: post
title: Varnish 安装与配置
tags: [notes]
---

###介绍

[Varnish][varnish]是一款高性能的开源HTTP加速器，挪威最大的在线报纸[Verdens Gang][VerdensGang]使用3台Varnish代替了原来的12台Squid，性能居然比以前更好。Varnish 的作者Poul-Henning Kamp是FreeBSD的内核开发者之一，他认为现在的计算机比起1975年已经复杂许多。在1975年时，储存媒介只有两种：内存与硬盘。但现在计算机系统的内存除了主存外，还包括了cpu内的L1、L2，甚至有L3快取。硬盘上也有自己的快取装置，因此Squid cache自行处理物件替换的架构不可能得知这些情况而做到最佳化，但操作系统可以得知这些情况，所以这部份的工作应该交给操作系统处理，这就是Varnish cache设计架构。目前很多互联网公司在使用Varnish，其中包括[Facebook](http://www.facebook.com "非死不可")。

###特性

* [VCL][1](Varnish Configuration Language)：区别于其他系统，Varnish采用了自身的配置语言来配置，非常容易上手，这些配置会被编译成二进制机器码，明显加快了执行速度。
* [Health checks][2]：完善的健康检查机制。
* [ESI][3](Edge Side Includes)：在HTML中嵌入动态脚本文件。
* [Directors][4]：后端服务器的调度方式：random，round-robin，client，hash，DNS。
* [Purging and banning][5]：强大的缓存清除功能，可以以正则表达式的形式清除缓存。
* [Logging in Varnish][6]：Varnish的log不是记录在文件中的，而是记录在共享内存中。当日志大小达到分配的共享内存容量，覆盖掉旧的日志。以这种方式记录日志比文件的形式要快很多，并且不需要磁盘空间。
* 丰富的管理程序：varnishadm，varnishtop，varnishhist，varnishstat以及varnishlog等。

###环境
	
OS: CentOS 5.5   
varnish: 2.1.5    

###安装

首先安装ncurses-devel，否则`varnishstat`，`varnishtop`都无法编译完成

	$ yum install ncurses-devel

接下来安装varnish

	$ wget http://repo.varnish-cache.org/source/varnish-2.1.5.tar.gz
	$ tar -zxvf varnish-2.1.5.tar.gz
	$ cd varnish-2.1.5
	$ ./configure --prefix=/usr/local/varnish-2.1.5
	$ make && make install

启动

	$ /usr/local/varnish2.1.5/sbin/varnishd -f \
	/usr/local/varnish2.1.5/etc/varnish/default.vcl \
	-T 127.0.0.1:2000 -a 0.0.0.0:80 -s file,/tmp,200M

其中`-f`用来指定配置文件，`-T`指定管理台的访问地址，`-a`指定Varnish监听地址，`-s`指定Varnish以文件方式来缓存资源，地址为/tmp，大小200MB。


###配置

	#后端处理器b1
	backend b1{
	    .host = "192.168.2.110";
	    .port = "81";
	    .connect_timeout = 5s;
	    .first_byte_timeout= 5s;
	    .probe = {
	        #health check
	        .url = "/check.txt";
	        .interval = 5s;
	        .timeout = 5s;
	        .window = 5;
	        .threshold = 3;
	    }
	}

	#后端处理器b2
	backend b2{
	    .host = "192.168.2.109";
	    .port = "81";
	    .connect_timeout = 5s;
	    .first_byte_timeout = 5s;
	    .probe = {
	        #health check
	        .url = "/check.txt";
	        .interval = 5s;
	        .timeout = 5s;
	        .window = 5;
	        .threshold = 3;
	    }
	}

	#以轮询方式实现负载均衡
	director d1 round-robin {
	    {
	        .backend = b1;
	    }

	    {
	        .backend = b2;
	    }
	}

	#acl
	acl purge {
	    "localhost";
	    "192.168.0.64";
	}

	sub vcl_recv {
	     # 设置director
	     set req.backend = d1;
		 
	     # 如果从后端返回的资源中含有Set-Cookie头的话，那么varnish不会进行缓存；
		 # 如果客户端发送了Cookie头的话，那么varnish会bypass（绕开）缓存，
		 # 直接发送到后端，并不会进行缓存，所以需要如下处理：
	    if ( !( req.url ~ ^/admin/) ) {
	        unset req.http.Cookie;
	    }

	    if (req.http.Cookie == "") {
	        remove req.http.Cookie;
	    }

	    if (req.restarts == 0) {
	        if (req.http.x-forwarded-for) {
	            set req.http.X-Forwarded-For =
	                req.http.X-Forwarded-For ", " client.ip;
	        } else {
	            set req.http.X-Forwarded-For = client.ip;
	        }
	     }

	     if (req.request != "GET" &&
	       req.request != "HEAD" &&
	       req.request != "PUT" &&
	       req.request != "POST" &&
	       req.request != "TRACE" &&
	       req.request != "OPTIONS" &&
	       req.request != "DELETE" &&
	       req.request != "PURGE") {

	         /* Non-RFC2616 or CONNECT which is weird. */
	         return (pipe);
	     }

	     # allow PURGE from localhost and 192.168.0...
	     if (req.request == "PURGE") {
	         if (!client.ip ~ purge) {
	             error 405 "Not allowed.";
	         }
	         return (lookup);
	     }

	     if (req.request != "GET" && req.request != "HEAD" && req.request != "PURGE") {
	         /* We only deal with GET and HEAD by default */
	         return (pass);
	     }

	     if (req.http.Authorization || req.http.Cookie) {
	         /* Not cacheable by default */
	         return (pass);
	     }
	     return (lookup);
	 }

	sub vcl_hit {
	     if (req.request == "PURGE") {
	        # Note that setting ttl to 0 is magical.
	        # the object is zapped from cache.
	        set obj.ttl = 0s;
	        error 200 "Purged.";

	     } else {
	        return (deliver);
	     }
	}

	sub vcl_miss {
	    if (req.request == "PURGE") {
	        error 404 "Not in cache.";
	    } else {
	        return (fetch);
	    }
	}

	sub vcl_fetch {
	     #设置TTL为1个小时
	     set beresp.ttl = 1h;
	     if (!beresp.cacheable) {
	         return (pass);
	     }

	     if (beresp.http.Set-Cookie) {
	         return (pass);
	     }

	     return (deliver);
	 }

	sub vcl_deliver {
	     return (deliver);
	}

###启动脚本

	$ wget -O varnishd https://raw.github.com/gist/3671408/3a51578bbd60a4cf8317bdc9508527b81eb23da5/varnishd
	$ cp varnishd /etc/init.d/varnishd
	$ chmod +x /etc/init.d/varnishd
	$ /etc/init.d/varnishd start

###Subroutine列表

* **vcl_recv**
在请求开始时候被调用，在请求已经被接收到并且解析后调用。目的就是决定是否处理这个请求，怎么处理，使用哪个后端。vcl_recv以`return`结束，参数可以为如下关键字：    
	**error code** [reason]：返回错误码给客户端，丢弃请求。   
	**pass**：转换到pass模式。控制权最后会转移到`vcl_pass`。    
	**pipe**：转换到pipe模式。控制权最后会转移到`vcl_pipe`。    
	**lookup**：在缓存中寻找请求对象。控制权最后会转移到`vcl_hit`或者`vcl_miss`，决定于对象是否在缓存中。   

* **vcl_pipe**
当进入pipe模式的时候被调用。在这个模式中，请求会被转移到后端，后续的数据不管是从客户端还是后端来的都会以不变的方式传送，直到连接关闭为止。vcl_pipe以`return`结束，参数可以为如下关键字：     
**error code** [reason]：返回错误码给客户端，丢弃请求。   
**pipe**：以pipe模式执行。  

* **vcl_pass**
当进入pass模式的时候会被调用。在这个模式中，请求会被传送到后端，然后后端的响应会被传送回客户端，但是响应不会进入缓存中。接下来通过相同客户端连接发起的请求会以普通的方式来处理。vcl_pass以`return`结束，参数可以为如下关键字：    
**error code** [reason]：返回错误码给客户端，丢弃请求。    
**pass**：以pass模式执行。    
**restart**：重新启动这个事务。增加了重启计数。如果重启的次数高于`max_restarts`，varnish会引起一个错误。   

* **vcl_hash**
你如果把想把数据加入到hash中，那么调用hash_data()。vcl_hash以`return`结束，参数可以为如下关键字：     
**hash**：执行hash逻辑。

* **vcl_hit**
如果请求的对象在缓存中被找到了，那么在缓存查找结束后被调用。vcl_hit以`return`结束，参数可以为如下关键字：   
**deliver**：deliver缓存对象到客户端。控制权最后会转移到`vcl_deliver`。   
**error code** [reason]：返回错误码给客户端，丢弃请求。   
**pass**：切换到pass模式。控制权最后会转移到`vcl_pass`。   
**restart**：重新启动这个事务。增加了重启计数。如果重启的次数高于`max_restarts`，varnish会引起一个错误。

* **vcl_miss**
如果请求的对象在缓存中没有被找到，那么在缓存查找结束后被调用。目的是为了决定是否去后端获取这个请求对象，并且要选择哪个后端。vcl_miss以return结束，参数可以为如下关键字：      
**error code** [reason]：返回错误码给客户端，丢弃请求。     
**pass**：切换到pass模式。控制权最后会转移到`vcl_pass`。      
**fetch**：去后端获取请求对象。控制权最后会转移到`vcl_fetch`。     

* **vcl_fetch**
当一个对象被成功从后端获取的时候此方法会被调用。vcl_fetch以`return`结束，参数可以为如下关键字：     
**deliver**：可能把对象放入缓存中，然后再deliver到客户端。控制权最后会转移到`vcl_deliver`。     
**error code** [reason]：返回错误码给客户端，丢弃请求。      
**esi**：以ESI形式来处理刚刚被获取到的对象。     
**pass**：切换到pass模式。控制权最后会转移到`vcl_pass`。     
**restart**：重新启动这个事务。增加了重启计数。如果重启的次数高于`max_restarts`，varnish会引起一个错误。     

* **vcl_deliver**当一个缓存的对象被deliver到客户端的时候，此方法会被调用。vcl\_deliver以`return`结束，参数可以为如下关键字：       
**deliver**：发送对象到客户端。     
**error code** [reason]：返回错误码给客户端，丢弃请求。     
**restart**：重新启动这个事务，增加重启计数。如果重启的次数高于`max_restarts`，varnish会引起一个错误。

* **vcl_error**
当遇见一个错误的时候会被调用，错误可能是跟后端有关系或者内部错误。vcl_error以`return`结束，参数可以为如下关键字：      
**deliver**：发送对象到客户端。      
**restart**：重新启动这个事务，增加重启计数。如果重启的次数高于`max_restarts`，varnish会引起一个错误。      

###重要变量

subroutine不带参数，一般通过全局变量来实现信息的传递。

如下变量在**backend**中有效：

* .host：backend的主机名或者IP。
* .port：backend的端口。

如下变量在**处理一个请求**（例如`vcl_recv`）的时候可用：

* client.ip：客户端IP地址。
* server.hostname：服务器的主机名。
* server.identity：服务器标示，当启动varnish的时候用`-i`参数来指定。如果varnish启动时候没有指定`-i`参数，那么server.identity会被设置为用`-n`参数所指定的实例名称。
* server.ip：服务器IP地址。
* server.port：服务器端口。
* req.request：请求类型（例如`GET`，`HEAD`）。
* req.url：请求的URL。
* req.proto：HTTP协议版本。
* req.backend：处理请求的后端服务器。
* req.backend.healthy：后端是否健康。health check需要在`backend`的`probe`中进行设置。
* req.http.header：相关的HTTP头。
* req.hash_always_miss：强迫对于本次请求的缓存查找结果为miss。如果设置为`true`，那么varnish将会忽略任何存在的缓存对象，一直从后端重新获取资源。
* req.hash_ignore_busy：在缓存查找时候忽略任何忙的对象。如果有两个服务器，彼此互相查找缓存内容，那么可以使用这个变量来避免潜在的死锁。

如下变量在**准备一个后端请求**(比如在`cache miss`或者`pass`，`pipe`模式)的时候可用：

* bereq.request：请求的类型（比如`GET`，`HEAD`）。
* bereq.url：请求的URL。
* bereq.proto：与后端服务器交互的HTTP协议版本。
* bereq.http.header：相关的HTTP头。
* bereq.connect_timeout：与后端连接的超时时间。
* bereq.first_byte_timeout：从后端返回第一个字节所需等待的秒数，在`pipe`模式中不可用。
* bereq.between_bytes_timeout：从后端返回的每个字节之间的时间间隔，以秒计。在`pipe`模式中不可用。

如下的变量在**请求对象从后端返回之后，在其被放入缓存之前**可用。换句话说，也就是在`vcl_fetch`中可用。

* beresp.proto：HTTP协议版本。
* beresp.status：后端返回的HTTP状态码（例如200,302等）。
* beresp.response：后端返回的状态内容（例如`OK`，`Found`）。
* beresp.cacheable：如果请求的结果是可以被缓存的，那么此变量为`true`。如果HTTP状态码为200, 203, 300, 301, 302, 404，410之一并且`pass`没有在`vcl_recv`中被调用，那么这个结果就是可以被缓存的。如果response的`TTL`和`grace time`都为0，那么`beresp.cacheable`将会为0。`beresp.cacheable`是可写的。
* beresp.ttl：缓存对象的生存时间，以秒为单位，这个变量是可写的。

在对象**已经存在于缓存中并被查询到**的时候，一般在`vcl_hit`和`vcl_deliver`中，如下的变量（大部分是read-only）可用：

* obj.proto：与后端交互的HTTP版本协议。
* obj.status：后端返回的HTTP状态码。
* obj.response：后端返回的HTTP状态内容。
* obj.cacheable：如果对象的beresp.cacheable为`true`，那么此变量的值为`true`。除非你强制delivery，否则`obj.cacheable`一直为`true`。
* obj.ttl：缓存对象的生存时间，以秒为单位，这个变量是可写的。
* obj.lastuse：从现在到对象最近一次访问所间隔的时间，以秒为单位。
* obj.hits：对象被发送到客户端的次数，0表示缓存查询miss了。

如下变量在**决定对象hash key**的时候可用：

* req.hash：hash key被用来关联一个缓存中的对象。在读写缓存的时候都会被用到。

如下变量在**准备把一个响应发送给客户端**时候可用：

* resp.proto：响应使用的HTTP协议版本。
* resp.status：将要返回的HTTP状态码。
* resp.response：将要返回的HTTP状态内容。
* resp.http.header：相关的HTTP头。


[1]: http://www.varnish-cache.org/docs/2.1/reference/vcl.html "vcl"
[2]: http://www.varnish-cache.org/docs/2.1/tutorial/advanced_backend_servers.html#health-checks "health check"
[3]: http://www.varnish-cache.org/docs/2.1/tutorial/esi.html "ESI"
[4]: http://www.varnish-cache.org/docs/2.1/reference/vcl.html#directors "directors"
[5]: http://www.varnish-cache.org/docs/2.1/tutorial/purging.html "purging"
[6]: http://www.varnish-cache.org/docs/2.1/tutorial/logging.html "logging"
[varnish]: https://www.varnish-cache.org/ "Varnish"
[VerdensGang]: http://www.vg.no "Verdens Gang"