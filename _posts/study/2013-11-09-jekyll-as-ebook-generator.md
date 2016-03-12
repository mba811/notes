---
layout: post
title: Jekyll 作为电子书生成器
tags: [study]
---

在考虑如何仿照 Jekyll 做电子书生成器，发现已经有了这种工具：

> Kitabu [https://github.com/fnando/kitabu](https://github.com/fnando/kitabu)
> 
> A framework for creating e-books from Markdown/Textile text markup using Ruby. Using the Prince PDF generator, you'll be able to get high quality PDFs.

Kitabu 可以初始化一个项目目录，将文本、图片、样式等资源分门别类放好，然后运行一个 `export` 命令生成多种电子书格式和静态网页。

过了不久我又觉得我想错了，既然 Jekyll 很好，为什么不把 Jekyll 当作电子书生成器？这时候又发现，其实已经有人这么用了：

> Ruby Hacking Guide Translation [https://github.com/ruby-hacking-guide/ruby-hacking-guide.github.com](https://github.com/ruby-hacking-guide/ruby-hacking-guide.github.com)

这个项目的基本结构就是一个 Jekyll 项目，只是添加了一个 `script/publish` 脚本用来生成电子书，问题解决了。另外，读者也不用自己下载源码编译，用 Github Release 功能可以由维护者编译好一份电子书供人下载。

既能利用 Github 协作，又能利用 Github Pages/Release 发布，这是我目前觉得最好的制作电子书方案。

## 复杂性

可以看到，用 Jekyll 制作电子书，实际上会成为一个软件项目。维护者需要了解 Jekyll 规范，需要编写 Ruby 或其他脚本代码，需要会用 Git 和 Github，没有一定编程基础是做不下来的，但我觉得这不是很大问题。

首先，对于书籍来说，数万字的管理本身就是一个复杂的事情，这时原稿的稳定性和易处理性要比易用性更重要。Github 是个非常通用的项目协作平台，只要是文件都能纳入版本管理中，这要比其他方案可靠很多。

其次，完全不懂编程的作者可以委托技术合作者管理。作者可以先用自己喜欢的方式写文章，然后再交由出版社让技术人员操作。如果作者希望自己一个人搞定，那就学习一些编程知识，就像王小波也是个程序员。技术写作者们可以分享一些模板简化操作。

最后，Github 本身也在不断提高自己的易用性，例如 Github for Windows/Mac 客户端，还有 Web 在线编辑都简化了不少操作。Github 改进易用性的速度要比大多数新项目从零做起的速度快。

## 其他方案

### Tablo

> [http://tablo.io/](http://tablo.io/)

这个网站是个一站式方案，作者在网站上创建书本目录，然后加入内容，最后一键发布到多个平台。其实在 writings.io 设计之初是想做这样的项目，不知不觉中走偏了，所以我看到这个网站的时候真是感慨不已。

也正因为自己尝试过，所以我认为这种方案有很大局限。它的所有格式化、版本管理、协作全部依赖网站本身的实现，不能和现有的工具良好的协作，所以只适合对协作和排版格式要求不高的个人文字作品。

### Atlas

> [http://chimera.labs.oreilly.com/about](http://chimera.labs.oreilly.com/about)

这是 O'Reilly 出版社的试验项目，之前已经写过[一篇博客](http://chloerei.com/2013/08/15/oreilly-atlas/)介绍。它基于 Github 做了一层包装，有自己的项目规范和协作流程，并且和出版社的电子书市场能很好的整合。

这可能是目前唯一把制作电子书的复杂性和易用性结合得很好的方案，他们设计的时候应该已经很深入研究过 Github 的优点和对于电子书制作流程的不足。只是现在还没有开放，不知道使用起来怎么样。

O'Reilly 有雄厚的研发实力，别的出版社应该立即跟进山寨一份吗？我觉得不必要，直接用 Github + Jekyll + Script 就行了。

## 整合

我曾经对自己的研发能力过于自信，想要自己解决所有问题，现在想起来真是浪费了大量时间。在能解决问题的情况下，能利用现有资源是最好的方案。

推荐想要搭建电子书制作工具的人，认真考虑一下基于 Jekyll/Github 的方案。