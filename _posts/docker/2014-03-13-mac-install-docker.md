---
layout: post
title: Mac OS X 下安装使用 Docker
tags: [docker]
---

云主机可以选择操作系统镜像快速创建主机，这比虚拟机更便捷了，我们本地也可以这么做了，因为有了 [Docker](https://www.docker.io/) 这个东西。它依赖于 LXC(Linux Container)，能从网络上获得配置好的 Linux 镜像，非常容易在隔离的系统中运行自己的应用。也因为它的底层核心是个 LXC，所以在 Mac OS X 下需要在 VirtualBox 中跑一个精小的 LXC(这里是一个 [Tiny Core Linux](http://tinycorelinux.net/)，完全在内存中运行，个头只约 24MB，启动时间小于 5 秒的 [boot2docker](https://github.com/boot2docker/boot2docker)) 虚拟机，构建在 VirtualBox 中。以后的通信过程就是 docker --> boot2docker --> container，端口或磁盘映射也是遵照这一关系。

理解了上面的关系，开始说说 Docker 安装过程

**1. 安装 [VirtualBox](https://www.virtualbox.org/)**, 不多讲, 因要在它当中创建一个 boot2docker-vm 虚拟机

**2. 安装 boot2docker**

brew install boot2docker

你也可以手工安装

curl https://raw.github.com/steeve/boot2docker/master/boot2docker > boot2docker; chmod +x boot2docker; sudo mv boot2docker /usr/local/bin

**3. 安装 Docker**

brew install docker

也可手工安装

curl -o docker http://get.docker.io/builds/Darwin/x86_64/docker-latest; chmod +x docker; sudo cp docker /usr/local/bin

**4. 配置 Docker 客户端**

export DOCKER_HOST=tcp://127.0.0.1:4243

把它写到 ~/.bash_profile 中，如果你是用的 bash 的话。我工作在 fish 下，所以在 ~/.config/fish/config.fish 中加了 set -x DOCKER_HOST tcp://127.0.0.1:4243

**5. boot2docker 初始化与启动**

boot2docker init

完成后就能在 VirtualBox 中看到一个叫做 boot2docker-vm的虚拟机，以后只需用 boot2docker 命令来控制这个虚拟机的行为，启动，停止等。

boot2docker up

启动，boot2docker-vm虚拟机，我们能在 VirtualBox 中看到该虚拟机变成 Running 状态

直接执行 boot2docker 可以看到可用的参数

Usage /usr/local/bin/boot2docker {init|start|up|save|pause|stop|restart|status|info|delete|ssh|download}

**6. 启动 Docker 守护进程**

sudo docker -d

这时可执行

boot2docker ssh，输入密码  tcuser 进到该虚拟机的控制台下，如果要用户名的话请输入docker

![boot2docker_vm](http://unmi.cc/wp-content/uploads/2014/05/boot2docker_vm1-800x539.png)

上面看到 Mac 启动了 4243 端口，在 boot2docker 虚拟机中也有 4243 端口，并在 /var/run/docker.sock 上监听。借此回顾下 docker 的通信过程，dock 命令是与 Docker daemon 在  Mac 上开启的  4243 端口通信，该端口映射到 boot2docker 的  4243 端口上，进而通过 /var/run/docker.sock 与其中的容器进行通信。

所以在执行  docker version 时如果没有启动 Docker daemon 会提示

2014/05/16 06:52:48 Cannot connect to the Docker daemon. Is 'docker -d' running on this host?

如果没有启动 boot2docker 会得到提示

Get http:///var/run/docker.sock/v1.11/version: dial unix /var/run/docker.sock: no such file or directory

Mac OS X -- boot2docker -- container 三者之间的关系，这张图很好的说明了

![docker-install](http://unmi.cc/wp-content/uploads/2014/05/docker-install-800x417.png)图片来自：[http://tech.uc.cn/?p=2726](http://tech.uc.cn/?p=2726)

* * *

上面  boot2docker, docker 都准备就绪了, 现在开始进入 dock  的操作了，有关于 docker 的命令可以参看这里 [http://blog.tankywoo.com/docker/2014/05/08/docker-4-summary.html](http://blog.tankywoo.com/docker/2014/05/08/docker-4-summary.html)。

本文原始链接 [http://unmi.cc/mac-os-x-experience-docker/](http://unmi.cc/mac-os-x-experience-docker/), 来自 [隔叶黄莺 Unmi Blog](http://unmi.cc/)

**1. 下载镜像，并加载启动容器**

docker images    #现在没有一个镜像  
docker pull learn/tutorial     #我们把这个拉下来试验，可用 docker search ubuntu 找到所有与 ubuntu 有关的镜像  
docker run -i -t  learn/tutorial  #加载镜像 learn/tutorial 并进到 shell 下，这样就直接连接到该容器中，退出后容器也退了  
docker ps     #在另一个终端中用这个命令，可以看到运行实例，即容器

现在我们在容器的控制台上 oot@95903c1a2bf7:/#，可以安装一个 apche2, curl 并启动 apache2，来测试下

root@95903c1a2bf7:/# apt-get update  
root@95903c1a2bf7:/# apt-get install apache2 curl  
root@95903c1a2bf7:/# apachectl start  
root@95903c1a2bf7:/# curl http://localhost  
<html><body><h1>It works!</h1>  
<p>This is the default web page for this server.</p>  
<p>The web server software is running but no content has been added, yet.</p>  
</body></html>

Apache2 正常启动了，在容器内可访问。但现在还无法从 Mac OS X 上对该 apache 服务进行访问，这需要端口映射，有两种方式。不过在端口映射之前还需保存下镜像的修改。

**2. 保存镜像**

如果前面用 docker run -i -t learn/tutorial 运行的镜像

在运行该镜像的容器中安装了软件，需要把新的内容保存到该镜像中去，否则下次启动该镜像又恢复成原样

uqiu@localhost ~> docker ps -l  
CONTAINER ID        IMAGE                   COMMAND             CREATED             STATUS              PORTS               NAMES  
95903c1a2bf7        learn/tutorial:latest   /bin/bash           6 minutes ago       Up 5 minutes        80/tcp              thirsty_colden

看到容器的 ID，然后执行

docker commit 95903c1a2bf7 learn/tutorial:latest  #把当前容器的修改提交到镜像 learn/tutorial  中去

以后再次运行该镜像就有了最新安装的内容了。

**2. 端口映射**

比如我们现在要做的映射关系是 Mac OS X(50080) --> boot2docker(40080) --> container(80)，如下图：

![docker-port-map](http://unmi.cc/wp-content/uploads/2014/05/docker-port-map-800x423.png)

图片修改自：[http://tech.uc.cn/?p=2726](http://tech.uc.cn/?p=2726)

可以有两种办法

1)

boot2docker ssh -L 50080:localhost:40080  #这条命令可以在  boot2docker-vm  运行时执行，建立多个不同的映射就是执行多次

docker run -i -t -p 40080:80 learn/tutorial  
root@c79b5070a972:/# apachectl start

然后在 Mac 的浏览器中打开 http://localhost:50080

![port-mapping-docker_1](http://unmi.cc/wp-content/uploads/2014/05/port-mapping-docker_1-800x275.png)

2)

VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port_50080:80,tcp,,50080,,40080"

docker run -i -t -p 40080:80 learn/tutorial  
root@c79b5070a972:/# apachectl start

这是直接修改了  boot2docker-vm 的配置，可以在 VirtualBox 中看到这条配置，配置 nat 命令见 [http://www.virtualbox.org/manual/ch06.html#natforward](http://www.virtualbox.org/manual/ch06.html#natforward). 也能建立许多的端口映射

![boot2docker-vm-port](http://unmi.cc/wp-content/uploads/2014/05/boot2docker-vm-port-800x215.png)

还能直接在这里编辑端口映射关系

* * *

Docker  的世界很精彩，其他内容基本就是怎么用好 Docker 命令，如管理镜像，容器，创建自己的镜像; Docker 有三种运行命令的方式，短暂方式，交互方式，daemon方式; 使用 ssh 来管理容器等等。

在 Mac 下使用 Docker 除了可用 boot2docker 作为 LXC，还有个替代品 VAGRANT 。

参考：

1. [利用Docker构建开发环境](http://tech.uc.cn/?p=2726)  
2. [Docker学习笔记之一，搭建一个JAVA Tomcat运行环境](http://www.blogjava.net/yongboy/archive/2013/12/12/407498.html)  
3. [Installing Docker on Mac OS X](http://docs.docker.io/installation/mac/)  
4. [https://github.com/boot2docker/boot2docker](https://github.com/boot2docker/boot2docker)  
5. [Docker 快速入门](http://cn.soulmachine.me/blog/20131026/)

本文链接 [http://unmi.cc/mac-os-x-experience-docker/](http://unmi.cc/mac-os-x-experience-docker/), 来自 [隔叶黄莺 Unmi Blog](http://unmi.cc/)