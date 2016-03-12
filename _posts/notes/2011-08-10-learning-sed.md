---
layout: post
title: Sed
tags: [notes]
---

###介绍

Sed（**s**tream **ed**itor），是个非常方便的流处理器，其执行流程如下： 拷贝输入流中第一行的内容到模式空间（pattern space），如果地址约束（address restriction）成立，那么对模式空间的内容执行第一个sed命令，然后接着执行下一条sed命令，当最后的操作执行完之后，输出模式空间的内容，读入下一行内容。

###地址约束（address restriction） 
就是对输入的行进行一个条件测试，如果满足就执行接下来的sed命令，否则不执行，主要有以下类型：    
* 5,8表示输入的行位于第5到第8行之间     
* /match/表示输入的行中匹配到了match    
* /start/,/stop/表示从匹配到start开始，到匹配到stop为止，输入行位于这个区间    
* 5,/stop/表示从第5行开始，到匹配到stop为止，输入行位于这个区间   

###语法格式

单条命令

	sed 'address command' < in > out

多命令

	sed -e 'address command'  -e 'address2 command2' < in > out 

如果命令太多，可以把命令放在一个文件script-file中

	sed -f script-file < in > out 

###常用命令

###1. s（substitute）这个命令是我们用的最多的，用来进行文本替换

把old.txt中的"foo"全部(g的作用)替换成"bar"

	sed 's/foo/bar/g' old.txt > new.txt

只替换第二个"foo"为"bar"
	
	sed 's/foo/bar/2' old.txt > new.txt

&代表匹配的字符内容，以下会把“foo”替换成“(foo)”

	echo foo | sed 's/[a-z]+/(&)/'

可以用1,2...9来引用正则表达式的子模式，以下的输出是“bar foo”
	
	echo foo bar | sed 's/([a-z]*) ([a-z]*)/2 1/'

默认情况下sed会把替换之后的内容完全输出，以下命令可以让sed只输出被修改过的内容

	sed -n 's/foo/(&)/p' new.txt

这只会对第5行至第8行进行处理，其中“5,8”为一个区间，与“s/foo/(foo)/”之间有一个空格

	sed '5,8 s/foo/(foo)/'


以下会对101行以后（包括101）的内容做替换，“$”代表最后一行

	sed '101,$ s/foo/bar/'


以下会对文本中含有"start"行的内容进行替换

	sed '/start/ s/foo/bar/'


如果一个表达式以反斜杠开始，那么接下来的字符就是分隔符。以下使用","来作为分隔符

	sed ',^#, s/[0-9][0-9]*//'


你可以指定两个正则表达式作为一个区间。以下会删除“start” “stop”两个关键词之间的注释

	sed '/start/,/stop/ s/#.*//'


你可以把行号和正则表达式合并在一起。以下将会从文件第一行开始到匹配到“start”为止，把这个范围内的注释删掉

	sed -e '1,/start/ s/#.*//'


###2. d（delete）这个命令用来删除必要的行

以下把11行之后的内容删掉

	sed '11,$ d'

以下为删除所有空白和tab

	sed -e 's/#.*//' -e 's/[ ^I]*$//' -e '/^$/ d'

###3. p（print） 这个命令用来控制sed的输出

打印前10行

	sed -n '1,10 p'


打印匹配的内容，和'grep match'一样的效果

	sed -n '/match/ p'

不打印匹配的内容，和'grep -v match'一样的效果

	sed -n '/match/ !p'


###4. q（quit）命令用来在适当的时候让sed退出

在第11行退出

	sed '11 q'

q命令不能用在区间中，以下是错的，因为你不可能让sed退出10次

	sed '1,10 q'


###5. w（write）这个命令用来把处理过的内容写入指定的文件

把"in"中的偶数写入文件"even"

	sed -n 's/^[0-9]*[02468]/&/w even' < in

###6. r（read）这个命令用来读入指定文件的内容

以下在包含INCLUDE的行后面插入"file"文件的内容

	sed '/INCLUDE/ r file' < in

###7. a（add）, c（chang）,i（insert）

* a 在匹配行后面添加一行   
* c 替换匹配内容的行     
* i 在匹配行前面添加一行   

在WORD后面插入一行

	sed '
		/WORD/ a
		Add this line after every line with WORD
	'

在WORD前面插入一行

	sed '
		/WORD/ i
		Add this line before evey line before WORD
	'
	
替换WORD所在行的内容

	sed '
		/WORD/ c
		Replace the current line with the line
	'

以上三个命令都可以插入多行

	sed '
		/WORD/ a
		Add this line
		This line
		And this line
	'

###8. = 命令用来输出行号

输出行号
	sed -n '/foo/ =' test.txt

输出最大行号

	sed -n '$=' test.txt

###9. y这个命令用来做转换用

把单词从小写装换成大写

	sed 'y/abcdef/ABCDEF/' file


###分组
你可以用「{」和「}」来把命令分组，如下

	#!/bin/sh
	# This is a Bourne shell script that removes #-type comments
	# between 'begin' and 'end' words.
	sed -n '
		/begin/,/end/ {
		s/#.*//
		s/[ ^I]*$//
		/^$/ d
		p
		}
	'
	
参考：

[http://www.grymoire.com/Unix/Sed.html](http://www.grymoire.com/Unix/Sed.html)
