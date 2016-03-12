---
layout: post
title: Mac 下用 Nginx + Passenger 部署 Rails 的运行环境
tags: [osx]
---

### 系统需求

  * Mac OSX Lion

### 步骤0 安装环境依赖

安装Xcode 4.1，Xcode4.2以及更高的版本在 Lion 仍然存在一些兼容性问题，强烈建议使用XCode 4.1，下载地址：
    
    https://developer.apple.com/downloads/download.action?path=Developer_Tools/xcode_4.1_for_lion/xcode_4.1_for_lion.dmg
    

安装RVM
    
    $ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
    

配置RVM自动加载，将下面这一行代码添加到`~/.bash_profile`中，然后退出iTerm并重新启动
    
    [[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm
    

安装 ruby-1.9.2-p290
    
    $ rvm install 1.9.2
    

设置系统默认使用 ruby-1.9.2
    
    $ rvm use 1.9.2 --default
    

### 步骤1 安装 Rails

安装Rails
    
    $ gem install rails
    

Rails安装完成后，创建一个rails项目，假定你的项目叫做：awesome project
    
    $ rails new awesome_project
    

启动Rails，并访问 http://localhost:3000
    
    $ cd awesome_project
    $ rails server
    

### 步骤2 安装 Passenger 和 Nginx

首先通过gem安装passenger
    
    $ gem install passenger
    

因为Nginx不支持动态module载入，所以需要通过Passenger来自动下载，编译，安装由Passenger修改版的Nginx:

安装Passenger + Nginx
    
    $ passenger-install-nginx-module
    

  1. Yes: download, compile and install Nginx for me. (recommended)  
The easiest way to get started. A stock Nginx 1.0.10 with Passenger  
support, but with no other additional third party modules, will be  
installed for you to a directory of your choice.

  2. No: I want to customize my Nginx installation. (for advanced users)  
Choose this if you want to compile Nginx with more third party modules  
besides Passenger, or if you need to pass additional options to Nginx's  
'configure' script. This installer will 1) ask you for the location of  
the Nginx source code, 2) run the 'configure' script according to your  
instructions, and 3) run 'make install'.

Whichever you choose, if you already have an existing Nginx configuration file,  
then it will be preserved.

Enter your choice (1 or 2) or press Ctrl-C to abort: 这里建议选择1

Please specify a prefix directory [/opt/nginx]: /usr/local/nginx  
  
当询问nginx的安装路径的时候，个人建议安装到`/usr/local/nginx`下

当安装完成后，会在console中提示如何配置Nginx  
  
Passenger会自动帮你将下面两行添加到Nginx的配置文件中`/usr/local/nginx/conf/nginx.conf`（很人性化）
    
    http {
      ...
      passenger_root /Users/Daniel/.rvm/gems/ruby-1.9.2-p290/gems/passenger-3.0.10;
      passenger_ruby /Users/Daniel/.rvm/wrappers/ruby-1.9.2-p290/ruby;
      ...
    }
    

* * *
    
    server {
      listen 80;
      server_name www.yourhost.com;
      root /somewhere/public;   # <--- be sure to point to 'public'!
      passenger_enabled on;
    }
    

请不要忘记将`nginx`命令行程序连接到`/usr/local/sbin`
    
    $ sudo ln -s /usr/local/nginx/sbin/nginx /usr/sbin/
    

### 步骤3 配置Nginx + Passenger + Rails

关于Nginx的配置，请参考Nginx的官方网站以及Passenger的官方网站

  * [http://wiki.nginx.org/Configuration](http://wiki.nginx.org/Configuration)
  * [http://www.modrails.com/documentation/Users%20guide%20Nginx.html](http://www.modrails.com/documentation/Users%20guide%20Nginx.html)

修改`hosts`文件，给你的项目一个本地域名, 比如`awesome_project.local`
    
    $ sudo vim /etc/hosts
    127.0.0.1 awesome_project.local
    

测试hosts
    
    $ ping awesome_project.local
    PING awesome_project.local (127.0.0.1): 56 data bytes
    64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.054 ms
    

继续配置Nginx, 这里我给出一个最小可运行的Nginx配置文件
    
    $ vim /usr/local/nginx/conf/nginx.conf
    

nginx.conf
    
    worker_processes  1;
    
    events {
      worker_connections  1024;
    }
    
    http {
      passenger_root /Users/Daniel/.rvm/gems/ruby-1.9.2-p290/gems/passenger-3.0.10;
      passenger_ruby /Users/Daniel/.rvm/wrappers/ruby-1.9.2-p290/ruby;
    
      include       mime.types;
      default_type  application/octet-stream;
      sendfile      on;
      keepalive_timeout  65;
    
      server {
        listen 80;
        server_name awesome_project.local;
        root /Users/Daniel/awesome_project/public;
        passenger_enabled on;
        rails_env development;
      }
    }
    

测试Nginx的配置文件语法是否正确
    
    $ sudo nginx -t
    nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
    nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
    

启动Nginx
    
    $ sudo nginx
    

如何在修改Nginx的配置文件后，让Nginx载入新配置
    
    $ sudo nginx -s reload
    

如何停止Nginx
    
    $ sudo nginx -s stop
    

如何在不停Nginx的情况下，重新启动Passenger
    
    $ cd path/to/your/awesome/project
    $ touch tmp/restart.txt
    

好了，这个时候你可以打开浏览器，访问你的`awesome_project`网站了
    
    http://awesome_project.local
    

* * *

#### 最后，希望你能够在Rails的开发中找到快乐！