---
layout: post
title: JavaScript 中的立即执行函数表达式
tags: [notes]
---

#前言
在使用JavaScript的时候经常会看见类似如下的函数调用方式：

	(function(){ 
		console.log("test");
	})();
	
或者

	(function(){ 
		console.log("test");
	}());

一些流行的库也是这样，比如[jQuery](http://code.jquery.com/jquery-1.10.2.js)

	(function( window, undefined ) {
		// code here
	}) ( window );

社区对此种用法的称呼不尽相同，其中包括「自执行匿名函数」（self-executing anonymous function），「立即执行函数表达式」（Immediately-Invoked Function Expression，以下简称IIFE），笔者倾向于第二种叫法。本文浅析一下IIFE是什么以及为什么要如此用。

# 立即执行函数表达式

普通的函数声明与调用方式有以下几种：

	// 声明函数f1
	function f1() {
		console.log("f1");
	}

	// 通过()来调用此函数
	f1();
	
	// 或者
	// 建立匿名函数并赋予变量f2
	var f2 = function() {
		console.log("f2");
	}
	
	// 通过()来调用此函数
	f2();

以上都是以显示的方式声明函数，然后再进行通过```()```来进行调用，那么如果想要直接调用匿名函数呢？可以做到吗？试一下：

	// 直接在匿名函数后边加()（方式A）
	function () {console.log("f1");}(); // SyntaxError: Unexpected token (
	
很不幸，出错了：```SyntaxError: Unexpected token (```。那么试试前言中的方法：

	// 在匿名函数外面套一个()，然后再用()来调用（方式B）
	(function(){ console.log("test");})(); // test
	
	// 在方式A的外层套一个()（方式C）
	(function(){ console.log("test");}()); // test
	
以上这两种方式都是可以正常运行的，如果仅仅到这里是不够的，必需要「知其然，知其所以然」，让我们看看为什么A不可以运行，而B和C是可以运行的。原来，JavaScript解释器会在默认的情况下把遇到的```function```关键字当作是函数声明语句([statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))来进行解释的，而函数声明语句是这样的：

	function name([param] [, param] [..., param]) {
	   statements
	}
	
A的调用方式明显是有语法错误的，所以才会抛出异常```SyntaxError: Unexpected token (```。但是B，C为什么可以正常运行？这是因为在JavaScript中```()```之间只能包含表达式（expression），所以在以B，C方式运行的时候，解释器把```()```中的内容当作表达式（expression）而不是语句(statement)来执行。为了能够更好的解释B，C调用方式的原理，在这里插入对```()```操作符的简单介绍：

	// 如果传入字面量（literal），则返回表达式（expression）
	(1) // 1
	(function(){console.log("f");}) // function () {console.log("f")}
	
这里不会对```()```做更深入的探讨，如果读者感兴趣，可以自行google。OK，我们先看B的调用方式：

	(function(){ console.log("test");})(); // test
	
由于把函数的声明写在了```()```之中，所以解释器以表达式（expression）来解析其中代码，而根据我们上面的介绍知道如果向```()```中传入函数声明会直接返回此函数，此步执行完成之后，临时结果用伪代码来表示的话，应该类似这样：

	anonymousFunction();

这个就是函数的通用调用方式，所以继续执行。对于C的调用方式就更加容易理解了，直接把```()```中的内容当作表达式来进行解释。

所以根据上面的解释，我们知道只要能让JavaScript解释器以「函数表达式」而不是「函数声明」来处理匿名函数的立即执行就可以了，把语句放在```()```之中只是其中的一种方法而已，根据这个思路我们可以用其他方式来实现同样的目的，比如：

	// 如果本身就是expression，那么根本不需要做任何处理
	var i = function(){ return 10; }();
	true && function(){ /* code */ }();
	0, function(){ /* code */ }();

	// 如果你不在乎返回值，可以这么做
	!function(){ /* code */ }();
	~function(){ /* code */ }();
	-function(){ /* code */ }();
	+function(){ /* code */ }();

	// 还有更奇葩的方式，但是不知道性能如何，来自
	// http://twitter.com/kuvos/status/18209252090847232
	new function(){ /* code */ }
	new function(){ /* code */ }()


# 为什么要用立即执行函数表达式

为什么要用立即执行函数表达式呢？有以下几个场景。

* 模拟块作用域
众所周知，JavaScript没有C或Java中的块作用域（block），只有函数作用域，在同时调用多个库的情况下，很容易造成对象或者变量的覆盖，比如：

liba.js

	var num = 1;
	// code....

libb.js

	var num = 2;
	// code....

如果在页面中同时引用```liba.js```和```liba.js```两个库，必然导致```num```变量被覆盖，为了解决这个问题，可以通过IIFE来解决：

liba.js

	(function(){
		var num = 1;
		// code....
	})();
	

libb.js

	(function(){
		var num = 2;
		// code....
	})();

经过改造之后，两个库的代码就完全独立，并不会互相影响。
	
* 解决闭包冲突

[闭包](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures)（closure）是JavaScript的一个语言特性，简单来说就是在函数内部所定义的函数可以持有外层函数的执行环境，即使在外层函数已经执行完毕的情况下，在这里就不详细介绍了，感兴趣的可以自行Google。我们这里只举一个由闭包引起的最常见的问题：

	var f1 = function() {
		var res = [];
		var fun = null;
		for(var i = 0; i < 10; i++) {
			fun = function() { console.log(i);};//产生闭包
			res.push(fun);
		}
	
		return res;
	}

	// 会输出10个10，而不是预期的0 1 2 3 4 5 6 7 8 9
	var res = f1();
	for(var i = 0; i < res.length; i++) {
		res[i]();
	}

修改成：

	var f1 = function() {
		var res = [];
		for(var i = 0; i < 10; i++) {
			// 添加一个IIFE
			(function(index) {
				fun = function() {console.log(index);};
				res.push(fun);
			})(i);
		}
	
		return res;
	}

	// 输出结果为0 1 2 3 4 5 6 7 8 9
	var res = f1();
	for(var i = 0; i < res.length; i++) {
		res[i]();
	}

* 模拟单例

在JavaScript的OOP中，我们可以通过IIFE来实现，如下：

	var counter = (function(){
		var i = 0; 
		return {
			get: function(){
				return i;
			},
			set: function( val ){
				i = val;
			},
			increment: function() {
				return ++i;
			}
		};
	}());

	counter.get(); // 0
	counter.set( 3 );
	counter.increment(); // 4
	counter.increment(); // 5

# 总结
本文浅析了JavaScript中的立即执行函数表达式（Immediately-Invoked Function Expression），指出其存在的原因和其原理，最后举了常用的应用场景，希望对大家有所帮助。


参考：  
[http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife](http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife)  
[http://blog.coolaj86.com/articles/how-and-why-auto-executing-function.html](http://blog.coolaj86.com/articles/how-and-why-auto-executing-function.html)  
[http://stackoverflow.com/questions/592396/what-is-the-purpose-of-a-self-executing-function-in-javascript](http://stackoverflow.com/questions/592396/what-is-the-purpose-of-a-self-executing-function-in-javascript)  
[http://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html](http://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html)  
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)
