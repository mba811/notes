---
layout: post
title: 使用 Git Hooks 实现自动项目部署
tags: [tools]
---

In [Technology](http://icyleaf.com/categories/Technology/) Tag: [git](http://icyleaf.com/tags/git/)

最近在某服务器上面搭建 git 开发和部署环境，git 开发环境很简单，按照 ProGit 一书的相关知识就可以轻松搞定，实现了类似 Github 的使用 SSH + 私有 Clone 的方式。

关于部署，实际上是自动部署，起初的想法是使用 bash shell 制定一个定时任务去不断 git pull 产品代码，后来记得 Git 带有 Hooks，索性在ProGit 一书翻了翻：

> Git 本身可以调用自定义的挂钩脚本，其中有两组：客户端和服务器端。客户端挂钩用于客户端的操作，如提交和合并。服务器端挂钩用于 Git 服务器端的操作，如接收被推送的提交。详情请查看 [ProGit 相关章节](http://progit.org/book/zh/ch7-3.html)

如果这样就简单了，利用服务器端调用想要的挂钩（Hook），即可实现自动部署的方案，为了保证不被肆意部署，特加了一个对需要部署 commit 的判断，利用读取 commit subject 并匹配想要的字符串才去部署，这样我认为是一个比较安全的部署方案。

Git的挂钩（Hook）主要包含：

  * applypatch-msg
  * post-update
  * pre-rebase
  * commit-msg
  * pre-applypatch
  * update
  * post-commit
  * pre-commit
  * post-receive
  * prepare-commit-msg

这里我们只需要使用 post-receive 这个 Hook：在接收 post(push)  
请求之后执行。其他大部分我也没有大多研究，不过看名字不算难理解，我觉得其中大部分包含 commit 的属于客户端。

好了，部署开始：

​1. 在服务器 git 仓库（注意是 bare 仓库，不是代码仓库）的 Hooks，编辑  
post-receive（如果没有自行创建），代码请看：

[http://gist.github.com/566767](http://gist.github.com/566767)）：

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
    
    #!/bin/sh
    
    
    #
    
    
    # git autodeploy script when it matches the string "[deploy]"
    
    
    #
    
    
    # @author    icyleaf <icyleaf.cn@gmail.com>
    
    
    # @link      http://icyleaf.com
    
    
    # @version   0.1
    
    
    #
    
    
    # Usage:
    
    
    #       1. put this into the post-receive hook file itself below
    
    
    #       2. `chmod +x post-recive` 
    
    
    #       3. Done!
    
    
     
    
    
    # Check the remote git repository whether it is bare
    
    
    IS_BARE=$(git rev-parse --is-bare-repository)
    
    
    if [ -z "$IS_BARE" ]; then
    
    
    	echo >&2 "fatal: post-receive: IS_NOT_BARE"
    
    
    	exit 1
    
    
    fi
    
    
     
    
    
    # Get the latest commit subject
    
    
    SUBJECT=$(git log -1 --pretty=format:"%s")
    
    
     
    
    
    # Deploy the HEAD sources to publish
    
    
    IS_PULL=$(echo "$SUBJECT" | grep "\[deploy\]")
    
    
    if [ -z "$IS_PULL" ]; then
    
    
    	echo >&2 "tips: post-receive: IS_NOT_PULL"
    
    
    	exit 1
    
    
    fi
    
    
     
    
    
    # Check the deploy dir whether it exists
    
    
    DEPLOY_DIR=/home/icyleaf/php/icyleaf/
    
    
    if [ ! -d $DEPLOY_DIR ] ; then
    
    
    	echo >&2 "fatal: post-receive: DEPLOY_DIR_NOT_EXIST: \"$DEPLOY_DIR\""
    
    
    	exit 1
    
    
    fi
    
    
     
    
    
    # Check the deploy dir whether it is git repository
    
    
    #
    
    
    #IS_GIT=$(git rev-parse --git-dir 2>/dev/null)
    
    
    #if [ -z "$IS_GIT" ]; then
    
    
    #	echo >&2 "fatal: post-receive: IS_NOT_GIT"
    
    
    #	exit 1
    
    
    #fi
    
    
     
    
    
    # Goto the deploy dir and pull the latest sources
    
    
    cd $DEPLOY_DIR
    
    
    #env -i git reset --hard
    
    
    env -i git pull
    

[view raw](https://gist.github.com/icyleaf/566767/raw/post-receive.sh)[post-receive.sh](https://gist.github.com/icyleaf/566767#file-post-receive-sh) hosted with ❤ by [GitHub](https://github.com/)

这里会先判断脚本所在目录是否是 bare git 仓库，然后获取最新 commit 的 subject，并匹配是否包含 [deploy] 字样，如果包含，则继续检查产品代码仓库路径是否存在，如果存在则执行 git pull 操作。

​2. 对刚才编辑的 post-receive 执行下面命令以保证脚本可执行：
    
    $ chmod +x post-receive
    

​3. 完成！

* * *

对于自定义脚本，其实不仅限于 bash shell，你可以使用你熟悉的语言，然后把你的脚本路径在 hooks 脚本中加载即可。

脚本还会继续更新，下面需要增加关于测试部分的相关判断和部署。 bash shell 还需要进一步学习，上面脚本是我第一次写，如有不妥之处，请指教，感谢！
