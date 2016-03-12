---
layout: post
title: Redis 协议
tags: [notes]
---

![Redis](http://{{ site.cdn }}/images/tech/redis.png "Redis")

原文：[Protocol specification][2]

Redis协议是从以下几个方面做的一个折中方案：

* 容易实现
* 机器解析要快
* 容易被人理解

###网络层

客户端通过创建到6379端口的TCP连接来连接到一个Redis服务器。每个Redis命令或者客户端和服务器之间传输的数据都以\r\n (CRLF)结束。

###请求

Redis可以接受由不同参数组成的命令。只要接收到一个命令，这个命令就会被执行，然后一个答复会被返回给客户端。

###新的统一请求协议

新的统一协议是在Redis1.2中引入的，但是在Redis2.0中成为了与Redis服务器交互的标准方式。在统一协议中所有发送到Redis服务器的参数都是二进制安全的。这是总体格式：

	*<number of arguments> CR LF
	$<number of bytes of argument 1> CR LF
	<argument data> CR LF
	...
	$<number of bytes of argument N> CR LF
	<argument data> CR LF
	
看下这个例子：

	*3
	$3
	SET
	$5
	mykey
	$7
	myvalue

这是以上命令以带引号的字符串展现的样子，这样就可以看到这个请求中每个字节的准确内容：

	"*3\r\n$3\r\nSET\r\n$5\r\nmykey\r\n$7\r\nmyvalue\r\n"

就像你一会将会看到的，这个格式还会被用在Redis回复中。这个被用于每个参数中的格式$6\r\nmydata\r\n叫做Bulk 回复。Redis实际上使用的统一请求协议会返回内容列表，叫做[Multi-bulk回复](#multi-bulk-reply)。它是由N个不同的Bulk 回复合在一起，并且有一个字符串前缀*&lt;argc&gt;\r\n其中&lt;argc&gt;是后面参数（Bulk回复）的数量。
	
###回复

Redis会以不同类型的回复对命令进行响应。可以通过服务器发送的第一个字节来判定回复的类型：

*  如果是单行回复，那么第一个字节是「+」
*  如果回复的内容是错误信息，那么第一个字节是「_」
*  如果回复的内容是一个整型数字，那么第一个字节是「:」
*  如果是bulk回复，那么第一个字节是「$」
*  如果是multi-bulk回复，那么第一个字节是「*」

###状态回复

一个状态回复（或者：单行回复）的格式是以「+」开头，以「\r\n」结束的单行字符串。举个例子：

	+OK
	
客户端库要返回「+」后面的所有内容，这个例子里边是字符串「OK」。

###错误回复

错误的发送方式和状态回复很像。唯一的不同是第一个字节用「-」替代了「+」。
错误回复只有当一些奇怪的事情发生时才会被发送，例如如果你想要用错误的数据类型执行一个操作，或者这个命令不存在等等。所以客户端库应该在接收到一个错误回复的时候抛出一个异常。

###整形回复

这个类型的回复就是一个代表整数以CRLF结束的字符串，并且用一个字节的字符「:」作为前缀。例如「:0\r\n」，或者「:1000\r\n」都是整形回复。
像INCR或者LASTSAVE命令使用整型回复来返回一个没有特别含义的整型数字。对于INCR来说返回的是增加后的数字，对于LASTSAVE来说是一个UNIX时间等等。
像EXISTS这样的命令会返回1表示true，返回0表示false。
其他命令像SADD，SREM和SETNX在操作实际完成的时候会返回1，否则返回0。
以下命令将会返回整型回复：SETNX, DEL, EXISTS, INCR, INCRBY, DECR, DECRBY, DBSIZE, LASTSAVE, RENAMENX, MOVE, LLEN, SADD, SREM, SISMEMBER, SCARD

###Bulk回复

Bulk回复被服务器用来返回一个二进制安全的字符串。
	
	C: GET mykey
	S: $6\r\nfoobar\r\n

服务器以这种方式来发送数据：第一行是一个字节的内容「$」，之后跟着具体内容的字节数，接下来是CRLF，然后具体数据内容被发送，接下来是额外的两个字节的CRLF。服务器实际发送的序列是：
	
	"$6\r\nfoobar\r\n"
	
如果请求的内容不存在，那么bulk回复将会使用特殊值-1作为数据长度，例如：

	C: GET nonexistingkey
	S: $-1
	
当请求对象不存在，客户端库API不要返回一个空字符串，应该是一个nil对象。举个例子一个Ruby库应该返回「nil」，一个C库应该返回NULL（或者在回复对象中设置一个特殊标记），等等。

###Multi-bulk回复
<div id="multi-bulk-reply"></div>
像LRANGE这类的命令需要返回多个值（列表中的每个元素是一个值，LRANGE需要返回多个元素）。这通过multiple bulk write来实现，其第一行指明后面有多少个bulk　write。一个multi bulk回复的第一个字节一直是*。例如：

	C: LRANGE mylist 0 3
	s: *4
	s: $3
	s: foo
	s: $3
	s: bar
	s: $5
	s: Hello
	s: $5
	s: World

就像你看到的，multi bulk 回复和使用统一协议发送命令到Redis服务器使用的是同样的格式。
服务器发送的第一行是*4\r\n，用来指出下面将会有四个bulk回复。然后每个bulk write将会被传送。
如果指定的key不存在，那么这个key被认为拥有一个空列表，然后0会做为multi bulk的数量被发送。例如：

	C: LRANGE nokey 0 1
	S: *0

当[BLPOP][1]命令超时，它将返回值为nil的multi bulk回复。这个类型的multi bulk的数量为-1并且应该被解释为nil值。例如：

	C: BLPOP key 1
	S: *-1
	
当这个发生的时候，一个客户端库API应该返回一个nil对象而不是一个空列表。区分一个空的列表和一个错误条件（比如[BLPOP][1]命令的超时条件）是必要的。

###Multi-Bulk回复中的Nil元素

一个multi bulk　回复的单个元素可能会存在-1的长度，用来指明这个元素不存在并且不是空字符串。这个可能发生在启用了GET模式选项的SORT命令，并且指定的key不存在。包含一个空元素的multi bulk回复的例子：

	S: *3
	S: $3
	S: foo
	S: $-1
	S: $3
	S: bar
	
第二个元素是nil。客户端库需要返回如下内容：
	
	["foo",nil,"bar"]

###多个命令和管道

一个客户端可以使用相同的连接来发送多个命令。Redis是支持管道的，所以客户端可以通过一次写操作发送多个命令，不需要读取服务器的回复才能发送下一个命令。所有的回复可以在最后读取。
通常情况下Redis服务器和客户端之间会有非常快的连接，所以客户端支持这个特性不是那么重要，但如果一个应用需要在很短的时间里发送大量的命令那么使用管道将会非常快。

###发送命令的旧协议

在统一请求协议之前，Redis使用一个不同的协议来发送命令，这个协议仍然被支持因为通过telnet它很容易手写。在这个协议中存在两种命令：

* inline 命令: 命令很简单，就是用空格把参数分隔开来的字符串。二进制安全是不可能的。
* Bulk   命令: bulk命令和inline命令几乎是一样的，但是最后一个参数为了能够接受二进制安全的内容，所以需要以特殊的方式进行处理。

###Inline命令

向Redis发送命令最简单的方法是使用inline命令。以下是一个服务器/客户端之间使用inline命令进行交互的例子（服务器以S:作为开始，客户端以C:作为开始）

	C: PING
	S: +PONG

以下是另一个例子，一个返回整数的inline命令：

	C: EXISTS somekey
	S: :0
	
由于「somekey」不存在，所以服务器返回「:0」。
注意EXISTS命令只带有一个参数。多个参数以空格进行分隔。

###Bulk命令

当一些命令以inline命令发送的时候为了使最后一个参数支持二进制安全，需要以一个特殊的格式发送。这些命令将会把最后一个参数作为「字节计数器」，然后大量的数据会被发送（这些数据可以为二进制安全，因为服务器知道有多少字节需要读取）。
看下面这个例子：

	C: SET mykey 6
	C: foobar
	S: +OK

这个命令的最后一个参数是「6」。这个指明了后面数据，字符串「foobar」的字节数。注意即使这些数据被额外的二个字节大小的CRLF所截断。所有bulk命令的准确格式是：把最后一个参数替换成后面数据的字节数，接下来是数据本身和CRLF。为了能够让程序员更加清晰的理解，这是上面例子中客户端发送的字符串：

	"SET mykey 6\r\nfoobar\r\n"

Redis有一个内部的列表，记录了什么命令是inline，什么命令是bulk，所以你需要参考这个来发送命令。强烈推荐使用新的统一请求协议来替代旧的协议。

[1]: http://redis.io/commands/blpop "BLPOP"
[2]: http://redis.io/topics/protocol "Redis protocol"
