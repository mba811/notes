---
layout: post
title: 域名详解
tags: [study]
---

###域名

[域名]就是用来唯一标示互联网上的服务器的，当我们访问一个网站时候，比如http://www.example.com/index.html，其中的`com`，`example.com`，`www.example.com`都是域名。域名不区分大小写，即`www.example.com`与`WWW.EXAMPLE.COM`一样。

###FQDN

[FQDN][1](Fully qualified domain name)即全域名，大多数指的是一个绝对域名，这个域名指明了其在DNS树级结构中的准确位置。它也指明了其中所有域的层 级，包括顶级域和根域。`www.example.com.`就是一个FQDN。 DNS resolver一般是按照FQDN的格式来解析一个域名的，如果被解析的域名 中不存在`.`，则DNS resolver会自动为其加上`.`。

###域名的等级

域名分为[顶级域名][2](Top Level Domain, TLD)，二级域名，三级域名等等。以`www.example.com`为例，其中`com`为顶级域名，`example`为`com`下的子域名，`www`是`example.com`下的子域名，这是一个树形的结构，最多可以有127个层 级。

	com
	  |--example
	      |-- www
	           .
	           .
	           .



`com`顶级域名，`example.com`为二级域名，`www.example.com`为三级域名，并且指明这个域名是一个WWW(World-Wide Web)服务器的主机名。

###hostname

hostname就是一个域名，但是这个域名要有IP指向。例如www.example.com和example.com都是hostname，但是com不是一个hostname。

###根域名

DNS系统是一个树状的层级结构，树根是[根域][3](root domian)。根域没有名字，在[DNS]系统中就用一个空字符串来表示。互联网上所有的FQDN都可以看成是以 根域来结尾，所以都是以分隔符`.`来结束的，例如`www.example.com.`。现在的DNS系统都不会要求域名以`.`来结束，即`www.example.com`就可以解析 了，但是现在很多DNS解析服务商还是会要求在填写DNS记录的时候以`.`来结尾域名。

全球共有13组根域名服务器，名字是`[a-z].root-servers.net`。10组在美国，其中的一些已经采用[Anycast]技术进行部署，剩下三组分为位于瑞典斯德哥尔摩(i)，荷兰 阿姆斯特丹(k)，日本东京(m)。

	root domain
	  |-- com
	       |-- example.com
	              |-- www.example.com

###顶级域名

当1980年DNS系统被创建的时候，域名空间被分成了两个部分，一部分是二个字母表示的国家域名和**gov**，**edu**，**com**，**mil**，**org**，**net**，**int**七个组织用域名。 更多的顶级域名见：[http://www.iana.org/domains/root/db/](http://www.iana.org/domains/root/db/)

###域名解析流程

以www.example.com为例，如果要解析这个域名，首先客户端要对根域名服务 器发起DNS查询请求，然后根域名服务器返回com顶级域名的IP地址，然后客户 端向com顶级域名服务器发起查询请求，com顶级域名服务器返回example.com 二级域名服务器的IP地址，客户端再向example.com域名服务器发起查询请求， example.com域名服务器返回www.example.com的IP地址，域名解析完毕。 为了提高解析速度，ISP会在自己的服务器上缓存DNS的解析结果，防止每次都 去递归查找，这个缓存的时间是由DNS记录的TTL来决定的。

![DNS](http://upload.wikimedia.org/wikipedia/commons/7/77/An_example_of_theoretical_DNS_recursion.svg "DNS")

###参考：

[http://en.wikipedia.org/wiki/Domain_Name_System](http://en.wikipedia.org/wiki/Domain_Name_System)    
[http://en.wikipedia.org/wiki/Domain_name](http://en.wikipedia.org/wiki/Domain_name)     


[域名]: http://en.wikipedia.org/wiki/Domain_name
[1]: http://en.wikipedia.org/wiki/FQDN "Fully qualified domain name"
[2]: http://en.wikipedia.org/wiki/TLD "Top Level Domain"
[3]: http://en.wikipedia.org/wiki/DNS_root_zone "root domain"
[DNS]: http://en.wikipedia.org/wiki/Domain_Name_System
[Anycast]: http://en.wikipedia.org/wiki/Anycast