---
layout: post
title: 动手编写Node的C++模块
tags: [notes]
---

#介绍

Node(或者Node.js)作为一门新兴的技术已经被越来越多的企业所使用，事件驱动的开发模式也为服务器端的开发注入了新的力量，Node很容易上手，只要你拥有JavaScript的基础和服务器端的开发经验。Node为开发者提供了两种方式来对其进行扩展：一种是通过JavaScript，一种是通过C/C++。在Node中使用JavaScript来编写模块是非常容易，也是最常用的方式，但是在一些场景中JavaScript的执行性能可能达不到要求（比如大量的位运算），不用发愁，Node还提供了C++的模块扩展接口，以提高执行的性能，本文就简要介绍下Node中C++模块的开发。

#目标

本文我们创建一个名为```hello```的模块，其包含一个名为```sayHello```的方法，如果用JavaScript来表示的话，可能是这样：

	exports.sayHello = function() {
		return "Hello World!";
	}
	
#概念

Node的JavaScript引擎用的是Google开源的[V8](https://developers.google.com/v8/) JavaScript引擎(Chrome浏览器所用的引擎)，所以简单介绍下v8中的一些概念：

1. Handle：一个handle就是指向一个对象的指针。v8中所有的对象都是使用handle来进行访问，之所以用它是因为v8的垃圾回收器需要。
2. HandleScope：可以把它想象成是多个Handle的一个容器。

除了v8之外，这里还要说明一下，用C++编写的模块和用JavaScript编写的模块在使用方式上并无区别，都是通过```require(...)```来进行调用，区别在于C++模块是系统编译好的二进制模块（在*nix下是````.so````，在win下是```dll```），并且扩展名为```.node```。在require```.node```模块的时候，系统通过```dlopen```函数来加载模块，不需要像JavaScript模块那样再进行编译，而是直接加载运行，这加快了执行的速度。

#编写

首先创建模块需要的目录```src```和源文件```hello.cc```

	$ mkdir -p hello/src && cd hello/src
	$ touch hello.cc

hello.cc的内容如下：

	#include <node.h>
	#include <v8.h>
	
	// 引入v8命名空间
	using namespace v8;
	
	// sayHello方法的具体逻辑
	Handle<Value> Method(const Arguments& args) {
		HandleScope scope;
		// 返回一个"Hello World!"字符串
		return scope.Close(String::New("Hello World!"));
	}
	
	// 初始化模块
	void init(Handle<Object> target) {　
		// 定义模块中的sayHello方法
	    NODE_SET_METHOD(target, "sayHello", Method);
	}
	
	// 定义"hello"模块
	NODE_MODULE(hello, init);

以上就是模块的所有代码，还是比较容易理解的，其中的```NODE_SET_METHOD```和```NODE_MODULE```是node.h中定义的宏，具体如下：

Node模块的数据结构定义：

	struct node_module_struct {
	  int version;
	  void *dso_handle;
	  const char *filename;
	  node::addon_register_func register_func;
	  node::addon_context_register_func register_context_func;
	  const char *modname;
	};

```NODE_SET_METHOD```定义：

	template <typename TypeName>
	inline void NODE_SET_METHOD(const TypeName& recv,
	                            const char* name,
	                            v8::FunctionCallback callback) {
	  v8::Isolate* isolate = v8::Isolate::GetCurrent();
	  v8::HandleScope handle_scope(isolate);
	  v8::Local<v8::FunctionTemplate> t = v8::FunctionTemplate::New(callback);
	  v8::Local<v8::Function> fn = t->GetFunction();
	  v8::Local<v8::String> fn_name = v8::String::NewFromUtf8(isolate, name);
	  fn->SetName(fn_name);
	  recv->Set(fn_name, fn);
	}
	
```NODE_MODULE```定义：

	#define NODE_MODULE(modname, regfunc)                                 \
	  extern "C" {                                                        \
	    NODE_MODULE_EXPORT node::node_module_struct modname ## _module =  \
	    {                                                                 \
	      NODE_STANDARD_MODULE_STUFF,                                     \
	      (node::addon_register_func) (regfunc),                          \
	      NULL,                                                           \
	      NODE_STRINGIFY(modname)                                         \
	    };                                                                \
	  }
	  
更加详细的内容，可以查看Node的[源码](https://github.com/joyent/node/blob/master/src/node.h)。
	  
#编译

Node为了实现跨平台的编译，采用了Google的[GYP](https://code.google.com/p/gyp/)(Generate Your Projects)来对项目进行管理。为了能够编译我们的项目，我们需要安装```node-gyp```：

	$ npm install -g node-gyp

安装完成之后，进入到源码目录，并且创建```binding.gyp```文件，因为```binding.gyp```是默认的项目定义文件。

	$ cd hello
	$ touch binding.gyp

并且```binding.gyp```的内容为：

	{
	  'targets': [
	    {
	      'target_name': 'hello',
	      'sources': [
	        'src/hello.cc'
	      ],
	      'dependencies': [
	      ]
	    }
	  ]
	}

之后我们以```binding.gyp```文件来生成```Makefile```等编译所需的文件：

	$ node-gyp configure

执行完成之后，在项目目录下会生成一个```build```目录，里边是gyp自动生成的一些编译所需文件。这一步完成之后，我们来进行编译操作：

	$ node-gyp build

如果执行没有问题的话，在```build/Release```目录下会生成```hello.node```文件，即我们创建的hello模块。

#运行

模块编译成功之后，我们来测试测试模块的功能。在项目目录下创建一个```test.js```文件，内容如下：

	var hello = require('./build/Release/hello.node');
	console.log(hello.sayHello());

执行此文件

	$ node test.js
	$ Hello World!
	
#总结

本文简要介绍了Node中C++模块的开发方式，v8中Handle和HandleScope的概念，以及gyp工具的使用，并实现了一个hello扩展模块。本文部分内容来自于[《深入浅出Node.js》](http://book.douban.com/subject/25768396/)。

更多内容可以查看Node源码：   
[https://github.com/joyent/node/blob/master/src/node.h](https://github.com/joyent/node/blob/master/src/node.h)   
[https://github.com/joyent/node/blob/master/src/node.cc](https://github.com/joyent/node/blob/master/src/node.cc)