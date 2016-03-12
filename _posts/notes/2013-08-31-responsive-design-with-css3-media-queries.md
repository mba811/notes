---
layout: post
title: 利用媒体查询进行响应式设计
tags: [notes]
---

原文：[http://webdesignerwall.com/tutorials/responsive-design-with-css3-media-queries](http://webdesignerwall.com/tutorials/responsive-design-with-css3-media-queries)

如今屏幕分辨率的范围已经从320px（iPhone）涵盖到2560px（大显示器）或者更高了。用户不单单在桌面电脑上浏览网站。用户如今会使用移动电话，小的笔记本，平板设备（比如iPad或者Playbook）来访问互联网。所以传统的固定宽度设计不再适用了。web设计需要有自适应能力。页面布局要可以自动的去适应所有的分辨率和设备。这个教程将会告诉你如何利用HTML5和CSS3媒体查询来创建一个跨浏览器的响应式设计。

#首先看个实例
在你开始之前，看下[最终demo](http://webdesignerwall.com/demo/adaptive-design/final.html)是什么样子。改变你浏览器的大小，然后看看页面布局在基于viewport（浏览器可视区域）宽度的情况下是如何自动的进行浮动的。

[![](http://{{ site.cdn }}/images/rwd-css3-mq/final-demo.jpg)](http://webdesignerwall.com/demo/adaptive-design/final.html)

##更多例子
如果你想看更多的例子，看一下下面我用媒体查询设计的[WordPress模板](http://themify.me/)：[Tisa](http://themify.me/themes/tisa)，[Elemin](http://themify.me/themes/elemin)，[Suco](http://themify.me/themes/suco)，[iTheme2](http://themify.me/themes/itheme2)，[Funki](http://themify.me/themes/funki)，[Minblr](http://themify.me/themes/minblr)和[Wumblr](http://themify.me/themes/wumblr)。


#概览

对于任何宽度大于1024px的分辨率，页面容器的宽度会为980px。媒体查询被用来检查如果viewport窄于980px，那么页面布局会变成流动宽度而不是固定宽度。如果viewport窄于650px，那么页面布局将会把内容容器和侧边栏展开为整体宽度，从而形成一个单栏的布局。

![](http://{{ site.cdn }}/images/rwd-css3-mq/design-overview.jpg)

#HTML代码
我不会去讲HTML代码的细节。下面是页面布局的整体结构。我拥有一个pagewrap容器，它把header，content，sidebar，footer包裹在了一起。

	<div id="pagewrap">
		<header id="header">
			<hgroup>
				<h1 id="site-logo">Demo</h1>
				<h2 id="site-description">Site Description</h2>
			</hgroup>
			<nav>
				<ul id="main-nav">
					<li><a href="#">Home</a></li>
				</ul>
			</nav>
			<form id="searchform">
				<input type="search">
			</form>
		</header>
	
		<div id="content">
			<article class="post">
				blog post
			</article>
		</div>
	
		<aside id="sidebar">
			<section class="widget">
				 widget
			</section>
		</aside>

		<footer id="footer">
			footer
		</footer>
	</div>
	
##HTML5.js
注意一下我在demo中使用了HTML5标签。低于9的IE浏览器不支持HTML5中引入的新元素，比如 `<header>`，`<article>`，`<footer>`，`<figure>`等等。在HTML文档中包含[html5.js](http://code.google.com/p/html5shiv/)这个Javscript文件可以使IE识别这些新元素。
		
	<!--[if lt IE 9]>
		<script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
	<![endif]-->
	
#CSS

##重置HTML5元素为块元素
下面的CSS将会把HTML元素（article，aside，figure，header，footer等等）重置为块元素。

	article, aside, details, figcaption, figure, footer, header, hgroup, menu, nav, section { 
	    display: block;
	}

##主结构CSS

这次我还是不会去讲细节。主容器pagewrap是980px宽。Header拥有一个固定的160px高度。容器content是600px宽并且向左浮动。sidebar是280px宽并向右浮动。

	#pagewrap {
		width: 980px;
		margin: 0 auto;
	}

	#header {
		height: 160px;
	}

	#content {
		width: 600px;
		float: left;
	}

	#sidebar {
		width: 280px;
		float: right;
	}

	#footer {
		clear: both;
	}

##第一步的Demo

这里是这个设计[demo](http://webdesignerwall.com/demo/adaptive-design/demo-step1.html)。注意媒体查询还没有实现。改变浏览器窗口的尺寸，你应该看到页面布局并不具有扩展能力。

#有关CSS3媒体查询

现在是有趣的部分－－[媒体查询](http://webdesignerwall.com/tutorials/css3-media-queries)

##包含媒体查询的JavaScript文件

Internet Explorer8或者更老的版本不支持CSS3媒体查询。你可以通过添加[css3-mediaqueries.js](http://code.google.com/p/css3-mediaqueries-js/)这个Javascript文件来使其支持媒体查询。

	<!--[if lt IE 9]>
		<script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
	<![endif]-->

##包含媒体查询的CSS文件

为媒体查询创建一个新的样式表。看下我之前的教程来搞清楚[媒体查询](http://webdesignerwall.com/tutorials/css3-media-queries)是如何工作的。

	<link href="media-queries.css" rel="stylesheet" type="text/css">

##Viewport小于980px（流动布局）

对于窄于980px的viewport，如下的规则将会被应用：

* pagewrap = 重置width为95%
* content = 重置width为60%
* sidebar = 重置width为30%

提示: 使用百分比（%）的值来使容器变得流动。

	@media screen and (max-width: 980px) {
		#pagewrap {
			width: 95%;
		}

		#content {
			width: 60%;
			padding: 3% 4%;
		}

		#sidebar {
			width: 30%;
		}
		#sidebar .widget {
			padding: 8% 7%;
			margin-bottom: 10px;
		}
	}

##Viewport小于650px（一栏布局）

接下来对窄于650px的viewport我拥有另一个CSS集合：

* header = 重置height为auto
* searchform = 重新定位searchform为离顶部5px
* main-nav = 重置`position`为`static`
* site-logo = 重置`position`为`static`
* site-description =  重置`position`为`static`
* content = 重置width为auto（这会使得容器展开为整体宽度）并且不进行浮动
* sidebar = 重置width为100%并且不进行浮动
<!-- hack -->
	@media screen and (max-width: 650px) {
		#header {
			height: auto;
		}

		#searchform {
			position: absolute;
			top: 5px;
			right: 0;
		}

		#main-nav {
			position: static;
		}

		#site-logo {
			margin: 15px 100px 5px 0;
			position: static;
		}

		#site-description {
			margin: 0 0 15px;
			position: static;
		}

		#content {
			width: auto;
			float: none;
			margin: 20px 0;
		}

		#sidebar {
			width: 100%;
			float: none;
			margin: 0;
		}

	}
	
##小于480px的Viewport
下面的CSS将会在viewport宽度小于480px（即横屏模式下iPhone屏幕的宽度）的时候生效。

* html = 禁止文本大小调整（text size adjustment）。默认情况下，iPhone放大了文本大小，这样读起来更加舒服。你可以通过添加`-webkit-text-size-adjust: none`来禁止文本大小调整。
* main-nav = 重置字体大小为90%
<!-- hack -->
	@media screen and (max-width: 480px) {

		html {
			-webkit-text-size-adjust: none;
		}

		#main-nav a {
			font-size: 90%;
			padding: 10px 8px;
		}

	}

#弹性图片
为了使图片具有弹性，只需要添加`max-width:100%`和`height:auto`。给图片加上`max-width:100%`和`height:auto`在IE7中是工作的，但是在IE8中不工作（是的，另一个奇怪的IE　bug）。为了解决这个问题，你需要为IE8添加`width:auto\9`。

	img {
		max-width: 100%;
		height: auto;
		width: auto\9; /* ie8 */
	}
	
#弹性的嵌入视频
为了使嵌入视频具有弹性，可以使用上面所提到的相同技巧。由于未知原因，（嵌入元素的）` max-width:100%`在Safari中不工作。解决方式是使用`width:100%`做为替代。

	.video embed,
	.video object,
	.video iframe {
		width: 100%;
		height: auto;
	}
	
#进行初始缩放的Meta标签（iPhone）

默认情况下，iPhone中的Safari会收缩HTML页面来适应iPhone屏幕。下面的meta标签告诉iPhone中的Safari使用设备的宽度做为viewport的宽度，并且禁用初始缩放比例。

	<meta name="viewport" content="width=device-width; initial-scale=1.0">

#最终Demo

查看最终demo并且调整你浏览器窗口的大小来看看真实工作的媒体查询。不要忘记用iPhone，iPad，Blackberry（新版本）和Android电话来访问demo，以便看看移动版本的样子。

[![](http://{{ site.cdn }}/images/rwd-css3-mq/final-demo.jpg)](http://webdesignerwall.com/demo/adaptive-design/final.html)

#总结
* 媒体查询的Javascript备胎：

css3-mediaqueries.js是使那些不支持媒体查询的浏览器可以使用媒体查询所必需的。

	<!--[if lt IE 9]>
		<script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script>
	<![endif]-->


* CSS媒体查询：

创建自适应设计的手段是根据viewport的宽度来用CSS重写页面布局结构。

	@media screen and (max-width: 560px) {

		#content {
			width: auto;
			float: none;
		}

		#sidebar {
			width: 100%;
			float: none;
		}

	}
	
* 具有弹性的图片：

使用`max-width:100%`和`height:auto`来使图片变得具有弹性。

	img {
		max-width: 100%;
		height: auto;
		width: auto\9; /* ie8 */
	}

* 具有弹性的嵌入视频：

使用`width:100%`和`height:auto`使嵌入视频具有弹性。

	.video embed,
	.video object,
	.video iframe {
		width: 100%;
		height: auto;
	}
	
* Webkit字体大小调整（Text Size Adjust）：

在iPhone上使用`-webkit-text-size-adjust:none`来禁用文本大小调整。

	html {
		-webkit-text-size-adjust: none;
	}

* 重置iPhone的Viewport和初始缩放比例：

下面的meta标签在iPhone上重置viewport和初始缩放比例：

	<meta name="viewport" content="width=device-width; initial-scale=1.0">
