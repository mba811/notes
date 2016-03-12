---
layout: post
title: 在 OS X 上安装 Docker 管理工具 Panamax
tags: [docker]
---

目前 Docker 很火，但是管理 Docker 容器却是一个难题，幸好有公司帮我们解决了这个问题，Panamax 就是其中很优秀的一个解决方案。为了能尝鲜，体验下 Panamax 的强大功能，于是准备在 OS X 下面安装一个玩玩。

## 安装 Homebrew 和 Homebrew Cask

1.安装 Homebrew
    
    ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
    

> 注：如果已经安装 Homebrew 的，这一步可以忽略

2.安装 Homebrew Cask
    
    brew install caskroom/cask/brew-cask
    

## 安装 VirtualBox 和 Vagrant

1.安装 VirtualBox
    
    brew cask install virtualbox
    

2.安装 Vagrant
    
    brew cask install vagrant
    

## 安装和初始化 Panamax

1.安装 Panamax
    
    brew install http://download.panamax.io/installer/brew/panamax.rb
    

2.初始化 Panamax
    
    panamax init
    

最后，你可以看到如下信息：

![请输入图片描述](http://segmentfault.com/img/bVcXGx)  
![请输入图片描述](http://segmentfault.com/img/bVcXGD)

3、访问 Panamx

可以看到如下：

![请输入图片描述](http://segmentfault.com/img/bVcXGZ)

> 注：看文章说，初始化完成，会自动在你的浏览器中打开，端口是 8888 ，但是囧，我的已经初始化好久了，还没有打开，有可能和网络有关  
注：其下载的一些东西可能在墙外，所以有些下载不了，请默认翻墙模式

悲剧的问题：

在出现如下：
    
    ==> panamax-vm: docker pull google/cadvisor:0.1.0
    

之后，一直给我刷新这个，大概刷新 10-20 分钟左右
    
    ==> panamax-vm: .
    ==> panamax-vm: .
    ==> panamax-vm: .
    ==> panamax-vm: .
    ==> panamax-vm: .
    ==> panamax-vm: .
    ==> panamax-vm: .
    

## panamax 命令介绍

  * panamax init - 这个是初始化命令
  * panamax up - 这个命令用于是的 Vagrant 机器运行 Panamax up 。用于启动 Panamax，你可以在重起你的电脑后使用
  * panamax stop - 这个命令是用于停止 Vagrant VM
  * panamax restart - 这个命令用于重起 panamax
  * panamax reinstall - 如果你对你现有的 Panamx VM 不满意的话，你可以使用这个命令重新安装。其首先会先删除你的应用程序和 CoreOS Vagrant VM 然后重装它们。你可以使用如下或是类似命令，指定 VM 的内存：
    
    panamax reinstall --memory=3072 
    

## 遇到的问题

在进行 panamx init 启动 VirtualBox 的时候，会出现如下报错信息：
    
    "Failed to load VMMR0.r0 (VERR_SUPLIB_WORLD_WRITABLE)"
    

这是因为没有权限导致的，解决办法是：
    
    #sudo chmod 755 /Applications 
    #sudo chmod 755 /Applications/Virtualbox.app
    

## 参考资料

  * [Simple how-to install Panamax Docker management on OS X](http://purevirtual.eu/2014/08/12/simple-how-to-install-for-panamax-docker-management-on-os-x/)
  * [Getting Started with Panamax: Creating an Internet-Accessible App](https://www.andrewmunsell.com/blog/getting-started-with-panamax)
