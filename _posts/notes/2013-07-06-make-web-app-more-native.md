---
layout: post
title: 把你的网站改造成一个iOS Web App
tags: [notes]
---

#前言

iOS上的一个Web App（下图中的「念」）和Native App(原生应用)在外观上看起来基本上一样，但是其使用的技术是HTML，CSS，Javascript，而不是原生应用所使用的Objective-C。

![](http://7q5cfr.com1.z0.glb.clouddn.com/app01.png)

本文简单介绍一下如何把一个Web站点改造成iOS上的Web App，这里假设你的网站是响应式设计（responsive design）或者已经做过移动端的适配。

#viewport

我们在HTML中加上viewport(这里假设用户已经对viewport有所了解)　meta标签：

	<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

其中`width=device-width`指的是移动浏览器所显示的宽度等于设备的物理宽度，`initial-scale=1.0`指的是初始缩放倍数为1.0(即不缩放)，`maximum-scale=1.0`指的是最大的缩放倍数是1.0，`user-scalable=no`指用户不可以手动进行缩放。这些参数请根据自己的情况进行调整。本站调整完成后，效果如下：

![](http://7q5cfr.com1.z0.glb.clouddn.com/app02.png)

#Icon

当用户通过safari访问我们网站的时候，用户是可以把网站的URL以一个快捷方式的形式添加到主屏幕的，展示形式跟原生的应用是一样，所以我们要给我们的网站添加应用Icon。

![](http://7q5cfr.com1.z0.glb.clouddn.com/app03.png)

iOS所用的icon是`png`格式的，其提供了`apple-touch-icon`和`apple-touch-icon-precomposed`两种icon，使用方式如下：

	<link rel="apple-touch-icon" href="/apple-touch-icon.png"/>
	<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-precomposed.png"/>
	
以上你只能选其一，二者的区别在于如果使用`apple-touch-icon`，那么iOS会给icon加上一些NB的效果，包括圆角，阴影，反光。如果使用`apple-touch-icon-precomposed`则iOS不会加这个效果。

如果你的网站也要可以在Ipad上访问，那么你还要针对不同的设备准备不同尺寸的icon，你可以通过`sizes`属性来指定icon的尺寸：

	<link rel="apple-touch-icon" href="touch-icon-iphone.png" />
	<link rel="apple-touch-icon" sizes="72x72" href="touch-icon-ipad.png" />
	<link rel="apple-touch-icon" sizes="114x114" href="touch-icon-iphone-retina.png" />
	<link rel="apple-touch-icon" sizes="144x144" href="touch-icon-ipad-retina.png" />
	
如果你不指定`size`属性，那么默认为`57x57`，我们可以看到`ipad`所需icon的尺寸是`72x72`，`retina屏幕的iphone`所需的尺寸是`114x114`，`retina屏幕的ipad`所需的尺寸是`144x144`。

如果没有当前设备所需尺寸的icon，那么iOS将会选用icon中所有大于此设备所需尺寸的最小的一个。如果没有比设备所需尺寸大的icon，那么选用最大的那个icon。如果有多个符合条件的icon，那么iOS会选择有`precomposed`关键词的那个。

如果在HTML中没有指定icon，那么iOS会到WEB根目录下寻找对应的icon。假设设备需要`57x57`的icon，那么iOS将以下面的顺序进行访问：

* apple-touch-icon-57x57-precomposed.png
* apple-touch-icon-57x57.png
* apple-touch-icon-precomposed.png
* apple-touch-icon.png

#启动界面

像原生应用一样，你也可以给Web App加上一个启动界面，很简单：

	<link rel="apple-touch-startup-image" href="/startup.png">

在`iPhone`和`iPod touch`上，尺寸大小必须为`320 x 460`。

#隐藏Safari用户栏

为了更加像原生应用，我们还可以对Safari的用户栏和地址栏进行隐藏，这个叫作`standalone`模式，加入以下meta进入此模式：

	<meta name="apple-mobile-web-app-capable" content="yes" />

你可以通过`window.navigator.standalone`来检测当前是否是`standalone`模式。效果如下：

![](http://7q5cfr.com1.z0.glb.clouddn.com/app04.png)

#链接问题

在Safari中，如果点击一个链接，那么Safari将会打开一个新的tab，显然做为一个应用这体验简直太差了，需要在HTML中加入以下JavaScript来阻止此行为：

	<script type="text/javascript" charset="utf-8">
	// Mobile Safari in standalone mode
	if(("standalone" in window.navigator) && window.navigator.standalone){
   
		// If you want to prevent remote links in standalone web apps opening Mobile Safari, change 'remotes' to true
		var noddy, remotes = true;

		document.addEventListener('click', function(event) {
	
			noddy = event.target;
	
			// Bubble up until we hit link or top HTML element. Warning: BODY element is not compulsory so better to stop on HTML
			while(noddy.nodeName !== "A" && noddy.nodeName !== "HTML") {
		        noddy = noddy.parentNode;
		    }
	
			if('href' in noddy && noddy.href.indexOf('http') !== -1 && (noddy.href.indexOf(document.location.host) !== -1 || remotes))
			{
				event.preventDefault();
				document.location.href = noddy.href;
			}

		},false);
	}
	</script>

以上代码来自[gist](https://gist.github.com/kylebarrow/1042026)。

#最后

Have fun　：）

参考：

[Configuring Web Applications](http://developer.apple.com/library/ios/#documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)

[Everything you always wanted to know about touch icons](http://mathiasbynens.be/notes/touch-icons)


