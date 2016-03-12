---
layout: post
title: 如何学习 Vim
tags: [tools]
---

这不是一份详细的列出每个 Vim 命令的教程，而是一个初学入门的路线图。

已经有很多介绍 Vim 的文章，我还是觉得有必要写一篇自己的文章，介绍我眼里的 vim。

![](http://7q5cfr.com1.z0.glb.clouddn.com/vim-mine.png)

## 为什么学 Vim

Vim 是一个老牌的编辑器，有大量的用户群。前身 Vi 诞生的时候键盘还没有方向键，也没有鼠标，由此发展出的操作方式让人感到门槛略高。但是它有几个理由吸引人去学习它：

  * 有很多人为它开发插件
  * 全键盘操作带来的效率
  * 能在 ssh 命令行环境使用
  * 几乎没有啥 Bug 和兼容性问题

Vim 是一个优秀的编辑器，但也不必捧为神器。作为程序员，编辑器是工作中接触最多的工具，熟练掌握一个编辑器很有必要。因为 Vim 的插件多、适应性强，自从我用熟之后就一直作为主力编辑器。节省挑选编辑器的时间，可以让我有时间做更有价值的事情。如果你已经在不同编辑器间游荡很久了，那么可以试试 Vim。

Vim 可定制性很高，基本上每个 Vim 用户都会维护一套自己的配置，以至于操作方式大不相同。我就试过一次接过同事的键盘，虽然同是 Vim 编辑器却完全不懂操作（他把 Esc 绑定到了 Caps Lock）。学习 Vim 之初不要用别人打包的配置文件，而是自己从零开始。我也不认为 Vim 需要那么多配置。

Vim 是个编辑器，并不能胜任所有工作，所以我不会拒绝用 IDE。尤其是有些平台的开发工具是为 IDE 优化的，我通常会选一个带有 Vi 键绑定的 IDE。

## 安装 Vim

如果你使用 Linux 系统，很可能已经预装了一份命令行版的 vim，可以在命令行敲入 `vim` 打开。但是我建议安装图形版的，图形版更美观，对鼠标支持也更好（我不拒绝用鼠标）。在 Ubuntu 下，安装命令为：
    
    sudo apt-get install vim-gtk
    

安装后的软件名叫 gvim。其他发行版，可以查阅发行版帮助文档。

![](http://7q5cfr.com1.z0.glb.clouddn.com/vim.png)

如果使用 Windows 系统，那么在官方网站的下载页面可以找到 windows 安装包：

> [http://www.vim.org/download.php](http://www.vim.org/download.php)

如果使用 OS X，那么需要去 macvim 的项目页下载：

> [https://code.google.com/p/macvim](https://code.google.com/p/macvim/)

## 打开帮助文档

假设你已经安装好了 gvim，然后在系统菜单中找到它的图标并启动，接下来就是学习 vim 的操作。

我不打算在这篇文章介绍怎么操作 vim，那可以写一整本书。而其实最好的操作教程，就是 vim 自带的帮助文档。

现在点击菜单项的 `帮助 - 纵览`，就打开了帮助菜单。

![](http://7q5cfr.com1.z0.glb.clouddn.com/vim-help-en.png)

现在文档是英文的，如果你觉得英文难以消化，那么可以安装中文帮助文档。我初学的时候英文阅读能力不佳，看的是中文文档。

中文文档的项目地址在：

> [http://vimcdoc.sourceforge.net/](http://vimcdoc.sourceforge.net/)

你可以在项目页侧栏找到下载链接，下载`vimcdoc-1.8.0.tar.gz`这个压缩包，然后解压，在命令行下 cd 到解压后的文件夹，执行：
    
    ./vimcdoc.sh -i
    

安装脚本会将帮助文档解压到用户配置目录，重新打开 gvim，帮助文档就是中文的了。

![](http://7q5cfr.com1.z0.glb.clouddn.com/vim-help-cn.png)

如果哪一天不需要中文文档了，同样用安装脚本卸载：
    
    ./vimcdoc.sh -u
    

Vim 的帮助文档是一份交互式的文档，你现在要做的，就是放开鼠标，从零开始，学习如何移动光标。（提示：教程在下面的**初步知识**部分才开始）

由于 Vim 使用了多模式的设计，这使得 Vim 的入门曲线比其他编辑器都要陡峭，可能要过很久才知道怎么在 Vim 里面输入一个字符。我一开始也不是直接就把 Vim 用作主力编辑器，而是经过几个月才用上手。我已经不记得当初为什么能坚持那么长一段时间了，也许就是钻牛角尖，但对比往后几年作为主力编辑器的时间，我认为是值得的。

## 插件管理器 vundle

我不建议刚开始学 Vim 的时候就折腾插件，我现在实际用到的插件也很少。如果你开始装插件了，注意不要手工管理插件，而是用这个非官方的插件管理器 vundle：

> [https://github.com/gmarik/vundle](https://github.com/gmarik/vundle)

你可以在项目主页看到怎么安装和使用 vundle。我使用的插件，可以在我的 github 配置备份中看到。

> [https://github.com/chloerei/vimrc/blob/master/.vimrc](https://github.com/chloerei/vimrc/blob/master/.vimrc)

## 这值得吗？

由于我已经把 Vim 用得比较熟了，并且领略到它的好处，所以让我再选的话还是选 Vim。你也可以考虑别的编辑器，比如 [Emacs](http://www.gnu.org/software/emacs/)，[Sublime Text](http://www.sublimetext.com/)，[Textmate](http://macromates.com/) ，它们都有不小的用户群和现成插件。

Vim 虽然入门曲线比另外几个陡峭，但熟悉并熟用一个编辑器要克服的问题是差不多的：

  1. 熟用快捷键
  2. 按照自己的习惯配置编辑器
  3. 如果前两点不能满足了，学习自己写插件

Vim 初期的门槛对比整个阶段的影响其实很小，而 Vim 久经考验的适应性，让我能安心把心思放在编码上。
