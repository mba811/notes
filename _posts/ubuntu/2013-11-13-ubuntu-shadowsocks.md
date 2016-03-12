---
layout: post
title: Ubuntu 下 Shadowsocks 客户端使用
tags: [ubuntu]
---

##一、安装shadowsocks客户端
 
```
sudo apt-get install python-pip  
pip install shadowsocks
```

##二、创建配置文件

在`/etc/`位置下创建配置文件

```
sudo vim /etc/shadowsocks.json
```

(个人感觉因为etc目录权限限制比较严所以需要sudo) 
  
然后运行以下命令  
  
```
sslocal -c /etc/shadowsocks.json
``` 
(读取文件不需要root权限故不使用sudo命令)  

出现如下提示既表示命令成功运行，可是开始畅游网络了。  

```
INFO: loading config from /etc/shadowsocks.json  
2015-02-17 00:00:22 INFO loading libcrypto from libcrypto.so.1.0.0  
2015-02-17 00:00:22 INFO starting local at 127.0.0.1:1081
```

###配置过程中出现的问题: 

因本地1080端口被占用，提示如下信息，更换配置文件中本地端口，同时在代理插件中更新即可解决。
  
```
xuanyuan@xuanyuan-laptop:~$ sudo sslocal -c /etc/shadowsocks.json   
INFO: loading config from /etc/shadowsocks.json  
2015-02-16 23:59:07 INFO loading libcrypto from libcrypto.so.1.0.0  
2015-02-16 23:59:07 INFO starting local at 127.0.0.1:1080  
2015-02-16 23:59:07 ERROR [Errno 98] Address already in use
```
  
###附录：

#####官方wiki：

[https://github.com/shadowsocks/shadowsocks/wiki](https://github.com/shadowsocks/shadowsocks/wiki)  

#####这里还有官方中文的wiki：

[https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E](https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)  

#####shadowsocks配置文件  

引用地址： [https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File](https://github.com/shadowsocks/shadowsocks/wiki/Configuration-via-Config-File)  

```
You can use a configuration file instead of command line arguments.  
Create a config file /etc/shadowsocks.json. Example:  
{  
"server":"my_server_ip",  
"server_port":8388,  
"local_address": "127.0.0.1",  
"local_port":1080,  
"password":"mypassword",  
"timeout":300,  
"method":"aes-256-cfb",  
"fast_open": false  
}  
Explanation of the fields:  
Name  
Explanation  
server  
the address your server listens  
server_port  
server port  
local_address  
the address your local listens  
local_port  
local port  
password  
password used for encryption  
timeout  
in seconds  
method  
default: "aes-256-cfb", see Encryption   
fast_open  
use TCP_FASTOPEN, true / false  
workers  
number of workers, available on Unix/Linux  
To run in the foreground:  
ssserver -c /etc/shadowsocks.json  
To run in the background:  
ssserver -c /etc/shadowsocks.json -d start  
ssserver -c /etc/shadowsocks.json -d stop 
``` 

####python-pip使用

引用地址：www.th7.cn/Program/Python/201408/256245.shtml  

#####一、pip定义  
pip是一个安装和管理 Python 包的工具 ,是easy_install的替代品。本文将详细说明安装pip的方法和使用pip的一些基本命令。  
  
#####二、安装pip  
本人使用的是Ubuntu 12.04，本系统缺省安装了Python2.7.3。  
首先通过下面的命令安装pip，pip是Python的一个安装和管理扩展库的工具。  
sudo apt-get install python-pip  
然后再安装Python开发环境，方便今后编译其他扩展库：  
sudo apt-get install python-dev  
  
#####三、pip安装、更新、卸载python包命令
 
*  安装python包，以django包为例  
python install django  
  
*  显示已安装包的详细信息  
python show django  
grace@grace-ThinkPad-Edge-E430c:~$ pip show django  
---  
Name: Django  
Version: 1.6.5  
Location: /usr/local/lib/python2.7/dist-packages  
Requires:   
  
*  显示哪些包已经过期了  
python list --outdate  
  
*  更新具体的python包  
python install --upgrade django  
  
*  卸载已经安装的python包  
python uninstall django  
  
其实pip还有很多其他的命令，可以参看官方地址： [https://pypi.python.org/pypi/pip](https://pypi.python.org/pypi/pip)