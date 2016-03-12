---
layout: post
title: 为什么还不快使用 git-flow ？
tags: [notes]
---

原文: [http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/][4]

今年一月份的时候，[@nvie][1]发表了「[A successful Git branching model][2]」，在这篇文章里他介绍了如何保持自己的Git仓库优雅并且整洁。除此之外，他还放出了[git-flow][3]：一组Git扩展，可以异常容易地使用这个模型。

我非常吃惊许多人之前根本没有听说过它，所以在这篇文章里我将要告诉你为什么它可以让你整天都心情愉悦。

![](http://7q5cfr.com1.z0.glb.clouddn.com/gitflow.png)


安装好git-flow之后，你可以在当前目录里开始一个新的仓库或者把一个已经存在的仓库转换成新的分支结构：

	$ git flow init

它会问你一组问题，但是你最好接受默认值：

	No branches exist yet. Base branches must be created now.
	Branch name for production releases: [master] 
	Branch name for "next release" development: [develop] 
	How to name your supporting branch prefixes?
	
	Feature branches? [feature/] 
	Release branches? [release/] 
	Hotfix branches? [hotfix/] 
	Support branches? [support/] 
	Version tag prefix? []
	

在你回答这些问题之后，git flow会自动把你的默认分支设置为_develop_（或者任何你自己命名的），这是你将要开始工作的地方。

现在，像之前一样简单得去使用Git，但是只在_develop_分支上开发一些小功能。如果你需要开发一个大一些的功能，那么就得以_develop_分支为基础创建一个特性分支（feature branch）。这里假设你想要添加一个登录页面：
	
	$ git flow feature start login

这会以我们的_develop_分支为基础创建一个名叫_feature/login_的新分支，然后并切换到这个分支上。提交代码并且当你完成了登录页面上的工作之后，简单的完成它：

	$ git flow feature finish login

这将会把_feature/login_合并回_develop_，并且删除特性分支。

当你的特性完成的时候，简单地开始一个release分支 - 当然，也是基于_develop_分支 - 来提升版本号并且修复release之前的最后几个bug：

	$ git flow release start v0.1.0

当你完成了一个release分支，它将会把你的修改合并到_master_和_develop_，所以你不用担心你的_master_会比_develop_提前。

最后一件使git-flow如此厉害的事情是其处理hotfixes的能力。你开始和完成一个hotfix分支就像处理其他分支一样，但是这个分支是基于_master_的，所以当生产环境上出现bug之后你可以迅速的修复它并且使用_finish_把分支合并回_master_和_develop_。

Awesome, right? Now, what are you waiting for?

[1]: http://twitter.com/nvie "@nvie"
[2]: http://nvie.com/posts/a-successful-git-branching-model/ "model"
[3]: http://github.com/nvie/gitflow "gitflow"
[4]: http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/ "why-arent-you-using-git-flow"