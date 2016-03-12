---
layout: post
title:  Ubuntu 上安装 Ruby 最新版的最佳方法
tags: [ubuntu]
---

TL;DR，先说结论：使用 [Brightbox 维护的 Ruby PPA](http://brightbox.com/docs/ruby/ubuntu/)。
    
    sudo apt-add-repository ppa:brightbox/ruby-ng
    sudo apt-get update
    sudo apt-get install ruby2.1 ruby2.1-dev
    sudo gem install bundler
    ruby -v
    

## 为什么安装 Ruby 是个问题？

Ubuntu 软件源中的 Ruby 版本较旧，Ubuntu LTS 14.04 的 Ruby 版本是 1.9.3，最新的 Ruby 版本是 2.1.1。旧版本运行一些脚本没有问题，但如果要开发自己的应用，最好安装最新版本的 Ruby。新版本的 Ruby 意味功能更完善、性能更好。

要安装最新版本 Ruby，最直接的方法是源码编译，然后修改 PATH。由于这些工作重复性较强，还要处理系统依赖等问题，于是有人把编译安装的过程写成脚本，然后发展成工具。这类工具之中出现较早也最多人用的是 RVM，RVM 提供了多版本安装、Gemset、自动切换版本等功能。但由于 RVM 的功能较多，代码越来越臃肿，且有一些现在已经显得过时了（Bundler 取代 Gemset），于是又有了功能更精简，符合另一些人口味的工具，主要有 [rbenv](https://github.com/sstephenson/rbenv) + [ruby-build](https://github.com/sstephenson/ruby-build)、[chruby](https://github.com/postmodern/chruby) + [ruby-install](https://github.com/postmodern/ruby-install) 等。

这些工具的本质都差不多，用脚本下载 Ruby 源码包并安装到某个自定义位置，然后修改环境变量。区别是，RVM 和 chruby 需要 source 一个脚本动态设置 PATH，而 rbenv 的 PATH 固定，但是为每个可执行文件包裹一个 shims 脚本来修改环境变量。哪种方法更干净见仁见智。

如果你有多版本管理需求，同时希望安装工作量尽可能少，推荐 RVM。

## 找寻更好

有时我也会觉得 RVM 有点累赘，例如在写脚本的时候需要 `source` RVM 的环境配置或者调用`wraper`。我只需要安装最新版 Ruby 而已，没有维护依赖多 Ruby 版本的应用的需求。并且我的桌面和服务器都使用 Ubuntu，不需要考虑跨发行版的方案。所以我希望有这样一个方案：

  1. 为 Ubuntu 优化，不需要跨发行版。
  2. 可以安装最新版本 Ruby，但允许停留在次要版本，只进行安全更新和 Bug 修复。
  3. 写系统脚本的时候，不用费心处理 PATH 之类的环境变量。

然后我发现 Brightbox 的 ruby-ng PPA 完全满足我的需求：

  1. 使用 deb 打包，apt 更新。
  2. 将 Ruby 版本区分为 Ruby1.9，Ruby2.0，Ruby2.1，可以并存不冲突。
  3. 取代系统默认的 Ruby，放在系统 PATH 之中，一般来说不用处理环境变量（除了有些环境例如 crontab 会清除所有环境变量）。

我想其它发行版也有类似的打包方案，例如 Arch Linux 的 AUR。

## 系统安装 vs 用户安装

也许有人会顾虑，将 Ruby、Gem、Bundler 安装到系统路径，是不是安装 gem 都需要 `sudo`？频繁的使用 `sudo` 可不是一个好习惯。RVM 等工具教会了我们将 gem 安装到用户目录，这样便于隔离用户逻辑，升级 Ruby 的时候可以简单的删除目录来清理文件。

我在初次安装 PPA 的时候也想将 gem 安装到用户目录，经过摸索，我发现可以在 `.bashrc` 的顶部添加这些配置来实现：
    
    # Add to top of .bashrc
    export GEM_HOME=~/.gem/ruby/2.1.0
    export GEM_PATH=~/.gem/ruby/2.1.0
    export PATH=~/.gem/ruby/2.1.0/bin:$PATH
    

> 注意，Gem 有一个 `--user-install` 参数，但是 Bundler 不会理会这个设置，而是读取`GEM_HOME` 这个环境变量。所以这里不设置 `--user-install`，而是使用环境变量。
> 
> 另外 Ubuntu 的 .bashrc 的顶上有一行判断，非交互式终端不会执行判断后的代码，所以为了最大适应性，需要把环境变量加到顶部。

但是把 gem 安装到用户目录后，我发现又陷入了环境变量泥潭，写脚本的时候需要关注户是否设置了这几个环境变量。工作量比用 RVM 时候更多了，因为 RVM 只要 source 一个文件，所以我放弃了这个方案。

接着我发现了更好的方案：Bundler，我这才意识到之前受到 RVM 的庇护，以至于没意识到 Bundler 要怎么用。安装项目依赖的时候可以使用这条命令：
    
    bundle install --path vendor/bundle
    

这样项目依赖的 gem 既不是安装在系统目录，也不是用户目录，而是当前目录的 `vendor/bundle` 文件夹内。这样安装的 gem 每个项目独立，要清除项目的时候只要把整个项目目录删除就好了。（注意设置 git 忽略这个目录）

部署环境同理，也是安装到 `vendor/bundle` 目录：
    
    bundle install --deployment
    

所以，一般情况把 Bundler 装到系统目录就够了，应用依赖装到应用目录中。关于 Bundler 的用法这里不再展开，可以查看官方网站：[http://bundler.io/](http://bundler.io/) 。

## 各取所需

回会头看，只是安装个 Ruby 就涌现了这么多管理工具是有点夸张。对于资深 Linux 用户来说，手工编译就很简单，不需要管理工具。对我来说，有 apt 更新让我更有安全感。

如果你喜欢别的方法安装，并且运作良好，那么也没什么必要改变现状。如果感觉现在的工具不好，又跟我一样只用 Ubuntu，那么推荐用 Brightbox 的 PPA。
