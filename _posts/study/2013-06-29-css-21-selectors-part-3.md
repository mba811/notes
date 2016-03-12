---
layout: post
title: CSS 2.1选择器第三部分
tags: [study]
---

原文：[http://www.456bereastreet.com/archive/200510/css_21_selectors_part_3/](http://www.456bereastreet.com/archive/200510/css_21_selectors_part_3/)

#CSS 2.1 选择器，第三部分

这是介绍CSS 2.1中可用的选择器的三篇系列文章中的第三篇也是最后一篇文章。[第一篇](css-21-selectors-part-1.html)是关于非常基础的内容，比如类型选择器，类和ID选择器，通配选择器，简单选择器。在[第二篇](/css-21-selectors-part-2.html)文章中，我介绍了组合因子（combinators），组合选择器，分组，以及属性选择器。

这次我将会近距离的看下伪类（pseudo-classes）和伪元素（pseudo-elements）。像我在第二篇文章中介绍的更加高级的选择器一样，伪类和伪元素还没有被所有主流浏览器所完全支持（译者注：浏览器支持情况请见[quirksmode](http://www.quirksmode.org/css/selectors/)），所以请记得当浏览器不支持的时候，检查一下到底发生了什么。你不希望你的内容在不支持这儿所讨论的CSS的浏览器中不能被访问。

##伪类和伪元素

伪类和伪元素能被用来格式化那些不能通过文档树中可用的信息来定位的元素。举个例子，不存在一个与一个段落中的第一行内容或者一个元素文本内容的第一个字母所关联的元素。

##伪类

伪类是用比名字，属性或者内容更加独特的信息来匹配元素的。

###:first-child

这个伪类会匹配一个元素的第一个子元素。让我们假设你想给一篇文章的第一个段落配一个特殊的样式。如果这篇文章被包含在一个类名为“article”的`div`元素中，那么下面的规则将会匹配每一篇文章中的第一个`p`元素：

	div.article p:first-child {
		font-style:italic;
	}

想匹配所有做为任何元素的第一个子元素而存在的`p`元素，你可以使用这个规则的选择器：

	p:first-child {
		font-style:italic;
	}

###:link和:visited

`link伪类`会对未访问和访问过的链接状态产生影响。这两种状态是相互独立的－一个链接在同一时间不可能既是访问过又是未访问过。

这些伪类只能应用到文档语言所决定的超链接源锚点（anchor）上。对于HTML来说，这指的是具有`href`属性的`a`元素。既然这不会影响其他元素，那么以下选择器是一样的：

	a:link
	:link

###:hover, :active, and :focus

动态伪类可以用来控制特定元素在用户发起特定动作时的展现形式。

`:hover`在当用户使用一个指针式设备指向一个元素但是并没有触发它的时候产生效果。大部分情况下这指的是使用一个鼠标，并让光标悬浮在一个元素上。

`:active`在当一个元素被用户触发的时候生效。对于鼠标用户来说这指的是你按下鼠标的按钮，并且保持按下的状态，直到你释放它为止。

`:focus`在当一个元素拥有焦点的时候会生效，比如当它接收键盘事件的时候。

一个元素可以同时匹配多个伪类。一个元素可以拥有焦点，同时又可以被鼠标悬浮，比如：

	input[type=text]:focus {
		color:#000;
		background:#ffe;
	}
	input[type=text]:focus:hover {
		background:#fff;
	}

第一个规则将会匹配拥有焦点的`input`元素，第二个规则将会匹配同样的元素当鼠标悬浮在它们上方时候。

###:lang

语言伪类可以用来给内容被定义在一个指定语言（人类语言，不是标记语言）中的元素加样式。下面的规则定义了在瑞典语中一个行内引用使用哪个引号：

	q:lang(sv) { quotes: "\201D" "\201D" "\2019" "\2019"; }

一个文档或者元素中使用的人类语言通常在HTML中是通过`lang`属性来指定的，在XHTML中是通过`xml:lang`属性来指定的。

##伪元素

伪元素可以让作者访问和格式化不能通过文档树中节点来访问的部分。

##:first-line

`:first-line`伪元素会对文本段落中的第一行产生影响。它只能应用到块级元素，行内块（inline-block）元素，表格标题或者表格单元格。

第一行的长度明显的取决于多种因素，包括字体大小和包含此文本元素的宽度。

下面规则将会应用到一个段落中的第一行文本：

	p:first-line {
		font-weight:bold;
		color;#600;
	}
	
注意可以应用到`:first-line`伪元素上的属性有一些限制。请查看[:first-line伪元素](http://www.w3.org/TR/CSS21/selector.html#first-line-pseudo)文档来获取可以应用的属性列表。

##:first-letter

这个伪元素可以让你定位一个元素的第一个字母或者数字，能够应用到块（block），列表项目，表格单元格，表格标题和行内块（inline-block）等元素上。

下面的规则将会应用到类名为“preamble”的元素的第一个字符：

	.preamble:first-letter {
		font-size:1.5em;
		font-weight:bold;
	}

像`:first-line`伪元素一样，`:first-letter`伪元素对于哪些属性可以应用也有一些限制。请查看[:first-letter伪元素](http://www.w3.org/TR/CSS21/selector.html#first-letter)的文档来获取可以应用的属性列表。

##:before and :after

在更多的经过讨论的CSS特性中，`:before`和`:after`伪元素可以用来向一个元素内容的前面或者后面插入生成好的内容。

这是`:before`如何被使用（来自我的文章[Custom borders with advanced CSS](http://www.456bereastreet.com/archive/200509/custom_borders_with_advanced_css/)）的一个例子：

	.cbb:before {
		content:"";
		display:block;
		height:17px;
		width:18px;
		background:url(top.png) no-repeat 0 0;
		margin:0 0 0 -18px;
	}
	
一个使用`:after`在一个链接文本后面插入一个链接的URL的例子：

	a:link:after {
		content: " (" attr(href) ") ";
	}

##IE问题

在你开始把已经学到的关于选择器的所有东西付诸使用之前，注意IE浏览器直到并且包括版本6在内，对于CSS2.1选择器的支持并不完整。下面是不支持或者有问题的：

* 子选择器
* 邻居兄弟选择器
* 属性选择
* 多个类选择器
* `:first-child`伪类
* 语言伪类
* `:before`和`:after`伪元素
* `:hover`伪类只在`a`元素中工作
* `:focus`伪类不支持。对于获得焦点的`a`元素，`:active`伪类会被应用。

幸运的是，看起来IE7将会对CSS2.1进行完整的支持。

##你已经有能量了 - 现在开始使用它

这就是所有的内容了。现在你已经学了所有关于CSS2.1选择器的内容，你已经有了相当多的能量可以使用当你设计你的文档的时候。为了保持你的标记整洁，并且增加可访问性和可用性，请小心的使用。并且记得思考在那些不支持更高级CSS选择器的旧浏览器中将会发生什么。