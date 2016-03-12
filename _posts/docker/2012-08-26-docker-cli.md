---
layout: post
title: Docker 命令行帮助
tags: [docker]
---

> 列出可用的命令，或者运行不带参数的docker或者执行docker帮助
    
        sudo docker
    
        useage of docker
    
            -D   默认false 允许调试模式(debug mode)
    
            -H   默认是 unix:///var/run/docker.sock tcp://[host[:port]]来绑定 或者 unix://[/path/to/socket]来使用(二进制文件的时候)，当主机ip host=[0.0.0.0],(端口)port=[4243] 或者 path=[/var/run/docker.sock]是缺省值，做为默认值来使用
    
            -api-enable-cors 默认flase 允许CORS header 远程api
    
            -b   默认是空，附加在已存在的网桥上，如果是用'none'参数，就禁用了容器的网络
    
            -bip 默认是空，使用提供的CIDR（Classless Inter-Domain Routing-无类型域间选路）标记地址动态创建网桥(dcoker0),和-b参数冲突
    
            -d   默认false 允许进程模式(daemon mode)
    
            -dns 默认是空，使docker使用指定的DNS服务器
    
            -g   默认是"/var/lib/docker":作为docker使用的根路径
    
            -icc 默认true，允许inter-container来通信
    
            -ip  默认"0.0.0.0" ：绑定容器端口的默认Ip地址
    
            -iptables 默认true 禁用docker添加iptables规则
    
            -mtu 默认1500 : 设置容器网络传输的最大单元(mtu)
    
            -p   默认是/var/run/docker.pid 进程pid使用的文件路径
    
            -r   默认是true 重启之前运行的容器
    
            -s   默认是空 ，这个是docker运行是使用一个指定的存储驱动器
    
            -v   默认false 打印版本信息和退出 
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c1.png)

> 当你的进程使用-d标识的时候,docker使用一个持久的进程来管理容器，docker使用相同的进程和客户端.
> 
> docker使用 -d -s 来映射存储程序，从而迫使docker运用映射的存储器来存储驱动程序.
> 
> docker使用 -d -dns 8.8.8.8，来设置所有的docker容器的DNS服务器.
> 
> docker使用 -d -D参数，来让进程输出debug信息
> 
> docker客户端，也可以使用DOCKER_HOST的环境变量参数来改变docker -H的参数设置
    
    docker -H tcp://0.0.0.0:4243 ps
    # or
    export DOCKER_HOST="tcp://0.0.0.0:4243"
    docker ps
    # both are equal
    

**attach**
    
    usage : docker attach CONTAINER
    
        attach 来运行一个容器
    
          -nostdin   默认参数false 不要附加stdin（输入）
          -sig-proxy 默认true Proxify所有接收信号流程(即使在non-tty模式)
    

> 你可以把docker从容器中分离出来运行，然后用CTRl -c来退出或者CTRL -\来获得一个异常堆栈的docker退出，当这个容器退出过程会>将退出代码返回给客户端
> 
> 使用docker stop来停止一个容器
> 
> 使用docker kill来杀死一个容器

**attach examples**
    
     ID=$(sudo docker run -d ubuntu /usr/bin/top -b)
     sudo docker attach $ID
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c2.png)

  
### build
    
    Usage: docker build [OPTIONS] PATH | URL | -
    通过指定文件代码来创建一个新的容器
    
      -t="":     库的名称和可选标记容器创建成功时候产生例如(ubuntu:widuu)
    
      -q=false:  抑制生成过程中的输出
    
      -no-cache: 构建镜像时不适用缓存
    
      -rm:       成功构建后去除中间容器
    

这个文件路径或者url被称为构建的环境，在生成过程中可以参考这些文件的环境，举个例子，使用ADD命令！当一个Dockerfile被给定位URL，则没有环境被设定，当一个git仓库被设置为URL时，这个仓库就被做为环境！

> 参见 Build Images (Dockerfile Reference)[###]

## [__](https://code.csdn.net/u010702509/docker_cli_2#examples)Examples:

> 非官方备注，前提是你的目录下有Dockerfile文件，我提供一下我写的测试文件
    
    # VERSION 0.0.1
    # 默认ubuntu server长期支持版本，当前是12.10
    FROM ubuntu
    # 签名啦
    MAINTAINER widuu "admin@widuu.com"
    
    # 更新源，安装ssh server
    RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe"> /etc/apt/sources.list
    RUN apt-get update
    RUN apt-get install -y openssh-server
    RUN mkdir -p /var/run/sshd
    
    # 设置root ssh远程登录密码为123456
    RUN echo "root:dgj99349" | chpasswd 
    
    # 容器需要开放SSH 22端口
    EXPOSE 22
    
    
    # SSH终端服务器作为后台运行
    ENTRYPOINT /usr/sbin/sshd -D
    

> 开始官方列子 sudo docker build .

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c21.png)

这个例子指定的Dockerfile文件路径在.所以本地目录中所有文件信息被推送到docker进程中去，docker进程会找到指定路径中的文件中的环境信息来构建，请记住后台程序会执行在远程机器上，不需要解析Dockerfile在客户端发生什么(你在哪里运行docker build).这就意味着文件中的所有信息都会被发送，不仅仅是前边Dockerfile列出的ADD

环境从本地机器中传输到Docker进程中，这就是你从客户端看到“Uploading context”的信息。
    
    sudo docker build -t vieux/apache:2.0 .
    

这个构建像以前的例子，但是他给镜像添加了tag,这个仓库的名字将会是vieux/apache，tag名称是2.0
    
    sudo docker build - < Dockerfile
    

这个从输入段读取Dockerfile没有环境，由于缺少环境，本地的所有环境不会发送到docker进程中去，由于没有本地的环境，Dockerfile 中的ADD只能从远程URl中获取
    
     sudo docker build github.com/creack/docker-firefox
    

这个将会从你的github的仓库中获取，并且使用获取的仓库作为环境，Dockerfile在存储在跟作用Dockerfile，请注意，您可以指定一个任意的git存储库使用 git:// 模式

## [__](https://code.csdn.net/u010702509/docker_cli_2#commit)commit
    
    Usage: docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
    
    通过容器的改变来创建一个新的镜像
    
      -m="": 提交信息
    
      -author="": 提交作者信息 (eg. "John Hannibal Smith <hannibal@a-team.com>"
    
      -run="": 配置应用镜像是执行 `docker run`.
               (ex: -run='{"Cmd": ["cat", "/world"], "PortSpecs": ["22"]}')
    

## [__](https://code.csdn.net/u010702509/docker_cli_2#%E6%8F%90%E4%BA%A4%E4%B8%80%E4%B8%AA%E4%B8%AD%E6%96%AD%E7%9A%84%E5%AE%B9%E5%99%A8)提交一个中断的容器
    
    $ sudo docker ps
    ID                  IMAGE               COMMAND             CREATED             STATUS              PORTS
    c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours
    197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours
    $ docker commit c3f279d17e0a  SvenDowideit/testimage:version3
    f5283438590d
    $ docker images | head
    REPOSITORY                        TAG                 ID                  CREATED             VIRTUAL SIZE
    SvenDowideit/testimage            version3            f5283438590d        16 seconds ago      335.7 MB
    

## [__](https://code.csdn.net/u010702509/docker_cli_2#%E6%94%B9%E5%8F%98%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C%E7%9A%84%E5%91%BD%E4%BB%A4)改变容器运行的命令

有时你运行一个应用容器服务，你需要迅速改变它，然后变回来

做为一个雷子，我们使用ls运行一个容器，然后我们改变镜像运行ls /etc
    
    $ docker run -t -name test ubuntu ls
    bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  selinux  srv  sys  tmp  usr  var
    $ docker commit -run='{"Cmd": ["ls","/etc"]}' test test2
    933d16de9e70005304c1717b5c6f2f39d6fd50752834c6f34a155c70790011eb
    $ docker run -t test2
    adduser.conf            gshadow          login.defs           rc0.d
    alternatives            gshadow-         logrotate.d          rc1.d
    apt                     host.conf        lsb-base             rc2.d
    ...
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c22.png)

## [__](https://code.csdn.net/u010702509/docker_cli_2#%E5%AE%8C%E6%95%B4%E7%9A%84-run-%E4%BE%8B%E5%AD%90)完整的 -run 例子

-run json hash 改变配置部分当运行的docker检查容器或者配置当容器运行检索的ImageId
    
    $ sudo docker commit -run='
    {
        "Entrypoint" : null,
        "Privileged" : false,
        "User" : "",
        "VolumesFrom" : "",
        "Cmd" : ["cat", "-e", "/etc/resolv.conf"],
        "Dns" : ["8.8.8.8", "8.8.4.4"],
        "MemorySwap" : 0,
        "AttachStdin" : false,
        "AttachStderr" : false,
        "CpuShares" : 0,
        "OpenStdin" : false,
        "Volumes" : null,
        "Hostname" : "122612f45831",
        "PortSpecs" : ["22", "80", "443"],
        "Image" : "b750fe79269d2ec9a3c593ef05b4332b1d1a02a62b4accb2c21d589ff2f5f2dc",
        "Tty" : false,
        "Env" : [
           "HOME=/",
           "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
        ],
        "StdinOnce" : false,
        "Domainname" : "",
        "WorkingDir" : "/",
        "NetworkDisabled" : false,
        "Memory" : 0,
        "AttachStdout" : false
    }' $CONTAINER_ID
    

### [__](https://code.csdn.net/u010702509/docker_cli_2#cp)CP
    
    Usage :docker cp CONTAINER:PATH HOSTPATH
    
    将容器的文件和文件夹复制到主机中
    
    path.  路径相对于文件系统的跟
    
    $ sudo docker cp 7bb0e258aefe:/etc/debian_version .
    $ sudo docker cp blue_frog:/etc/hosts .
    

### [__](https://code.csdn.net/u010702509/docker_cli_2#diff)diff
    
    Usage:docker diff CONTAINER
    
    更改的文件和目录列表容器的文件系统
    

有3中列出不同的diff事件

  1. A - Add
  2. D - Delete
  3. C - Change

例子
    
    $ sudo docker diff 7bb0e258aefe
    
    C /dev
    A /dev/kmsg
    C /etc
    A /etc/mtab
    A /go
    A /go/src
    A /go/src/github.com
    A /go/src/github.com/dotcloud
    A /go/src/github.com/dotcloud/docker
    A /go/src/github.com/dotcloud/docker/.git
    ....
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c23.png)

### events
    
    Usage :docker events
    
    从服务器获取实时事件
    
    -since="": 显示以前创建的事件流()
    

## [__](https://code.csdn.net/u010702509/docker_cli_3#examples)Examples
    
    在这个例子里你需要运行两个shell
    

**Shell 1:监听事件**
    
    sudo docker events
    

**Shell 2:开始和关闭一个容器**
    
    $ sudo docker start 4386fb97867d
    $ sudo docker stop 4386fb97867d
    

**Shell 1: 再一次。。你会看到下面的事件**
    
    [2013-09-03 15:49:26 +0200 CEST] 4386fb97867d: (from 12de384bfb10) start
    [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) die
    [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) stop
    

**查看事件从一个指定的过去的时间段**
    
      $ sudo docker events -since 1378216169
      [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) die
      [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) stop
    
      $ sudo docker events -since '2013-09-03'
      [2013-09-03 15:49:26 +0200 CEST] 4386fb97867d: (from 12de384bfb10) start
      [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) die
      [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) stop
    
      $ sudo docker events -since '2013-09-03 15:49:29 +0200 CEST'
      [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) die
      [2013-09-03 15:49:29 +0200 CEST] 4386fb97867d: (from 12de384bfb10) stop
      docker events
    

### [__](https://code.csdn.net/u010702509/docker_cli_3#export)export
    
     Usage: docker export CONTAINER(容器)
    
     导出文件系统作为一个tar文档发送到stdout
    

**例子**
    
    $ sudo docker export red_panda > latest.tar
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c31.png)

### [__](https://code.csdn.net/u010702509/docker_cli_3#history)history
    
    Usage: docker history [OPTIONS] IMAGE
    
    显示镜像的历史操作
    
      -notrunc=false: 不截断输出
      -q=false: 只显示数字id
    

**查看docker:last最新创建的镜像**
    
      $ docker history docker
      ID                  CREATED             CREATED BY
      docker:latest       19 hours ago        /bin/sh -c #(nop) ADD . in /go/src/github.com/dotcloud/docker
      cf5f2467662d        2 weeks ago         /bin/sh -c #(nop) ENTRYPOINT ["hack/dind"]
      3538fbe372bf        2 weeks ago         /bin/sh -c #(nop) WORKDIR /go/src/github.com/dotcloud/docker
      7450f65072e5        2 weeks ago         /bin/sh -c #(nop) VOLUME /var/lib/docker
      b79d62b97328        2 weeks ago         /bin/sh -c apt-get install -y -q lxc
      36714852a550        2 weeks ago         /bin/sh -c apt-get install -y -q iptables
      8c4c706df1d6        2 weeks ago         /bin/sh -c /bin/echo -e '[default]\naccess_key=$AWS_ACCESS_KEY\nsecret_key=$AWS_SECRET_KEYn' > /.s3cfg
      b89989433c48        2 weeks ago         /bin/sh -c pip install python-magic
      a23e640d85b5        2 weeks ago         /bin/sh -c pip install s3cmd
      41f54fec7e79        2 weeks ago         /bin/sh -c apt-get install -y -q python-pip
      d9bc04add907        2 weeks ago         /bin/sh -c apt-get install -y -q reprepro dpkg-sig
      e74f4760fa70        2 weeks ago         /bin/sh -c gem install --no-rdoc --no-ri fpm
      1e43224726eb        2 weeks ago         /bin/sh -c apt-get install -y -q ruby1.9.3 rubygems libffi-dev
      460953ae9d7f        2 weeks ago         /bin/sh -c #(nop) ENV GOPATH=/go:/go/src/github.com/dotcloud/docker/vendor
      8b63eb1d666b        2 weeks ago         /bin/sh -c #(nop) ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/goroot/bin
      3087f3bcedf2        2 weeks ago         /bin/sh -c #(nop) ENV GOROOT=/goroot
      635840d198e5        2 weeks ago         /bin/sh -c cd /goroot/src && ./make.bash
      439f4a0592ba        2 weeks ago         /bin/sh -c curl -s https://go.googlecode.com/files/go1.1.2.src.tar.gz | tar -v -C / -xz && mv /go /goroot
      13967ed36e93        2 weeks ago         /bin/sh -c #(nop) ENV CGO_ENABLED=0
      bf7424458437        2 weeks ago         /bin/sh -c apt-get install -y -q build-essential
      a89ec997c3bf        2 weeks ago         /bin/sh -c apt-get install -y -q mercurial
      b9f165c6e749        2 weeks ago         /bin/sh -c apt-get install -y -q git
      17a64374afa7        2 weeks ago         /bin/sh -c apt-get install -y -q curl
      d5e85dc5b1d8        2 weeks ago         /bin/sh -c apt-get update
      13e642467c11        2 weeks ago         /bin/sh -c echo 'deb http://archive.ubuntu.com/ubuntu precise main universe' > /etc/apt/sources.list
      ae6dde92a94e        2 weeks ago         /bin/sh -c #(nop) MAINTAINER Solomon Hykes <solomon@dotcloud.com>
    

**带图的都是我本地自己测试的**

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c32.png)

### [__](https://code.csdn.net/u010702509/docker_cli_3#images)images
    
    Usage: docker images [OPTIONS] [NAME]
    
    列出镜像
    
      -a=false: 显示所有镜像(默认情况下过略掉了用于构建的镜像)
      -notrunc=false: 不截断输出
      -q=false: 指输出镜像id
      -tree=false: 用树形结构输出镜像
      -viz=false:  用graphviz输出镜像图(非官方备注，你需要安装apt-get install graphviz)
    

**列出最近创建的镜像**
    
    $ sudo docker images | head
    REPOSITORY                    TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    <none>                        <none>              77af4d6b9913        19 hours ago        1.089 GB
    committest                    latest              b6fa739cedf5        19 hours ago        1.089 GB
    <none>                        <none>              78a85c484f71        19 hours ago        1.089 GB
    docker                        latest              30557a29d5ab        20 hours ago        1.089 GB
    <none>                        <none>              0124422dd9f9        20 hours ago        1.089 GB
    <none>                        <none>              18ad6fad3402        22 hours ago        1.082 GB
    <none>                        <none>              f9f1e26352f0        23 hours ago        1.089 GB
    tryout                        latest              2629d1fa0b81        23 hours ago        131.5 MB
    <none>                        <none>              5ed6274db6ce        24 hours ago        1.089 GB
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c33.png)

**列出全部的镜像ID** $ sudo docker images -notrunc | head REPOSITORY TAG IMAGE ID CREATED VIRTUAL SIZE 77af4d6b9913e693e8d0b4b294fa62ade6054e6b2f1ffb617ac955dd63fb0182 19 hours ago 1.089 GB committest latest b6fa739cedf5ea12a620a439402b6004d057da800f91c7524b5086a5e4749c9f 19 hours ago 1.089 GB 78a85c484f71509adeaace20e72e941f6bdd2b25b4c75da8693efd9f61a37921 19 hours ago 1.089 GB docker latest 30557a29d5abc51e5f1d5b472e79b7e296f595abcf19fe6b9199dbbc809c6ff4 20 hours ago 1.089 GB 0124422dd9f9cf7ef15c0617cda3931ee68346455441d66ab8bdc5b05e9fdce5 20 hours ago 1.089 GB 18ad6fad340262ac2a636efd98a6d1f0ea775ae3d45240d3418466495a19a81b 22 hours ago 1.082 GB f9f1e26352f0a3ba6a0ff68167559f64f3e21ff7ada60366e2d44a04befd1d3a 23 hours ago 1.089 GB tryout latest 2629d1fa0b81b222fca63371ca16cbf6a0772d07759ff80e8d1369b926940074 23 hours ago 131.5 MB 5ed6274db6ceb2397844896966ea239290555e74ef307030ebb01ff91b1914df 24 hours ago 1.089 GB

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c34.png)

**显示图像图片**
    
    $ sudo docker images -viz | dot -Tpng -o docker.png
    

(这里不引用官方的了,我本地测试的)

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c35.png)

**显示镜像的曾次结构**
    
    $ sudo docker images -tree
    
    ├─8dbd9e392a96 Size: 131.5 MB (virtual 131.5 MB) Tags: ubuntu:12.04,ubuntu:latest,ubuntu:precise
    └─27cf78414709 Size: 180.1 MB (virtual 180.1 MB)
      └─b750fe79269d Size: 24.65 kB (virtual 180.1 MB) Tags: ubuntu:12.10,ubuntu:quantal
        ├─f98de3b610d5 Size: 12.29 kB (virtual 180.1 MB)
        │ └─7da80deb7dbf Size: 16.38 kB (virtual 180.1 MB)
        │   └─65ed2fee0a34 Size: 20.66 kB (virtual 180.2 MB)
        │     └─a2b9ea53dddc Size: 819.7 MB (virtual 999.8 MB)
        │       └─a29b932eaba8 Size: 28.67 kB (virtual 999.9 MB)
        │         └─e270a44f124d Size: 12.29 kB (virtual 999.9 MB) Tags: progrium/buildstep:latest
        └─17e74ac162d8 Size: 53.93 kB (virtual 180.2 MB)
          └─339a3f56b760 Size: 24.65 kB (virtual 180.2 MB)
            └─904fcc40e34d Size: 96.7 MB (virtual 276.9 MB)
              └─b1b0235328dd Size: 363.3 MB (virtual 640.2 MB)
                └─7cb05d1acb3b Size: 20.48 kB (virtual 640.2 MB)
                  └─47bf6f34832d Size: 20.48 kB (virtual 640.2 MB)
                    └─f165104e82ed Size: 12.29 kB (virtual 640.2 MB)
                      └─d9cf85a47b7e Size: 1.911 MB (virtual 642.2 MB)
                        └─3ee562df86ca Size: 17.07 kB (virtual 642.2 MB)
                          └─b05fc2d00e4a Size: 24.96 kB (virtual 642.2 MB)
                            └─c96a99614930 Size: 12.29 kB (virtual 642.2 MB)
                              └─a6a357a48c49 Size: 12.29 kB (virtual 642.2 MB) Tags: ndj/mongodb:latest
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c36.png)

### [__](https://code.csdn.net/u010702509/docker_cli_3#import)import
    
    Usage: docker import URL|- [REPOSITORY[:TAG]]
    
    创建一个空文件系统映像和引用压缩文件的内容
    (.tar, .tar.gz, .tgz, .bzip, .tar.xz, .txz) into it, 然后选择性的标记.
    

此时，这个URl必须指向一个文件存档(.tar, .tar.gz, .tgz, .bzip, .tar.xz, or .txz) 包含一个跟文件系统，如果你想引用一个本地的文件压缩文件，你可以使用从输入端使用参数

## [__](https://code.csdn.net/u010702509/docker_cli_3#examples)Examples

**输入一个远程的地址**
    
    sudo docker import http://example.com/exampleimage.tgz
    

**你可以引用我的url速度快，[http://docker.widuu.com/ubuntu.tar](http://docker.widuu.com/ubuntu.tar)**

**输入一个本地的文件**

通过管道输入docker
    
    cat exampleimage.tgz | sudo docker import - exampleimagelocal:new
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c37.png)

注意本例子中 sudo 你必须保留文件的所有权(特别是root所有权)在tar文档，如果当你的tar不是root(或者 sudo 命令)，那么所有权可能不会保留！

### [__](https://code.csdn.net/u010702509/docker_cli_3#info)info
    
    Usage: docker info
    
    显示整个文件系统
    
    $ sudo docker info
    Containers: 292
    Images: 194
    Debug mode (server): false
    Debug mode (client): false
    Fds: 22
    Goroutines: 67
    LXC Version: 0.9.0
    EventsListeners: 115
    Kernel Version: 3.8.0-33-generic
    WARNING: No swap limit support  
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c38.png)

### insert
    
    Usage: docker insert IMAGE URL PATH
    
    通过url给镜像路径增加文件
    

使用指定的镜像作为父镜像增加一层新的文件，insert命令不修改原始奖项，而新的镜像的内容包含父镜像和新文件

### [__](https://code.csdn.net/u010702509/docker_cli_4#examples)Examples

增加github文件
    
    $ sudo docker insert 8283e18b24bc https://raw.github.com/metalivedev/django/master/postinstall /tmp/postinstall.sh
    06fd35556d7b
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#inspect)inspect
    
    Usage: docker inspect CONTAINER|IMAGE [CONTAINER|IMAGE...]
    
    返回容器底层信息
    
      -format="": 格式化输出的格式
    

默认情况下，这将使所有结果放在一个JSON数组中，如果指定格式，就安装指定的格式来执行输出每个结果

Go的text/template包描述格式化的细节

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c41.png)

### [__](https://code.csdn.net/u010702509/docker_cli_4#examples)Examples

获得一个容器ip地址
    
    sudo docker inspect -format='{{.NetworkSettings.IPAddress}}' $INSTANCE_ID
    

列出所有绑定的端口地址
    
    sudo docker inspect -format='{{range $p, $conf := .NetworkSettings.Ports}} {{$p}} -> {{(index $conf 0).HostPort}} {{end}}' $INSTANCE_ID
    

查找一个特定端口映射

当放字段名是一个数字时.Field语法不工作，网络设置端口部分包含一个内部端口影响到外部地址和端口的对象的列表！如果你要找出数字公共端口，你使用索引来查找特定端口映射，索引0包含的第一个对象！我们要求主机端口字段是获得公共地址！
    
    $ sudo docker inspect -format='{{(index (index .NetworkSettings.Ports "8787/tcp") 0).HostPort}}' $INSTANCE_ID
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#kill)kill
    
    Usage: docker kill CONTAINER [CONTAINER...]
    
    杀死一个正在运行的容器（发送终止信号） 
    

> 已知问题（kill） issue 197标明docker kill将很难删除目录和删除容器

### [__](https://code.csdn.net/u010702509/docker_cli_4#load)load
    
    Usage: docker load < ubuntu.tar
    
    从输入流中加载一个压缩包，恢复包含的所有镜像和tag 
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#login)login
    
    Usage: docker login [OPTIONS] [SERVER]
    
    登陆你在docker服务器注册的账号 index.docker.io
    
    -e="": email
    -p="": password
    -u="": username
    
    如果你想登陆你的私有的仓库你可以添加的你服务器名称
    
    example:
    docker login localhost:8080
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#logs)logs
    
    Usage: docker logs [OPTIONS] CONTAINER
    
    取出容器的log日志
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c42.png)

docker logs命令提供一个便利的任何执行时间的记录，当你使用docker run的时候并不能保证执行顺序（当你docker logs你就可能没有产生一些执行记录）

### [__](https://code.csdn.net/u010702509/docker_cli_4#port)port
    
    Usage: docker port [OPTIONS] CONTAINER PRIVATE_PORT
    
    查找私有地址转换到共有地址端口
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c43.png)

### [__](https://code.csdn.net/u010702509/docker_cli_4#ps)ps
    
    Usage: docker ps [OPTIONS]
    
    List containers
    
      -a=false: 查看所有的镜像 默认情况下只查看正在运行的镜像
      -notrunc=false: 不把输出截断
      -q=false: 仅仅只显示镜像ID
    

运行docker ps查看2个容器

$ docker ps CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 4c01db0b339c ubuntu:12.04 bash 17 seconds ago Up 16 seconds webapp d7886598dbe2 crosbymichael/redis:latest /redis-server --dir 33 minutes ago Up 33 minutes 6379/tcp redis,webapp/db

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c44.png)

### [__](https://code.csdn.net/u010702509/docker_cli_4#pull)pull
    
    Usage: docker pull NAME
    
    从远程仓库中获取镜像
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#push)push
    
    Usage: docker push NAME
    
    推送本地镜像到远程仓库
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#restart)restart
    
    Usage: docker restart [OPTIONS] NAME
    
    重启正在运行的远程仓库
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#rm)rm
    
    Usage: docker rm [OPTIONS] CONTAINER
    
    删除一个或者所有的容器
    
    -link="": Remove the link instead of the actual container
    

> 已知问题（kill） issue 197标明docker kill将很难删除目录和删除容器

## [__](https://code.csdn.net/u010702509/docker_cli_4#examples)Examples
    
    $ sudo docker rm /redis
    /redis
    

这将删除容器下引用的链接/reids
    
    $ sudo docker rm -link /webapp/redis
    /webapp/redis
    

这将删除/webapp /redis底层之间的联系，容器中会删除所有的网络通信
    
    $ sudo docker rm `docker ps -a -q`
    

这个命令会删除所有停止运行的容器，docker ps -a -q会返回所有的镜像ID通过这些镜像ID再执行rm命令删除它们，当然正在运行的是不会被删除的

### [__](https://code.csdn.net/u010702509/docker_cli_4#rmi)rmi
    
    Usage: docker rmi IMAGE [IMAGE...]
    
    删除一个或者多个镜像
    

## [__](https://code.csdn.net/u010702509/docker_cli_4#%E5%88%A0%E9%99%A4%E6%A0%87%E8%AE%B0%E7%9A%84%E9%95%9C%E5%83%8F)删除标记的镜像
    
    镜像可以通过它们的长或者短ID删除或者他们的镜像名称，如果一个镜像有一个或者多个名称，在删除镜像之前，他们每一个都必须要删除
    
    $ sudo docker images
    REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
    test1                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
    test                      latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
    test2                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
    
    $ sudo docker rmi fd484f19954f
    Error: Conflict, cannot delete image fd484f19954f because it is tagged in multiple repositories
    2013/12/11 05:47:16 Error: failed to remove one or more images
    
    $ sudo docker rmi test1
    Untagged: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
    $ sudo docker rmi test2
    Untagged: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
    
    $ sudo docker images
    REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
    test1                     latest              fd484f19954f        23 seconds ago      7 B (virtual 4.964 MB)
    $ sudo docker rmi test
    Untagged: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
    Deleted: fd484f19954f4920da7ff372b5067f5b7ddb2fd3830cecd17b96ea9e286ba5b8
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#run)run
    
    Usage: docker run [OPTIONS] IMAGE[:TAG] [COMMAND] [ARG...]
    
    Run a command in a new container
    
      -a=map[]: /u010702509/docker_cli_4/file/附加标准输入、输出或者错误输出
      -c=0: 共享CPU格式（相对重要）
      -cidfile="": 将容器的ID标识写入文件
      -d=false: 分离模式，在后台运行容器，并且打印出容器ID
      -e=[]: /u010702509/docker_cli_4/file/设置环境变量
      -h="": 容器的主机名称
      -i=false: 保持输入流开放即使没有附加输入流
      -privileged=false: 给容器扩展的权限
      -m="": 内存限制 (格式: <number><optional unit>, unit单位 = b, k, m or g)
      -n=true: 允许镜像使用网络
      -p=[]: /u010702509/docker_cli_4/file/匹配镜像内的网络端口号
      -rm=false:当容器退出时自动删除容器 (不能跟 -d一起使用)
      -t=false: 分配一个伪造的终端输入
      -u="": 用户名或者ID
      -dns=[]: /u010702509/docker_cli_4/file/自定义容器的DNS服务器
      -v=[]: /u010702509/docker_cli_4/file/创建一个挂载绑定：[host-dir]:[container-dir]:[rw|ro]. 如果容器目录丢失，docker会创建一个新的卷
      -volumes-from="": 挂载容器所有的卷
      -entrypoint="": 覆盖镜像设置默认的入口点
      -w="": 工作目录内的容器
      -lxc-conf=[]: /u010702509/docker_cli_4/file/添加自定义 -lxc-conf="lxc.cgroup.cpuset.cpus = 0,1"
      -sig-proxy=true: 代理接收所有进程信号 (even in non-tty mode)
      -expose=[]: /u010702509/docker_cli_4/file/让你主机没有开放的端口
      -link="": 连接到另一个容器 (name:alias)
      -name="": 分配容器的名称，如果没有指定就会随机生成一个
      -P=false: Publish all exposed ports to the host interfaces 公布所有显示的端口主机接口
    

docker会先创建一个新的可写的容器层来指定镜像，通过指定命令来启动他，docker run相当于运行API/containers/create在/container/(id)/start之前

docker run命令可以用结合docker commit来改变正在运行的容器！

### [__](https://code.csdn.net/u010702509/docker_cli_4#%E7%9F%A5%E9%81%93%E9%97%AE%E9%A2%98-run-volumes-from)知道问题(run -volumes-from)

Issue 2702: “lxc-start: Permission denied - failed to mount” 这是AppArmor一个权限的问题，请参见问题解决方案
    
    $ sudo docker run -cidfile /tmp/docker_test.cid ubuntu echo "test"
    

这个命令将会创建一个容器，并且在终端打印'test',cidfile参数使docker试图穿件一个文件并且写入容器的id,如果这个文件存在，docker就会返回一个错误，如果docker runexists,docker就会关闭这个文件！
    
    $ sudo docker run -t -i -rm ubuntu bash
    root@bc338942ef20:/# mount -t tmpfs none /mnt
    mount: permission denied
    

这条命令没有执行，因为在默认情况下，大多数有潜在危险内核命令都被舍弃，包括capsysadmin(需要挂载文件系统)，然而 -privileged参数会让其执行
    
    $ sudo docker run -privileged ubuntu bash
    root@50e3f57e16e6:/# mount -t tmpfs none /mnt
    root@50e3f57e16e6:/# df -h
    Filesystem      Size  Used Avail Use% Mounted on
    none            1.9G     0  1.9G   0% /mnt
    

-privileged参数给容器所有功能，同时它也强迫升级了所有设备的执行权限，换句话说，这个容器已经有了所有主机能做的事情，这个参数的存在允许特殊的用例，像docker内运行docker
    
    $ sudo docker  run -w /path/to/dir/ -i -t  ubuntu pwd
    

-w 让命令在在目录/path/to/dir内运行,如果路径不存在创建，在容器外运行

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c45.png)
    
    $ sudo docker  run  -v `pwd`:`pwd` -w `pwd` -i -t  ubuntu pwd
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c46.png)

-v参数安装当前工作目录到容器中，-w参数让命令在当前目录执行，通过pwd改变目录的返回值，这种组合命令使容器在当前目录下中心命令！
    
    $ sudo docker run -p 127.0.0.1:80:8080 ubuntu bash
    

容器的8080端口绑定在主机127.0.0.1的80端口。端口重定向解释了docker的端口是如何工作的
    
    $ sudo docker run -expose 80 ubuntu bash
    

让主机系统开放80端口供容器连接
    
    $ sudo docker run -name console -t -i ubuntu bash
    

这会创建一个新的容器并且运行bash，容器的名字是console

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c47.png)
    
    $ sudo docker run -link /redis:redis -name console ubuntu bash
    

-link表示会将连接一个名为/redis的容器到新建的一个容器别名叫做redis,新的容器可以通过redis容器的环境变量访问网络和环境， -name参数会将名字console分配给新建的容器！

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c48.png)
    
    $ sudo docker run -volumes-from 777f7dc92da7,ba8c0c54f0f2:ro -i -t ubuntu pwd
    

-volumes-from 参数将从引用容器中挂载所有定义的卷. 容器引用列表可以用逗号分隔或者重复的引用 -volumes-from参数. 容器ID可以用:ro或者:rw来作为后缀挂载卷来标识只读或者读写权限，默认情况下，挂载的权限(读写权限)和引用容器的权限相同.

### [__](https://code.csdn.net/u010702509/docker_cli_4#%E4%B8%80%E4%B8%AA%E5%AE%8C%E6%95%B4%E7%9A%84%E4%BE%8B%E5%AD%90)一个完整的例子
    
    $ sudo docker run -d -name static static-web-files sh
    $ sudo docker run -d -expose=8098 -name riak riakserver
    $ sudo docker run -d -m 100m -e DEVELOPMENT=1 -e BRANCH=example-code -v $(pwd):/app/bin:ro -name app appserver
    $ sudo docker run -d -p 1443:443 -dns=dns.dev.org -v /var/log/httpd -volumes-from static -link riak -link app -h www.sven.dev.org -name web webserver
    $ sudo docker run -t -i -rm -volumes-from web -w /var/log/httpd busybox tail -f access.log
    

这个实例展示了5个容器设置测试一个web应用程序的改变：

后台运行一个已经准备好的镜像卷 static-web-files，镜像里含有CSS,images,和静态html, (Dockerfile中的卷指令允许web server使用这些文件);

运行一个预先准备好的riakserver,给这个容器命名为riak,并且开放8098端口让其它镜像可以连接它！

give the container name riak and expose port 8098 to any containers that link to it;

运行一个appserver镜像，限制它的内存时100MB，设置两个环境变量DEVELOPMENT和BRANCH，并且依只读模式将本地文件pwd的目录挂载绑定到你容器的/app/bin下

运行webserver,将容器443端口映射到外部的docker主机的1443端口，设置DNS服务dns.dev.org，创建一个卷来存放日志文件(所以我们可以从另外的主机访问它)，导入静态文件从开放的8098端口，从static容器，连接riak和app的所有开放端口，最后我们设置hostname web.sven.dev.org,与生成的SSL证书一致.

最终，我们创建了一个容器，可以运行tail -f access.log来查看web容器的卷日志，设置工作目录是/var/log/httpd. -rm表示的意思是如果这个容器存在，我们就删除容器的层

### [__](https://code.csdn.net/u010702509/docker_cli_4#save)save
    
    Usage: docker save image > repository.tar
    
    通过标准输出流存储镜像包含所有父层的、tag、version
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#search)search
    
    Usage: docker search TERM
    
    搜索docker 镜像
    
     -notrunc=false: 不截断输出
     -stars=0: 仅仅输出带有lastest标记的镜像
     -trusted=false: 只显示被信任的构建
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#start)start
    
    Usage: docker start [OPTIONS] CONTAINER
    
    启动一个停止的容器
    

-a=false: 附加标准输入和输出，并转发所有进程信号 -i=false: 附加标准输入

### [__](https://code.csdn.net/u010702509/docker_cli_4#stop)stop
    
    Usage: docker stop [OPTIONS] CONTAINER [CONTAINER...]
    
    关闭一个容器
    
      -t=10: 等待停止容器需要的时间
    

这个容器主进程接收终止进程信号，在一段时间内，终止

### [__](https://code.csdn.net/u010702509/docker_cli_4#tag)tag
    
    Usage: docker tag [OPTIONS] IMAGE REPOSITORY[:TAG]
    
    给镜像仓库添加标签
    
      -f=false: Force
    

### [__](https://code.csdn.net/u010702509/docker_cli_4#top)top
    
    Usage: docker top CONTAINER [ps OPTIONS]
    
    查看容器内运行的进程
    

![](http://7q5cfr.com1.z0.glb.clouddn.com//docker-cli/c49.png)

### [__](https://code.csdn.net/u010702509/docker_cli_4#version)version

显示docker的版本和最后更新版本信息

### [__](https://code.csdn.net/u010702509/docker_cli_4#wait)wait
    
    Usage: docker wait [OPTIONS] NAME
    
    等待容器停止，打印出退出编码
