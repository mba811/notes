---
layout: post
title: Docker-GitLab 部署
tags: [docker]
---

docker用来隔离应用还是很方便的，一来本身的操作较为简单，二来资源占用也比虚拟机要小得多，三来也较为安全，因为像数据库这样的应用不会再全局暴露端口，同时应用间的通信通过加密和端口转发，更加安全。

Gitlab是目前比较流行的开源类Github代码管理平台。Gitlab使用Rails开发，使用PostgreSQL或MySQL数据库，Redis做缓存。一般自己搭建私有代码仓库，Gitlab通常是首选。这里简单介绍一下dockerized Gitlab。

Gitlab的docker镜像早已有人做好了，并且维护相当不错。大家可以前往其[GitHub仓库](https://github.com/sameersbn/docker-gitlab)了解该镜像的情况。官方repo的readme中已经有详细的安装配置方案，这里我简单的梳理一下部署流程。

## 安装Docker

这里以Ubuntu 14.04发行版为例，在bash中输入一下命令安装最新的docker：
    
    sudo apt-get purge docker.io
    curl -s https://get.docker.io/ubuntu/ | sudo sh
    sudo apt-get update
    sudo apt-get install lxc-docker
    

**注意:** 如果你使用了阿里云最新推出的[Docker镜像](http://market.aliyun.com/imageproduct/16-123824001-jxsc000057.html?spm=5176.7114037.1996646101.1.ScExvj&pos=1)，那么可以省略本节的步骤。因为阿里云的这个镜像已经自带了1.2版的docker，版本比较新，可以直接使用。

## 安装docker-gitlab

使用如下命令可以使Docker下载对应版本的Gitlab镜像:
    
    docker pull sameersbn/gitlab:7.5.3
    

上面的命令下载7.5.3版的Gitlab，如果想下载最新版本，可以输入以下命令:
    
    docker pull sameersbn/gitlab:latest
    

待下载完成后就算完成安装了。  
也可以Clone刚才的提到的[仓库](https://github.com/sameersbn/docker-gitlab)，然后在本机上build镜像：
    
    git clone https://github.com/sameersbn/docker-gitlab.git
    cd docker-gitlab
    docker build --tag="$USER/gitlab" .
    

注意上面最后一行命令结尾有一个"."符号，不要掉了。

## 安装PostgreSQL

Gitlab推荐使用PostgreSQL作为数据库。既然使用了docker，那么我们为何不考虑把所有的组件都用docker包装起来？我们一样可以下载PostgreSQL的镜像完成安装，这种安装更加便捷。

首先输入以下命令下载PostgreSQL镜像：
    
    docker pull sameersbn/postgresql:latest
    

然后我们要为数据库默认的表空间建立目录以存放数据：
    
    mkdir -p /opt/postgresql/data
    

这里`/opt/postgresql/data`部分可以替换成你自己希望建立的地址。  
如果是使用SELinux，那么还需要改变一下这个目录的安全设置：
    
    sudo chcon -Rt svirt_sandbox_file_t /opt/postgresql/data
    

如果没有使用SELinux，可以跳过上面一条命令。

最后使用以下命令行启动数据库：
    
    docker run --name=postgresql -d \
      -e 'DB_NAME=gitlabhq_production' -e 'DB_USER=gitlab' -e 'DB_PASS=password' \
      -v /opt/postgresql/data:/var/lib/postgresql \
      sameersbn/postgresql:latest
    

这里，"-e"选项后面的内容请不要随意变更，这里的配置都是Gitlab默认的数据库配置，如果没有在后面Gitlab镜像启动的设置里面做相应的修改的话，这里的修改会让程序无法正常运行。

## 安装Redis

同样，我们可以使用docker来安装Redis：
    
    docker pull sameersbn/redis:latest
    

然后启动它:
    
    docker run --name=redis -d sameersbn/redis:latest
    

## 启动gitlab

在最终启动Gitlab之前，我们还需要为Gitlab创建一个目录用来存放提交上来的代码，docker-gitlab内部使用`/home/git/data`这个目录存放代码，我们在容器外部创建一个目录然后在启动的时候挂载到这个路径即可：
    
    mkdir -p /opt/gitlab/data
    mkdir -p /opt/gitlab/backups
    

同样，如果使用SELinux，需要修改目录的安全配置:
    
    sudo chcon -Rt svirt_sandbox_file_t /opt/gitlab/data
    sudo chcon -Rt svirt_sandbox_file_t /opt/gitlab/backups
    

在完成上面所有的步骤以后，我们可以用以下命令启动Gitlab：
    
    docker run --name='gitlab' -d \
      -e 'GITLAB_PORT=10080' -e 'GITLAB_SSH_PORT=10022' \ 
      -e 'GITLAB_BACKUPS=monthly' \
      -p 10022:22 -p 10080:80 \
      -v /opt/gitlab/data:/home/gitl/data \
      -v /opt/gitlab/backups:/home/git/data/backups
      sameersbn/gitlab:7.5.3
    

上面的命令将使用10080作为Gitlab的Web访问端口，10022将作为ssh push和pull代码的端口。  
在本地可以使用浏览器打开`http://localhost:10080`来访问Gitlab，初始登录网站使用root账户，用户名为`root`，密码为：`5iveL!fe`，登录后需要立即修改密码。

**这里解释一下各参数：**

-d: 后台运行  
-e：配置Gitlab运行的环境变量，这个参数很重要，具体有哪些环境变量，后面列举  
-p: 端口转发规则  
-v: 共享目录挂载，即docker容器内外数据共享

Gitlab的环境变量配置比较多，这里列举一下比较重要的Gitlab的环境变量：

  * **GITLAB_HOST:** 这个是Gitlab服务器的hostname，你需要将此设定为网站的域名或者ip（不带端口号），默认值为`localhost`，这个值会被Gitlab用来生成repo的链接，所以必须要设置。否则，在创建的repo中，会发现所有的repo链接都是以`localhost`为hostname。
  * **GITLAB_PORT** Gitlab网站的访问端口，这里的设置要结合端口转发一起设置，否则会导致网站无法访问，默认值为`80`
  * **GITLAB_SSH_PORT** Gitlab的SSH代码提交方式使用的SSH端口，这里的设置要结合端口转发一起设置，否则会导致代码无法提交，默认值为`22`。如果是在VPS上部署，这个值请使用别的端口，比如上面提到的`10022`端口，否则会与VPS原本的SSH端口产生冲突，造成SSH无法登录VPS
  * **GITLAB_BACKUPS** Gitlab的自动备份配置，有`disable`, `daily`, `weekly`, `monthly`四个可选值，默认为`disable`。建议打开自动备份
  * **GITLAB_BACKUP_DIR** Gitlab自动备份目录，默认值为`/home/git/data/backups`

其他的参数请参考repo中的[README.md](https://github.com/sameersbn/docker-gitlab/blob/master/README.md).
