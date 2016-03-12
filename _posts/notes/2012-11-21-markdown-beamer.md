---
layout: post
title: 用 Markdown 写 Beamer 幻灯片，及中文问题
tags: [notes]
---

# 前提

## 安装pandoc

参见[pandoc文档](http://johnmacfarlane.net/pandoc/installing.html)

## 安装LaTeX

  * Windows: CTex
  * Ubuntu: TexLive
  * OS X: MacTex

# 输出并编辑beamer模板
    
    pandoc -D beamer > beamer_template.tex
    

其中"beamer_template.tex"为自行指定的模板文件名。

编辑beamber_template.tex文件加入中文支持。在"\ifxetex"语句下增加：
    
    \usepackage{xeCJK}                 % 设置中英文字体
    \setCJKmainfont{楷体}               % 中文字体
    \setmainfont{Arial}                % 英文字体
    \setromanfont{Courier New}
    \setmonofont{Courier New}
    \linespread{1.2}\selectfont        % 行距
    \XeTeXlinebreaklocale "zh"         % 中文自动换行
    \XeTeXlinebreakskip = 0pt plus 1pt % 字之间加0pt至1pt间距
    \parindent 0em                     % 段缩进
    \setlength{\parskip}{20pt}         % 段间距
    

其中字体名称要根据本系统中存在的字体进行替换。

# 用pandoc支持的Markdown写slide

Pandoc支持扩展的markdown语法，具体参见文档[pandoc's markdown](http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown)。

Slide格式见文档[Producing slide shows with Pandoc](http://johnmacfarlane.net/pandoc/README.html#producing-slide-shows-with-pandoc)。

# 生成beamer pdf格式slide

假设slide文件为slide.md
    
    pandoc -t beamer --latex-engine=xelatex --template=beamer_template.tex slide.md -V theme=Warsaw -o slide.pdf
    

在上面命令中，指定LaTeX engine为xelatex，使用的模板为前面编辑的beamer_template.tex，生成的slide文件为slide.pdf

# 生成HTML格式slide
    
    pandoc -t dzslides -s --mathjax slide.md -o slide.html