---
layout: post
title: Sphinx支持简体中文搜索
tags: [study]
---

# 问题：

sphinx-doc 默认不支持简体中文的自动检索，生成的html文档检索单个中文词时会显示无匹配结果，搜索整句时才会有匹配。

# 解决：

通过添加[中文搜索支持插件](https://github.com/bosbyj/sphinx.search.zh_CN)来实现。

## 大致步骤：

  1. 安装[结巴中文分词](https://github.com/fxsjy/jieba)
    
    pip install jieba

详细的安装方法[参考这里](https://github.com/fxsjy/jieba/tree/jieba3k)。
  2. 安装[sphinx.search.zh_CN 插件](https://github.com/bosbyj/sphinx.search.zh_CN)：  
复制 zh_CN.py 到 sphinx 的 search 目录下，如： C:\Python27\Lib\site-packages\sphinx\search  
打开 search 目录下的 __init__.py 找到：
    
    from sphinx.search import en, ja
    languages = {
     'en': en.SearchEnglish,
     'ja': ja.SearchJapanese,
    }

修改成：
    
    from sphinx.search import en, ja, zh_CN
    languages = {
     'en': en.SearchEnglish,
     'ja': ja.SearchJapanese,
     'zh_CN': zh_CN.SearchChinese
    }

  3. 在 sphinx 工程的 conf.py 中添加 `language = 'zh_CN'` ，**或者**：`html_search_language = `zh_CN` `然后就可以 make html 了。

## 解决过程遇到的问题：

  1. python2

按照上述顺序安装后，依然不能检索中文，考虑应该是插件没有起作用，进入安装目录：../site-pakeages/sphinx/search/后发现，zh_CN.py 没有生成对应的pyc文件，确实没起作用，随后运行ipython，查看import语句是否生效，也就是上面修改的：
    
    from sphinx.search import en, ja, zh_CN

提示不能导入 name zh_CN。  
不解，随后将sphinx卸载重新安装：
    
    pip uninstall sphinx
    pip install sphinx
    pin install --upgrade sphinx

然后再次运行import，竟没有再报错，正常导入了！  
最后在sphinx工程的文件夹运行：
    
    make clean
    make html

在生成语句中多了以下几句：
    
    Building Trie..., from ***/anaconda/lib/python2.7/site-packages/jieba/dict.txt
    loading model from cache /tmp/jieba.cache
    loading model cost 0.798622131348 seconds.
    Trie has been built succesfully.

用firefox打开生成的网页，可以检索中文了！

  2. python3  
另一台电脑装了python3的版本，在经历过以上各种折腾后依然不能正常使用。发现问题出在jieba这个包上面，当使用：
    
    pip install jieba

总会有一些error出现，考虑可能是这个包不兼容python3，尝试到[PyPI](http://pypi.python.org/)站点查找这个[jieba](https://pypi.python.org/pypi/jieba/0.33)包的信息，发现它的[jieba_github](https://github.com/fxsjy)站点，并在介绍里发现它有一个专门的python3版本[jieba3k](https://github.com/fxsjy/jieba/tree/jieba3k)，安装方法在github里面也有介绍，简单可以：
    
    pip install jieba3k

最终，python3 版本也搞定了！