---
layout: post
title: CentOS 常用命令
tags: [centos]
---

##关机与重启命令

###Linux centos重启命令：

    reboot
    shutdown -r now 立刻重启(root用户使用)
    shutdown -r 10 过10分钟自动重启(root用户使用)
    shutdown -r 20:35 在时间为20:35时候重启(root用户使用) 

如果是通过shutdown命令设置重启的话，可以用`shutdown -c`命令取消重启

###Linux centos关机命令：

    halt 立刻关机
    poweroff 立刻关机
    shutdown -h now 立刻关机(root用户使用)
    shutdown -h 10 10分钟后自动关机

