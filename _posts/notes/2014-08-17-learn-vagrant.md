---
layout: post
title: Vagrant 介绍
tags: [notes]
---

![](http://7q5cfr.com1.z0.glb.clouddn.com/Vagrant.png)

#介绍

Vagrant可以为你提供可配置、可再生、便携的工作环境，它主要是一个中间层技术，它的下层是VirtualBox, VMware, AWS或者其他[provider](http://docs.vagrantup.com/v2/providers/)，它的上层是[provisioning工具](http://docs.vagrantup.com/v2/provisioning/)，比如shell scripts, Chef, or Puppet等可以自动化安装和配置软件的工具。

#对你有什么用

* 对于开发人员来说，Vagrant可以帮你统一团队成员的开发环境。如果你或者你的伙伴创建了一个[Vagrantfile](http://docs.vagrantup.com/v2/vagrantfile/)，那么你只需要执行```vagrant up```就行了，所有的软件都会安装并且配置好。团队成员可以通过相同的Vagrantfile来创建他们的开发环境，无论他们是在Linux, Mac OS X, 或者Windows下，这样就可以保证你团队成员的代码是跑在相同的环境中，从而避免令人烦躁的【在我的机器上是可以的】问题。

* 对于运维人员来说，Vagrant可以给你提供一次性，并且与线上一致的服务器环境，你可以利用VirtualBox或者VMware来测试你的shell scripts, Chef cookbooks, Puppet modules等管理脚本。你不需要再苦逼的登录到线上服务器提心吊胆的测试了，Vagrant可以解救你。

* 对于设计人员来说，Vagrant可以帮你处理一切，你只需要专注在设计上就好了。一旦开发人员帮你配置好了Vagrant之后，你只需要执行```vagrant up```，然后开始设计。

#安装

Vagrant的安装非常简单，直接[下载](http://www.vagrantup.com/downloads)对应操作系统的版本就可以了。

#第一印象

	$ vagrant init hashicorp/precise32
	$ vagrant up

执行以上命令之后，你已经拥有了一个Ubuntu 12.04 LTS 32-bit系统运行在VirtualBox中。
你可以通过```vagrant ssh```登录到这个虚拟机中，如果你不需要它了，可以通过```vagrant destroy```来销毁。

#建立项目

建立Vagrant项目的第一步是配置Vagrantfile。执行如下命令

	$ mkdir my_vagrant
	$ cd my_vagrant
	$ vagrant init

这会在当前目录下生成一个Vagrantfile文件，这个文件就是一切的开始，对了，你最好把它添加到版本库中，这样你的小伙伴也可以通过它来初始化开发环境了。

#Box

Vagrant使用的image叫做box，如果你执行过上面的命令，那么你已经在本地拥有了一个box。如果没有执行，那么你需要执行

	$ vagrant box add hashicorp/precise32

这会从[Vagrant Cloud](https://vagrantcloud.com/)中下载hashicorp/precise32。
我们接下来需要配置我们的项目来使用这个box，编辑Vagrantfile文件并修改为：

	Vagrant.configure("2") do |config|
	  config.vm.box = "hashicorp/precise32"
	end

除了hashicorp/precise32，你可以在Vagrant Cloud找到更多适合你的box。

#启动

	$ vagrant up

就这么简单。完成之后，你就拥有了一个Ubuntu系统，你可以通过

	$ vagrant ssh

登录它，然后随意执行任何命令，除了```rm -rf /```，原因接下来说明。

#目录同步

虽说如此容易的启动一个虚拟机的确很酷，但不是所有人都喜欢通过终端来编辑文件（Vim党和Emacs党勿喷），所以Vagrant提供了一个目录同步的功能。默认情况下Vagrant会把你的项目目录（存储Vagrantfile的那个）与虚拟机中的```/vagrant```进行同步（这就是为什么你不要执行```rm -rf /```的原因，否则你会把项目目录删掉）。我们可以登录到虚拟机上验证一下。

	$ vagrant up
	...
	$ vagrant ssh
	...
	vagrant@precise32:~$ ls /vagrant
	Vagrantfile

如果你不确信，可以创建一个文件看看：

	vagrant@precise32:~$ touch /vagrant/foo
	vagrant@precise32:~$ exit
	$ ls
	foo Vagrantfile

怎么样？没骗你吧。通过目录同步功能，你还可以继续使用最爱的编辑器来修改虚拟机中的文件。

#配置

假设我们的业务需要安装Apache，传统的做法是在虚拟机上手动安装并配置，如果这样那么使用Vagrant的人都需要重复一遍。幸好Vagrant提供了自动配置（automated provisioning）的功能。通过这个特性，Vagrant会在你执行```vagrant up```的时候自动安装所需的软件。

在你的项目目录（即包含Vagrantfile的目录）下创建Bash脚本bootstrap.sh，内容如下：

	#!/usr/bin/env bash

	apt-get update
	apt-get install -y apache2
	rm -rf /var/www
	ln -fs /vagrant /var/www

接下来，我们来配置让Vagrant在启动虚拟机的时候自动执行以上脚本，在Vagrantfile中添加如下内容：

	Vagrant.configure("2") do |config|
	  config.vm.box = "hashicorp/precise32"
	  config.vm.provision :shell, path: "bootstrap.sh"
	end

provision这一行告诉Vagrant使用shell provisioner来配置虚拟机，要执行的脚本是bootstrap.sh。

接下来执行```vagrant up```来启动虚拟机，之后你可以登录到虚拟机来验证Apache时候已经安装成功：

	$ vagrant ssh
	...
	vagrant@precise32:~$ wget -qO- 127.0.0.1

#网络

总是在终端里边访问Apache不是什么好的主意，所以这个部分我们会对Vagrant的网络进行配置，让它可以通过宿主机器（Host machine）来访问。

我们用端口映射来实现对Apache服务的访问，编辑Vagrantfile文件如下：

	Vagrant.configure("2") do |config|
	  config.vm.box = "hashicorp/precise32"
	  config.vm.provision :shell, path: "bootstrap.sh"
	  config.vm.network :forwarded_port, host: 4567, guest: 80
	end

```forwarded_port```这一行把宿主机器的4567端口映射到了客户机器（Guest machine）的80端口。然后通过```vagrant reload```重启虚拟机，重启完成之后你用浏览器打开
http://127.0.0.1:4567就可以访问到WEB页面了。


#参考

* [http://docs.vagrantup.com/v2/getting-started/index.html](http://docs.vagrantup.com/v2/getting-started/index.html)
* [http://docs.vagrantup.com/v2/getting-started/project_setup.html](http://docs.vagrantup.com/v2/getting-started/project_setup.html)
* [http://docs.vagrantup.com/v2/getting-started/boxes.html](http://docs.vagrantup.com/v2/getting-started/boxes.html)
* [http://docs.vagrantup.com/v2/getting-started/up.html](http://docs.vagrantup.com/v2/getting-started/up.html)
* [http://docs.vagrantup.com/v2/getting-started/provisioning.html](http://docs.vagrantup.com/v2/getting-started/provisioning.html)
