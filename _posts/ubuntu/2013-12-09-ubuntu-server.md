---
layout: post
title: Ubuntu Server - Dockr
tags: [ubuntu]
---

研究[docker](http://docker.com/)这一轻量级容器引擎，研究docker对日常开发测试工作以及产品部署运维工作能带来哪些便利。前些时候刚刚将工作环境从 [Ubuntu](http://tonybai.com/tag/ubuntu)搬到了Mac Air上，对Mac OS X的一切均不甚熟悉，给docker研究带来了不便，于是打算在[VirtualBox](http://virtualbox.org/)中安装一Ubuntu Server作为docker之承载平台。这里记录一下安装配置过程，主要为了备忘，如果能给其他人带来帮助，我会甚感欣慰。

docker官方对[ubuntu](http://ubuntu.com/)的支持是蛮好的。docker对Linux内核版本有要求，要>=3.8，Ubuntu Server目前最新版本14.04.1恰符合这一要求，其kernel version = 3.13.0-32。

**一、VirtualBox安装Ubuntu Server 14.04.1**

VirtualBox安装Ubuntu OS做过了不止一遍，即便是换成最新的14.04.1 Server版，差别也没有太多，无非是按照安装提示，逐步Next。这里给Ubuntu Server 14.04分配了1G Memory, 32G动态硬盘空间。

【配置源】

  默认情况下，/etc/apt/sources.list中只有一组源：cn.archive.ubuntu.com/ubuntu。这个国外源的下载速度显然无法满足我的要求，于是我把我常用的sohu源加入sources.list中，并且放在前面：

  deb http://mirrors.sohu.com/ubuntu/ trusty main restricted  
  deb http://mirrors.sohu.com/ubuntu/ trusty-security main restricted  
  deb http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted  
  deb http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted  
  deb http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted

  deb-src http://mirros.sohu.com/ubuntu/ trusty main restricted  
  deb-src http://mirrors.sohu.com/ubuntu/ trusty-security main restricted  
  deb-src http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted  
  deb-src http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted  
  deb-src http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted

  公司采用代理访问外网，于是还得在/etc/apt/apt.conf中加上代理的设置，否则无法更新源，也就无法安装第三方软件：

  Acquire::http::Proxy "http://username:passwd@proxyhost:proxyport";

 【乱码处理】

  由于安装时候选择了中国区域（locale zh_CN.UTF-8），因此在VirtualBox的窗口中直接执行命令的提示信息可能是乱码。对于Server，我们一般是不会直接通过其主机显示 器登录使用的，都是通过终端访问，但在未安装和开启ssh服务和未配置端口转发前，我们只能先凑合这个窗口了。可先将/etc/default /locale中的LANGUAGE由"zh_CN:zh"改为"en_US:en"， logout后重新登录就可以看到非乱码的英文提示信息了。

【安装VirtualBox增强组件】

  Ubuntu Server默认是不安装图形桌面的，只有一个命令行窗口，连鼠标都无法使用。因此增强组件安装的意义没有桌面系统那么强烈。我能想到的只有“共享目录”这一个功能有些用处。

  安装方法也不难，按下面步骤逐步操作即可：

  sudo apt-get install build-essential linux-headers-$(uname -r) dkms gcc g++  
  sudo mnt /dev/cdrom /mnt  
  cd /mnt  
  sudo bash ./VBoxLinuxAdditions.run

  如果结果都是"done"，重启后就ok了。

【安装ssh服务】

    ssh服务由openssh-server提供：  
    sudo apt-get openssh-server  
     
   安装成功后，ssh server服务就会自动启动起来。

   不过我们还是需要修改一些配置，比如允许Root登录：打开/etc/ssh/sshd_config，将PermitRootLogin后面的内容改为yes。  
     
【设置端口转发】

  前面说过，对于Server，我们更多是在其他主机上通过ssh或telnet远程访问该Server并执行各种操作。由于这里是VirtualBox安 装的虚拟机，其他主机无法看到这台Server，我们需要设置端口转发将外部访问的数据转发给这个内部虚拟Server。

  我们通过VirtualBox软件提供的图形界面即可完成这个操作：  
    1、“设置”这个虚拟机  
    2、在“网络”标签中，点击“端口转发”按钮，进入端口转发规则添加窗口。  
    3、添加一条规则：  
          名称：ssh-rules  
          协议：TCP  
          主机IP、子系统IP可以为空。  
          主机端口：2222  
          子系统端口：22  
   4、配置结束

    配置结束后，我们在宿主机上netstat -an|grep 2222，可以看到VirtualBox增加了该端口2222的监听。

  现在我们就可以在其他机器上通过ssh -l tonybai 宿主机ip -p 2222的方式登录到我们新安装的这台虚拟Server了。

    
**二、安装docker**

docker目前的最新版本号是1.2.0，但14.04源中的docker还是正式稳定版1.0之前的版本，显然这是无法满足我的要求的。我们只能另外添加docker源来安装最新版docker。

  【安装docker】

    我们在/etc/apt/sources.list中加入下面这个源：  
       deb [http://mirror.yandex.ru/mirrors/docker/](http://mirror.yandex.ru/mirrors/docker/) docker main  
    
    执行apt-get update。

    sudo apt-get install lxc-docker

正在读取软件包列表… 完成  
正在分析软件包的依赖关系树        
正在读取状态信息… 完成        
将会安装下列额外的软件包：  
  aufs-tools cgroup-lite git git-man liberror-perl lxc-docker-1.2.0  
建议安装的软件包：  
  git-daemon-run git-daemon-sysvinit git-doc git-el git-email git-gui gitk  
  gitweb git-arch git-bzr git-cvs git-mediawiki git-svn  
下列新软件包将被安装：  
  aufs-tools cgroup-lite git git-man liberror-perl lxc-docker lxc-docker-1.2.0  
升级了 0 个软件包，新安装了 7 个软件包，要卸载 0 个软件包，有 59 个软件包未被升级。  
需要下载 7,477 kB 的软件包。  
解压缩后会消耗掉 35.4 MB 的额外空间。  
您希望继续执行吗？ [Y/n] y

  这个源里的docker居然是最新版。于是安装之。安装后，我们执行docker version来确认一下安装是否成功。

  tonybai@ubuntu-Server-14:~$ docker version  
Client version: 1.2.0  
Client API version: 1.14  
Go version (client): go1.3.1  
Git commit (client): fa7b24f  
OS/Arch (client): linux/amd64  
2014/09/26 13:56:53 Get http:///var/run/docker.sock/v1.14/version: dial unix /var/run/docker.sock: permission denied

  【为docker设置http代理】

    在公司内使用代理才能访问到外网，于是我们也需要为docker命令设置代理以使其顺利执行命令。

    我们安装的docker实际上分为两部分，docker命令行和docker daemon。两者是C/S结构，docker命令行将用户的请求转发给docker daemon，后者会真正与外部通信完成各种操作。

    于是我们可以这样为docker daemon设置http_proxy:  
    sudo service docker stop  
    sudo http_proxy='[http://user:passwd@proxyhost:port](http://baim:xxx@proxy.neusoft.com:8080/)' docker -d &

    这样设置启动后，我们可以通过下面命令测试设置是否ok：

      sudo docker search ubuntu

    如果你看到下面信息，说明设置成功了：

    tonybai@ubuntu-Server-14:~$ sudo docker search ubuntu  
[info] GET /v1.14/images/search?term=ubuntu  
[b36518a9] +job search(ubuntu)  
[b36518a9] -job search(ubuntu) = OK (0)  
NAME                                             DESCRIPTION                                     STARS                                   OFFICIAL   AUTOMATED  
ubuntu                                           Official Ubuntu base image                      709                                     [OK]        
dockerfile/ubuntu                                Trusted automated Ubuntu (http://www.ubunt…   24                                                 [OK]  
crashsystems/gitlab-docker                       A trusted, regularly updated build of GitL…   20                                                 [OK]  
ubuntu-upstart                                   Upstart is an event-based replacement for …   13                                      [OK]         
… ….

 

© 2014, [bigwhite](http://tonybai.com/). 版权所有.

Related posts:

  1. [解决BuildBot构建结果mail无法发送的问题](http://tonybai.com/2011/05/31/solve-the-problem-that-buildbot-can-not-send-mail/)
  2. [Ubuntu 12.04修复记](http://tonybai.com/2013/08/07/ubuntu-12-04-repairing-notes/)
  3. [关于Python Package下的Module import方式](http://tonybai.com/2013/01/24/the-module-import-way-under-python-package/)
  4. [再谈C语言位域](http://tonybai.com/2013/05/21/talk-about-bitfield-in-c-again/)
  5. [将Unity换成Gnome3](http://tonybai.com/2012/12/06/replace-unity-with-gnome3/)