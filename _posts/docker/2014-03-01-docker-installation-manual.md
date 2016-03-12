---
layout: post
title: Docker 安装手册
tags: [docker]
---

##Docker支持的安装方式

**简介**：Docker有很多种安装的选择，其中支持最好的是Ubuntu系统。Docker有很多种安装的选择，我们推荐您在Ubuntu下面安装，因为docker是在Ubuntu下面开发的，安装包测试比较充分，可以保证软件包的可用性。Mac, windows和其他的一些linux发行版本无法原生运行Docker，可以使用虚拟软件创建一个ubuntu的虚拟机并在里面运行docker。

## Ubuntu下面安装docker

###Ubuntu Precise 12.04 (LTS) (64-bit)下面安装docker

**简介**：本篇文章介绍如何在ubunu 12.04版本下面安装docker

  * [内核要求：](http://www.docker.org.cn/book/install/20_install-docker-under-ubuntu-precise.html#0)
  * [安装Docker：](http://www.docker.org.cn/book/install/20_install-docker-under-ubuntu-precise.html#1)

#### 内核要求：

由于LXC的一个bug，Docker在3.8内核下面运行最佳。Ubuntu的Precise版本内置的是3.2版本的内核，因此我们首先需要升级内核。安装下面的步骤可以升级到3.8内核，并内置AUFS的支持。同时还包括了通用头文件，这样我们就可以激活依赖于这些头文件的包，比如ZFS，VirtualBox的增强功能包。
    
    # install the backported kernel
    sudo apt-get update
    sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
    
    # reboot
    sudo reboot

#### 安装Docker：  


Docker有deb格式的安装包，安装起来非常的容易。首先添加Docker库的密钥。
    
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

然后把Docker的库添加到apt的源列表中，更新并安装lxc-docker包。
    
    sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\
    > /etc/apt/sources.list.d/docker.list"
    sudo apt-get update
    sudo apt-get install lxc-docker

安装过程中会有一个警告信息，输入"yes"继续安装即可。安装成功之后，可以下载ubuntu镜像并启动一个镜像来验证安装是否正常。
    
    sudo docker run -i -t ubuntu /bin/bash

 成功运行之后，输入exit退出即可。


###Ubuntu Raring 13.04 和 Saucy 13.10 (64 bit)下面安装docker

**简介**：本篇文章讲述如何在Ubuntu Raring 13.04 和 Saucy 13.10 (64 bit)下面安装docker。

  * [激活AUFS文件系统支持](http://www.docker.org.cn/book/install/21_install-docer-under-ubuntu-Raring-and-Saucy.html#0)
  * [安装](http://www.docker.org.cn/book/install/21_install-docer-under-ubuntu-Raring-and-Saucy.html#1)

#### 激活AUFS文件系统支持

Ubuntu的Raring版本内置了3.8版本的内核，因此我们不需要安装AUFS系统。但是并不是所有的系统都激活了AUFS系统的支持。Docker 0.7版本中AUFS的支持是可选项，以驱动的方式存在，如果可能的话，我们还是建议使用AUFS系统。使用下面的命令来确保AUFS安装成功。
    
    sudo apt-get update
    sudo apt-get install linux-image-extra-`uname -r`

#### 安装

Docker有Debian包，所以安装起来非常的方便。  
警告：请注意，从0.6版本开始安装步骤已经发生了变化，如果你是从比较早的版本升级上来，还需要执行下面的命令。  
  


首先把Docker库的密钥添加到本地。
    
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

把Docker库添加到apt的源列表中，执行update命令，然后安装lxc-docker包。  

    
    sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\
    > /etc/apt/sources.list.d/docker.list"
    sudo apt-get update
    sudo apt-get install lxc-docker

现在来确认安装是否成功，可以下载ubuntu镜像并启动一个容器。
    
    sudo docker run -i -t ubuntu /bin/bash 

