---
layout: post
title: Markdown 和 reStructuredText
tags: [study]
---

# Markdown

[Markdown](http://daringfireball.net/projects/markdown/) 可以说是目前最流行的标记语言，Github 对它的支持很好，许多论坛、博客也支持这种语法。

作者博客从2011年6月11日开始使用 Markdown 写作，文章托管在 [Github](http://github.com/zrong/blog) 上。在 WordPress 中，我采用 [Markdown On Save](https://wordpress.org/plugins/markdown-on-save/) 插件来实现 Markdown 的解析。

但是，Markdown 在没有扩展的情况下，是非常简单的，就算写个博客都嫌不够。

例如，标准的 Markdown 不支持脚注、不支持表格、也不支持 [TOC](http://en.wikipedia.org/wiki/Table_of_contents) ，而这些在博客写作中都是常用的功能。

虽然在 Markdown 中可以直接使用 HTML 语法来实现这些需求，但对于表格这种变态的实现来说，手写还不如不写吧。

正因为如此，Markdown 发展出许多扩展。例如 [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) 、 [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)以及 [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) 。

[Markdown On Save](https://wordpress.org/plugins/markdown-on-save/) 使用的就是 [Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) 这个扩展。

# reStructuredText

开始学习 Python 之后，我又接触到 [reStructuredText](http://docutils.sourceforge.net/rst.html) 这个标记语言。它是 Python 官方的文档写作语言，主要用于 Python 的文档写作，Python 还提供了一个 [Docutils](http://docutils.sourceforge.net/index.html) 实现它的解析。

reStructuredText 的复杂程度远远大于 Markdown 。而且，Python 官方文档使用了 [sphinx](http://sphinx-doc.org/) 这个生成器来生成文档，它对 reSturcturedText 又进行了一定程度的扩展，使得其 [语法](http://sphinx-doc.org/rest.html) 更加复杂。

当然，reStructuredText 的复杂也就意味着它更强大。表格、脚注、无所不在的链接、注释支持、内置的语法高亮都让其更加适合撰写 [技术文档](http://doc.zengrong.net/1201/hhl/client/)。当然，写博客更也不在话下。

# 详细对比

[常用轻量级标记语言对照](http://www.worldhello.net/gotgithub/appendix/markups.html) 这篇文章是目前最好的对比文章。其中对比了 Markdown 、reStructuredText 、Textile 、 AsciiDoc 和 Org-mode 这五种常见的标记语言的特点和语法。

用这篇文章来作为 reStructuredText 的入门也不错，尤其是在熟悉 Markdown 的基础上。

# 其他标记语言

这个表列出了更多的标记语言：[List of plain-text markup and tools](http://superuser.com/questions/209897/text-formatter-tools/209902#209902) ，维基百科上的 [Comparison of document markup languages](http://en.wikipedia.org/wiki/Comparison_of_document_markup_languages) 一文也对这些语言进行了比较。

# 生成器

有文档标记语言，就必须有生成器。

[Pandoc](http://johnmacfarlane.net/pandoc/) 是个异常强大的生成器，看看这张图就知道它有多牛B了：

![](http://7q5cfr.com1.z0.glb.clouddn.com/diagram.png)

作者博客在使用 Markdown 写作之前，还有 500 多篇文章使用的是 HTML。我用 Pandoc 将它们都转到了 Markdown 。虽然结果并不是非常完美，但也尚可接受。

这里有一篇关于生成器比较的文章可以一看： [Comparison of documentation generators](http://en.wikipedia.org/wiki/Comparison_of_documentation_generators)
