---
layout: post
title: Hello Node.js
tags: [notes]
---

### 一、前言

很久之前就想系统的学习nodejs技术了，但是由于很多事情，忙不太过来，今天晚上下定决心要入门这门技术，所以一口气看完了《Node.js开发指南》和《Node.js中文文档》，总算是真正入门了，接下来就总结一部分，剩下的只有明天再总结了。

### 二、Node.js简介

一句话简单说明一下node.js是什么东西。

> Node.js 是一个让 JavaScript 运行在服务端的开发平台。

### 三、Node.js的安装

学习了Node.js，觉得如果在window下学习这门技术的话还太成熟，第三方的支持不太好，所以只介绍linux或者mac上面的安装。node.js在window上面的安装直接下载安装包，一直下一步就可以装好，环境变量也会自动配置好，npm（node package manage）在新版本也会相应的装上。

对于mac环境安装也比较简单，介绍两种安装方式。

1.homebrew（注意一直使用admin权限吧，大多都是权限文件夹，可以在终端开始使用sudo -s命令来一直使用admin权限）
    
    $ sudo brew install node
    

这样下载完了就算安装上了，我们可以来检测一下你的安装（软件配置必须要的步骤）。
    
    # 查看node版本号
    $ node -v 
    # 查看npm版本号和清单
    $ npm -v && npm list
    

如果npm没有安装上，那么可以使用以下命令安装。
    
    $ sudo curl http://npmjs.org/install.sh | sh
    

2.使用源码编译的方式（其实有方便的尽量就避免麻烦的，嘿嘿）

> 下载地址：[http://nodejs.org](http://nodejs.org/)

执行典型的安装命令即可（注意到/bin目录，或者有configure文件的目录）。
    
    $ ./configure && make && sudo make install
    

### 四、Node.js的多版本管理器n

> 下载地址：[https://github.com/visionmedia/n](https://github.com/visionmedia/n)

如上执行源码安装命令即可。

当然，如果你安装了npm，就有更方便的方式，这也是很迷人的地方。
    
    $ sudo npm install -g n
    

接下来检查一下安装情况。
    
    $ n --version
    

这样就可以使用自由切换使用node的版本了。用如下命令安装指定版本的node。
    
    $ n version
    

下载后，会默认下载到：/usr/local/n/versions/目录

如果你安装后，再使用”$ n version”命令就是指定使用的默认版本号，也可以使用“$ n use version xxx.js”来暂时使用某个node版本执行xxx.js文件。

### 五、万事具备，只欠Hello Node.js

每个语言入门都可以写一个hello xxx，示好。

在node中，这个很简单，只需要进入REPL模式，暂时不要管这个模式是什么，我们先使用。这个用截图来直观说明一下。

进入REPL模式的命令：
    
    $ node
    

接下来直接输入：
    
    console.log('hello node.js!');
    

就可以在终端打印出：
    
    >hello node.js!
    >undefined
    

写过js的应该对最后会输出undefined并不吃惊，先可以不管。

![Mou icon](http://blog.51yuekan.com/images/post/hello-node.js.png)

下面解释一下REPL模式：

其实这个模式不用解释，经过上面的例子应该就有直观的认识啦，类似mysql、python等的shell交互的方式，可以让你输入马上就得到反馈，输出结果到屏幕上面。在node中可以连续按两次ctrl+c退出（但是python要使用exit()函数，很不舒服）。

上面是一种最简单的方式，还是介绍一种正规的方式吧，既然是js的运行平台，我们就写一个js文件：hello.js，内容很简单，如下：
    
    console.log('hello node.js!');
    console.log('%d\n', 60);
    

在终端执行（注意要进入hello.js所在目录哦）：
    
    $ node hello.js
    

就可以在终端直接输出结果了。

> 为什么我加入了“console.log(‘%d\n’, 60);”这一句呢？那是因为这可以测试出其实可以用c语言printf的方式来写console.log()。

### 六、Node.js终于来到了浏览器

前面的例子都是在控制台里面的，没什么意思，我们接下来写一个我们典型的B/S访问的方式。当然这就离不开HTTP了，查看了一下node的源码，新版本默认是http 1.1，支持http和https这两种协议。

我们先建一个http.js文件，里面的内容对于后台的开发人员就很熟悉了。
    
    var http = require('http');
    http.createServer(function(req, res) {
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.write('<h1>Node.js</h1>');
        res.end('<p>Hello Node.js</p>');
    }).listen(8888);
    
    console.log("HTTP server is listening at port 8888.");
    

以上代码监听的port：8888，这其实就是建立了一个request，我们在浏览器地址栏中输入：[http://localhost:8888/](http://localhost:8888/) 即可以访问我们打印的内容。

我们先在终端运行后台服务：
    
    $ node http.js
    

![Mou icon](http://blog.51yuekan.com/images/post/http-node.js.png)

我们来分析一下这个简单的程序，其实从require的加载方式我们并不陌生，这是现在模块加载在前端普遍使用的函数名，那么为什么我们的js代码就能够这样来请求和访问呢？我们来简单的看看源码的解析。

#### 1. 首先我们先看看源码的http.js文件。
    
    var Server = exports.Server = server.Server;
    function createServer() { return new Server(requestListener) };
    

在这个文件中，我们知道createServer()方法其实返回的就是Server类，这个类是由server这个实例来的。

#### 2. 接下来我们再追踪一下_http_server.js文件。

这个要问怎么找到这些文件的，其实是凭直觉和猜测，再加上模块化的调试方法，可以找到http.js文件的加载模块，就可以找到相应的文件了。
    
    function requestListener() {};
    function connectionListener(socket) {}
    Server.prototype.setTimeout = function(msecs, callback) {};
    

再这个文件中，我们可以看出和Socket原理差不多，使用定时器来实现端口的监听，但是在源码中可以找到主定时器，这个概念来源于游戏开发，在全局设置定时器，这样大大提升了性能，不得不佩服作者的才能。

#### 3. 那么我们node底层是c/c++来实现的，我们可以追踪一下node的启动。

我们先找到c语言的main文件，后来发现好多node_开头的，大楷就是系统和行一点的文件了，在node_main.cc文件中发现了：node::Start(argc, argv);函数，但是找不到具体实现，那么就肯定是在依赖文件中，通过头文件的声明，找到了，其实实现位于node.cc文件。

#### 4. node.cc文件来看node启动流程。

函数大致的执行顺序为：
    
    int Start(int argc, char** argv)
    V8::Initialize();
        CreateEnvironment() //创建当前环境
        SetupProcessObject() //启动当前环境的进程
        Load() //加载当前环境
        context() //引用上下文
        uv_run //开始异步事件运行
        RunAtExit //删除异步事件链表
    

上面可以看出其实就是通过一系列初始化，最后达到平台的建立。

#### 5. module的加载实质

其实我们经过上面的分析，我们大致知道的node的构成，那么我们能够在node.js（猜想：这个js文件是c和js沟通的桥梁）中，发现传入了process对象。源码结构如下：
    
    (function(process) {});
    //本地模块的加载接口
    NativeModule.require();
    

我们来看一看根据process来绑定c语言的模块。
    
    var HTTPParser = process.binding('http_parser').HTTPParser;
    

是不是就很清晰啦，接下来我们看看模块是怎么注册的。
    
    //现在产生的插件会放置在node_module文件夹下应该很清晰了
    //定义了一个指针结构
    node_module_struct* mod
    //注册自己的模块
    mod->register_func(Handle<Object> target) 
    //为注册的模块设置一个上下文
    mod->register_context_func 
    //最后加载到模块缓存，这里也是实现延迟加载的本质
    cache->Set(module, exports)
    

在模块注册后，还需要做一些准备工作才能够真正的加载到我们的js文件。
    
    //node.cc文件中执行绑定
    static void Binding(const FunctionCallbackInfo<Value>& args);
    //node_javascript.cc文件中会定义你的js
    void DefineJavaScript(Handle<Object> target);
    

#### 6. 在module.js文件中，得到真正的js文件的加载，平台调用。

在这里作者的module数据结构设置得比较简单，抛去其它得留下结构如下：
    
    module: {
        id: '',
        parent: [],
        export: {},
        filename,
        children: []
    }
    

这样我们这个http程序就能够很好得解释啦。其实知道了原理后面得知识就是看看api和写法了。
