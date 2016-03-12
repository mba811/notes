---
layout: post
title: 安装 DNSMasq
tags: [notes]
---

连接 VPN 之后，经常打不开国内的某些网站。经高人指点，这是 DNS 的问题，推荐使用[DNSMasq](http://www.thekelleys.org.uk/dnsmasq/doc.html)，在本机架设简易的 DNS 服务器，让 VPN 使用本地服务器解析域名。以下对安装的过程做一记录，以备不时之需。

## 安装 DNSMasq

我使用的系统是 Mac OS X，因此使用 homebrew 安装。安装方式如下：
    
    $ brew up & brew install dnsmasq
    
DNSMasq 比较小，下载过程很快。

然后，根据 homebrew 的提示，执行几个命令：
    
    cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
    sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons
    sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
    

这三个命令的作用分别是：

  * 把配置示例复制到指定位置；
  * 把 DNSMasq 加入启动列表，开机自启动；
  * 手动启动 DNSMasq。

为了操作方便，我还定义了两个命令别名，用于手动启动和停止 DNSMasq：
    
    alias start-dnsmasq="sudo launchctl start homebrew.mxcl.dnsmasq"
    alias stop-dnsmasq="sudo launchctl stop homebrew.mxcl.dnsmasq"
    

## 配置 DNSMasq

DNSMasq 的配置文件是 `/usr/local/etc/dnsmasq.conf`。在文本编辑器中打开，在文件末尾写入如下设置（[via](https://ruby-china.org/topics/20270)）：
    
    server=8.8.8.8
    server=8.8.4.4
    
    #Bilibili
    server=/bilibili.com/114.114.114.114
    
    #fm
    server=/jing.fm/114.114.114.114
    server=/xiami.com/114.114.114.114
    
    #Baidu
    server=/baidu.com/114.114.114.114
    server=/bdstatic.com/114.114.114.114
    
    #QQ
    server=/qq.com/114.114.114.114
    server=/qcloud.com/114.114.114.114
    
    #Taobao
    server=/taobao.com/114.114.114.114
    server=/tmall.com/114.114.114.114
    server=/alipay.com/114.114.114.114
    server=/alibaba.com/114.114.114.114
    server=/taobaocdn.com/114.114.114.114
    
    #Sina
    server=/sina.com.cn/114.114.114.114
    server=/weibo.com/114.114.114.114
    
    #163
    server=/163.com/114.114.114.114
    server=/youdao.com/114.114.114.114
    server=/netease.com/114.114.114.114
    
    #Zhihu
    server=/zhihu.com/114.114.114.114
    server=/zhimg.com/114.114.114.114
    
    #Apple
    server=/apple.com/114.114.114.114
    server=/autonavi.com/114.114.114.114
    
    #Sohu
    server=/sohu.com/114.114.114.114
    server=/sogou.com/114.114.114.114
    
    #Ifeng
    server=/ifeng.com/114.114.114.114
    server=/360.cn/114.114.114.114
    
    
    #Youku
    server=/youku.com/114.114.114.114
    server=/ykimg.com/114.114.114.114
    server=/tudou.com/114.114.114.114
    server=/soku.com/114.114.114.114
    
    #douban
    server=/douban.com/114.114.114.114
    
    #TV
    server=/pptv.com/114.114.114.114
    server=/ppstream.com/114.114.114.114
    server=/pps.tv/114.114.114.114
    server=/letv.com/114.114.114.114
    server=/56.com/114.114.114.114
    server=/58.com/114.114.114.114
    server=/iqiyi.com/114.114.114.114
    server=/cntv.cn/114.114.114.114
    
    #ShopMall
    server=/51buy.com/114.114.114.114
    server=/yixun.com/114.114.114.114
    server=/yihaodian.com/114.114.114.114
    server=/dangdang.com/114.114.114.114
    server=/qunar.com/114.114.114.114
    server=/meituan.com/114.114.114.114
    server=/jd.com/114.114.114.114
    server=/360buyimg.com/114.114.114.114
    
    #Xunlei
    server=/xunlei.com/114.114.114.114
    server=/kankan.com/114.114.114.114
    
    #Ruby China
    server=/ruby-china.org/114.114.114.114
    
    #CDN
    server=/tanx.com/114.114.114.114
    server=/aliyun.com/114.114.114.114
    server=/upaiyun.com/114.114.114.114
    
    #Map
    server=/amap.com/114.114.114.114
    server=/hvvc.us/114.114.114.114
    
    #cn
    server=/cn/114.114.114.114
    
    #guokr
    server=/guokr.com/114.114.114.114
    

如果之前已经启动 DNSMasq，现在重启。

## 配置 VPN

打开 VPN 的高级设置，在 DNS 选项卡中填入 `127.0.0.1`，让 VPN 使用本地 DNS 服务器解析域名。
