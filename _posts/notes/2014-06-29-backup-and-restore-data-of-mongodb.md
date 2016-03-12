---
layout: post
title: Mongodb 数据的备份与恢复
tags: [notes]
---

![](http://7q5cfr.com1.z0.glb.clouddn.com/mongodb.png)

#写在前面

本文已经假设你已经安装好了Mongodb(2.4)，并且已经开启了[auth](http://docs.mongodb.org/v2.4/reference/configuration-options/#auth)。

#用户
首先我们添加备份和恢复数据所需的用户，这个用户需要有[readWrite](http://docs.mongodb.org/v2.4/reference/user-privileges/#readWrite)和[userAdmin](http://docs.mongodb.org/v2.4/reference/user-privileges/#userAdmin)权限

	$ mongo
	$ use admin
	$ db.auth("admin", "youradminpasswd");
	$ use backupdb
	$ db.addUser({ user: "backup", pwd: "passwd", roles: [ "readWrite", "userAdmin" ] })

#备份

我们使用[mongodump](http://docs.mongodb.org/v2.4/reference/program/mongodump/#bin.mongodump)来进行数据的备份（注意：mongodump不会备份local数据库中内容）。

mongodump可以通过以下两种方式来进行数据的备份：

* 连接到[mongod](http://docs.mongodb.org/v2.4/reference/program/mongod/#bin.mongod)或者[mongos](http://docs.mongodb.org/v2.4/reference/program/mongos/#bin.mongos)
* 直接访问数据文件

这个工具可以备份整个服务器、单个database或者单个collection的数据，也可以通过查询语句只备份collection中的部分数据。

如果不带任何参数直接执行mongodump，那么它会去连接本地（127.0.0.1或者localhost）27017端口上的MongoDB实例，并且会创建名为```dump```的备份。

先看第一种方式：

	$ mongodump --host mongodb.example.net --port 27017 --db test --collection some --username backup --password passwd

以上会使mongodump连接到mongodb.example.net:27017上的mongod，并且把db```test```中的```some```collection备份到dump目录下。

再看第二种方式：

	$ mongodump --dbpath /data/db --out /data/backup --db test --username backup --password passwd
	
在这种方式下不需要运行mongod实例，如果已经运行了，必须要停掉。``` --dbpath```指定了数据库文件的位置。 mongodump会直接读取数据库文件，在读取过程中会lock数据文件夹，以防其他Mongodb实例写入而导致数据不一致。```--out```指定了备份存放的文件夹。

注意：从Mongodb```2.2```版本开始，mongodump使用的数据格式与旧版本的mongod实例不兼容。所以不要使用新版本（>=2.2）的mongodump去备份旧数据。

#恢复
使用mongodump备份的数据，需要使用[mongorestore](http://docs.mongodb.org/v2.4/reference/program/mongorestore/#bin.mongorestore)来恢复。

mongorestore恢复数据的方式与mongodump相对应，也是分为两种：

* 连接到[mongod](http://docs.mongodb.org/v2.4/reference/program/mongod/#bin.mongod)或者[mongos](http://docs.mongodb.org/v2.4/reference/program/mongos/#bin.mongos)
* 直接写入到数据文件

mongorestore既可以恢复整个备份也可以恢复一部分。

第一种方式：

	$ mongorestore --host mongodb.example.net --port 27017 --db test --collection some --username backup --password password /data/backup

以上会从/data/backup中恢复数据，其中只恢复```test```db中```some```collection到mongodb.example.net:27017中。如果不指定```--host```和```--port```option，那么mongorestore会默认使用localhost:27017。

如果只想恢复部分数据，可以使用```--filter```option：

	$ mongorestore --filter '{"field": 1}'

以上会把dump文件夹数据中```field```为```1```的document恢复到mongod中。

第二种方式：

	$ mongorestore --dbpath /data/db --journal /data/backup

以上可以在mongod没有运行的情况下把数据恢复到```/data/db```。```--journal```option可以确保mongorestore在日志中记录所有的操作，这可以防止恢复操作异常中断（断电、磁盘故障）而引起的数据损坏。

#参考

* [http://docs.mongodb.org/v2.4/tutorial/backup-with-mongodump/](http://docs.mongodb.org/v2.4/tutorial/backup-with-mongodump/)
* [http://docs.mongodb.org/v2.4/reference/program/mongorestore/](http://docs.mongodb.org/v2.4/reference/program/mongorestore/)
* [http://docs.mongodb.org/v2.4/reference/program/mongodump/](http://docs.mongodb.org/v2.4/reference/program/mongodump/#bin.mongodump)
