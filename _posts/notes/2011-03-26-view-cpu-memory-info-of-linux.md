---
layout: post
title: Linux 下查看内存，CPU 信息
tags: [notes]
---

###内存信息

使用[free]查看内存信息：

	$ free -m
					total       used       free     shared    buffers     cached
	Mem:              222         136         86          0         29       60
	-/+ buffers/cache:             47        175
	Swap:             1905          0       1905


* total：总共的内存大小
* used：已经被使用的内存
* free：空闲的内存
* shared：共享的内存大小
* buffers：用来做缓冲的内存
* cached：用来做cache的内存


**Mem**这行是以操作系统的角度去看待内存的使用，可以看到我们总共的内存是222M(total1)，使用了136M(used1)，有86M的空闲(free1)，29M的缓冲(buffers1)，60M的缓存(cached1)。

**\-/+ buffers/cache**这行是以应用程序的角度去看待内存的使用，对于应用来说**buffers**和**cached**的内存是就是空闲的内存，在需要的时候是 可以直接拿来用的，所以：

	used = used1 – buffers1 – shared1 = 136 – 29 – 60 = 47，
	free = free1 + buffers1 + shared1 = 86 + 29 + 60 = 175。

**Swap**这行是交换区的使用情况，如果used很大的话，说明内存不够用了。

PS:跑的虚拟机，内存有些小，见笑~~~

###CPU信息

Linux系统中的CPU信息存在于`/proc/cpuinfo`文件中，如果想了解全部的信息，可以直接查看这个文件。

有多少个物理CPU？

	cat /proc/cpuinfo | grep 'physical id' | sort | uniq |wc -l


有多少个虚拟CPU？

	cat /proc/cpuinfo | grep ^processor | sort | uniq |wc -l


CPU是几个核心的？

	cat /proc/cpuinfo | grep 'cpu cores' | uniq

如何查看每个CPU的使用情况？执行[top]指令，然后按1就可以看到CPU的使用情况了。

###参考

[http://www.php-oa.com/2008/04/04/linux-free.html](http://www.php-oa.com/2008/04/04/linux-free.html)

[free]: http://linux.die.net/man/1/free "free"
[top]: http://linux.die.net/man/1/top "top"