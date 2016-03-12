---
layout: post
title: CommonJS Modules/1.0 规范
tags: [notes]
---

_译者：[CommonJS Modules/1.0](http://wiki.commonjs.org/wiki/Modules/1.0) 是目前JavaScript模块化的事实标准，虽然其已经被 [CommonJS Modules/1.1](http://wiki.commonjs.org/wiki/Modules/1.1) 所替代，但是1.0的适用范围非常广，支持者也很多，其中包括Flusspferd, GLUEscript, GPSEE, JSBuild, Narwhal (0.1), Persevere, RingoJS, SproutCore 1.1/Tiki, [node.js][1], TeaJS (formerly v8cgi), [CouchDB][2], Smart Platform, Yabble, Wakanda, XULJet等，所以翻译此规范还是很有必要的，以下为正文。_

此规范指出了如何编写可以在同类模块系统中所共用的模块，这类模块系统可以同时在客户端和服务端，以[安全的](http://wiki.commonjs.org/wiki/Modules/Secure)或者不安全的方式已经被实现了或者通过语法扩展可以被未来的系统所支持。这些模块需要提供顶级作用域的私有性，并提供从其他模块导入单例对象到自身并且可以导出自身API的能力。含蓄的说，这个规范定义了如果一个模块系统要支持共用模块，那么它需要提供的最少的功能特性。

#契约

##模块上下文

1. 在一个模块中，存在一个自由的变量"require"，它是一个函数。
	1. 这个"require"函数接收一个模块标识符。
	2. "require"返回外部模块所输出的API。
	3. 如果出现依赖闭环(dependency cycle)，那么外部模块在被它的传递依赖（[transitive dependencies](http://en.wikipedia.org/wiki/Transitive_dependency)）所require的时候可能并没有执行完成；在这种情况下，"require"返回的对象必须至少包含此外部模块在调用require函数（会进入当前模块执行环境）之前就已经准备完毕的输出。（译者：如果难理解，看下面的[例子](#Module-Context)。）
	4. 如果请求的模块不能返回，那么"require"必须抛出一个错误。
2. 在一个模块中，会存在一个名为"exports"的自由变量，它是一个对象，模块可以在执行的时候把自身的API加入到其中。
3. 模块必须使用"exports"对象来做为输出的唯一表示。

##模块标识符

1. 模块标识符是一个以正斜杠分隔的多个"term"组成的字符串。
2. 一个term必须是一个驼峰格式的标识符，"."或者".."。
3. 模块标识符可以不加文件扩展名，比如".js"。
4. 模块标识符可以是「相对的」或者「顶级的」(top-level)。如果一个模块标识符的第一个term是 "."或者".."，那么它是「相对的」。
5. 顶级标识符是概念上的模块命名空间的根。
6. 相对标识符是相对于在其内部调用了"require"的模块的标识符来进行解析的。

##未规范
此规范对如下关于协同工作能力方面的重要内容未进行规范：

1. 模块是否可以通过数据库，文件系统或者工厂函数进行存储，或者可以通过链接库进行内部交换。
2. 模块加载器是否应该支持PATH变量用来解析模块标识符。

##单元测试

* [Unit Tests at Google Code](http://code.google.com/p/interoperablejs/) by Kris Kowal
* [Unit Tests Git Mirror](http://github.com/ashb/interoperablejs/tree/master) by Ash Berlin

##实例代码

math.js

	exports.add = function() {
	    var sum = 0, i = 0, args = arguments, l = args.length;
	    while (i < l) {
	        sum += args[i++];
	    }
	    return sum;
	};

increment.js

	var add = require('math').add;
	exports.increment = function(val) {
	    return add(val, 1);
	};

program.js

	var inc = require('increment').increment;
	var a = 1;
	inc(a); // 2

<span id="Module-Context"></span>
##依赖闭环解释（译者添加）

因为node.js完全实现了CommonJS Modules/1.0规范，那么我们用其来解释CommonJS Modules/1.0中的依赖闭环问题。看如下代码：

a.js

	console.log('a starting');
	exports.done = false;
	var b = require('./b.js');
	console.log('in a, b.done = %j', b.done);
	exports.done = true;
	console.log('a done');

b.js

	console.log('b starting');
	exports.done = false;
	var a = require('./a.js');
	console.log('in b, a.done = %j', a.done);
	exports.done = true;
	console.log('b done');

main.js

	console.log('main starting');
	var a = require('./a.js');
	var b = require('./b.js');
	console.log('in main, a.done=%j, b.done=%j', a.done, b.done);
	
当main.js加载a.js的时候，a.js加载b.js，同时，b.js想要加载a.js，这时候就产生了依赖闭环的问题，为了避免无限循环，需要打破这个闭环。根据CommonJS Modules/1.0规范中的说明「在这种情况下，"require"返回的对象必须至少包含此外部模块在调用require函数（会进入当前模块执行环境）之前就已经准备完毕的输出。」，有些绕，让我们从依赖闭环产生的地方跟踪，b.js需要require a.js，这里b.js做为当前模块，a.js相对于b.js来说是外部模块，那么a.js的输出应该是在其require b.js之前（即「进入当前模块执行环境」）就应该返回，执行过程如下：

a.js

	console.log('a starting');
	exports.done = false;
	// 只执行到这里，然后exports返回给调用模块(b.js)，以下被丢弃
	var b = require('./b.js');
	console.log('in a, b.done = %j', b.done);
	exports.done = true;
	console.log('a done');

然后b.js继续执行完成。以下是执行结果：

	$ node main.js
	main starting
	a starting
	b starting
	in b, a.done = false
	b done
	in a, b.done = true
	a done
	in main, a.done=true, b.done=true

注意，虽然main.js同时require了a.js和b.js，但是根据node.js的[模块缓存策略](http://nodejs.org/api/modules.html#modules_caching)，模块只执行一次。
	
参考：

[http://wiki.commonjs.org/wiki/Modules/1.0](http://wiki.commonjs.org/wiki/Modules/1.0)

[http://nodejs.org/api/modules.html#modules_cycles](http://nodejs.org/api/modules.html#modules_cycles)


[1]: http://nodejs.org/api/modules.html "nodejs"
[2]: http://couchdb.apache.org/ "CouchDB"