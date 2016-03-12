---
layout: post
title: Diff 与 Patch
tags: [notes]
---

###问题
假设/path/to/a目录下有a.txt，b.txt两个文件(内容如下)，我们如何才能通过打补丁的方式把a.txt升级为b.txt呢？
a.txt，b.txt的内容如下：

	$ cat a/a.txt
		a
		b
		c

	$ cat a/b.txt
		a
		d
		c
		f

###创建补丁文件 

利用[diff]命令来完成，更多的选项可以通过`diff --help`查看

	diff -uN a/a.txt b/b.txt > a.patch

###补丁文件格式

	--- a/a.txt
	+++ a/b.txt
	@@ -1,3 +1,4 @@
	 a
	-b
	+d
	 c
	+f

`--- a/a.txt`中的`---`表示旧文件      
`+++ a/b.txt`中的`+++`表示新文件     

`@@ -1,3 +1,4 @@`为内容变更区域：

`-1,3`表示旧文件(a.txt)的变更区域，从第1行开始，变动的行数是3，即1到3行会有变化   
`+1,4`表示新文件(b.txt)的变更区域，从第1行开始，变动的行数是4，即1到4行会有变化     
`+`表示添加新的一行 `+d`表示添加了一行，内容是`d`     
`-`表示删除了这一行 `-b`表示删除了b这一行     

###打补丁 

利用[patch]命令来完成，参数可以通过`patch --help`查看，常用方式有两种：

如果使用参数`-p0`，那就表示直接用补丁文件中的路径来查找文件(即a/a.txt)来进行patch操作，如果我们在/path/to目录下，我们如下打补丁：

	patch -p0 < a.patch

如果使用参数`-p1`，那就表示忽略补丁文件中的第一层目录(即a/a.txt中的a)，从当前目录中直接查找a.txt来进行patch操作，如果我们在/path/to/a目录下，我们如下打补丁：

	patch -p1 < a.patch

打好补丁以后，我们来看a.txt文件的内容：

$ cat a.txt
	a
	d
	c
	f

以上只是一个小小的demo，复杂的情况还需要去深入研究:)

###参考：

[http://en.wikipedia.org/wiki/Diff](http://en.wikipedia.org/wiki/Diff)

[diff]: http://en.wikipedia.org/wiki/Diff
[patch]: http://en.wikipedia.org/wiki/Patch
