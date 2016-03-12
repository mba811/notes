---
layout: post
title: 响应式 web 设计 (Responsive web design) 三步曲
tags: [notes]
---

原文：[http://webdesignerwall.com/tutorials/responsive-design-in-3-steps](http://webdesignerwall.com/tutorials/responsive-design-in-3-steps)


响应式web设计无可厚非现在是个很时髦的技术。如果你仍然对响应式设计不熟悉的话，看下我之前发表过的[响应式站点](http://webdesignerwall.com/trends/inspiration-fluid-responsive-design)列表。对于新手，[响应式设计](http://webdesignerwall.com/tutorials/responsive-design-with-css3-media-queries)可能听起来有一点复杂，但是它实际上比你想的要简单。为了帮助你快速的上手响应式设计，我写了一个快速上手教程。我保证你通过三步就可以学会响应式设计的基本逻辑和媒体查询（media query）（假设你有基本的CSS知识）。

#第一步　 Meta标签（看[demo](http://webdesignerwall.com/demo/responsive-design/index.html)）

大部分移动浏览器会把HTML页面缩放成较宽的viewport的宽度，这样内容就可以屏幕上正确的展示了。你可以使用`viewport`这个meta标签来重置这个行为。下面的viewport标签告诉浏览器使用设备宽度（device-width）做为viewport的宽度，并且禁用初始的缩放比例。在`<head>`中加入这个meta标签。
	
	<meta name="viewport" content="width=device-width, initial-scale=1.0">

Internet Explorer 8或者更老的浏览器不支持媒体查询。你可以使用[media-queries.js](http://code.google.com/p/css3-mediaqueries-js/)或者[respond.js](https://github.com/scottjehl/Respond)来在IE中添加对媒体查询的支持。

	<!--[if lt IE 9]>
		<script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
	<![endif]-->
	
#第二步　HTML结构

在这个例子中，我拥有一个由头部，内容容器，侧边栏，以及一个底部构成的基本的页面布局。头部拥有一个固定的180px高度，内容容器600px宽，然后侧边栏是300px宽。

![](http://7q5cfr.com1.z0.glb.clouddn.com/page-structure.png)


#第三步　媒体查询（Media Query）

[CSS3媒体查询](http://webdesignerwall.com/tutorials/css3-media-queries) 是进行响应式设计的戏法。它跟写if条件一样，来告诉浏览器对于特定的viewport宽度如何渲染页面。

下面的规则集在当viewport宽度小于等于980px的时候生效。基本上，我把所有容器的宽度从像素值改成了百分比值，这样容器就会变得具有流动性(fluid)。

![](http://7q5cfr.com1.z0.glb.clouddn.com/980px-or-less.png)

然后对于宽度小于或等于700px的viewport，指定`#content`和`#sidebar`为自动宽度，并且移除浮动，所以他们可以以全宽度进行展示。

![](http://7q5cfr.com1.z0.glb.clouddn.com/700px-or-less.png)

对于宽度小于等于480px（移动设备屏幕）的，重置`#header`的高度为`auto`，修改`h1`的字体大小为24px，并且隐藏`#sidebar`。

![](http://7q5cfr.com1.z0.glb.clouddn.com/480px-or-less.png)

你可以想写多少媒体查询就写多少。我在demo中只展示了三个媒体查询。媒体查询的目的是对于指定的viewport宽度可以通过应用不同的CSS规则来获得不同的布局。媒体查询可以在同一个样式表中或者在一个单独的文件中。

#总结

这个教程打算告诉你响应设计的基础知识。如果你想要看更深入的教程，看看我上一篇教程：[《利用媒体查询进行响应式设计》](/responsive-design-with-css3-media-queries.html)。


