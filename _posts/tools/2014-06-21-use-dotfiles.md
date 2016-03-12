---
layout: post
title: Dotfiles 备份与同步配置文件
tags: [tools]
---

由于手贱玩Yosemite，重装了次系统后发现,应用什么的很容易就找回来了，但是对于各种配置文件来说就没有那么容易了，以前的`.bash_profile`,`.vimrc`等等又要重新配置重新安装各种插件了，真心很是麻烦。后来发现有个叫[dotfiles](http://dotfiles.github.io/)的项目，旨在将这些配置文件集中到一起惊醒版本管理和云储存，然后在新的电脑中连接这些文件，使得他们和你原来电脑使用起来基本一样。

其最为明显的优点有：

  1. 可以备份的同时，在多台电脑间同步你的设置
  2. 将同一个应用的所有配置文件放在一起，可以很方便的进行版本控制
  3. 方便分享

## 同步与使用

大部分的dotfiles使用者都会选择[DropBox](http://fatestigma.github.io/blog/2014/08/02/use-dotfiles/www.dropbox.com)来作为他们同步软件，但是由于国情，很明显这对于我们并不是一个很好的选择。我个人以前非常偏好使用金山快盘，但是金山快盘不会同步`.`开头的文件，所以弃用了，经过各种测试，发现[百度云](http://pan.baidu.com/)好像是支持`.`开头文件的同步。所以目前就先用[百度云](http://pan.baidu.com/)了；当然不仅仅是使用同步工具来同步，另外还使用git来对我的配置文件进行集中地版本控制，最后可以放在GitHub上与他人分享。

首先说下我的总文件夹名称和大家选用一样的`.dotfiles`（我想当初叫这个名字主要也是大部分配置文件都是以点开头的），然后将`.dotfiles`放在云盘中，并采用软连接放入`~/`中，最后再通过各种软连接来将里面的各个配置文件放到原来的位置。

## 内容

首先将各个配置文件归类放在`.dotfiles`文件夹下，
	
	.dotfiles
	├── .git
	├── .gitignore
	├── install.sh	# 对所有配置文件的软连接
	├── README.md
	├── config		# 各种小型的配置文件
		├── gitconfig
		├── gitignore_global
		└── ssh
	├── osx			# 放置对Mac电脑的偏好设置修改文件
		└── osx_config.sh
	├── script 		# 放置几个安装脚本
		├── homebrew_install.sh
		└── python_install.sh
	├── vim			# 放置我的`.vimrc`等和Vundle的bundle
		├── vimrc
		└── bundle
	└── zsh			# 放置zsh和oh-my-zsh的配置文件
		└── zshrc
	

将各个配置文件归类后，并且添加一个`install.sh`来将所有的配置文件软连接归为：
	
	# vim
	ln -s ~/.dotfiles/vim ~/.vim
	ln -s ~/.dotfiles/vim/vimrc ~/.vimrc
	
	# for oh-my-zsh
	ln -s ~/.dotfiles/zsh/zshrc ~/.zshrc
	
	# for config files
	ln -s ~/.dotfiles/config/gitconfig ~/.gitconfig
	ln -s ~/.dotfiles/config/gitignore_global ~/.gitignore_global
	ln -s ~/.dotfiles/config/ssh ~/.ssh
	

另外通过使用`osx_config.sh`来配置一台新的Mac的偏好设置，通过`homebrew_install.sh`来安装之前的电脑brew过的应用，通过`python_install.sh`来安装之前安装过的库，同时也可以看出这三个文件需要定时的管理和维护。

`.gitignore`文件中主要记录不想被同步到GitHub上的内容，其中主要就是`ssh`文件夹下的内容了。
	
	# privacy in ssh
	config/ssh

>在V2EX的帖子「[__菜鸟如何入门dotfiles管理？](https://v2ex.com/t/70242)」，看看怎么理解dotfiles。

#### 菜鸟如何入门dotfiles管理？

前几个月就知道dotfiles这个东西，可是怎么都玩不会，在这里请教一下大家。

1.**dotfiles是什么？**

我自己的理解:linux下（mac下）有各种app，每个人会根据自己的喜好和习惯来设置一些（快捷键，变量等等），而dotfiles就是保存了这些自定义设置的文件，如果换一台电脑，只要你备份了dotfiles文件，一样可以快速回归到自己熟悉的设置。

2.**如何使用dotfiles?**

我自己的理解：a.在系统中使用一个文件夹，通过ln命令，将不同的app，不同的系统设置文件都指引到这个文件夹，这样就可以在这个文件夹管理所有的系统app setting了？。

3.**进阶**   
既然都统一到了一个文件夹，那么，就可以通过git，dropbox来进行备份，分享，也可以clone下其他人的dotfiles。

不知道自己的理解有没有错，话说中文基本没有入门教程，英文也就是github的那个和一些使用dropbox来备份的。

#### 理解ln命令

dotfiles 这东西，翻译过来就是「点文件」，就是以`.`开头的文件，常见的如`.bash_profile`,`.vimrc`,`.zshrc`等等。在linux,osx等系统中，这些dotfiles默认都是隐藏的，在shell下要看到这些文件得用`ls -a`命令。像程序猿们，常用到vim, sublime text2, iterm ,zsh 等等相关的工具和开发环境，为了用得顺手，也会根据个人习惯和喜好改改不同工具、环境的配置。

这些点文件，就是我们今天谈的dotfiles，一般来说，配置好了，也不常改，可以万一换了个机器或者坏了个机器，如果要重新配置自己的配置文件，那就蛋疼了。所以，大家应该要会利用好、管理好好自己机器的dotfiles，放到改放的地方，做好备份。

要管理好dotfiles文件，首先得了解一个linux下的命令`ln`。 ln命令是linux下一个十分重要也有用的命令，她可以为一个文件在另外一个地方建立一个同步的链接，类似于快捷键。

`ln [参数][源文件或目录][目标文件或目录]`

在我们备份dotfiles中常用的参数有`ln -s`软链接，s是代号symbolic的意思，所谓软链接，她只会在你选定的位置上生成一个镜像，而不会占用磁盘空间，而如果使用`ln`不带参数的话，则就是硬链接，会在选定的位置上生成一个和源文件大小相同的文件，占用磁盘空间。

#### 使用git备份

接下来就是管理自己的dotfiles了，首先找个地方创建一个文件
	
	mkdir dotfiles //创建一个dotfiles的文件夹放在你喜欢的地方   
	cd dotfiles
	pwd //看看文件夹位置
	/Users/luolei/Dropbox/dotfiles  //我放在dropbox里
	git init //创建git
	

下面来把系统里的那些dotfiles软连接到这个文件夹
	
	cd ~/ 去到根目录
	mv .vimrc ~/Users/luolei/Dropbox/dotfiles/vimrc
	mv .zshrc  ~/Users/luolei/Dropbox/dotfiles/zshrc
	ln -s ~/Users/luolei/Dropbox/dotfiles/vimrc .vimrc
	ln -s ~/Users/luolei/Dropbox/dotfiles/zshrc .zshrc
	

以上举例软链接了.vimrc , .zshrc到dotfiles，其他的文件类似。软链接做好了后，接下来，你可把这些推到github上（首先要在github上创建一个repo），记得添加一个`.gitignore`。
	
	cd ~/Users/luolei/Dropbox/dotfiles/
	git add .
	git commit -m '创建dotfils'
	git remote add origin git@github.com:username/dotfiles.git
	git push -u origin master
	

好啦，备份完了，现在，由于你努力学习工作，终于神舟换IBM，mini换rmbp，要恢复下dotfiles。
	
	git clone git@github.com:username/dotfiles.git dotfiles
	rm -rf .vimrc .zshrc //首先删除自身机器上原有的dotfiles
	ln -s dotfiles/vimrc .vimrc
	ln -s dotfiles/zshrc .zshrc
	

dotfiles管理核心理念就是上面所说的，几本掌握好`ln`命令和git（或者其他备份方法）就好，大家可以参考一下[__dotfiles.github.io](http://dotfiles.github.io/)上分享的大神的dotfiles，其实如果大家追求更加自动化，可以自己写`.sh`脚本把什么软链接都自动化。比如说paulmillr分享的[__dotfiles](https://github.com/paulmillr/dotfiles) .

咳咳，最后再放一个自己的[__dotfiles](https://github.com/foru17/luolei-dotfiles)，请忽视吧。