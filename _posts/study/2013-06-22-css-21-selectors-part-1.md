---
layout: post
title: CSS 2.1选择器第一部分
tags: [study]
---

原文：[http://www.456bereastreet.com/archive/200509/css_21_selectors_part_1/](http://www.456bereastreet.com/archive/200509/css_21_selectors_part_1/)

#CSS 2.1 选择器，第一部分

当你开始使用CSS的时候，最先接触的东西是选择器。选择器显然是CSS的基础内容，但是很少有开发者能发挥其全部的潜力。当然你只使用类型（type）, ID, 类（class）就可以完成大量的工作，但是选择器还有更多的内容。

学习如何正确使用CSS 2.1中所有可用的选择器能够真正的帮助你保持你的HTML更加整洁。它将最小化不必要的class属性以及额外添加到标记中的div和span元素。听起来不错，是吧？

那为什么所有的选择器没有被广泛的使用呢？最重要的原因是包括IE6在内的IE浏览器对其的支持不足。其他现在的浏览器都支持大部分或者所有的CSS 2.1选择器。在使用这系列文章介绍的一切内容之前，请注意一下这个问题。（译者注：浏览器支持情况请见[quirksmode](http://www.quirksmode.org/css/selectors/)）

好消息是Internet Explorer 7将会对选择器有更好的支持。你将来可以使用它们了，现在正好是学习所有可用选择器的最佳时间。

因为有如此多的CSS选择器，用一篇文章来解释它们会变的非常的冗长。为了使信息更容易的被消化，我把它们分成了三个部分：

* 第一部分，也就是这篇文章，解释选择器的基础加上通配（universal）, 类型（type）, id和类（class）选择器。
* 第二部分是关于组合因子（combinators），合并选择器（ combined selectors），分组（grouping），以及属性选择器。
* 第三部分将会是关于伪类（pseudo-classes）和伪元素（pseudo-elements）。

我将会在几周内发表这些文章，并且在它们发表的时候挨个更新其指向其他部分的链接。

好了。我们开始了。

##选择器基础

先看看最基础的。一个CSS选择器是由一个用来匹配文档树中所有元素的模式组成的。当模式中的所有条件都为true的时候，这个选择器就完成了匹配，然后其定义的规则将会被应用在匹配的一个或多个匹配元素上。看下这个非常简单的CSS规则：

	p { color:#f00; }

选择器是CSS规则中大括号开始之前（"{"）的部分。这儿的选择器是`p`，其将会匹配文档中的所有`p`元素，并且使其包含的所有文本变成红色。非常基础。
	
##选择器概览

好了，刚才是一个非常简单的例子。接下来我将会描述其他的选择器，事情显然将会变得有些复杂。在开始之前，看下CSS 2.1所有选择器语法的一个概览（基于CSS 2.1, 5.1 Pattern matching中的表格内容）：

<table>
    <caption>CSS 2.1选择器语法概览</caption>
    <thead>
        <tr>
            <th scope="col">选择器类型</th>
            <th scope="col">模式</th>
            <th scope="col">描述</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>通用（Universal）</td>
            <td>*</td>
            <td>匹配任何元素。</td>
        </tr>
        <tr>
            <td>类型（Type）</td>
            <td>E</td>
            <td>匹配任意E元素。</td>
        </tr>
        <tr>
            <td>类（Class）</td>
            <td>.info</td>
			<td>匹配任何<code>class</code>属性中包含<code>info</code>值的元素。</td>
        </tr>
        <tr>
            <td>ID</td>
            <td>#footer</td>
            <td>匹配任何 <code>id</code>等于<code>footer</code>的元素。</td>
        </tr>
        <tr>
            <td>后代（Descendant）</td>
            <td>E F</td>
            <td>匹配任何是E元素后代的F元素。</td>
        </tr>
        <tr>
            <td>孩子（Child）</td>
            <td>E &gt; F</td>
            <td>匹配任何是E元素孩子的F元素。</td>
        </tr>
        <tr>
            <td>邻居（Adjacent）</td>
            <td>E + F</td>
            <td>匹配任何前面是兄弟元素E的F元素。</td>
        </tr>
        <tr>
            <td>属性（Attribute）</td>
            <td>E[att]</td>
			<td>匹配任何拥有<code>att</code>属性的E元素，无论属性的值是什么。</td>
        </tr>
        <tr>
            <td>属性（Attribute）</td>
            <td>E[att=val]</td>
            <td>匹配任何<code>att</code>属性值等于<code>val</code>的E元素。</td>
        </tr>
        <tr>
            <td>属性（Attribute）</td>
            <td>E[att~=val]</td> <td>匹配任何E元素，其<code>att</code>属性值是一个以空格分割的列表，其中之一的值等于<code>val</code>。</td>
        </tr>
        <tr>
            <td>属性（Attribute）</td>
            <td>E[att|=val]</td> <td>匹配任何E元素，其<code>att</code>属性值是以连字符（"-"）分割的列表，其中的值以<code>val</code>开头。</td>
        </tr>
        <tr>
            <td>:first-child伪类</td>
            <td>E:first-child</td>
            <td>匹配任何E元素，当E是其父元素的第一个孩子元素。</td>
        </tr>
        <tr>
            <td>链接（link）伪类</td>
            <td>E:link<br>E:visited</td>
            <td>匹配没有访问过（:link）的或者已经访问过（:visited）的链接。</td>
        </tr>
        <tr>
            <td>动态伪类</td>
            <td>E:active<br>E:hover<br>E:focus </td>
            <td>匹配特定用户行为的E元素。</td>
        </tr>
        <tr>
            <td>:language伪类</td>
            <td>E:lang(c)</td>
            <td>匹配使用语言c的E元素。</td>
        </tr>
        <tr>
            <td>:first-line伪元素</td>
            <td>E:first-line</td>
            <td>匹配E元素第一行的内容。</td>
        </tr>
        <tr>
            <td>:first-letter伪元素</td>
            <td>E:first-letter</td>
            <td>匹配E元素的第一个字母。</td>
        </tr>
        <tr>
            <td>:before和:after伪元素</td>
            <td>E:before<br>E:after</td>
            <td>用来向元素内容前面或者后面插入创建的内容。</td>
        </tr>
    </tbody>
</table>

我将会在这两部分文章中更加详细的介绍这些选择器类型的每一个，所以请继续阅读。表格中和这篇文章之后使用的一些术语可能需要一些说明：

####后代
文档树中一个元素的孩子，孙子或者后代元素。

####祖先
文档树中一个元素的父亲，祖父或者祖先元素。

####孩子
一个元素的直接后代。文档树中此二者之间没有其他元素。

####父亲
一个元素的直接祖先。文档树中此二者之间没有其他元素。

####兄弟
拥有同样父亲元素的元素。


##简单选择器和组合选择器（Simple and combined selectors）

选择器有两个基本的分类：简单（simple）和组合（combined）。

一个简单选择器（simple selector）由一个类型选择器（type selector）或者通配选择器（universal selector）开头，后面跟着零个或多个属性选择器（attribute selectors），ID选择器（ID selectors），或者伪类（pseudo-classes）。下面的规则包含一个简单选择器的例子：
	
	p.info { background:#ff0; }

一个组合选择器（combined selector，有时候叫上下文选择器）由两个或者多个以连接符分割的简单选择器构成。这是一个非常简单的组合选择器的例子：

	div p { font-weight:bold; }
	
以上规则将会对应用到所有是div元素的后代的p元素上。

一个伪元素可以加在一个选择器后面。在一个组合选择器中，一个伪元素只能加在最后一个简单选择器后面。
组合选择器，连接符，以及伪类的细节可以在此系列的第二部分和第三部分找到。

##通配选择器

通配选择器以星号，“\*”来展现，其可以匹配文档中的所有元素。在样式表中很难看见通配选择器，但是它实际上经常是和类选择器与ID选择器合用。如果通配选择器不是一个简单选择器中的唯一组件，“*”将会被忽略：

*  `.myclass`等于`*.myclass`
*  `#myid`等于`*#myid`

通配选择器中的一个用法已经变得非常流行，那就是用它来把所有元素的外边距（margin）和内边距（padding）都设置为0：

	* { margin:0; padding:0; }

这个情况有时候会以[Global White Space Reset](http://leftjustified.net/journal/2004/10/19/global-ws-reset/)被提及到。

##类型选择器

一个类型选择器会匹配特定元素类型的所有实例。下面的规则会匹配文档中所有的段落元素并且设置它们的字体大小为`2em`：
	
	p { font-size:2em; }
	
##类选择器

类选择器是以一个句号“.”来表示的，并且用`class`属性来定位目标元素。下面的规则将会应用到所有拥有“info”类名的`p`元素：

	p.info { background:#ff0; }

你可以把多个类名赋给一个元素 - `class`属性可以拥有以空格分割的类名列表。类选择器可以用来定位只拥有多个类名的元素。下面的规则将会匹配其类名列表中同时包含“info”和“error”的`p`元素：

	p.info.error { color:#900; font-weight:bold; }
	
**注意**：多类选择器在当前版本的IE浏览器（译者注：IE6及以下版本）中不工作，但是在IE7中会被支持。

元素类型没有必要必须指定。如果留空，那么其效果和使用通配选择器来代替类型选择器是一样的。下面的规则将会匹配任何拥有“info”类名的元素，不管它们的类型：
	
	.info { background:#ff0; }

##ID选择器

ID选择器是以hash符号“#”来表示的，并且用`id`属性的值来定位目标元素。下面的规则将会应用到一个`id`为“info”的元素，不管它们的类型是什么：
	
	#info { background:#ff0; }
	
如果你同时指定了一个元素类型，这个规则将会只应用到那个类型的元素上：

	p#info { background:#ff0; }

很重要的是请记住ID选择器比类选择器优先级更高，并且一个文档中的一个id值必须是唯一的。因此一个ID选择器只能应用到一个文档中的一个元素上。

##待续

好了，以上就是这个系列文章的第一部分。在[第二部分](/css-21-selectors-part-2.html)中我将要带你们看看组合因子（combinators）, 组合选择器，分组，和属性选择器，然后第三部分我将会近距离的看一下伪类和伪元素。