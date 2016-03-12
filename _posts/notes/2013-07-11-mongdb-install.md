---
layout: post
title: MongoDB Install
tags: [notes]
---

### 一、前言

其实之前还未接触nosql类型，我觉得这项技术应该是值得学习和研究的。如果读者有扎实的Linux基础和MySQL等知识，那么配置mongodb应该是比较容易的。下面我们按照步骤来吧。

### 二、下载需要的安装包

我是第一次配置mongodb，没有像redmine这个软件一样有太多依赖，直接下载即可，这里我下载的2.2.x版本（注意系统版本），比较新。

[Mongodb下载](http://www.mongodb.org/downloads)

### 三、建立良好的目录（所有软件配置的重点）

  1. 不应该有空格（好的情况是被转义，坏的情况会出现错误，不好发现）
  2. 目录结构不要太深（有时候需要命令操作，麻烦，也许还有其它坏处吧，这是经验之谈）
  3. 见名知意（这个和code命名一样，不解释啦吧）

下载完安装包后，直接解压，将根目录改为：”mongodb”，以下是目录截图。

![](http://7q5cfr.com1.z0.glb.clouddn.com/mongodb-install-1.png)

但是在mac或者linux上面安装不需要那么麻烦，只要你有包集成软件，很方便，这也是为什么我喜欢mac或者linux的原因。
    
    brew update
    brew install mongodb
    

不过brew需要读者自己安装哦。

### 四、建立数据存储、操作日志常用文件夹

如上图中，我建立了：
    
    data/db
    log/mongodb.log
    

如果你调用的是有读写权限的目录，那么你需要手动开启权限哦，chown命令即可。

建立好之后，我们启动mongodb会在data/db目录下产生一些文件，最开始我们不需要关心，mongodb.log是记录日志的文件（在mac终端可以利用touch命令创建文件，其实这点确实没有windows方便），对于维护很重要。其实这些和mysql能够找到相似的地方，不信大家可以翻开mysql安装目录结构看一看哦。

### 五、建立配置文件

mongodb默认没有提供conf配置文件，如果你要问我为什么知道。。。其实我也是猜的，数据库配置文件，mysql—>.ini，svn版本管理软件—>.conf，你只需要尝试一下，看能够识别不就能够知道，但是最后确认还是去官方文档确认的，这里只是说一说当没有文档的时候，我们可以用这种方式来做配置。

配置文件名称：
    
    mongodb.conf
    

其中写入：
    
    dbpath=../data/db
    logpath=../log/mongodb.log
    logappend=true
    

上面参数的意思，其实从字面上就能够理解了。

  1. datapath： data文件的存放路径（根据自己的实际情况）
  2. logpath：同理，log文件的路径（根据自己的实际情况）
  3. logappend：log是追加，还是覆盖的方式，我们这里最好用追加吧，好观察。

### 六、启动mongodb

启动很简单，但是很容易忘记一件事情，就是非install方式，会忘记配置环境变量，环境变量的配置我在之前文章[Mac环境下SVN升级详解](http://60sky.com/blog/2014/01/11/mac-svn-update/)中有介绍。这里我们还是进入mongodb的bin目录进行操作，保证不错才是王道。

终端启动命令如下：
    
    ./mongod -f ../mongodb.conf 
    

这里你必须指定-f参数，紧跟conf文件路径（根据你的存放位置，我的位置如上图），不然就会按照默认方式，出现不能发现文件的问题，因为我没有建，嘿嘿。

### 七、配置验证（搞配置的必要环节）

如果终端输出以下内容即安装成功：
    
    $ ./mongod -f ../mongodb.conf 
    all output going to: ../log/mongodb.log
    

如果你没有指定log文件，log的信息会打印到终端，这里我们指定了，看起来也清晰，就在log文件中，读者可以打开看看。

我们再进入mongodb输入点命令试试，很简单，输入1＋1就行，它会给你执行结果 2，好玩。（不过不要关掉你启动的窗口哦，当然也有后台模式，读者可以google一下，只是一条命令）

进入mongodb的命令：
    
    $ ./mongo
    

执行后输出的内容（1+1是我自己输入的）：
    
    MongoDB shell version: 2.2.2
    connecting to: test
    > 1+1
    2
    > 
    

得到以上结果，恭喜你，已经安装完毕了，尽情的玩吧。
