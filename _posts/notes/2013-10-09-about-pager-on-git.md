---
layout: post
title: Git 和 Pager 的那点事
tags: [notes]
---

> Git 几乎所有命令都提供分页器，即当命令输出超过一页时，自动在每页输出后暂停，可以按空格继续显示，按 q 退出。

默认 git 的 `pager = less -FRSX`，这个可以通过两种方式更改：

命令：
    
    $ git config --global core.pager "less -FRSX"
    

配置文件：
    
    $ vim ~/.gitconfig
    

了不起了通过设置自动匹配的色彩来增强可读性
    
    $ git config --global color.ui on
    

随着 [tig](http://jonas.nitro.dk/tig) 的出现，给 git 的增加了一个强大武装武器。（不明观众看这里先对它有个感官概念：[tig, the ncurses front-end to Git](http://gitready.com/advanced/2009/07/31/tig-the-ncurses-front-end-to-git.html)）

可能大家不知道 tig 本身就可以也是一个 pager，因此我们可以在 git config 默认替换之：
    
    $ git config --global core.pager tig
    

自从这样配置之后，让我幸福了好几年。直到…今天发现一个怪异的问题，使用任何需要显示 tig pager 的地方设置的配色是以代码形式显示，而不是解析成了终端显示的颜色。最近忍不住升级到了 10.9 并更新了一些命令，因此也没搞清楚到底是哪里出了问题。只能先把 git 自带的 color 渲染给关闭才解决了这个问题
    
    $ git config --global color.ui off
    

以下是我的个人 `.gitconfig` 等配置文件(随用随更新)

1
    
    *.pbxproj binary
    

[view raw](https://gist.github.com/icyleaf/868866/raw/.gitattributes)[.gitattributes](https://gist.github.com/icyleaf/868866#file-gitattributes) hosted with ❤ by [GitHub](https://github.com/)

12345678910111213141516171819202122232425
    
    [user]
    
    
    	name = icyleaf
    
    
    	email = icyleaf.cn@gmail.com
    
    
    [core]
    
    
    	excludesfile = ~/.gitignore
    
    
    	attributesfile = ~/.gitattributes
    
    
    	# Install it: http://jonas.nitro.dk/tig/
    
    
            pager = tig 
    
    
    [alias]
    
    
    	st = status
    
    
    	clone = clone --recursive URL
    
    
    [log]
    
    
    	date = local
    
    
    [diff]
    
    
    	rename = copy
    
    
    [status]
    
    
    	color = auto
    
    
    [merge]
    
    
            # Install it: http://kdiff3.sourceforge.net/ (For Mac: `brew install kdif3`)
    
    
    	tool = kdiff3
    
    
    [push]
    
    
    	default = current
    
    
    [color]
    
    
            # Conflict with tig
    
    
    	ui = off
    

[view raw](https://gist.github.com/icyleaf/868866/raw/.gitconfig)[.gitconfig](https://gist.github.com/icyleaf/868866#file-gitconfig) hosted with ❤ by [GitHub](https://github.com/)

123456789101112131415161718192021222324252627282930313233343536373839404142
    
    # Xcode project
    
    
    *\*.xcodeproj
    
    
    !*.xcodeproj/project.pbxproj
    
    
    *\project.xcworkspace
    
    
    *\xcuserdata
    
    
     
    
    
    *\build
    
    
    *\*.pbxuser
    
    
    *\*.mode1v3
    
    
    *\*.mode2v3
    
    
    *\*.perspectivev3
    
    
    *\target/*
    
    
    *\.DS_Store
    
    
    *\profile
    
    
     
    
    
    # Xcode workspace
    
    
    *\*.xcworkspace
    
    
     
    
    
    # Eclipse android project
    
    
    *\bin
    
    
    *\gen
    
    
     
    
    
    # e textediotr project config file
    
    
    *\.eprj
    
    
     
    
    
    # Eclipse project config file and folders
    
    
    *\.project
    
    
    *\.settings
    
    
    *\.buildpath
    
    
     
    
    
    # netbeans project config folders
    
    
    *\nbproject
    
    
     
    
    
    # PHPStorm project config folders
    
    
    *\.idea
    
    
     
    
    
    # the Other SCM files
    
    
    *\.svn
    
    
    *\.hg
    
    
     
    
    
    # Others
    
    
    *.pyc
    

[view raw](https://gist.github.com/icyleaf/868866/raw/.gitignore)[.gitignore](https://gist.github.com/icyleaf/868866#file-gitignore) hosted with ❤ by [GitHub](https://github.com/)

Posted on Oct 16 2013
  *[Oct 16 2013]: 2013-10-16T08:00:00.000Z
