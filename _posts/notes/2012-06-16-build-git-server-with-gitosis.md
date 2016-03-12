---
layout: post
title: 使用 Gitosis 搭建 Git 服务器
tags: [notes]
---

![Git](http://{{ site.cdn }}/images/tech/git.jpg "Git")

###1.安装gitosis

首先是获取gitosis（这里假设你已经安装过git）：

	git clone git://github.com/res0nat0r/gitosis.git

接下来安装gitosis：

	sudo python setup.py install

如果出现以下错误：

	Traceback (most recent call last):
	 File "setup.py", line 2, in ?
	 from setuptools import setup, find_packages
	 ImportError: No module named setuptools
	 
或者
	-bash: python: command not found

那么你还需要安装python-setuptools：

	sudo yum install python-setuptools

接下来添加用来管理仓库的用户，用户名任意，我们这里使用git：

	useradd git

Mac用户在「系统偏好设置　»　用户与群组 」中添加。

修改PATH，使git用户可以调用git：

	vi /home/git/.bashrc
	PATH=/usr/local/bin:/usr/local/git/bin:$PATH

创建key pair，并拷贝public key到/tmp下，这样可以确保gitosis-init命令对其有读取权限：

	ssh-keygen -t rsa
	cp ~/.ssh/id_rsa.pub /tmp/id_rsa.pub

以git用户来执行gitosis-init命令：
	
	sudo -H -u git gitosis-init &lt; /tmp/id_rsa.pub

此时/home/git下增加了两个目录：

	gitosis
	repositories

其中gitosis是gitosis的根目录，repositories是仓库存放目录。

如果出现以下错误：

	if install git from source, otherwise:
	raise child_exception
	OSError: [Errno 2] No such file or directory

那么做个symlink：
	
	ln -s /usr/local/bin/git /usr/bin/git

给脚本post-update赋予可执行权限：

	sudo chmod 755 /home/git/repositories/gitosis-admin.git/hooks/post-update

###2. 添加新仓库

gitosis的管理是通过git来管理的，clone一下：
	
	git clone git@localhost:gitosis-admin.git

如果出现以下错误：

	Cloning into gitosis-admin...
	ssh: connect to host 192.168.1.30 port 22: Connection refused
	fatal: The remote end hung up unexpectedly 

那么确认当前机器openssh是否已经启动，Mac用户通过&#8221;系统偏好　-&gt;　共享　-&gt;　远程登录&#8221;进行设置。

	cd gitosis-admin
	ls -l
	-rw-r--r--  1 weizhifeng  staff  124  6 14 13:45 gitosis.conf
	drwxr-xr-x  3 weizhifeng  staff  102  6 14 13:46 keydir

keydir目录用来存放用户的public key(.pub文件)，gitosis.conf为配置文件。

看一下配置文件：

	cat gitosis.conf
	[gitosis]
	
	[group gitosis-admin]
		members = Mac
		writable = gitosis-admin


其中group代表一个组，writable是仓库名，members是此仓库的成员，可以有多个成员，用空格进行分割。

添加一个新仓库：

	[group test]
		members = Mac
		writable = test

把更改提交并push到git@localhost:gitosis-admin.git：

	git commit -a -m "添加新仓库test"
	git push


在本地创建一个仓库，并push到git@localhost:test.git，gitosis会在/home/git/repositories自动创建test.git这个仓库：

	mkdir test
	cd test 
	touch README
	git init
	git remote add origin git@localhost:test.git
	git push origin master


###3. 添加用户

假设我们要添加的用户为jeremy，那么需要创建key pair：

	ssh-keygen -t rsa

假设生成的public key为~/.ssh/jeremy.pub

	cd gitosis-admin

修改gitosis.conf，修改后为如下：

	[group test]
	members = Mac jeremy
	writable = test

注意.pub文件名和你要在members中添加的用户名要完全一样。

拷贝jeremy.pub到keydir中：

	cp　~/.ssh/jeremy.pub keydir/

把更改push到gitosis-admin.git：

	git commit -a -m "添加jeremy到test仓库"
	git push


接下来把private key分发给jeremy，然后他就可以从自己的机器上进行clone了：

	git clone git@SERVER_HOSTNAME:test.git

如果出现以下错误：

	ERROR:gitosis.serve.main:Repository read access denied
	fatal: The remote end hung up unexpectedly

是因为使用了内容相同，名字不同的public key(.pub)。

###4.其他

如果SSH使用的不是22端口，那么请如下修改：

	vi ~/.ssh/config
	Host myserver.com
	Port 2345      

###5. 参考

[http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way/][1]    
[http://lukhnos.org/blog/en/archives/162/][2]   
[http://blog.longwin.com.tw/2011/03/linux-gitosis-git-server-2011/][3]


[1]: http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way/"
[2]: http://lukhnos.org/blog/en/archives/162/
[3]: http://blog.longwin.com.tw/2011/03/linux-gitosis-git-server-2011/

