---
layout: post
title: 不是像素的像素不是像素
tags: [notes]
---

原文：[http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html](http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html)

译者：看标题晕菜了吧？哈哈。

**昨天（译者：原文成于2010年4月）[John Gruber说了一下](http://daringfireball.net/2010/04/why_960_by_640)关于新iPhone提高像素密度的事情（以960x640替代480x320），并且分析了苹果为什么这么做。他也预测了这对于web开发者来说意味着是什么。**

我现在刚好对移动电话上的浏览器宽度和高度进行深入研究，所以我可以很有把握的说这些变化中的99%不会对web开发者产生任何影响。

剩下的1%可能会有一些诡异，但是我期望苹果将会引入一个像素中间层，以此来解决这个问题。（后来[John指出](http://daringfireball.net/linked/2010/04/19/android-dips)Android平台上已经存在了类似的中间层。） 

在我们开始之前提前声明：因为它们对于web开发者来说并不重要，所以我几乎忽略了正规的屏幕尺寸，并且我也没有真正的讨论显示器的原理，像素密度，以及其他复杂概念。
提前对我可能在这使用的错误术语表示歉意。

#web开发者需要什么

我很确定web开发者到底对什么感兴趣。他们需要的是CSS像素。是的，就是在CSS的声明比如`width: 300px`或者`font-size: 14px`中使用的「像素」。

这些像素跟设备的实际像素密度甚至是传闻中将会到来的中间层没有任何关系。它们本质上是为我们web开发者创建的一个抽象结构。

解释它最简单的方式就是想想缩放。如果用户进行放大操作，一个拥有`width: 300px`的元素将会占有屏幕越来越多的空间，以设备（物理）像素进行度量的话就会变得越来越宽。但对于CSS像素来说，无论怎样，宽度还是300px，缩放效果是通过尽可能的对CSS像素进行拉伸来实现的。

当缩放比例正好为100%的时候，一个CSS像素等于一个设备像素（尽管将要到来的中间层将会替换这儿的设备像素。）下图描绘了这个情形。这儿看不到更多的像素，因为一个CSS像素完全覆盖了一个设备像素。

![csspixels_100](http://{{ site.cdn }}/images/a-pixel-is-not/csspixels_100.gif)

（_我应该适当告诉你们一下“zoom 100%”在web开发中几乎没有意义。缩放等级对于我们来说并不重要；我们需要知道的是屏幕上现在有多少个CSS像素。_）

接下来的两幅图说明了当用户缩放的时候会发生什么。第一幅图展示的是当用户进行缩小操作的时候设备像素（深蓝色背景）和CSS像素（半透明前景）的样子。CSS像素变得小了；一个设备像素覆盖了多个CSS像素。第二幅图展示的当用户进行放大操作时候设备像素和CSS像素的样子。一个CSS像素现在覆盖了多个设备像素。

![csspixels_out](http://{{ site.cdn }}/images/a-pixel-is-not/csspixels_out.gif)
![csspixels_in](http://{{ site.cdn }}/images/a-pixel-is-not/csspixels_in.gif)

所以我们那些拥有`width: 300px`的元素实际上一直是300个CSS像素宽，而且一个CSS像素等于多少个设备像素是根据当前的缩放比例来决定的。
（你可以在iPhone上用`screen.width`除以`window.innerWidth`来计算出缩放比例。在这方面浏览器普遍不兼容；期望在不远的未来会有一个完整的报告。并且，做为一个web开发者你对缩放比例没有兴趣，但是对设备屏幕上有多少个像素（设备像素或者CSS像素）感兴趣。）

这个体系将不会发生变化。如果变了的话，所有针对iPhone进行过优化的站点将会立刻变得严重错乱，但这正是苹果不惜一切代价想避免的。

所以，一个完全缩小的网站将会仍然以980个CSS像素的形式进行展示，一个CSS像素等于多少个设备像素对于我们来说不重要。

#有点棘手

无论怎样，这儿有两个棘手的地方（tricky bits）：`device-width`媒体查询（media query）和` <meta name="viewport" width="device-width"> `标签。两个都是工作在设备像素下，而不是CSS像素，因为他们是基于web页面的环境，而不是基于设备内部的CSS运行机制。

##媒体查询

`device-width`媒体查询以设备像素为单位对设备的宽度进行度量。`width`媒体查询以CSS像素的形式对页面的整体宽度进行度量，iPhone上其值至少为980px，原因我接下来解释。

![mobile_mediaqueries](http://{{ site.cdn }}/images/a-pixel-is-not/mobile_mediaqueries.jpg)

`device-width`媒体查询按照如下形式工作：

	div.sidebar {
		width: 300px;
	}

	@media all and (max-device-width: 320px) {
		// styles assigned when device width is smaller than 320px;
		div.sidebar {
			width: 100px;
		}

	}

现在sidebar是300个CSS像素宽，除了当设备宽度等于320设备像素或者小于320设备像素，在这种情况下sidebar将会变成100个CSS像素宽。（跟的上吗？这个有点复杂。）

顺便说下，理论上你可以使用以厘米或者英寸为单位的媒体查询对设备屏幕进行查询。不幸的是这个好像并没有被所有的设备所完全支持，甚至iPhone也是。问题在于物理单位比如英寸经常是被转换成（CSS）像素；因此`width: 1in`在我所测试过的浏览器上等于96个像素（这相当多）。所以这些媒体查询不可靠。

# \<meta\>标签

通常情况下`<meta name="viewport" width="device-width">`甚至更有用。这个标签，最早是苹果特有的，但同时也被更多的移动浏览器所支持，使布局视窗（layout　viewport）完全地对设备进行适配。

什么是布局视窗？它是浏览器用来计算拥有百分比宽度，比如`div.sidebar {width: 20%}`的元素尺寸的区域。通常情况它要比设备屏幕稍微大一些：iPhone上为980px，Opera上为850px，Android上为800，等等。	

![mobile_layoutviewport](http://{{ site.cdn }}/images/a-pixel-is-not/mobile_layoutviewport.jpg)

如果你添加` <meta name="viewport" width="device-width">`，那么布局视窗的宽度会被限制为以设备像素为单位的设备的宽度；在iPhone的情况下为320。

如果你的页面能够在屏幕中挤下的话那么这将会产生影响。看下这个没有任何CSS`width`语句，并且没有`<meta>`标签的页面。它充分利用了整个布局视窗可用宽度。

![mq_none](http://{{ site.cdn }}/images/a-pixel-is-not/mq_none.jpg)

这可能不是你想要的。你想要文本漂亮的显示在屏幕上。那是`<meta name="viewport" width="device-width">`的任务。当你添加它的时候，布局视窗会被收缩（为320px在iPhone的情况下），然后文字大小就合适了。

![mq_yes](http://{{ site.cdn }}/images/a-pixel-is-not/mq_yes.jpg)

#苹果做的改变

苹果的分辨率改变了之后对`device-width`媒体查询和`<meta>`标签会产生什么影响？当然我不能很确定，但是我期望对于web开发者来说什么都没有变化。

## \<meta\> 标签

`<meta>`标签是最容易解释的。苹果故意发明它恰恰就是为了能使开发者在iPhone屏幕上适配他们的内容，并且已经把它推送给了开发者。这意味着现在它承受不起像读取设备宽度那样通过`<meta>`标签去修改设备的宽度。

事实上，Nexus One已经开始追随苹果了。它的官方屏幕宽度（肖像模式，即竖排模式）是480px，但是当你应用了`<meta>`标签，它表现出的屏幕宽度是320px，即官方宽度的三分之二。

如果我理解的正确，这就是当John Gruber在[谈论](http://daringfireball.net/linked/2010/04/19/android-dips) Nexus的显示器和它丢失的一个子像素（sub-pixel），以及因此而少了三分之一像素时候所描述的情况。那刚好与Nexus对`<meta>`标签的解释相吻合。

所以事实上Google已经插入了一个似乎叫dips；[设备无关像素](http://developer.android.com/guide/practices/screens_support.html)（device-independent pixels，译者注：应该为density-independent pixel，「密度无关像素」）的层。这个层位于官方屏幕尺寸与web开发者所用的CSS像素之间。

我期望当新的iPhone使用`<meta>`标签进行查询的时候，可以复制Nexus的技巧，并声明屏幕大小为320px（换句话说是正常分辨率的一半）。之所以是一半而不是三分之二是因为新iPhone的像素密度比Nexus高（或者其他）。

##媒体查询

现在只留下`device-width`媒体查询这一个唯一的问题了。Nexus使用`480px`做为屏幕宽度，尽管事实上在这320px更合适。我们不得不看看苹果是怎么做的。

更重要的问题是dips是否也将被用在媒体查询中。总的看来我想说我们需要它；正规的设备尺寸对于web开发者来说并不重要：我们想知道我们可以在屏幕上得到多少内容，从这看来dips是最合适的。

不幸的是Nexus还没有这么做；所有跟媒体查询有关的`device-width`仍然是480px，并不是`320px`。但是也许苹果能够为web开发者解决这个问题。

所以情况对于普通网站和那些使用`<meta>`标签的网站来说很清晰；对于使用媒体查询的网站来说没那么清晰。

敬请期待。