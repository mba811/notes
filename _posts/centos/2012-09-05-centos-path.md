---
layout: post
title: CentOS 系统下修改环境变量PATH路径的方法
tags: [centos]
---

电脑脑中必不可少的就是操作系统。而Linux的发展非常迅速，有赶超微软的趋势。这里介绍Linux的知识，让你学好应用Linux系统。比如要把/etc/apache/bin目录添加到PATH中，方法有三：

1.#PATH=$PATH:/etc/apache/bin  
使用这种方法,只对当前会话有效，也就是说每当登出或注销系统以后，PATH 设置就会失效

2.#vi /etc/profile  
在适当位置添加 PATH=$PATH:/etc/apache/bin （注意：＝ 即等号两边不能有任何空格）  
这种方法最好,除非你手动强制修改PATH的值,否则将不会被改变

3.#vi ~/.bash_profile  
修改PATH行,把/etc/apache/bin添加进去  
这种方法是针对用户起作用的

注意：想改变PATH，必须重新登陆才能生效，以下方法可以简化工作：

如果修改了/etc/profile，那么编辑结束后执行source profile 或 执行点命令 ./profile,PATH的值就会立即生效了。  
这个方法的原理就是再执行一次/etc/profile shell脚本，注意如果用sh /etc/profile是不行的，因为sh是在子shell进程中执行的，即使PATH改变了也不会反应到当前环境中，但是source是在当前 shell进程中执行的，所以我们能看到PATH的改变。

这样你就学会Linux系统下修改环境变量PATH路径的方法。
