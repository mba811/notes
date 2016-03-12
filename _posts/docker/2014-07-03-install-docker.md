---
layout: post
title: Docker 的安装
tags: [docker]
---

[Docker ](http://docker.com/)是一个开源的引擎，通过轻量级、便携、自给自足、能执行于任何环境的容器，自动化应用程序的部署。

官方网站上有各种环境下的 安装指南，这里主要介绍下Ubuntu和CentOS系列的安装。

## Ubuntu 系列安装 Docker

### 通过系统自带包安装

Ubuntu 14.04 版本系统中已经自带了 Docker 包，可以直接安装。
    
    $ sudo apt-get update
    $ sudo apt-get install -y docker.io
    $ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
    $ sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io
    

如果使用操作系统自带包安装 Docker，目前安装的版本是比较旧的 0.9.1。 要安装更新的版本，可以通过使用 Docker 源的方式。

### 通过Docker源安装最新版本

要安装最新的 Docker 版本，首先需要安装 apt-transport-https 支持，之后通过添加源来安装。
    
    $ sudo apt-get install apt-transport-https
    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
    $ sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
    $ sudo apt-get update
    $ sudo apt-get install lxc-docker
    

### 14.04 之前版本

如果是较低版本的 Ubuntu 系统，需要先更新内核。
    
    $ sudo apt-get update
    $ sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
    $ sudo reboot
    

然后重复上面的步骤即可。

安装之后启动 Docker 服务。
    
    $ sudo service docker start
    

## CentOS 系列安装 Docker

Docker 支持 CentOS6 及以后的版本。

### 安装 Docker —— CentOS-7

Docker（重新编译自 RHEL 7）已收录在 CentOS-Extras 软件库内。你只须执行
    
    $ sudo yum install docker
    

要是你想采用一个（普遍会）较新版，并追纵上游源码的 docker，你可加入并启用虚拟化 SIG 的软件库：
    
    [virt7-testing]
    name=virt7-testing
    baseurl=http://cbs.centos.org/repos/virt7-testing/x86_64/os/
    enabled=1
    gpgcheck=0
    

安装 docker 后，你必须引导该服务才能应用它。
    
    $ sudo systemctl start docker
    

若要开机时引导 docker 服务：
    
    $ sudo systemctl enable docker
    

## 安装 Docker —— CentOS-6

![](http://wiki.centos.org/ArtWork/WikiDesign?action=AttachFile&do=get&target=icon-admonition-alert.png)

**在 CentOS-6 上安装 Docker 须要采用 [EPEL 软件库](https://fedoraproject.org/wiki/EPEL)。启用 [EPEL](https://fedoraproject.org/wiki/EPEL) 后，你便能继续以下的安装程序**

要在 CentOS-6 上安装 docker，请利用以下指令安装 [docker-io](http://dl.fedoraproject.org/pub/epel/6/x86_64/repoview/docker-io.html) 组件：
    
    $ sudo yum install docker-io
    

安装 docker 后，你必须引导该服务才能应用它。
    
    $ sudo service docker start
    

若要开机时引导 docker 服务：
    
    $ sudo chkconfig docker on
    

## 应用 Docker

在缺省情况下，docker 必须由 root 或是通过 sudo 的权限执行。你也可以把一个用户加进 docker 群组来让该用户才接执行 docker。

![](http://wiki.centos.org/ArtWork/WikiDesign?action=AttachFile&do=get&target=icon-admonition-alert.png)

请留意要是该用户逃出了容器之外，这样做也许会让他能提升权限。
    
    $ sudo usermod -a -G docker <你的用户>
    

要从 [Docker Hub](https://registry.hub.docker.com/_/centos/) 取得最新的稳定版 CentOS 官方映像：
    
    $ sudo docker pull centos
    

这个指令只会取出标签为 centos:latest 的映像，该标签永远指向最新的稳定版 CentOS 发行版本，现时为 CentOS 7（_centos:centos7_）。若要访问其它版本的 CentOS 映像，例如 CentOS 6：
    
    $ sudo docker pull centos:centos6
    

要查看已下载至本地的映像：
    
    $ sudo docker images centos
    REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    centos              centos6             a30bc9f3097e        3 days ago          215.8 MB
    centos              latest              dade6cb4530a        3 days ago          224 MB
    centos              centos7             dade6cb4530a        3 days ago          224 MB
    

要通过 docker 执行基本的 cat 指令：
    
    $ sudo docker run centos:latest cat /etc/centos-release
    CentOS Linux release 7.0.1406 (Core)
    

## Docker 映像

Docker 映像是利用 [ami_creator ](https://github.com/katzj/ami-creator)连同 [sig-cloud-instance-build 计划](https://github.com/CentOS/sig-cloud-instance-build)内的 kickstart 档所创建而成的。

完成品已收录于 [sig-cloud-images ](https://github.com/CentOS/sig-cloud-instance-images)计划内，按版本分类。

## 进一步阅读

有关 Docker 计划的详尽数据及[文档](https://docs.docker.com/)，请拜访它的[官方网站](https://docker.com/)。源代码已收录在 Docker 的 [GitHub 网页](https://github.com/docker/docker)。

### CentOS7

CentOS7 系统 CentOS-Extras 库中已带 Docker，可以直接安装：
    
    $ sudo yum install docker
    

安装之后启动 Docker 服务，并让它随系统启动自动加载。
    
    $ sudo service docker start
    $ sudo chkconfig docker on
    


### CentOS6

对于 CentOS6，可以使用 EPEL 库安装 Docker，命令如下
    
    $ sudo yum install http://mirrors.yun-idc.com/epel/6/i386/epel-release-6-8.noarch.rpm
    $ sudo yum install docker-io
    

## 在 coreos 中使用 docker

详见[内容](http://www.dockerpool.com/article/1413260011)

其他操作系统的安装可以查看官方的安装文档说明。
