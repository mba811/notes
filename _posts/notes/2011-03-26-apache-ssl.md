---
layout: post
title: Apache SSL 配置
tags: [notes]
---

###概念

开始之前介绍几个SSL相关的概念：

* CA(Certification Authority，认证中心)
* SSL (Secure Sockets Layer,安全套接层协议)
* TLS(Transport Layer Security，传输层安全)
* CSR (Certificate Signing Request，证书签名请求)

###流程

需要启用SSL的服务器自己生成CSR，然后让权威的CA进行签名认证或者 自己创建一个CA证书，然后给自己的CSR签名，不过这样的证书不会被浏览器认可。
以下步骤确保已经安装了[openssl]，windows版本的[在这][1]。

创建一个自签名的CA证书：

	openssl req -x509 -newkey rsa:1024 -keyout ca.key -out ca.crt
	
生成服务器CSR：

	openssl req -newkey rsa:1024 -keyout server.key -out server.csr

如果不使需要CA证书签名的话，进行如下操作：

	openssl req -x509 -days 1024 -key server.key -in server.csr > server.crt

如果需要用CA证书签名：

	openssl x509 -req -in server.csr -out server.crt -CA ca.crt -CAkey ca.key -CAcreateserial

查看证书：

	openssl x509 -noout -text -in server.crt

验证证书：

	openssl verify -CAfile ca.crt server.crt

生成客户端CSR：

	openssl req -newkey rsa:1024 -keyout client.key -out client.csr

用CA证书签名：

	openssl x509 -req -in client.csr -out client.crt -CA caca.crt -CAkey caca.key -CAcreateserial

转换，使证书可以安装到浏览器：

	openssl pkcs12 -export -clcerts -in client.crt -inkey client.key -out client.pfx


修改配置文件`httpd-ssl.conf`：

	SSLCertificateFile conf/ssl.crt/server.crt #服务器证书
	SSLCertificateKeyFile conf/ssl.key/server.key #服务器私钥
	SSLCACertificateFile conf/ssl.crt/ca/ca.crt  #CA证书
	SSLVerifyClient require #强制浏览器必须安装了证书才能访问


###证书链介绍

1. 生成Root CA私钥与证书： 生成RootCA私钥 —> 使用私钥生成CSR —> 生成自签名根证书（用来给二级CA证书签名）。

2. 生成二级CA 私钥与证书：（假如有两个二级CA， 分别负责管理服务器端和客户端证书）     
2.1 生成ServerCA私钥 —> 使用私钥生成CSR —> 使用根证书签名生成二级证书（ServerCA证书用来给服务器证书签名）。      
2.2 先生成ClientCA私钥 —> 使用私钥生成 CSR—> 使用根证书签名生成二级证书（ClientCA证书用来给客户端证书签名）。       

3. 生成服务器端与客户端的私钥与证书：       
3.1 生成ServerA私钥 —> 使用私钥生成CSR —> 使用ServerCA证书签名生成三级证书（ServerA证书）。      
3.2 生成ClientA私钥 —> 使用私钥生成 CSR—> 使用ClientCA证书签名生成三级证书（ClientA证书）。       
3.3 先生成ClientB私钥 —> 使用私钥生成 CSR—> 使用ClientCA证书签名生成三级证书（ClientB证书）。       

...可以生成N个客户端证书

###证书结构：

	RootCA
	|
	|-------ServerCA
	|          |
	|          |--------ServerA
	|
	|-------ClientCA
	            |
	            |------- ClientA
	            |------- ClientB
	            |......


###参考：

[http://www.shenmiguo.com/archives/2009/284_linux-apache-ssl-guide.html](http://www.shenmiguo.com/archives/2009/284_linux-apache-ssl-guide.html)

[1]: http://www.slproweb.com/products/Win32OpenSSL.html
[openssl]: http://www.openssl.org/ "open ssl"
