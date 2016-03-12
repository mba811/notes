---
layout: post
title: 利用 kappa 搭建私有 NPM 仓库
tags: [notes]
---

![](http://7q5cfr.com1.z0.glb.clouddn.com/npm.png)

#写在前面

采用Node.js开发私有或者商业项目的时候，我们需要创建一些内部使用的module供多个项目之间共用，这些module显然你不想发布到社区，默认情况下```npm publish```和```npm install```都是对[registry.npmjs.org](https://registry.npmjs.org/)进行操作，而我们需要的是这样一种仓库：对于一个module，首先操作私有仓库，如果私有仓库中不存在此module，则操作官方的仓库，架构大致如下：

![](http://7q5cfr.com1.z0.glb.clouddn.com/kappa-proxy.png)

本文采用eBay开源的[kappa](https://github.com/krakenjs/kappa)代理和[NPM官方registry](https://github.com/npm/npm-registry-couchapp)来实现以上架构。

#CouchDB

[CouchDB](http://docs.couchdb.org/en/latest/intro/index.html)是专门为Web而生的数据库，它以JSON的格式对数据进行存储，数据库的操作是通过HTTP REST方法进行。CouchDB需要的版本为1.5.0及以上。（以下是在Mac上安装CouchDB的方法，其他系统安装方法见[官方文档](http://docs.couchdb.org/en/latest/install/index.html)）

	$ brew install automake libtool erlang icu4c spidermonkey 
	$ brew link icu4c
	$ brew link erlang
	$ brew install couchdb

编辑配置文件```local.ini```（/usr/local/etc/couchdb/local.ini），添加如下内容：

	[couch_httpd_auth]
	public_fields = appdotnet, avatar, avatarMedium, avatarLarge, date, email, fields, freenode, fullname, github, homepage, name, roles, twitter, type, _id, _rev
	users_db_public = true

	[httpd]
	secure_rewrites = false

	[couchdb]
	delayed_commits = false
	
之后，部署自启动脚本并启动CouchDB

	$ ln -sfv /usr/local/opt/couchdb/*.plist ~/Library/LaunchAgents
	$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.couchdb.plist

创建数据库```registry```

	$ curl -X PUT http://localhost:5984/registry

添加管理员```admin```，密码为```admin```：

	$ HOST="http://127.0.0.1:5984"
	$ curl -X PUT $HOST/_config/admins/admin -d '"admin"'

#NPM Registry

接下来我们来安装NPM Registry应用

	$ git clone git://github.com/npm/npm-registry-couchapp
	$ cd npm-registry-couchapp
	$ npm install
	
之后，同步```ddoc```到```_design/scratch```

	$ echo "_npm-registry-couchapp:couch=http://admin:admin@localhost:5984/registry" >> ~/.npmrc
	$ npm start

确认views已经load：
	
	$ npm run load
	LOADING: fieldsInUse
	HTTP/1.1 200 OK
	Server: CouchDB/1.5.0 (Erlang OTP/R16B03)
	ETag: "DPBK9T7XVY4LAM7S6CS2G2JLZ"
	Date: Thu, 01 May 2014 06:06:01 GMT
	Content-Type: text/plain; charset=utf-8
	Content-Length: 0
	Cache-Control: must-revalidate

把ddoc从```_design/scratch```copy到```_design/app```：

	$ npm run copy
	> npm-registry-couchapp@2.1.4 copy /Users/weizhifeng/dev/node/npm-registry-couchapp
	> bash ./copy.sh

	Did you already run the load-views.sh script? (type 'yes')
	yes
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100    41  100    41    0     0  10408      0 --:--:-- --:--:-- --:--:-- 13666
	{"ok":true,"id":"_design/app","rev":"1-36f4a9b735467b7817ae26b547524fd2"}
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	100  4869  100  4869    0     0  1051k      0 --:--:-- --:--:-- --:--:-- 1188k
	{"ok":true,"id":"_design/_auth","rev":"2-ebde50b9ecf5a06240c0e3c4166c847f"}

以上完成之后，我们就可以把npm的仓库指向自己刚搭建的仓库：

	$ echo "registry = http://admin:admin@localhost:5984/registry/_design/app/_rewrite" >> ~/.npmrc
	
为了能发布module，需要添加npm用户：

	$ npm adduser

用户添加完成之后，可以把私有module进行发布：

	$ cd your-own-package
	$ npm publish

发布完成之后，可以在CouchDB的[管理界面](http://127.0.0.1:5984/_utils/database.html?registry)中找到module的信息。

这样，我们就搭建好了私有的npm仓库，并且已经可以向其中发布module了。那么接下来我们要实现架构图中的```Proxy```。
	
#Kappa


[Kappa](https://github.com/krakenjs/kappa)是由eBay创建的一个基于[npm-delegate](https://npmjs.org/package/npm-delegate)和[hapi](https://github.com/spumko/hapi)的npm代理，通过kappa我们不需要复制整个公共的仓库数据就能创建自己的私有仓库。

安装kappa

	$ npm install -g kappa
	
下载[npm-proxy](https://github.com/JeremyWei/npm-proxy)应用：
	
	$ git clone https://github.com/JeremyWei/npm-proxy.git
	$ cd npm-proxy && npm install

修改config.json文件：

	{
		"servers": [
			{
				"host": "localhost",
				"port": 8100
			},
			{
				"host": "localhost",
				"port": 8143,
				"tls": {
					"requestCert": true,
					"rejectUnauthorized": true,
					"key": "file:./key.pem",
					"cert": "file:./cert.pem"
				}
			}
		],
		"plugins": {
			"kappa": {
				"vhost": "npm.myorg.com",
				"paths": [
					"http://admin:admin@localhost:5984/registry/_design/app/_rewrite/",
					"https://registry.npmjs.org/"
				]
			}
		}
	}

* ```vhost```是虚拟主机，可以修改成自己需要的，但是要注意与下面的内容保持一致。
* ```paths```数组第一个元素是本地私有仓库，第二个元素是公共仓库。

之后添加```npm.myorg.com```到hosts

	$ echo "127.0.0.1 npm.myorg.com" >> /etc/hosts

在启动proxy之前需要```npm cache clean```清除一下缓存，否则很有可能在```npm start```时候出现以下错误：

	2013-12-02T23:19:33.688Z HAPI,HANDLER,ERROR {"msec":0.15241800248622894}
	2013-12-02T23:19:33.689Z HAPI,RESPONSE,ERROR unspecified
	
启动proxy：

	$ npm start
	> kappa -c config.json

	Kappa listening on localhost:8100
	Kappa listening on localhost:8143

启动成功之后就可以把registry地址换成代理地址：
	
	echo "registry = http://npm.myorg.com:8100" >> ~/.npmrc

完成之后，我们在自己的项目中测试代理是否工作正常：

	$ npm install 
	npm http GET http://npm.myorg.com:8100/mongodb
	npm http GET http://npm.myorg.com:8100/mocha

如果以上没有问题，那么恭喜你终于有了自己的私有npm仓库。

#参考

* [https://github.com/npm/npm-registry-couchapp](https://github.com/npm/npm-registry-couchapp)
* [https://github.com/krakenjs/kappa](https://github.com/krakenjs/kappa)
* [http://clock.co.uk/blog/how-to-create-a-private-npmjs-repository](http://clock.co.uk/blog/how-to-create-a-private-npmjs-repository)
* [http://stackoverflow.com/questions/10386310/how-to-install-a-private-npm-module-without-my-own-registry](http://stackoverflow.com/questions/10386310/how-to-install-a-private-npm-module-without-my-own-registry)