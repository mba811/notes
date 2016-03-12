---
layout: post
title: JavaScript Prototype
tags: [notes]
---

##前言

JavaScript做为一门前端开发语言越来越受到关注，对于后端开发者（JAVA，C++，PHP等）来说想要掌握JavaScript，其中最容易出现问题的地方就是对象模型以及原型链继承模型，本文作者根据自身的经验和理解，对JavaScript的对象和__prototype__（原型）进行介绍。

##对象

在JavaScript中数据类型分为原生数据类型（Primitive type 其中包括数字，字符串，布尔，undefined，null）和对象（Object）。注意不要把__String__，__Number__，__Boolean__等包装对象与原生类型混淆，看下面的例子：

	var string = "string";
	console.log(typeof string); //string

	string = new String("string");
	console.log(typeof string); //object
	

在JavaScript中不存在类的概念，所有的东西都是对象，这对于初次接触JavaScript的开发者来说会有些难理解，但是等你熟悉了这个模型之后就会发觉这么设计的简单与优雅。如果你想要快速创建一个对象，那么如下：

	var book = {name: "JavaScript Good Parts"}
	console.log(book.name); // JavaScript Good Parts
	

如果要在JavaScript中创建一个自定义对象，那么首先需要创建构造函数（Constructor，与JAVA类中的构造函数不是一个概念），一个构造函数就是一个函数对象（在里边定义对象的属性），你可以通过function关键词来创建构造函数，也可以使用全局对象Function（注意Function是运行环境中预定义的对象）来创建。构造函数创建之后，就可以使用new操作符后面紧跟构造函数的形式（new functionName()）来创建对象，如下：

	// 使用function关键词
	function Book() {
		this.name = "JavaScript Good Parts";
	}
	
	// 效果与上面一样
	var Book = function() {
		this.name = "JavaScript Good Parts";
	}
	
	var book = new Book();  // create Object
	console.log(book.name); // JavaScript Good Parts
	
	// 使用Function全局对象
	var Book = new Function("this.name = 'JavaScript Good Parts 2';");
	
	var book = new Book();  // create Object
	console.log(book.name); // JavaScript Good Parts2

JavaScript环境中预先定义了很多[全局对象][1]，其中包括：__Function__，__Object__，
__Array__，__Boolean__，__Date__，__Number__，__String__，__RegExp__，__Math__。

##原型

有了对象，那么我们就需要继承，这也是OOP的精华所在，在JAVA,C++等OO的语言中继承是在定义类的时候进行指定的，既然JavaScript中没有类的概念，那么其实现继承的方式需要与众不同了，对，这就是接下来的重点内容：__Prototype__（原型）。

根据我们上面的介绍，知道JavaScript中没有类，只有对象，那么怎么进行继承呢？在JavaScript中，为了实现继承，其在全局对象（除了Math）中加入了__prototype__对象属性，用户可以把能够复用的行为放在__prototype__对象之中，可以把__prototype__看成是一个对象的模板，看如下代码：

	// constructor
	function Book(name, price) {
		this.name = name;
		this.price = price;
	}
	
	// 在prototype中添加print函数
	Book.prototype.print = function(){
		console.log("name: " + this.name + "    price: " + this.price);
	};
	
	// 创建对象
	var book = new Book("JavaScript Good Parts", "35.00元");
	var book2 = new Book("JavaScript权威指南 第6版", "128.00元");
	
	// 输出书籍信息
	book.print();  //name: JavaScript Good Parts    price: 35.00元
	book2.print(); //name: JavaScript权威指南 第6版    price: 128.00元

在以上的例子中book，book2两个对象并没有print函数，但是却输出了书籍信息，这是因为其连接到了__Book.prototype__对象，从而继承了其中的print函数，这就是JavaScript中实现继承的方式。

如果我们需要为Book对象添加一个通用属性（type，用以标识书籍种类），那么添加如下代码（prototype对象是运行时绑定，所以如下代码可以正常运行）：

	Book.prototype.type = "JavaScript";
	
	console.log(book.type); // JavaScript
	console.log(book2.type); // JavaScript


看过以上的例子之后，我们在看下JavaScript的原型链是如何工作的，看如下的代码：

	// constructor
	function Book(name, price) {
		this.name = name;
		this.price = price;
	}
	
	// prototype对象
	Book.prototype = {
		type: "JavaScript (defined in prototype)"
	};
	
	// 创建对象
	var book = new Book("JavaScript Good Parts", "35.00元");
	console.log(book.type) // JavaScript (defined in prototype)
	
	var book2 = new Book("JavaScript权威指南 第6版", "128.00元");
	book2.type = "JavaScript (defined by self)";
	console.log(book2.type) // JavaScript (defined by self)

book对象并没有定义type属性，所以其输出是来自prototype对象，book2对象显示定义了type属性，其输出的是自身信息。

在__prototype__模型中，如果一个对象试图访问一个属性，那么JavaScript先去对象本身中查找，如果存在则返回；如果不存在，那么去对象连接到的__prototype__对象中查找，如果存在返回属性值，不存在则返回__undefined__。逻辑如下：

	if (对象拥有此属性) {
		// 返回属性值
	} else {
		// 去Book.prototype查找
		if (Book.prototype中存在此属性) {
			// 返回属性值	
		} else {
			// 返回undefined
		}
	}

	+--------------+
	|Book.prototype|
	+--------------+
	   ^        ^
	   |        |
	+-----+  +------+
	| book|  | book2|
	+-----+  +------+

JavaScript中提供了一个函数__hasOwnProperty__来检测属性是来自对象本身还是从prototype中继承来的：

	var book = new Book("JavaScript Good Parts", "35.00元");
	console.log(book.hasOwnProperty("type")) // false
	
	var book2 = new Book("JavaScript权威指南 第6版", "128.00元");
	book2.type = "JavaScript (defined by self)";
	console.log(book2.hasOwnProperty("type")) // true

接下来我们来看下在JavaScript中整个原型链是如何工作的。JavaScript的每个对象都会有一个__\_\_proto\_\___属性，用来指定对象连接到的原型对象，看个简单的例子：

	// constructor
	function Book(name, price) {
		this.name = name;
		this.price = price;
	}
		
	// 创建对象
	var book = new Book("JavaScript Good Parts", "35.00元");

	// 因为book是通过构造函数Book创建的，其连接到的原型是Book.prototype，
	// 所以以下输出为true
	console.log(book.__proto__ === Book.prototype) //true
	
	// Book.prototype.__proto__的值指向Object.prototype
	console.log(Book.prototype.__proto__ === Object.prototype) // true
	console.log(book.__proto__.__proto__ === Object.prototype); // true
			
	// Object.prototype是原型链的终点
	console.log(Object.prototype.__proto__); //null
	console.log(book.__proto__.__proto__.__proto__); // null
	
以下是原型链示意图：

	        book              Book.prototype      Object.prototype
	  +---------------+	    +---------------+     +---------------+
	  |	+-----------+ |     | +-----------+ |     | +-----------+ |
	  |	| __proto__ |-----> | | __proto__ |-----> | | __proto__ |-----> null
	  |	+-----------+ |     | +-----------+ |     | +-----------+ |
	  +---------------+     +---------------+     +---------------+
	  
以上文字，随感而发，如有纰漏，敬请见谅。

##参考

[https://developer.mozilla.org/en-US/docs/JavaScript/Guide/](https://developer.mozilla.org/en-US/docs/JavaScript/Guide/)

[1]: https://developer.mozilla.org/en-US/docs/JavaScript/Guide/Predefined_Core_Objects
