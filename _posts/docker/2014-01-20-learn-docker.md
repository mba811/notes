---
layout: post
title: Docker 初体验
tags: [docker]
---

## Introduction

Docker 是基于 LXC 的应用程序容器技术, 最近一年在虚拟化, 云计算领域非常的  
流行, 它不但使得应用程序的自动化部署大大方便了, 甚至可以和 openstack 这样的  
IaaS 平台整合, OMG!

以下以 ubuntu 系统来体验以下 docker.

## Install

安装 docker
    
    # apt-get install linux-image-extra-`uname -r`
    # apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
    # echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list
    # apt-get update
    # apt-get install -y lxc-docker

测试
    
    # 如果没有 ubuntu 镜像的话, 会从仓库下载一份, 喝杯咖啡吧
    root@ubuntu:~# docker run -i -t ubuntu /bin/echo 'hello world'
    hello world

## More usage

前面说过, docker 是应用程序的容器, 实际上, 可以把应用程序以 daemon 的模式  
运行, 用 docker 提供的命令行接口和这些程序交互(e.g. I/O, 结束进程)

以一个简单的例子说明, 执行一个简单的脚本, 不断的在屏幕上打印 hello world
    
    # docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
    26f90b990b8cc0e592b0ffc5310584647e0e763d6c9794d65e9b3225a380ac81 #返回 container id

执行 docker 命令之后, 屏幕上除了打印一个 该 docker 容器的 ID 外, 什么也没有

要获取程序的输出, 可以用 logs 子命令, 该命令捕获程序的输出
    
    # docker logs 26f90b990b8cc0e592b0ffc5310584647e0e763d6c9794d65e9b3225a380ac81
    hello world
    hello world
    hello world
    hello world
    hello world
    hello world

使用 attach 把该程序恢复到前台执行, 有点类似于 bash 下的 fg 指令, 如果对 gdb  
很熟悉的朋友, 把他当做 gdb 下的 attach 也未尝不可
    
    # docker attach -sig-proxy=false 26f90b990b8cc0e592b0ffc5310584647e0e763d6c9794d65e9b3225a380ac81
    docker ps
    CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
    6c82bf81c5e3        ubuntu:12.04        /bin/sh -c while tru   2 minutes ago       Up 2 minutes                            kickass_franklin

后台的程序可以使用 stop 直接结束
    
    # docker stop 26f90b990b8cc0e592b0ffc5310584647e0e763d6c9794d65e9b3225a380ac81

如果想看当前容器运行着多少程序, 可以使用 ps 命令查看
    
    # docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
    7cf51b1d50358971ff699639fac4b6ea0e0a6d99e8dd6861addf200f2580681a
    # docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
    06fd537af32d05f99c1e26a19483d1ad0fe5dfb7512ab6a693ae23120ac43955
    # docker ps
    CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
    06fd537af32d        ubuntu:12.04        /bin/sh -c while tru   3 seconds ago       Up 3 seconds                            loving_franklin     
    7cf51b1d5035        ubuntu:12.04        /bin/sh -c while tru   4 seconds ago       Up 4 seconds                            stoic_galileo

## Deploy a web app

下面使用 docker 部署稍微复杂一点的程序, 一个使用 python 的 flask 框架编写  
的简单的 web 程序

先把程序的容器镜像下载下来
    
    # docker pull shykes/pybuilder # pull 一个新的 image
    # URL=http://github.com/shykes/helloflask/archive/master.tar.gz #传递给前面镜像的 buildapp 程序

使用镜像内置的 buildapp 程序构建程序
    
    # BUILD_JOB=$(docker run -d -t shykes/pybuilder:latest /usr/local/bin/buildapp $URL)

查看 build 状态
    
    # docker attach -sig-proxy=false $BUILD_JOB

把刚才做的修改保存为 mydocker/hello
    
    # BUILD_IMG=$(docker commit $BUILD_JOB mydocker/hello)

最后, 运行我们的 web 程序
    
    # WEB_WORKER=$(docker run -d -p 5000 $BUILD_IMG /usr/local/bin/runapp)

查看服务器的运行状态
    
    # docker logs $WEB_WORKER
     * Running on http://0.0.0.0:5000/

查找被映射到容器外的端口, 并用该端口测试是否部署成功
    
    # WEB_PORT=$(sudo docker port $WEB_WORKER 5000 | awk -F: '{ print $2 }')
    # curl http://127.0.0.1:$WEB_PORT
      Hello world!

## Resources

  * [Docker Installation](http://docs.docker.io/en/latest/installation/ubuntulinux/)
  * [Deploy Python Web App](http://docs.docker.io/en/latest/examples/python_web_app/)
