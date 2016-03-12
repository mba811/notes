---
layout: post
title: 在 Mac OSX 上安装 gentoo-prefix
tags: [osx]
---

## 什么是Ports系统？

众所周知Mac OSX是一个UNIX操作系统。UNIX操作系统的认证是Open Group给的，只有经过他们认证之后才能自称自己是UNIX操作系统并使用UNIX的商标（毕竟人家这是注册商标，不能随便用）。

得益于UNIX标准的建立，越来越多在Linux上开发的开源软件已经可以直接在UNIX下编译安装而不需要考虑跨平台问题，这意味着在Mac OSX上也可以享受绝大多数Linux下开源软件的最新版本。

注意到，我这里提到的是“编译安装”。即使你是一个对编译安装感觉很平常的Linuxer，在Mac下编译安装也不是一件简单的事情，因为你需要自行解决一大堆依赖。有多复杂，你可以尝试安装一次LFS。虽然Linux下常见的包管理系统Mac下没有，但是Mac上有相对于包管理系统来说更加强大的Ports系统（IMO），让这些软件包的安装过程自动化了。

Ports系统的历史还真不好找，从wiki获得的信息是，这是早期BSD系统就开始有的一套比较“原始”的包管理系统：自动从官方网站下载源码包、解压、打补丁、编译安装，并维护各个软件包之间的依赖关系。使用Ports系统，可以通过自动化方式从源码安装软件，而无需考虑其背后的繁琐操作。

除了BSD系统上的Ports系统之外，Linux下常见的Ports系统有：Gentoo Portage和ArchLinux ABS。在Mac OSX上也有多个Ports系统可以选择，比较有名的有`MacPorts`和`HomeBrew`。`Gentoo-prefix`反而是一个不太常用的Mac下的Ports系统。

## 为什么推荐Gentoo-prefix呢？

`Gentoo-prefix`使用的portage系统fork自Gentoo Portage，里面的ebuild文件是针对prefix环境专门修改过的。

上面提到了，Gentoo-prefix是一个不太常用的Ports系统。那么为什么要推荐这个Ports系统呢？

稍微列了一下Gentoo-prefix的优势：

  * 有Gentoo这个比较知名的Linux发行版做靠山，软件包的兼容性由一个庞大的社区保证
  * 软件包类别全面（看了一下好像还是MacPorts里的包多…就不夸耀Gentoo-prefix的包多了…）
  * 出现问题的时候，找个Gentoo-er基本可以帮你搞定问题，不需要另一个Mac用户

由于其实我没用过MacPorts，所以更加具体的优势我真说不上来…嗯那就这样了，下一个问题就是Gentoo-prefix的安装使用了。

## Gentoo-prefix 安装使用

Gentoo官方提供了安装Gentoo-prefix到Mac的详细方法，英文过关的同学请自行参考英文原文：[http://www.gentoo.org/proj/en/gentoo-alt/prefix/bootstrap-macos.xml](http://www.gentoo.org/proj/en/gentoo-alt/prefix/bootstrap-macos.xml)

简单地介绍一下大致的步骤： 安装过程需要在终端中进行。打开_"应用程序"->"实用工具"->"终端"_（如果你有其他常用的终端，自行选择就可以）。

首先需要定义一个EPREFIX环境变量。后续所有的ebuild文件及通过Gentoo-prefix安装好的程序都会被安装到EPREFIX定义的目录下。假设这个目录是~/Gentoo，在终端里执行：
    
    export EPREFIX="$HOME/Gentoo"
    

然后将该目录下的bin文件夹位置添加到PATH里，方便后面的bootstrap动作。
    
    export PATH="$EPREFIX/usr/bin:$EPREFIX/bin:$EPREFIX/tmp/usr/bin:$EPREFIX/tmp/bin:$PATH"
    

准备工作已经完成，下面就是枯燥的敲命令了。以下代码可以直接复制粘贴到终端里批量执行：
    
    curl http://overlays.gentoo.org/proj/alt/browser/trunk/prefix-overlay/scripts/bootstrap-prefix.sh?format=txt -o bootstrap-prefix.sh
    chmod 755 bootstrap-prefix.sh
    ./bootstrap-prefix.sh $EPREFIX tree
    ./bootstrap-prefix.sh $EPREFIX/tmp make
    ./bootstrap-prefix.sh $EPREFIX/tmp wget
    ./bootstrap-prefix.sh $EPREFIX/tmp sed
    ./bootstrap-prefix.sh $EPREFIX/tmp python
    ./bootstrap-prefix.sh $EPREFIX/tmp coreutils6
    ./bootstrap-prefix.sh $EPREFIX/tmp findutils
    ./bootstrap-prefix.sh $EPREFIX/tmp tar15
    ./bootstrap-prefix.sh $EPREFIX/tmp patch9
    ./bootstrap-prefix.sh $EPREFIX/tmp grep
    ./bootstrap-prefix.sh $EPREFIX/tmp gawk
    ./bootstrap-prefix.sh $EPREFIX/tmp bash
    ./bootstrap-prefix.sh $EPREFIX portage
    # 以上已经安装好基本portage，下面是安装基本命令
    emerge --oneshot sed
    emerge --oneshot --nodeps bash
    emerge --oneshot pax-utils
    emerge --oneshot --nodeps wget
    emerge --oneshot --nodeps baselayout-prefix
    emerge --oneshot --nodeps xz-utils
    emerge --oneshot --nodeps m4
    emerge --oneshot --nodeps flex
    emerge --oneshot --nodeps bison
    emerge --oneshot --nodeps binutils-config
    emerge --oneshot --nodeps binutils-apple # 注意，这里根据官方文档，需要判断系统内gcc版本。不过我估计现在Snow Leopard都是gcc 4.2.1以上了，直接写这个地址了。如果出现问题，请参考官方文档的说明
    emerge --oneshot --nodeps gcc-config
    emerge --oneshot --nodeps gcc-apple
    emerge --oneshot coreutils
    emerge --oneshot findutils
    emerge --oneshot tar
    emerge --oneshot grep
    emerge --oneshot patch
    emerge --oneshot gawk
    emerge --oneshot make
    emerge --oneshot --nodeps file
    emerge --oneshot --nodeps eselect
    env FEATURES="-collision-protect" emerge --oneshot portage
    rm -Rf $EPREFIX/tmp/*
    hash -r
    emerge --sync
    USE=-git emerge -u system
    

接下来要编辑`make.conf`文件。`make.conf`是Portage系统的控制文件，在这里可以对软件包编译时使用的参数进行控制。另外还有一个比较重要的，在`make.conf`里可以加上恰当的CPU类型参数，让gcc编译时根据CPU类型进行优化，提高性能。

目前常见的CPU类型有：

  * CoreDuo: 对应`-march=prescott`
  * CoreDuo2:对应`-march=nocona`
  * i3,i5,i7: 我的make.conf里写的是`-march=prescott`，不知道是不是我写错了…现在一会儿也查不到…

执行以下命令编辑`make.conf`文件，其中`${my_cpu_flags}`用`-march=XXX`替换
    
    echo 'USE="unicode nls"' >> $EPREFIX/etc/make.conf
    echo 'CFLAGS="-O2 -pipe ${my_cpu_flags}"' >> $EPREFIX/etc/make.conf
    echo 'CXXFLAGS="${CFLAGS}"' >> $EPREFIX/etc/make.conf
    

最后执行
    
    emerge -e system
    

使用Gentoo-prefix中的gcc对system这个软件包集合进行重编译。编译完成后，整个安装过程就完成了

安装完成后，执行
    
    cd $EPREFIX/usr/portage/scripts
    ./bootstrap-prefix.sh $EPREFIX startscript
    

得到一个用于进入prefix环境的脚本，位于`~/Gentoo/startprefix`中。

通过Gentoo-prefix安装软件非常简单，下面以pygtk为例：
    
    sh ~/Gentoo/startprefix
    emerge pygtk
    

## 优化设置

如果你希望在打开终端的时候直接使用prefix过的环境，可以通过以下步骤进行（假设你的prefix环境中的bash在`/Users/yegle/Gentoo/bin/bash`）：

  1. 将prefix环境中的bash加入到`/etc/shells`中。使用`sudo vi /etc/shells`进行编辑，在最后一行加上prefix环境中bash的完整路径（例如：`/Users/yegle/Gentoo/bin/bash`）
  2. 更换用户的默认shell。执行`chsh -s /Users/yegle/Gentoo/bin/bash`，输入你的密码保存设置
  3. 编辑`~/.bash_profile`文件，加入两行：`export EPREFIX="$HOME/Gentoo"`和`export PATH="$EPREFIX/usr/bin:$EPREFIX/bin:$EPREFIX/tmp/usr/bin:$EPREFIX/tmp/bin:/sbin:$PATH"`。

重新打开终端程序，你会发现shell的提示符颜色已经变成绿色了，并且可以直接在这个环境里执行emerge程序进行程序安装了

## Gentoo-prefix常见命令

以下是一些小技巧，方便对Gentoo-prefix的使用：

### 对本地Portage树进行更新
    
    emerge --sync
    

### 对通过portage安装的所有软件进行升级
    
    emerge --deep --newuse --update world
    

### 使用KEYWORD从portage中搜索软件包
    
    # 需要先安装eix
    eix KEYWORD
    

### 更新本地Portage树
    
    # 效果等同于emerge --sync，但是可以在执行完毕后告诉你哪些包可供升级
    eix-sync
    

### 列出PKGNAME软件的文件
    
    # 需要先安装gentoolkit
    equery file PKGNAME
