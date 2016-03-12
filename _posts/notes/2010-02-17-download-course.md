---
layout: post
title:  批量下载优质课程的方法收集
tags: [notes]
---

## 项目背景

这是一个收集批量下载优质课程方法的项目。目前已经添加了批量下载它们的方法：

  * courera
  * railscast
  * TED

未来将补充完善其它优质课程的网站。当然，你也有更简单的应用办法，就是使用我已经解析之后的下载列表文件。放在`download`目录中。

## [](https://gitcafe.com/mba811/metacourse#%E7%94%A8%E6%B3%95)用法

将本项目git clone到本地。
    
    git clone https://github.com/ouyangzhiping/metacourse.git
    cd metacourse
    

## [](https://gitcafe.com/mba811/metacourse#courera%E8%AF%BE%E7%A8%8B)courera课程

### [](https://gitcafe.com/mba811/metacourse#%E5%AE%89%E8%A3%85courera)安装courera

### [](https://gitcafe.com/mba811/metacourse#%E5%87%86%E5%A4%87python%E7%8E%AF%E5%A2%83)准备python环境
    
    brew install python
    sudo easy_install pip
    pip install -r requirements.txt
    

### [](https://gitcafe.com/mba811/metacourse#%E4%B8%8B%E8%BD%BDcourera%E8%AF%BE%E7%A8%8B)下载courera课程

用法：
    
    cd courera
    python coursera-dl -u username -p password 课程编号 
    

如何查找课程编号？以网址为例：
    
    https://class.coursera.org/sna-002/class/index 
    

这里面的sna-002就是课程编号。 username与password填入courera的登录邮箱与密码。

例子：
    
    python coursera-dl -u username -p pasword socialpsychology-001
    

### [](https://gitcafe.com/mba811/metacourse#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)注意事项

如果下载网速较慢，建议使用翻墙vpn。

## [](https://gitcafe.com/mba811/metacourse#railscast%E8%AF%BE%E7%A8%8B)railscast课程

### [](https://gitcafe.com/mba811/metacourse#%E5%87%86%E5%A4%87clojure%E7%8E%AF%E5%A2%83)准备clojure环境
    
    brew install clojure
    

### [](https://gitcafe.com/mba811/metacourse#%E4%B8%8B%E8%BD%BDrailscast%E8%AF%BE%E7%A8%8B)下载railscast课程

#### [](https://gitcafe.com/mba811/metacourse#%E5%85%8D%E8%B4%B9%E8%AF%BE%E7%A8%8B)免费课程
    
    cd railscast-downloader
    clj railscast-download.clj -rss your-rss-link -type media-format    
    

`media-format`一般填写mp4。例子：
    
    clj railscast-download.clj -rss http://feeds.feedburner.com/railscasts -type mp4
    

该免费网址需要翻墙。

### [](https://gitcafe.com/mba811/metacourse#%E6%94%B6%E8%B4%B9%E8%AF%BE%E7%A8%8B)收费课程

将`your-rss-link`改为订阅后的rss网址。

## [](https://gitcafe.com/mba811/metacourse#%E4%B8%8B%E8%BD%BDted%E8%AF%BE%E7%A8%8B)下载TED课程

### [](https://gitcafe.com/mba811/metacourse#%E6%AF%8F%E6%97%A5%E6%9B%B4%E6%96%B0%E7%9A%84metalink%E6%96%87%E4%BB%B6)每日更新的metalink文件

  * [Download TED talks with Chinese, Simplified subtitles](http://metated.petarmaric.com/download.zh-cn.html)

### [](https://gitcafe.com/mba811/metacourse#%E8%A7%A3%E6%9E%90%E5%90%8E%E7%9A%84%E4%B8%8B%E8%BD%BD%E5%88%97%E8%A1%A8)解析后的下载列表

已经解析之后的下载地址列表文件：[TED_download.list](https://raw.github.com/ouyangzhiping/metacourse/master/download/TED_download.list)

将该文件下载下来，然后使用迅雷打开即可。

### [](https://gitcafe.com/mba811/metacourse#%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)注意事项

  * 有150g以上的硬盘闲置空间。
  * 请务必使用离线与加速功能。这样，你可以享受到我已经加速过的部分视频，其它网友也可以享受到你带来的好处。
  * 将这一千多个视频都保存在一个文件夹里面。

## [](https://gitcafe.com/mba811/metacourse#%E6%9B%B4%E6%96%B0)更新

批量更新下载脚本：
    
    git submodule foreach git pull && git pull
    

## [](https://gitcafe.com/mba811/metacourse#%E6%84%9F%E8%B0%A2)感谢

  * [jplehmann/coursera](https://github.com/jplehmann/coursera)
  * [bayan/railscast-downloader](https://github.com/bayan/railscast-downloader)
  * [petar / metaTED — Bitbucket](https://bitbucket.org/petar/metated/)

## [](https://gitcafe.com/mba811/metacourse#author)Author

  * zhiping ([http://yangzhiping.com](http://yangzhiping.com/))