---
layout: post
title: 跨浏览器的 Inline-Block
tags: [notes]
---

原文：[http://blog.mozilla.org/webdev/2009/02/20/cross-browser-inline-block/](http://blog.mozilla.org/webdev/2009/02/20/cross-browser-inline-block/)

啊，inline-block，如此的让人难以琢磨，并且又有诱人的显示方式宣传，只是其承诺的很多，兑现的却如此少。我已经收到这样的PSD文件有很多次：

![gallery-view](http://blog.mozilla.org/webdev/files/2009/02/gallery-view.jpg "gallery-view")

看了之后我就开始哭了。

通常情况下，这个类型的布局是小菜一碟。固定宽度，固定高度，左侧浮动（`float:left`），然后就完成了。但但但是，这个设计需要与可变数量的内容一起工作，也就是说如果这些块中的一个比其他块拥有更多的内容，它将会破坏整个布局：

![float-broken](http://blog.mozilla.org/webdev/files/2009/02/float-broken.jpg "float-broken")

由于第一个画廊项目比其他的高，所以第五个项目就相对于它进行左侧浮动而不是位于它下面。总体上来说我们想要一个拥有表格一样伸缩性的布局，但真正合适的应该是一个语义标记。

我们以拥有一个无序列表，并且`display`被设置为`inline-block`的一个简单页面开始：

	<ul>
	    <li>
	        <h4>This is awesome</h4>
	        <img src="http://farm4.static.flickr.com/3623/3279671785_d1f2e665b6_s.jpg"
	        alt="lobster" width="75" height="75"/>
	    </li>
	...
	<ul>

	<style>
	    li {
	        width: 200px;
	        min-height: 250px;
	        border: 1px solid #000;
	        display: inline-block;
	        margin: 5px;
	    }
	</style>
	
在Firefox 3，Safari 3和Opera中看起来OK：

![step1](http://blog.mozilla.org/webdev/files/2009/02/step1.jpg "step1")

明显的，垂直对齐有点问题。恩，但这并不是真正的有问题，因为这是正确的行为，但这不是我们想要的。

这儿发生的事情是每个`<li>`的[baseline](http://dev.w3.org/csswg/css3-linebox/#baseline)是以父元素`<ul>`的baseline来对齐的。什么是baseline，你问道？一图抵千言：

![baseline](http://blog.mozilla.org/webdev/files/2009/02/baseline.gif)

baseline是那条穿过上面文字的黑线。行内（inline）或者行内块（inline-block）元素的[默认垂直对齐的值](http://www.w3.org/TR/CSS21/visudet.html#propdef-vertical-align)是baseline，也就是说元素的baseline将会与其父元素的baseline进行对齐。这是第一个包含基准线在内的行内块：

![baseline-inline-block](http://blog.mozilla.org/webdev/files/2009/02/baseline-inline-block.jpg)

你可以看见，每个基准线都和文本“This is the baseline”的基准线所对齐。那个文本并没有在一个`<li>`之中，它只是父元素`<ul>`的一个简单的文本节点，用来标识父元素的基准线在哪。

但是，解决这个问题的方法很简单：`vertical-align:top`，其效果是一个好看的网格：
![inline-block-2](http://blog.mozilla.org/webdev/files/2009/02/inline-block-2.jpg)

除了它仍然不在Firefox 2，IE 6和7中工作以外。

![inline-block-ff2](http://blog.mozilla.org/webdev/files/2009/02/inline-block-ff2.jpg)

让我们来开始处理Firefox 2。

Firefox 2不支持行内块，但是它支持一个显示效果与行内块一样，Mozilla所特有的显示属性`-moz-inline-stack`。当我们把它加到`display:inline-block`前面，FF2将会忽略那个声明（译者注：即`display:inline-block`）并保持`-moz-inline-stack`，因为它不支持行内块。支持行内块的浏览器将会使用它（译者注：`display:inline-block`），并且忽略前面的显示属性（译者注：`-moz-inline-stack`）。

	<style>
	    li {
	        width: 200px;
	        min-height: 250px;
	        border: 1px solid #000;
	        display: -moz-inline-stack;
	        display: inline-block;
	        vertical-align: top;
	        margin: 5px;
	    }
	</style>
	
不幸的是，它有一个小bug：

![inline-block-3](http://blog.mozilla.org/webdev/files/2009/02/inline-block-3.jpg)

老实说，我不知道什么导致了这个bug。但是这儿有快速解决的方法。把`<li>`中的所有内容包含在一个`<div>`之中。
	
	<li>
	        <div>
	            <h4>This is awesome</h4>
	            <img src="http://farm4.static.flickr.com/3623/3279671785_d1f2e665b6_s.jpg"
	            alt="lobster" width="75" height="75"/>
	        </div>
	</li>

这好像`reset`了`<li>`中的一切，然后适当的展示它们。
	
![inline-block-2](http://blog.mozilla.org/webdev/files/2009/02/inline-block-2.jpg)

现在，我们来看IE7。IE7不支持行内块，但是我们可以用小把戏让它来渲染`<li>`，就好像它们是行内块一样。应该怎么做？[hasLayout](http://haslayout.net/haslayout)，一个为所有乐趣而生的IE魔法属性。你不能以`hasLayout:true;`或者以任何类似这样的形式给一个元素明确的设置`hasLayout`，但是你可以使用其他的声明比如`zoom:1`来触发它。
	
从技术上来说，`hasLayout`意味着一个设置了`hasLayout`的元素要对自己和其子元素的渲染负责（把它们通过`min-height`和`width`属性组合起来，然后你就得到了与`display:block`很像的行为）。它就像魔法仙尘一样，你可以把它们洒在渲染问题上，然后这些问题就烟消云散了。

当我们向`<li>`元素中加入`zoom:1`和`*display:inline`（[star hack to target IE6 & 7)](http://www.ejeliot.com/blog/63)）的时候，我们就使得IE7以`inline-block`的形式显示它们：
	
	<style>
	    li {
	        width: 200px;
	        min-height: 250px;
	        border: 1px solid #000;
	        display: -moz-inline-stack;
	        display: inline-block;
	        vertical-align: top;
	        margin: 5px;
	        zoom: 1;
	        *display: inline;
	    }
	</style>

![inline-block-ie7](http://blog.mozilla.org/webdev/files/2009/02/inline-block-ie7.jpg)

呼！（译者注：松口气）基本上完成了。就剩下IE 6了：

![inline-block-ie6](http://blog.mozilla.org/webdev/files/2009/02/inline-block-ie6.jpg)

IE 6不支持`min-height`，但是很感谢（译者注：反讽）其对`height`属性的不正确处理，所以我们可以用它来代替。设置`_height`（[IE6 underscore hack](http://www.ejeliot.com/blog/63)）的值为250px将会使所有`<li>`元素的高度为250px，并且如果它们内容超过了这个值，它们将会展开来进行适配。所有其他浏览器会忽略`_height`。
	
那么经过所有的努力之后，这就是最终的CSS和HTML了：

	<style>
	    li {
	        width: 200px;
	        min-height: 250px;
	        border: 1px solid #000;
	        display: -moz-inline-stack;
	        display: inline-block;
	        vertical-align: top;
	        margin: 5px;
	        zoom: 1;
	        *display: inline;
	        _height: 250px;
	    }
	</style>

	<li>
	        <div>
	            <h4>This is awesome</h4>
	            <img src="http://farm4.static.flickr.com/3623/3279671785_d1f2e665b6_s.jpg"
	            alt="lobster" width="75" height="75"/>
	        </div>
	</li>
