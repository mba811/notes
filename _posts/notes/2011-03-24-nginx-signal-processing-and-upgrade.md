---
layout: post
title: Nginx 信号处理与平滑升级
tags: [notes]
---

[Nginx]进程分为**master**进程和**worker**进程，我们可以通过信号来控制**master**进程。默认情况下，Nginx会把它的**master**进程id写到`/usr/local/nginx/logs/nginx.pid`中。你可以在编译的时候通过`./configure`来指定，或者在配置文件中用`pid`来配置。

**Master**进程能够接收并处理如下的信号：

* ERM, INT（快速退出，当前的请求不执行完成就退出） 
* QUIT （优雅退出，执行完当前的请求后退出）
* HUP （重新加载配置文件，用新的配置文件启动新worker进程，并优雅的关闭旧的worker进程） 
* USR1 （重新打开日志文件） 
* USR2 （平滑的升级nginx二进制文件） 
* WINCH （优雅的关闭worker进程） 

**Worker**进程也可以接收并处理一些信号：

* TERM, INT （快速退出） 
* QUIT （优雅退出） 
* USR1 （重新打开日志文件） 


###用HUP信号使Nginx加载新的配置文件

当Nginx接收到`HUP`信号的时候，它会尝试着去解析并应用这个配置文件，如果没有问题，那么它会创建新的**worker**进程，并发送信号给旧的 **worker**进程，让其优雅的退出。接收到信号的旧的**worker**进程会关闭监听**socket**，但是还会处理当前的请求，处理完请求之后，旧的 **worker**进程退出。如果Nginx不能够应用新的配置文件，那么仍将用旧的配置文件来提供服务。

###在线升级Nginx二进制文件

当你想升级Nginx到一个新的版本，增加或减少module的时候，你需要替换Nginx的二进制文件，你可以平滑的实现它，没有请求会丢失。

首先，用新的二进制文件替换掉旧的，然后发送`USR2`信号给**master**进程。**master**进程会把自己的**.pid**文件重命名为**.oldbin**（例 如，**/usr/local/nginx/logs/nginx.pid.oldbin**），然后执行新的二进制文件，从而启动一个新的**master**进程和新的**worker**进程：

	 PID  PPID USER    %CPU   VSZ WCHAN  COMMAND
	33126     1 root     0.0  1164 pause  nginx: master process /usr/local/nginx/sbin/nginx
	33134 33126 nobody   0.0  1368 kqread nginx: worker process (nginx)
	33135 33126 nobody   0.0  1380 kqread nginx: worker process (nginx)
	33136 33126 nobody   0.0  1368 kqread nginx: worker process (nginx)
	36264 33126 root     0.0  1148 pause  nginx: master process /usr/local/nginx/sbin/nginx
	36265 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36266 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36267 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)



在这个时候，有两个Nginx实例在运行，一起处理进来的请求。为了让旧的实例退出，你需要发送`WINCH`信号给旧的**master**进程，这样旧**master**进程的**worker**进程就会优雅的退出：

		PID  PPID USER    %CPU   VSZ WCHAN  COMMAND
	33126     1 root     0.0  1164 pause  nginx: master process /usr/local/nginx/sbin/nginx
	33135 33126 nobody   0.0  1380 kqread nginx: worker process is shutting down (nginx)
	36264 33126 root     0.0  1148 pause  nginx: master process /usr/local/nginx/sbin/nginx
	36265 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36266 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36267 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)


一段时间后，旧的**worke**r进程都已经退出了，只有新的**worker**进程处理进来的请求：

	PID  PPID USER    %CPU   VSZ WCHAN  COMMAND
	33126     1 root     0.0  1164 pause  nginx: master process /usr/local/nginx/sbin/nginx
	36264 33126 root     0.0  1148 pause  nginx: master process /usr/local/nginx/sbin/nginx
	36265 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36266 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36267 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)


这个时候你仍然可以通过以下几个步骤回滚到旧的服务，因为旧**master**进程并没有关闭其监听的**socket**： 发送`HUP`信号给旧的**master**进程，它会启动**worker**进程并且不需要重新加载配置文件 发送`QUIT`信号给新的**master**进程，让它优雅的终止其**worker**进程发送`TERM`信号给新的**master**进程，强制其退出 如果一些原因，新的**worker**进程没有退出，发送`KILL`信号给它们 当新的**master**进程退出之后，旧的**master**进程会删除其**pid**文件名中的后缀**.oldbin**，这样一切就又变成升级之前的样子。 如果一个升级已经成功，然后你想只保留新的server，那么发送`QUIT`信号给旧的**master**进程让新的server来提供服务：

	PID  PPID USER    %CPU   VSZ WCHAN  COMMAND
	36264     1 root     0.0  1148 pause  nginx: master process /usr/local/nginx/sbin/nginx
	36265 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36266 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)
	36267 36264 nobody   0.0  1364 kqread nginx: worker process (nginx)


原文：[http://wiki.nginx.org/NginxCommandLine](http://wiki.nginx.org/NginxCommandLine)

[Nginx]: http://nginx.org/ "Nginx"
