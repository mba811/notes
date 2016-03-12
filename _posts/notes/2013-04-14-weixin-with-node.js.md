---
layout: post
title: 微信 API For Node.js
tags: [notes]
---

![Node.js](http://{{ site.cdn }}/images/tech/nodejs.jpg "Node.js")

# 介绍

`weixin-api`是对微信公众平台[消息接口][1]的Node.js实现。

* NPM：[https://npmjs.org/package/weixin-api](https://npmjs.org/package/weixin-api)
* Github：[https://github.com/JeremyWei/weixin_api](https://github.com/JeremyWei/weixin_api)


# 安装

	$ npm install weixin-api

# 功能

### 消息监听

根据微信公众平台的API规范，对四种消息进行监听，通过传递`callback`来对消息进行处理，其中callback只有一个参数`msg`，不同消息其`msg`内容不同，各属性代表的含义请对照微信公众平台的API规范。

*   `textMsg(function(msg) {})`  
	说明：监听文本消息  
	msg结构：  
		{  
			toUserName : '',
			fromUserName : '',
			createTime : '',
			msgType : 'text',
			content : '',
			msgId : ''
		}  
		
*   `imageMsg(function(msg) {})`  
	说明：监听图片消息  
	msg结构：  
		{  
			toUserName : '',
			fromUserName : '',
			createTime : '',
			msgType : 'image',
			picUrl : '',
			msgId : ''
		} 
	
*   `locationMsg(function(msg) {})`  
	说明：监听位置信息  
	msg结构：  
		{  
			toUserName : '',
			fromUserName : '',
			createTime : '',
			msgType : 'location',
			locationX : '',
			locationY : '',
			scale : '',
			label : '',
			msgId : ''
		}

*  `urlMsg(function(msg) {})`  
	说明：监听链接信息  
	msg结构：  
		{  
			toUserName : "",
			fromUserName : "",
			createTime : "",
			msgType : "link",
			title : "",
			description : ""
			url : ""
			msgId : ""
		} 
		
*  `eventMsg(function(msg) {})`  
	说明：监听事件信息  
	msg结构：  
		{  
			toUserName : "",
			fromUserName : "",
			createTime : "",
			msgType : "event",
			event : "",
			eventKey : ""
		} 
	
### 消息回复

消息的回复统一使用`sendMsg(msg)`方法，其中`msg`可以为以下内容：

* 回复文本消息

		var msg = {
		    fromUserName : "",
		    toUserName : "",
		    msgType : "text",
		    content : "(╯°□°）╯︵ ┻━┻",
		    funcFlag : 0
		};

* 回复音乐消息

		var msg = {
		    fromUserName : "",
		    toUserName : "",
		    msgType : "music",
		    title : "向往",
		    description : "李健《向往》",
		    musicUrl : "",
		    HQMusicUrl : "",
		    funcFlag : 0
		};
	
* 回复图文消息

		var articles = [];
	    articles[0] = {
	        title : "PHP依赖管理工具Composer入门",
	        description : "PHP依赖管理工具Composer入门",
	        picUrl : "http://weizhifeng.net/images/tech/composer.png",
	        url : "http://weizhifeng.net/manage-php-dependency-with-composer.html"
	    };

	    articles[1] = {
	        title : "八月西湖",
	        description : "八月西湖",
	        picUrl : "http://weizhifeng.net/images/poem/bayuexihu.jpg",
	        url : "http://weizhifeng.net/bayuexihu.html"
	    };

	    articles[2] = {
	        title : "「翻译」Redis协议",
	        description : "「翻译」Redis协议",
	        picUrl : "http://weizhifeng.net/images/tech/redis.png",
	        url : "http://weizhifeng.net/redis-protocol.html"
	    };

	    // 返回图文消息
	    var msg = {
	        fromUserName : "",
	        toUserName : "",
	        msgType : "news",
	        articles : articles,
	        funcFlag : 0
	    }

# 示例
我们使用[express][2]来创建一个简单的微信应用

##安装express
	
	$ npm install express -g


##创建应用


	$ express myweixin
	
`cd myweixin`修改`package.json`，添加对weixin-api的依赖：

	{
	  "name": "application-name",
	  "version": "0.0.1",
	  "private": true,
	  "scripts": {
	    "start": "node app.js"
	  },
	  "dependencies": {
	    "express": "3.1.1",
	    "jade": "*",
	    "xml2js" : "0.2.6",
	    "weixin-api" : "*"
	  }
	}

然后执行`npm install`

##app.js

	var express = require('express'),
		weixin = require('weixin-api'),
		app = express();

	// 解析器
	app.use(express.bodyParser());

	// 接入验证
	app.get('/', function(req, res) {
		
		// 签名成功
		if (weixin.checkSignature(req)) {
			res.send(200, req.query.echostr);
		} else {
			res.send(200, 'fail');
		}
	});

	// config
	weixin.token = '你的token';

	// 监听文本消息
	weixin.textMsg(function(msg) {
		console.log("textMsg received");
		console.log(JSON.stringify(msg));

		var resMsg = {};

		switch (msg.content) {
			case "文本" :
				resMsg = {
					fromUserName : msg.toUserName,
					toUserName : msg.fromUserName,
					msgType : "text",
					content : "(╯°□°）╯︵ ┻━┻",
					funcFlag : 0
				};
				break;
	
			case "音乐" :
				resMsg = {
					fromUserName : msg.toUserName,
					toUserName : msg.fromUserName,
					msgType : "music",
					title : "向往",
					description : "李健《向往》",
					musicUrl : "",
					HQMusicUrl : "",
					funcFlag : 0
				};
				break;
		
			case "图文" :
		
				var articles = [];
				articles[0] = {
					title : "PHP依赖管理工具Composer入门",
					description : "PHP依赖管理工具Composer入门",
					picUrl : "http://weizhifeng.net/images/tech/composer.png",
					url : "http://weizhifeng.net/manage-php-dependency-with-composer.html"
				};

				articles[1] = {
					title : "八月西湖",
					description : "八月西湖",
					picUrl : "http://weizhifeng.net/images/poem/bayuexihu.jpg",
					url : "http://weizhifeng.net/bayuexihu.html"
				};

				articles[2] = {
					title : "「翻译」Redis协议",
					description : "「翻译」Redis协议",
					picUrl : "http://weizhifeng.net/images/tech/redis.png",
					url : "http://weizhifeng.net/redis-protocol.html"
				};
	
				// 返回图文消息
				resMsg = {
					fromUserName : msg.toUserName,
					toUserName : msg.fromUserName,
					msgType : "news",
					articles : articles,
					funcFlag : 0
				}
		}

		weixin.sendMsg(resMsg);
	});

	// 监听图片消息
	weixin.imageMsg(function(msg) {
		//
		console.log("imageMsg received");
		console.log(JSON.stringify(msg));
	});

	// 监听位置消息
	weixin.locationMsg(function(msg) {
		//
		console.log("locationMsg received");
		console.log(JSON.stringify(msg));
	});

	// 监听链接消息
	weixin.urlMsg(function(msg) {
		//
		console.log("urlMsg received");
		console.log(JSON.stringify(msg));
	});

	// Start
	app.post('/', function(req, res) {
	
		// loop
		weixin.loop(req, res);

	});

	app.listen(3000);

##启动

	$ cd myweixin 
	$ node app.js

#结尾
这个package还有很多不完善的地方，欢迎fork，：）

[1]: http://mp.weixin.qq.com/wiki/index.php?title=%E6%B6%88%E6%81%AF%E6%8E%A5%E5%8F%A3%E6%8C%87%E5%8D%97 
[2]: http://expressjs.com/