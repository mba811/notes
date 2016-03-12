---
layout: post
title: CR, LF, CR/LF区别与关系
tags: [notes]
---

###前言

在文本处理中，[CR]（**C**arriage **R**eturn），[LF]（**L**ine **F**eed），CR/LF是不同操作系统上使用的换行符，具体如下：

* Dos和Windows采用回车+换行`CR/LF`表示下一行
* 而UNIX/Linux采用换行符`LF`表示下一行
* 苹果机(MAC OS系统)则采用回车符`CR`表示下一行

###区别

CR与LF区别如下：

* CR用符号`\r`表示，十进制ASCII代码是`13`，十六进制代码为`0x0D`
* LF使用`\n`符号表示，ASCII代码是`10`，十六制为`0x0A`

所以Windows平台上换行在文本文件中是使用`0d 0a`两个字节表示，而UNIX和苹果平台上换行则是使用`0a`或`0d`一个字节表示。

###问题

一般操作系统上的运行库会自动决定文本文件的换行格式。如一个程序在Windows上运行就生成`CR/LF`换行格式的文本文件，而在Linux上运行就生成`LF`格式换行的文本文件。在一个平台上使用另一种换行符的文件文件可能会带来意想不到的问题，特别是在编辑程序代码时。有时候代码在编辑器中显示正常，但在编辑时却会因为换行符问题而出错。很多文本/代码编辑器带有换行符转换功能，使用这个功能可以将文本文件中的换行符在不同格式单互换。

在不同平台间使用FTP软件传送文件时，在ASCII文本模式传输模式下，一些FTP客户端程序会自动对换行格式进行转换。经过这种传输的文件字节数可能会发生变化。如果你不想FTP修改原文件，可以使用bin模式(二进制模式)传输文本。

###参考

[http://en.wikipedia.org/wiki/Carriage_return](http://en.wikipedia.org/wiki/Carriage_return)     
[http://en.wikipedia.org/wiki/Line_feed](http://en.wikipedia.org/wiki/Line_feed)

[CR]: http://en.wikipedia.org/wiki/Carriage_return
[LF]: http://en.wikipedia.org/wiki/Line_feed