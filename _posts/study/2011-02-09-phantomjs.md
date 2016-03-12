---
layout: post
title: PhantomJS
tags: [study]
---

From: <http://javascript.ruanyifeng.com/tool/phantomjs.html#toc0>

来自[《JavaScript 标准参考教程（alpha）》][1]，by 阮一峰 

   [1]: http://javascript.ruanyifeng.com/

目录 

## 概述 

有时，我们需要浏览器处理网页，但并不需要浏览，比如生成网页的截图、抓取网页数据等操作。[PhantomJS][2]的功能，就是提供一个浏览器环境的命令行接口，你可以把它看作一个“虚拟浏览器”，除了不能浏览，其他与正常浏览器一样。它的内核是WebKit引擎，不提供图形界面，只能在命令行下使用，我们可以用它完成一些特殊的用途。 

   [2]: http://phantomjs.org/

PhantomJS是二进制程序，需要[安装][3]后使用。使用下面的命令，查看是否安装成功。 
    
       [3]: http://phantomjs.org/download.html

phantomjs --version

## REPL环境 

phantomjs提供了一个完整的REPL环境。键入phantomjs，就进入了该环境。 
    
    $ phantomjs

这时会跳出一个phantom提示符，就可以输入Javascript命令了。 
    
    phantomjs> 1+2
    3
    
    phantomjs> function add(a,b) { return a+b; }
    undefined
    
    phantomjs> add(1,2)
    3

按ctrl+c可以退出该环境。 

下面，我们把上面的add()函数写成一个文件add.js文件。 
    
    // add.js
    
    function add(a,b){ return a+b; }
    
    console.log(add(1,2));
    
    phantom.exit();

上面的代码中，console.log()的作用是在终端窗口显示，phantom.exit()则表示退出phantomjs环境。一般来说，不管什么样的程序，这一行都不能少。 

现在，运行该程序： 
    
    $ phantomjs add.js

终端窗口就会显示结果为3。 

下面是更多的例子。 
    
    phantomjs> phantom.version
    {
      "major": 1,
      "minor": 5,
      "patch": 0
    }
    
    phantomjs> console.log("phantom is awesome")
    phantom is awesome
    
    phantomjs> window.navigator
    {
      "cookieEnabled": true,
      "language": "en-GB",
      "productSub": "20030107",
      "product": "Gecko",
      // ...
    }
    

## 基本用法 

### 加载网页 

下面，我们用PhantomJS加载网页。新建一个文本文件page.js，写入下面的代码： 
    
    // page.js
    
    var page = require('webpage').create();
    
    page.open('http://slashdot.org', function (s) {
        console.log(s);
        phantom.exit();
    });

第一行require('webpage').create() 表示加载网页模块，并创建一个实例。 

第二行open()方法，接受两个参数。第一个参数是网页的网址，这里我们打开的是著名新闻网站[Slashdot][4]，第二个参数是回调函数，当网页打开后，该函数将会运行，它的参数是状态提示（status），如果打开成功，该参数的值就是success。运行page.js，屏幕将会显示success。 

   [4]: http://slashdot.org

### 执行代码 

打开网页以后，可以使用page实例的evaluate方法，在页面中执行代码。 
    
    var page = require('webpage').create();
    
    page.open(url, function(status) {
      var title = page.evaluate(function() {
        return document.title;
      });
      console.log('Page title is ' + title);
      phantom.exit();
    });
    

网页内部的console语句，以及evaluate方法内部的console语句，默认不会显示在命令行。这时可以采用onConsoleMessage回调函数，上面的例子可以改写如下。 
    
    var page = require('webpage').create();
    
    page.onConsoleMessage = function(msg) {
      console.log('Page title is ' + msg);
    };
    
    page.open(url, function(status) {
      page.evaluate(function() {
        console.log(document.title);
      });
      phantom.exit();
    });
    

上面代码中，evaluate方法内部有console语句，默认不会输出在命令行。这时，可以用onConsoleMessage方法监听这个事件，进行处理。 

### 加载资源 

如果网页实例向远程服务器请求资源，这时HTTP请求（request）和HTTP回应可以用onResourceRequested和onResourceReceived追踪。 
    
    var page = require('webpage').create();
    
    page.onResourceRequested = function(request) {
      console.log('Request ' + JSON.stringify(request, undefined, 4));
    };
    
    page.onResourceReceived = function(response) {
      console.log('Receive ' + JSON.stringify(response, undefined, 4));
    };
    
    page.open(url);
    

上面代码会以JSON格式，输出所有HTTP请求和HTTP回应的头信息。 

page实例的includeJs方法，用于页面加载外部脚本。 
    
    var page = require('webpage').create();
    page.open('http://www.sample.com', function() {
      page.includeJs("http://path/to/jquery.min.js", function() {
        page.evaluate(function() {
          $("button").click();
        });
        phantom.exit()
      });
    });
    

上面的例子在页面中注入jQuery脚本，然后点击所有的按钮。需要注意的是，由于是异步加载，所以`phantom.exit()`语句要放在`page.includeJs()`方法的回调函数之中，否则页面会过早退出。 

### 接受参数 

修改page.js，使得它可以从命令行接受参数。 

system模块可以加载操作系统变量，system.args就是参数数组。 
    
    var page = require('webpage').create(),
        system = require('system'),
        t, address;
    
    // 如果命令行没有给出网址
    if (system.args.length === 1) {
        console.log('Usage: page.js <some URL>');
        phantom.exit();
    }
    
    t = Date.now();
    address = system.args[1];
    page.open(address, function (status) {
        if (status !== 'success') {
            console.log('FAIL to load the address');
        } else {
            t = Date.now() - t;
            console.log('Loading time ' + t + ' ms');
        }
        phantom.exit();
    });

使用方法如下： 
    
    phantomjs page.js http://www.google.com

## 应用 

Phantomjs可以实现多种应用。 

### 过滤资源 

处理页面的时候，有时不希望加载某些特定资源。这时，可以对URL进行匹配，一旦符合规则，就中断对资源的连接。 
    
    page.onResourceRequested = function(requestData, request) {
      if ((/http:\/\/.+?\.css$/gi).test(requestData['url'])) {
        console.log('Skipping', requestData['url']);
        request.abort();
      }   
    };
    

上面代码一旦发现加载的资源是CSS文件，就会使用`request.abort`方法中断连接。 

### 截图 

最简单的生成网页截图的方法如下。 
    
    var page = require('webpage').create();
    page.open('http://google.com', function () {
        page.render('google.png');
        phantom.exit();
    });

page对象代表一个网页实例；open方法表示打开某个网址，它的第一个参数是目标网址，第二个参数是网页载入成功后，运行的回调函数;render方法则是渲染页面，然后以图片格式输出，该方法的参数就是输出的图片文件名。 

除了简单截图以外，还可以设置各种截图参数。 
    
    var page = require('webpage').create();
    page.open('http://google.com', function () {
        page.zoomFactor = 0.25;
        console.log(page.renderBase64());
        phantom.exit();
    });

zoomFactor表示将截图缩小至原图的25%大小；renderBase64方法则是表示将截图（PNG格式）编码成Base64格式的字符串输出。 

下面的例子则是使用了更多参数。 
    
    // page.js
    
    var page = require('webpage').create();
    
    page.settings.userAgent = 'WebKit/534.46 Mobile/9A405 Safari/7534.48.3';
    page.settings.viewportSize = { width: 400, height: 600 };
    
    page.open('http://slashdot.org', function (status) {
        if (status !== 'success') {
        console.log('Unable to load!');
        phantom.exit();
      } else {
            var title = page.evaluate(function () {
          var posts = document.getElementsByClassName("article");
              posts[0].style.backgroundColor = "#FFF";
              return document.title;
          });
    
        window.setTimeout(function () {
          page.clipRect = { top: 0, left: 0, width: 600, height: 700 };
            page.render(title + "1.png");
            page.clipRect = { left: 0, top: 600, width: 400, height: 600 };
          page.render(title + '2.png');
            phantom.exit();
        }, 1000);   
      }
    });

上面代码中的几个属性和方法解释如下： 

  * settings.userAgent：指定HTTP请求的userAgent头信息，上面例子是手机浏览器的userAgent。
  * settings.viewportSize：指定浏览器窗口的大小，这里是400x600。
  * evaluate()：用来在网页上运行Javascript代码。在这里，我们抓取第一条新闻，然后修改背景颜色，并返回该条新闻的标题。
  * clipRect：用来指定网页截图的大小，这里的截图左上角从网页的(0. 0)坐标开始，宽600像素，高700像素。如果不指定这个值，就表示对整张网页截图。
  * render()：根据clipRect的范围，在当前目录下生成以第一条新闻的名字命名的截图。

### 抓取图片 

使用官方网站提供的[rasterize.js][5]，可以抓取网络上的图片，将起保存在本地。 
    
       [5]: https://github.com/ariya/phantomjs/blob/master/examples/rasterize.js

phantomjs rasterize.js http://ariya.github.com/svg/tiger.svg tiger.png

使用[rasterize.js][6]，还可以将网页保存为pdf文件。 
    
       [6]: https://github.com/ariya/phantomjs/blob/master/examples/rasterize.js

phantomjs rasterize.js 'http://en.wikipedia.org/w/index.php?title=Jakarta&printable=yes' jakarta.pdf

### 生成网页 

phantomjs可以生成网页，使用content方法指定网页的HTML代码。 
    
    var page = require('webpage').create();
    page.viewportSize = { width: 400, height : 400 };
    page.content = '<html><body><canvas id="surface"></canvas></body></html>';
    phantom.exit();

官方网站有一个[例子][7]，通过创造svg图片，然后截图保存成png文件。 

   [7]: https://github.com/ariya/phantomjs/blob/master/examples/colorwheel.js

![][8]

   [8]: https://lh3.googleusercontent.com/-xSIzxPtJULw/TVzeP4NPMDI/AAAAAAAAB10/k-c8jB6I5Cg/s288/colorwheel.png

## 参考链接 

  * [Testing JavaScript with PhantomJS][9]
  * [Phantom Quick Start][10]
  * Ariya Hidayat, [Web Page Clipping with PhantomJS][11]
  * BenjaminBenBen, [Using PhantomJS WebServer][12]
  * Ariya Hidayat, [Capturing Web Page Without Stylesheets][13]: 过滤CSS文件

   [9]: http://net.tutsplus.com/tutorials/javascript-ajax/testing-javascript-with-phantomjs/
   [10]: https://github.com/ariya/phantomjs/wiki/Quick-Start
   [11]: http://ariya.ofilabs.com/2013/04/web-page-clipping-with-phantomjs.html
   [12]: http://benjaminbenben.com/2013/07/28/phantomjs-webserver/
   [13]: http://ariya.ofilabs.com/2013/06/capturing-web-page-without-stylesheets.html

## 功能链接 

  * [论坛][14]
  * [Markdown源码][15]
  * [修订历史][16]

   [14]: https://github.com/ruanyf/jstutorial/issues
   [15]: https://raw.github.com/ruanyf/jstutorial/gh-pages/tool/phantomjs.md
   [16]: https://github.com/ruanyf/jstutorial/commits/gh-pages/tool/phantomjs.md

## 留言 

[comments powered by Disqus][17]

   [17]: http://disqus.com

[版权声明][18] | last modified on 2013-08-07 

   [18]: http://javascript.ruanyifeng.com/introduction/license.html

