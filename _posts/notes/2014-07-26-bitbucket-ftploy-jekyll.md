---
layout: post
title: 使用 BitBucket 和 FTPloy 私有 Jekyll 源码
tags: [notes]
---

## 为何用BitBucket&FTPloy？

网站使用[Jekyll](http://jekyllrb.com/)建成之后，放置在[Github Pages](https://pages.github.com/)的源码属于公开的访问，处于一些个人安全考虑，最重要的是放置在自己的FTP空间会相对安全一些，所以，我采用了支持私有源码的[BitBucket](https://bitbucket.org/)作为Git仓库，并使用[FTPloy](https://ftploy.com/)作为Deploy方案，实现了自动发布到个人FTP的目的。

![](http://zh811.qiniudn.com/FTPloy.png)

### 注册并使用BitBucket

Github私有源码的话，是需要美刀的，作为小站长，一个虚拟空间的钱还是要省的，所以，去注册[BitBucket](https://bitbucket.org/)吧：）注册完成之后，创建一个仓库，如 **_下图_** 所示，注意可选 **私人仓库** 选项。当然，也可以按右上角的**import**导入你的Github仓库。 

![](http://zh811.qiniudn.com/Bitbucket.png)

### 注册并使用FTPloy

[FTPloy](https://ftploy.com/)是一个自动监控Git库分支变更并发布到FTP的网站，支持Bitbucket和Github，当然为了简便起见，你也可以直接监控你的Github仓库，能使用的，应该都不用教啦！FTPloy_免费版_仅支持**一**个Project，但是对于小站长来说，监控一个就足够啦！所以，如**_下面_**两幅图，先新建Server，后新建Project。 

![](http://zh811.qiniudn.com/Server-FTPloy.png)

![](http://zh811.qiniudn.com/Project-FTPloy.png)

### 本地生成Jekyll页面并Push

本地安装网上教程太多，不多说啦，直接**CMD**：**_jekyll build_** 

然后就是如何Push，我使用的是[BitBucket](https://bitbucket.org/)，所以，Mac用户可以去下载下载[SourceTree](http://www.sourcetreeapp.com/download/)，自行探索吧，这就不教程了。

## 结语

经过以上部署之后，你的Jekyll网站在本地编辑并Build之后，使用SourceTree提交并推送之后，FTPloy就会自动将你的新网站PUSH到你的FTP，完美，安心！祝你使用愉快！