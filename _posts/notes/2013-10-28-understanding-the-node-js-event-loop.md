---
layout: post
title: 理解 Node.js 中的 Event Loop
tags: [notes]
---

原文：[http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/](http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/)

node.js中的第一个基本论点是I/O是昂贵的：

![](http://7q5cfr.com1.z0.glb.clouddn.com/io-cost.png) 

所以当前编程技术中最大的浪费是等待I/O的完成。这里有几种用来处理这个性能影响的方法（来自Sam Rushing）：

* 同步：你同一时间只处理一个请求，每次一个。优点：简单；缺点：任何一个请求都可以挂起所有其他请求。
* fork新进程：你启动一个新进程来处理每个请求。优点：简单；缺点：无法很好的扩展，大量的连接意味着大量的进程。```fork()```是Unix程序员的锤子。因为它可用，所以每个问题看起来都像一个钉子。但是通常情况下都是大炮打蚊子。

* 线程：启动新的线程来处理每个请求。优点：简单，比使用```fork```对内核更加友好，因为线程占用更少的系统开销；缺点：你的机器可能没有线程，而且面向线程的编程由于担心对共享资源的访问控制，可能会很快地变得非常复杂。

第二个基本的论点是「每个连接一个线程」的模式是很耗内存的：[比如每个人都展示过的关于Apache与Nginx对比过的内存消耗图]


Apache是多线程的：它的一个请求产生一个[线程](http://httpd.apache.org/docs/2.0/mod/worker.html)（或者[进程](http://httpd.apache.org/docs/2.0/mod/prefork.html)，取决于配置文件）。随着并发连接数的增加以及为了服务更多的并发客户端而对更多线程的需求，你可以看见系统统开销是如何吃掉内存的。Nginx和Node.js不是多线程，因为多个线程和多个进程会需要大量的内存。它们都是单线程的，但是是事件驱动。通过在一个线程中处理多个连接，这消除了由上千个线程/进程所产生的系统消耗。


#Node.js为你的代码保持一个独立的线程...

它真的是以一个独立线程在运行：你不能做任何并行的代码执行；举个「休眠」的例子，这将会阻塞服务器1秒钟：

	while(new Date().getTime() < now + 1000) {
	   // do nothing
	}
	
所以当代码运行的时候，node.js将不会对任何来自客户端的其他请求进行响应，因为它只有一个线程用来执行你的代码。或者如果你有一些耗CPU的代码，比如说，图片缩放，那将会仍然阻塞所有其他请求。

#…一切都是并行的除了你的代码

没有办法在一个单独的请求中让代码以并行的方式运行。因为所有的I/O是事件驱动和异步的，所以如下代码不会阻塞服务器：

	c.query(
	   'SELECT SLEEP(20);',
	   function (err, results, fields) {
	     if (err) {
	       throw err;
	     }
	     res.writeHead(200, {'Content-Type': 'text/html'});
	     res.end('<html><head><title>Hello</title></head><body><h1>Return from async DB query</h1></body></html>');
	     c.end();
	    }
	);
	
如果你在一个请求中执行它，其他请求可以被很好的处理，而与此同时数据库在执行它的休眠操作。


#为什么这很棒？我们什么时候从同步转换到异步/并行执行。

同步执行是好的，因为它简化了写代码的过程（对比线程，并发问题已经有变成WTF的趋势）。

在node.js中，你不需要担心背后发生了什么：当你进行I/O操作的时候去用回调吧；并且你的代码永远不会被中断同时那些I/O操作在不需要承受「每个请求一个线程/进程」模式所耗费的资源的情况下（比如Apache中的内存消耗），将不会阻塞其他请求。

拥有异步I/O是很好的，因为I/O比大部分代码都昂贵并且我们应该做些比傻等I/O更有意义的事情。

![bucket_3](http://{{ site.cdn }}/images/tech/bucket_3.gif "bucket_3")

event loop指的是「一个用来处理外部事件并且把它们转换成对回调的调用的实体。」所以I/O调用是Node.js可以从一个请求切换到另一个请求的关键点。在一次I/O调用中，你的代码保存了回调然后把控制权返回给node.js运行时环境。回调将会在之后数据可用的时候被调用。

当然，在后台，那里会有[DB访问和执行进程所需的线程和进程](http://stackoverflow.com/questions/3629784/how-is-node-js-inherently-faster-when-it-still-relies-on-threads-internally)。但是，这些都不会明确的暴露在你的代码里边，所以你不需要担心它们，但是需要知道I/O的相互影响，比如，在每个请求看来，数据库操作或者其他进程将会是异步的，因为这些线程的执行结果是通过event loop返回到你的代码的。对比Apache模型，这种方式会需要更少的线程和线程开销，因为每个连接不需要一个线程了；只当你需要一些其他的东西需要绝对的以并行方式执行的时候，尽管那样这个管理也是由Node.js来执行的。

除了I/O调用之外，Node.js期望所有请求都快速返回；比如[CPU密集的工作应该被分开给另一个进程](http://stackoverflow.com/questions/3491811/node-js-and-cpu-intensive-requests)，你可以通过事件与这个进程进行交互，或者使用一个抽象层次比如[WebWorkers](http://blog.std.in/2010/07/08/nodejs-webworker-design/)。这（显然）意味着你不能使你的代码并行化除非你在后台使用另一个线程，你通过事件和这个线程交互。基本上，所有发出事件（比如EventEmitter的实例）的对象都支持异步的基于事件的交互，并且你可以通过这个方式来和阻塞代码进行交互，比如使用的文件，套接字或者子进程等在Node.js中是EventEmitter实例的对象。[多核心](http://developer.yahoo.com/blogs/ydn/posts/2010/07/multicore_http_server_with_nodejs/)可以通过这个方案来实现；看一下：node-http-proxy。

#内部实现

node.js的内部实现依赖[libev](http://software.schmorp.de/pkg/libev.html)来提供event loop，并用[libeio](http://software.schmorp.de/pkg/libeio.html)来补libev的不足，用线程池来提供异步I/O。想了解更多，可以看一下[libev](http://pod.tst.eu/http://cvs.schmorp.de/libev/ev.pod)文档。（注：joyent现在把libev和libeio已经合并成为[libuv](https://github.com/joyent/libuv)，现在node使用的是libuv）

#我们在Node.js该如何使用异步编程

Tim Caswell在他优秀的[PPT](http://creationix.com/jsconf.pdf)中描述了这个模式：

* 第一级函数（[First-class functions](http://en.wikipedia.org/wiki/First-class_function)）。比如说，我们把函数做为数据进行传递，打乱它们的顺序并且在需要的时候执行它们。

* 函数组合([Function composition](http://en.wikipedia.org/wiki/Function_composition_(computer_science)))。即在事件触发的I/O中，在一些事情发生之后调用匿名函数或者闭包。

* 回调计数器。对于基于事件的回调，你不能保证I/O事件以任何特定的顺序触发。所以如果你需要多个查询都执行完成，通常情况下你只需要对任何并行的I/O操作进行计数，并且检查所有必须的操作都已经完成当你必须等待这个结果的时候；比如，在事件回调中[记录已经返回了的数据库查询的数量](http://stackoverflow.com/questions/4631774/coordinating-parallel-execution-in-node-js)，并且当你拥有了所有的数据才继续执行。这些查询将会以并行的方式运行如果I/O库支持的话（比如，通过连接池）。

*  Event loop。像之前提到的，你可以把阻塞代码包装到一个基于事件的抽象（evented abstraction）之中。比如，运行一个子进程并且在它执行完成的时候返回数据。

它真的如此简单！
