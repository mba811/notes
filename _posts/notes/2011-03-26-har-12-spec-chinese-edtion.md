---
layout: post
title: HAR(HTTP Archive) 规范
tags: [notes]
---

[HAR][1]（HTTP Archive），是一个用来储存HTTP请求/响应信息的通用文件格式，基于[JSON]。这个格式的出现可以使HTTP监测工具以一种通用的格式导出所收集的数据，这些数据可以被其他支持HAR的HTTP[分析工具]（包括[Firebug]，[httpwatch]，[Fiddler]等）所使用，来分析网站的性能瓶颈。目前HAR规范最新版本为[HAR 1.2][1]。HAR文件必须是UTF-8编码，有无BOM无所谓。

###HAR数据结构：

一个HAR文件就是一个JSON对象，如下：

	{
	    "log": {
	        "version" : "1.2",
	        "creator" : {},
	        "browser" : {},
	        "pages": [],
	        "entries": [],
	        "comment": ""
	    }
	}

* version [string] – 版本，默认为1.1。
* creator [object] – 创建HAR文件的程序名称和版本信息。
* browser [object, 可选] – 浏览器的名称和版本信息。
* pages [array, 可选] – 页面列表，如果应用不支持按照page分组，可以省去此字段。
* entries [array] – 所有HTTP请求的列表。
* comment [string, 可选]（new in 1.2） – 注释。

注：每个页面对应一个 对象，每个HTTP请求对应一个对象。如果HTTP的监测分析工具不能把请求按照page分组，那么为空。

###&lt;creator&gt; & &lt;browser&gt;

这两个对象的结构是一样的

	"creator": {
	    "name": "Firebug",
	    "version": "1.6",
	    "comment": "",
	}

	"browser": {
	    "name": "Firefox",
	    "version": "3.6",
	    "comment": ""
	}
	
* name [string] – HAR生成工具或者浏览器的名称。
* version [string] – HAR生成工具或者浏览器的版本。
* comment [string, 可选]（new in 1.2） – 注释。

###&lt;pages&gt;
	
这个对象保存了页面列表，格式如下：

	"pages": [
	    {
	        "startedDateTime": "2009-04-16T12:07:25.123+01:00",
	        "id": "page_0",
	        "title": "Test Page",
	        "pageTimings": {...},
	        "comment": ""
	    }
	]

* startedDateTime [string] – 页面开始加载的时间(格式ISO 8601 – YYYY-MM-DDThh:mm:ss.sTZD, 例如2009-07-24T19:20:30.45+01:00)。
* id [string] – page的唯一标示，entry会用到这个id来和page关联在一起。
* title [string] – 页面标题。
* pageTimings[object] – 页面加载过程中详细的时间信息。
* comment [string, 可选]（new in 1.2） – 注释。

###&lt;pageTimings&gt;

这个对象描述了在页面加载过程中各个事件发生的时间点。所有的时间都是以毫秒计算的。如果有的时间无法计算出来，那么相应字段置为-1。

	"pageTimings": [
	    {
	        "onContentLoad": 1720,
	        "onLoad": 2500,
	        "comment": ""
	    }
	]

*  onContentLoad [number, 可选] – 页面内容加载时间，相对于页面开始加载时间的毫秒数（page.startedDateTime）。如果时间不适用于当前的请求，那么置为-1。
* onLoad [number,可选] – 页面加载时间（即onLoad事件触发的时间）。相对于页面开始加载时间的毫秒数（page.startedDateTime）。如果时间不适用于当前的请求，那么置为-1。
* comment [string, 可选]（new in 1.2） – 注释。

由于不同浏览器实现不一样，onContentLoad属性可能代表**DOMContentLoad**事件触发，也可能代表document.readyState等于**interactive**。

###&lt;entries&gt;

这个对象包含了一个数组，数组中每个元素的内容就是一个HTTP请求的相应信息。用startedDateTime来排序的话可以加快数据导出的速度。HAR分析工具要确保此数组是按照startedDateTime排序的。

	"entries": [
	    {
	        "pageref": "page_0",
	        "startedDateTime": "2009-04-16T12:07:23.596Z",
	        "time": 50,
	        "request": {...},
	        "response": {...},
	        "cache": {...},
	        "timings": {},
	        "serverIPAddress": "10.0.0.1",
	        "connection": "52492",
	        "comment": ""
	    }
	]

* pageref [string, unique, 可选] – 页面id，如果不支持按照page分组，那么字段为空。
* startedDateTime [string] – 请求开始时间 (格式ISO 8601 – YYYY-MM-DDThh:mm:ss.sTZD)。
* time [number] – 请求消耗的时间，以毫秒为单位。这个值是timings对象中所有可用(值不为-1) timing的和。
* request [object] – 请求的详细信息。
* response [object] – 响应的详细信息。
* cache [object] – 缓存使用情况的信息。
* timings [object] – 请求/响应过程（round trip）的详细时间信息。
* serverIPAddress [string, 可选]（new in 1.2） – 服务器IP地址。
* connection [string, 可选]（new in 1.2）– TCP/IP连接的唯一标示。 如果程序不支持，直接忽略此字段。
* comment [string, optional]（new in 1.2） – 注释。

###&lt;request&gt;
	
这个对象包含了请求的详细信息

	"request": {
	    "method": "GET",
	    "url": "http://www.example.com/path/?param=value",
	    "httpVersion": "HTTP/1.1",
	    "cookies": [],
	    "headers": [],
	    "queryString" : [],
	    "postData" : {},
	    "headersSize" : 150,
	    "bodySize" : 0,
	    "comment" : "",
	}

* method [string] – 请求方法(GET，POST，...)。
* url [string] – 请求的绝对URL(fragments are not included)。
* httpVersion [string] – 请求HTTP版本。
* cookies [array] – cookie列表。
* headers [array] – header列表。
* queryString [object] – 查询字符串信息。
* postData [object, 可选] – Post数据信息。
* headersSize [number] – HTTP请求头的字节数。如果不可用，设置为-1。
* bodySize [number] – 请求body字节数（POST数据）。如果不可用，设置为-1。
* comment [string, 可选]（new in 1.2）– 注释。

###&lt;response&gt;

这个对象包含响应的详细信息。

	"response": {
	    "status": 200,
	    "statusText": "OK",
	    "httpVersion": "HTTP/1.1",
	    "cookies": [],
	    "headers": [],
	    "content": {},
	    "redirectURL": ",
	    "headersSize" : 160,
	    "bodySize" : 850,
	    "comment" : ""
	}

* status [number] – 响应状态。
* statusText [string] – 响应状态描述。
* httpVersion [string] – HTTP版本。
* cookies [array] – cookie列表。
* headers [array] – header列表。
* content [object] – 响应内容的详细信息。
* redirectURL [string] – Location响应头中的重定向URL。
* headersSize [number]*  – HTTP请求头的字节数。如果不可用，设置为-1。
* bodySize [number] – 接收的body字节数。如果响应来自缓存(304)，那么设置为0。如果不可用，设置为-1。
* comment [string, optional]（new in 1.2） – 注释。
注：headersSize – 响应头大小只对从服务器接收到的header进行计算。被浏览器加上的header不计算在内，但是会加在header列表中。

###&lt;cookies&gt;
		
这个对象包含了所有的cookie（在和中被使用）。

	"cookies": [
	    {
	        "name": "TestCookie",
	        "value": "Cookie Value",
	        "path": "/",
	        "domain": "www.janodvarko.cz",
	        "expires": "2009-07-24T19:20:30.123+02:00",
	        "httpOnly": false,
	        "secure": false,
	        "comment": "",
	    }
	]

* name [string] – cookie名称。
* value [string] – cookie值。
* path [string, 可选] – cookie Path。
* domain [string, 可选] – cookie域名。
* expires [string, 可选] – cookie过期时间。(格式ISO 8601 – YYYY-MM-DDThh:mm:ss.sTZD, 例如2009-07-24T19:20:30.123+02:00)。
* httpOnly [boolean, 可选] – 如果cookie只是在HTTP下有效，此值设置为true，否则设置为false。
* secure [boolean, 可选]（new in 1.2） – 如果cookie通过ssl传送，此值设置为true，否则设置为false。
* comment [string, 可选]（new in 1.2） – 注释。

###&lt;headers&gt;
	
这个对象包含了所有的header（可以在**lt;request&gt;**and**&lt;response&gt;**中使用）

	"headers": [
	    {
	        "name": "Accept-Encoding",
	        "value": "gzip,deflate",
	        "comment": ""
	    },

	    {
	        "name": "Accept-Language",
	        "value": "en-us,en;q=0.5",
	        "comment": ""
	    }
	]


###&lt;queryString&gt;
	
这个对象包含了查询字符串中所有的paramter-value对（嵌在对象中）。

	"queryString": [
	    {
	        "name": "param1",
	        "value": "value1",
	        "comment": ""
	    },

	    {
	        "name": "param1",
	        "value": "value1",
	        "comment": ""
	    }
	]

###&lt;postData&gt;
	
这个对象描述了POST的数据（嵌在对象中）

	"postData": {
	    "mimeType": "multipart/form-data",
	    "params": [],
	    "text" : "plain posted data",
	    "comment": ""
	}

* mimeType [string] – POST数据的MIME类型。
* params [array] – POST参数列表 (in case of URL encoded parameters)。
* text [string] – POST数据的纯文本形式(Plain text posted data)。
* comment [string, optional]（new in 1.2） – 注释。
注意：text和params字段是互斥的。

###&lt;params&gt;

POST请求参数列表（嵌在 对象中）

	"params": [
	    {
	        "name": "paramName",
	        "value": "paramValue",
	        "fileName": "example.pdf",
	        "contentType": "application/pdf",
	        "comment": ""
	    }
	]

* name [string] – POST参数名。
* value [string, 可选] – POST参数的值，或者POST文件的内容。
* fileName [string, 可选] – POST文件的文件名。
* contentType [string, 可选] – POST文件的类型。
* comment [string, 可选]（new in 1.2） – 注释。

###&lt;content&gt;

这个对象描述了响应内容的详细情况（嵌在 对象中）

	"content": {
	    "size": 33,
	    "compression": 0,
	    "mimeType": "text/html; charset="utf-8",
	    "text": "n",
	    "comment": ""
	}


* size [number] – 返回内容的字节数。如果内容没有被压缩，应该和response.bodySize相等；如果被压缩，那么会大于response.bodySize。
* compression [number, 可选] – 节省的字节数。如果无法提供此信息，则忽略此字段。
* mimeType [string] – 响应文本的MIME类型 (Content-Type响应头的值)。MIMIE类型的字符集也包含在内。
* text [string, 可选] – 从服务器返回的响应body或者从浏览器缓存加载的内容。这个字段只能用文本型的内容来填充。字段内容可以是HTTP decoded(decompressed &amp; unchunked)的文本，或者是经编码（例如，base64）过的响应内容。如果信息不可用，忽略此字段。
* encoding [string, 可选]（new in 1.2） – 响应内容的编码格式，例如”base64″。如果text字段的内容是经过了HTTP解码(decompressed &amp; unchunked)的，那么忽略此字段。
* comment [string, optional]（new in 1.2） – 注释。

在设置text字段之前，HTTP响应内容已经完成了解码(decompressed &amp; unchunked)，然后把原始字符编码转换成UTF-8。字段内容同样可以使用base64进行编码，但是HAR工具必须有解码base64的能力。对字段编码还可以把二进制内容包含进HAR文件中。

这有另一个例子，原始响应内容是：

	"content": {
	    "size": 33,
	    "compression": 0,
	    "mimeType": "text/html; charset="utf-8",
	    "text": "PGh0bWw+PGhlYWQ+PC9oZWFkPjxib2R5Lz48L2h0bWw+XG4=",
	    "encoding": "base64",
	    "comment": ""
	}


###&lt;cache&gt;

这个对象包含了命中的浏览器缓存信息

	"cache": {
	    "beforeRequest": {},
	    "afterRequest": {},
	    "comment": ""
	}

* beforeRequest [object, 可选] – 在请求之前缓存的状态。如果信息不可用，可以忽略此字段。
* afterRequest [object, 可选] – 在请求之后缓存的状态。 如果信息不可用，可以忽略此字段。
* comment [string, 可选]（new in 1.2） – 注释。

如果缓存信息为空，那么对象如下（或者也可以直接删掉cache这个字段）：

	"cache": {}

如果在请求之前，缓存信息不可用，并且在请求之后也没有对内容进行缓存，那么对象如下：

	"cache": {
		"afterRequest": null
	}

如果在请求前后都没有缓存存在，那么对象如下：

	"cache": {
	    "beforeRequest": null,
	    "afterRequest": null
	}

以下对象表示请求之前没有缓存，但是在请求后，下载的内容被存在了本地缓存中。

	"cache": {
	    "beforeRequest": null,
	    "afterRequest": {
	        "expires": "2009-04-16T15:50:36",
	        "lastAccess": "2009-16-02T15:50:34",
	        "eTag": ",
	        "hitCount": 0,
	        "comment": ""
	    }
	}

beforeRequest和afterRequest对象使用以下相同的结构：

	"beforeRequest": {
	    "expires": "2009-04-16T15:50:36",
	    "lastAccess": "2009-16-02T15:50:34",
	    "eTag": ",
	    "hitCount": 0,
	    "comment": "“”
	}

* expires [string, 可选] – 缓存过期时间。
* lastAccess [string] – 缓存最后被访问的时间。
* eTag [string] – Etag
* hitCount [number] – 缓存被访问的次数。
* comment [string, 可选]（new in 1.2） – 注释

###&lt;timings&gt;

这个对象描述了请求/响应过程的各个阶段。时间都是以毫秒为单位。

	"timings": {
	    "blocked": 0,
	    "dns": -1,
	    "connect": 15,
	    "send": 20,
	    "wait": 38,
	    "receive": 12,
	    "ssl": -1,
	    "comment": ""
	}

* blocked [number, 可选] – 建立网络连接时在队列里边等待的时间。如果时间对于当前请求不可用，置为-1。
* dns [number, 可选] – DNS查询时间。如果时间对于当前请求不可用，置为-1。
* connect [number, 可选] – 建立TCP连接所需的时间。如果时间对于当前请求不可用，置为-1。
* send [number] – 发送HTTP请求到服务器所需的时间。
* wait [number] – 等待服务器返回响应的时间。
* receive [number] – 接收服务器响应（或者缓存）所需时间。
* ssl [number, 可选]（new in 1.2） – SSL/TLS验证花费时间。如果这个字段被定义了，那么这个时间也会被包含进connect字段中(为了向后兼容HAR1.1)。如果时间对于当前请求不可用，置为-1。
* comment [string, 可选]（new in 1.2） – 注释。

**send**，**wait**，**receive**时间是必须提供的，并且不能为负数。导出工具如果不能提供blocked,dns,connect和ssl等时间，那么这些时间可以被忽略。如果工具可以提供这些时间，但是不适用的话，可以把这些值设置为-1。 例如，当请求使用了一个存在的连接，connect会被置为-1。

一个请求所花的时间等于这些时间的和，不包括值为-1的。     
**entry.time == entry.timings.blocked + entry.timings.dns + entry.timings.connect + entry.timings.send + entry.timings.wait + entry.timings.receive**

###自定义字段

HAR规范允许自定义字段，但是要遵循如下规则：

* 自定义字段（field）和元素（element）必须以下划线开头(规范中的字段必须不能以下划线开头)
* HAR工具必须忽略所有自定义字段和元素，如果这个HAR文件不是当前HAR工具生成的。
* 当HAR工具不知道如何解析非自定义字段的时候，忽略它们。
* 当文件包含了已经被废弃的非自定义字段时候，HAR工具可以拒绝解析此文件。

###版本格式

HAR规范的版本号有如下规则：
<主版本号>.<副版本号>
主版本号表示规范的向后兼容性，副版本号表示增量修改。所以，任何向后兼容的修改都会增加副版本号。如果一个存在的字段被废弃了，那么主版本号要增加(例如2.0)。

Examples:
1.2 -> 1.3(向后兼容)      
1.111 -> 1.112 (向后兼容)      
1.5 -> 2.0 (2.0不兼容1.5)       
如果HAR工具只支持HAR1.1，那么以下的代码可以被用来检测不被兼容的版本。      

	if (majorVersion != 1 || minorVersion < 1)
	{
		throw “Incompatible version”;
	}
	
这个例子中，如果被解析文件的版本为0.8,0.9,1.0等，那么工具会抛出异常，但是1.1, 1.2, 1.112等版本可以被解析。2.x版本直接拒绝解析。


原文：[http://www.softwareishard.com/blog/har-12-spec/](http://www.softwareishard.com/blog/har-12-spec/)    
Google Group: [http://groups.google.com/group/http-archive-specification【需翻墙】](http://groups.google.com/group/http-archive-specification)


[JSON]: http://www.ietf.org/rfc/rfc4627.txt "JSON"
[分析工具]: http://www.softwareishard.com/blog/har-adopters/ "分析工具"
[Firebug]: http://getfirebug.com/ "Firebug"
[httpwatch]: http://www.httpwatch.com/ "httpwatch"
[Fiddler]: http://www.fiddler2.com/fiddler2/ "Fiddler"

[1]: http://www.softwareishard.com/blog/har-12-spec/ "har-12-spec"