---
layout: post
title: JavaScript中的参数传递
tags: [notes]
---

#前言
JavaScript从做为浏览器上唯一的开发语言，到近期HTML5以及Node.js等后端技术的兴起，其发展势头已经不可限量，本文主要介绍JavaScript中函数参数的传递方式，是[《JavaScript高级程序设计第三版》](http://book.douban.com/subject/10546125/)的读书笔记。

#变量类型

JavaScript中的变量有5个基本数据类型（Undefined, Null, Boolean, Number, String）和引用数据类型（Object，Function，Array等）。

<table class="table_border">
    <caption>数据类型</caption>
    <thead>
        <tr>
            <th scope="col">类型</th>
            <th scope="col">可能的值</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Undefined</td>
            <td>只有undefined，比如：var message; alert(message == undefined)//true</td>
        </tr>
        <tr>
            <td>Null</td>
            <td>只有null</td>
        </tr>
        <tr>
            <td>Boolean</td>
            <td>true和false</td>
        </tr>
        <tr>
            <td>Number</td>
            <td>整数或者浮点数，例如：1或者3.14e10</td>
        </tr>
        <tr>
            <td>String</td>
            <td>任何字符串，例如：var a = "string" </td>
        </tr>
    </tbody>
</table>

#基本类型和引用类型的区别
基本类型和引用类型的变量声明方式是一样的：

	// 声明一个String类型的变量
	var str = "string";
	
	// 声明一个引用类型的变量，并添加属性
	var person = new Object();
	person.name = "Jeremy";
	
二者的区别主要在于对变量内容保存的方式，基本类型的变量中存储的就是简单的数据段，而引用类型变量存储的是指向对象的引用，比如：

	// 基本类型的变量复制，可以看出基本类型变量存储的就是变量的值
	var num1 = 1;
	var num2 = num1;
	console.log(num2); //1
	
	// 引用类型的变量复制，可以看出引用变量中存储的是指向对象的引用
	// Object对象存储于堆中，而obj1和obj2分别存储了指向此Object对象的引用
	var obj1 = new Object();
	obj1.name = "Jeremy";
	var obj2 = obj1;
	obj2.name = "James";
	console.log(obj1.name); //James
	
#参数传递
在JavaScript中无论是基本类型还是引用类型，函数参数都是按值传递的，先来看基本类型：

	function test(num) {
		num += 10;
		return num;
	}

	var num = 10;
	var res = test(num);
	console.log(num); //10 外部变量并未受到影响
	console.log(res); //20

接下来看引用类型：

	function setName(obj) {
		obj.name = "James";
	}

	var person = new Object();
	person.name = "Jeremy";
	setName(person);
	console.log(person.name);//James 影响到了外部引用变量

乍一看，这不就是按引用传递吗？怎么会是按值传递呢？之所以是按值传递是因为当调用setName(person)的时候，实际上是把person所指向的对象的引用进行了复制，然后传递给了setName函数，这样在函数setName内部对此引用进行操作时候是会影响到此引用所指向的对象，即外部person所指向的对象。

总的来说，基本类型的参数传递复制的是具体的值，而引用类型的参数传递复制的是这个引用变量存储的对对象的引用。

为了进一步证明引用类型的参数传递是按值传递而不是按引用传递的，请看：

	function setName(obj) {
		obj.name = "James";
		obj = new Object();
		obj.name = "Leon"
	}

	var person = new Object();
	person.name = "Jeremy";
	setName(person);
	console.log(person.name);//James

以上代码输出的是James，如果是按引用传递，那么以上代码输出的是Leon。实际上，当执行```obj.name = "James"```的时候，引用所指向的对象的值已经发生了改变，当在对obj进行覆盖的时候，obj的值是一个指向局部对象的引用，而这个引用无法对外部的对象产生影响，并且此对象会在函数执行结束之后销毁。

参考:[《JavaScript高级程序设计第三版》](http://book.douban.com/subject/10546125/)













