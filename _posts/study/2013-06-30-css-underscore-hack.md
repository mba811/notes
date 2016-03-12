---
layout: post
title: The Underscore Hack
tags: [study]
---

原文：[http://wellstyled.com/css-underscore-hack.html](http://wellstyled.com/css-underscore-hack.html)

译者注：**The Underscore Hack**是用来解决IE6，7下CSS　bug的一种方法。

让我们以Petr Pisar发现的三个简单事实开始。

1. 下划线（“_”）在CSS2.1标准中所定义的CSS标识符中是[允许的](http://www.w3.org/TR/CSS21/syndata.html#tokenization)
2. 浏览器必须忽略未知的CSS属性
3. Windows下的MSIE5以上浏览器会忽略任何CSS属性名开头的“_”

所以，一个CSS的定义，比如`_color:red`是：

1. 正确的，因为CSS2.1标准允许它（即使一些只实现了CSS2.0的软件校验器，说这是一个bug：但是它们是错的，这个用法是正确的）。
2. 被除了WinIE（译者注：Windows下的IE浏览器）之外的任何浏览器所忽略
3. 在WinIE中被当作`color:red`处理

因此，这个IE的bug/特性是用来设置只有WinIE（MacIE没有这个bug/特性）才能理解的CSS属性的非常简单和清晰的方法。用来修复比如在WinIE中忘了实现的`position:fixed`规则非常的容易（去看[例子](http://wellstyled.com/files/css-underscore-hack/example-position.html)）。

	#menu {
	   position: fixed;
	   _position: absolute;
	   ...
    }


可以用同样的方法来修复WinIE中没有实现的`min-height`属性（去看[例子2](http://wellstyled.com/files/css-underscore-hack/example-minheight.html)）：

	#box {
	   min-height: 300px;
	   height: auto;
	   _height: 300px;
	   ...
    }
 
注意：这使用了WinIE的另一个bug，把`overflow:visible`当作`height:auto`来解决以上的问题。更多的细节，请看[The "min-height" Hack](http://wellstyled.com/css-minheight-hack.html)。

我在Windows下的MSIE 5，5.5，6和Opera；Mac OSX下的MSIE 5，Safari，Camino；以及Mozilla和Firefox下进行了测试。（译者注：我没在现代浏览器上测试过。）
