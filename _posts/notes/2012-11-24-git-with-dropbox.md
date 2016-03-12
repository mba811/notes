---
layout: post
title: 把 Dropbox 改造为 Git 私有仓库
tags: [notes]
---

![](http://7q5cfr.com1.z0.glb.clouddn.com/git.png) ![](http://7q5cfr.com1.z0.glb.clouddn.com/dropbox.png)　

##前言

[Git][1]作为强大的分布式版本控制工具，越来越受欢迎。大量的开源项目可以在[Github][2]上发布，不过项目是公共可见的，即人人可以fork。
对于一些用户，他们也有自己的项目，但是还不太想立刻就把项目开源出来，有可能是因为还没有完成，所以他们需要通过Git临时性地管理他们的「私有项目」，Github上虽然有私有项目托管服务，不过性价比不高。

[Dropbox][3]（墙）是最流行的云存储服务，通过Dropbox我们可以实现对Git私有项目的托管。

##思路

我们的思路是在Dropbox客户端的目录中建立Git仓库，然后我们clone此仓库到本地仓库，然后我们进行提交操作，完成提交之后，我们执行push操作，
那么本地的数据会被push到Dropbox客户端目录的仓库中，之后Dropbox客户端会把仓库文件的更改同步到Dropbox服务器。

	+------------+            +-----------+              +---------+
	|  Dropbox   |  --Sync->  |  Dropbox  |   --Clone->  | Working |
	|   Server   |  <-Sync--  |   Client  |   <-Push---  |  Space  |
	+------------+            +-----------+              +---------+
	
##实现

我们现在Dropbox的目录中创建一个裸git仓库

	$ cd ~/Dropbox
	$ git init --bare project.git

完成之后，我们clone这个仓库

	$ cd ~
	$ git clone ~/Dropbox/project.git project
	$ cd project

提交并且push

	$ touch README
	$ git add .
	$ git commit -m "init commit"
	$ git push origin master

完成之后，Dropbox会把你push的内容同步到服务器，你通过[https://www.dropbox.com/](https://www.dropbox.com/)可以查看到仓库的内容。

##参考

[http://stackoverflow.com/questions/1960799/using-gitdropbox-together-effectively](http://stackoverflow.com/questions/1960799/using-gitdropbox-together-effectively)

[1]: http://git-scm.com/ "Git"
[2]: https://github.com "Github"
[3]: https://www.dropbox.com/ "Dropbox"

