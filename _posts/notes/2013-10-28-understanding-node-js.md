---
layout: post
title: 理解Node.js
tags: [notes]
---

原文：[http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb](http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb)

当我把[Node.js](http://nodejs.org/)介绍给人们的时候，通常会产生两种反应。总的来说人们不是可以正确的理解它，就是被搞得非常糊涂。

如果你到目前为止还处于第二组里，下面是我想要用来讲解node的内容：

* 它是一个命令行工具。你下载一个tarball文件，编译并安装源码。
* 它可以让你在你的终端中通过输入'node my_app.js'来运行JavaScript程序。
* JS是被[V8 javascript引擎](http://code.google.com/p/v8/)（使Google Chrome运行如此之快的东西）所执行的。
* Node提供了一个用来访问网络和文件系统的JavaScript API。

__“但是我可以做任何我想要做的事情用：ruby，python，php，java， ... ！”__

我听到了你的声音。你是对的。Node不是该死的独角兽，它不会帮你做你自己的工作，抱歉。它只是一个工具，并且它大概也不会完全地替换你常用的工具，至少当前不会。

__“说重点！”__

好的，我会的。Node总的来说非常优秀当你需要同时做许多事情的时候。你有过写了一部分代码然后说“我想要这个并行执行”吗？好吧，在node中任何东西都是并行运行的，除了你的代码。

__“嘿？”__

是的没错，任何东西都是并行的，除了你的代码。为了理解它，把你的代码想像成是国王，然后node是它的仆从军队。

新的一天以仆从叫醒国王并且问他是否需要什么而开始了。国王给了这个仆从一个任务列表，然后回去继续睡觉去了（汗）。这个仆从把这些任务分配给了他的同僚，然后他们开始工作。

当一个仆从完成了一个任务，他会在国王领地外边排成一条线来汇报。国王一次让一个仆从进来，然后听取他的报告。有时候国王会在仆从出去的时候给仆从更多的任务。

生活是美好的，因为国王的仆从并行的执行他的任务，但是一次只报告一个结果，所以国王可以专注。*

__“那是异想天开，但是你能结束这个愚蠢的比喻并且用geek的方式告诉我吗？”__

当然。一个简单的node程序看起来可能是这样的：

	var fs = require('fs')
	  , sys = require('sys');

	fs.readFile('treasure-chamber-report.txt', function(report) {
	  sys.puts("oh, look at all my money: "+report);
	});

	fs.writeFile('letter-to-princess.txt', '...', function() {
	  sys.puts("can't wait to hear back from her!");
	});

你的代码给了node两个任务用来读写一个文件，然后就休眠了。当node完成了一个任务，跟它对应的回调就会被触发。但是在同一时间只能有一个回调触发。在那个回调完成了执行之前，所有其他的回调不得不排队等待。进一步说，对于回调触发的顺序是无法保证的。

__“所以我不必担心代码在同一时间访问同一个数据结构？”__

你理解了！这就是JavaScript的单进程/事件循环设计的全部美之所在。

__“非常好，但是为什么我应该用它？”__

一个原因是效率。在一个web应用中，你主要的响应时间消耗通常是执行数据库查询的时间之和。通过node，你可以一次执行你所有的查询，把响应时间减少为执行最慢查询所花费的时间。

另一个原因是JavaScript。你可以使用node来在浏览器和你的后端之间共享代码。JavaScript也正在变成一个真的通用语言。不管过去你用的是python，ruby，java，php或者其他语言，你都可能用过一些JS，对吗？

最后一个原因是原始速度。V8时刻在向成为全球最快的动态语言解释器之一而努力。我想不到任何其他语言在速度上有像如今的JavaScript一样有如此突飞猛进的提升。进一步说，node的I/O能力非常轻量级，可以使你可以尽可能的完全利用你系统完全的I/O能力。

__“所以你在说我应该从现在起用node来写我所有的应用？”__

是和不是。一旦你开始抡node这个锤子，那么显然一切开始看起来都像一个钉子。但是如果你当前的工作有一个期限，你可以通过以下来决定：

* 是否低响应时间/高并发重要？Node真的很擅长它。
* 项目有多大？小项目问题不大。大项目应该小心的评估（可用的库，修复一个bug所需的资源或者two upstream等等。）。

__“node能运行在Windows上吗？”__

不行。如果你使用的是windows，你需要运行一个Linux虚拟机（我推荐[VirtualBox](http://www.virtualbox.org/)）。node对Windows的支持在计划中了，但是接下来的几个月不要屏住呼吸除非你想对port提供帮助。（译者：现在node可以完美的运行在Windows上）

__“我能在node中访问DOM吗？”__

好问题！不行，DOM是浏览器中的东东，并且node的JS引擎（V8）幸好跟那些混乱的东西是完全分离的。不过，有人在以node模块的形式来实现DOM，可能带来令人兴奋的可能性比如对客户端代码进行单元测试。

__“难道事件驱动编程不是真的非常难吗？”__

这取决于你。如果你已经学过如何在浏览器中摆弄AJAX调用和用户事件，那么使用node不会是一个问题。

同时，测试驱动开发可以真正的帮助你以一个可维护的设计做为开始。

__“谁在用它？”__

在[node wiki](http://wiki.github.com/ry/node)（滚动到"Companies using Node"）有一个小的/不全的列表。[Yahoo](http://www.yuiblog.com/blog/2010/04/09/node-js-yui-3-dom-manipulation-oh-my/)正在为YUI对node进行实验，[Plurk](http://www.plurk.com/)正在使用它处理大规模的comet，并且Paul Bakaus（由于jQuery UI而出名）正在创建一个令人兴奋的游戏引擎，其中后端使用了一些node代码。[Joyent](http://www.joyent.com/)已经雇佣了Ryan Dahl（node作者）并且大力资助开发。

对了，Heroku也刚刚声明了[支持对node.js的托管](http://blog.heroku.com/archives/2010/4/28/node_js_support_experimental/)。

__“我能去哪里学更多？”__

Tim Caswell正在运作优秀的[How To Node](http://howtonode.org/)博客。在twitter上Follow [#nodejs](http://search.twitter.com/search?q=%23nodejs)。订阅[邮件列表](http://groups.google.com/group/nodejs)。然后去IRC频道逛逛，#node.js（是的，名字中包含这个点）。我们在那的划艇分数快达到200了：）。

我也将会继续在debuggable.com这里写文章。

当前就写这么多了。如果你有其他问题欢迎留言。

--注解：

*: 这个比喻显然是太简单了，但是在现实之中找到一个与非阻塞概念相对应的对照物很难。