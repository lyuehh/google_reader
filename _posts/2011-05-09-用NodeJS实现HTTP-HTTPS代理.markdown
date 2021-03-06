---
layout: post
title:  "用NodeJS实现HTTP/HTTPS代理"
date:   2011-05-09 08:36:43
author: iGFW
categories: program
---

## 用NodeJS实现HTTP/HTTPS代理
### by iGFW
### at 2011-05-09 08:36:43
### original <http://igfw.tk/archives/2516>

<p>作者：<a title="查看 longbill 的站点" rel="external" href="http://php.js.cn/">longbill</a><br>
原文：http://cnodejs.org/blog/?p=885</p>
<p>身在天朝，难免会用到代理的时候。 比如在学校内网用代理免费上外网，在墙内用代理上404网站等。</p>
<p>现在使用的代理大部分为HTTP和Socket代理。 Socket代理更底层，需要本地解析域名，而HTTP代理则是基于HTTP协议之上的，不需要本地解析域名。下面我讲讲HTTP(S)代理的设计思路以及NodeJS代码实现。</p>
<p><strong><span></span>HTTP协议</strong></p>
<p>HTTP协议简单说来就是浏览器把一串字符串发送到目标服务器，然后把目标服务器返回回来的一串字符串显示给用户。</p>
<p>浏览器发送的这串字符主要分为两个部分，一部分是头，里面包含目标服务器域名，当前请求的文件路径等信息。另一部分是正文，一般的GET请求没有正文。</p>
<p>服务器返回来的字符串也分为头和正文。</p>
<p><strong>HTTP代理原理</strong></p>
<p>HTTP代理需要做的事情就是接收浏览器发来的请求字符串，再从请求字符串的头部分找出浏览器请求的目标主机，然后直接把这串请求字符串发给目标主机，再把目标主机返回的数据发给浏览器。 “什么？就这么简单？” “呃。。是啊，但这还没完。。”</p>
<p>现代浏览器一般都是默认采用HTTP/1.1版本，并且默认会发送Connection: keep-alive请求。  这些信息是写在请求的头部的，意思是通知目标服务器采用keep-alive技术继续处理后续的请求。  但是我们做的代理程序要想支持keep-alive是比较麻烦的。所以干脆就把这个篡改成Connection: close。  这样就可以保证浏览器请求的每个文件都会单独发送一个HTTP请求。</p>
<p><strong>下面是NodeJS代码实现</strong></p>
<div>
<div>
<pre>var net = require(&#39;net&#39;);
var local_port = 8893;

//在本地创建一个server监听本地local_port端口
net.createServer(function (client)
{

	//首先监听浏览器的数据发送事件，直到收到的数据包含完整的http请求头
	var buffer = new Buffer(0);
	client.on(&#39;data&#39;,function(data)
	{
		buffer = buffer_add(buffer,data);
		if (buffer_find_body(buffer) == -1) return;
		var req = parse_request(buffer);
		if (req === false) return;
		client.removeAllListeners(&#39;data&#39;);
		relay_connection(req);
	});

	//从http请求头部取得请求信息后，继续监听浏览器发送数据，同时连接目标服务器，并把目标服务器的数据传给浏览器
	function relay_connection(req)
	{
		console.log(req.method+&#39; &#39;+req.host+&#39;:&#39;+req.port);

		//如果请求不是CONNECT方法（GET, POST），那么替换掉头部的一些东西
		if (req.method != &#39;CONNECT&#39;)
		{
			//先从buffer中取出头部
			var _body_pos = buffer_find_body(buffer);
			if (_body_pos &lt; 0) _body_pos = buffer.length;
			var header = buffer.slice(0,_body_pos).toString(&#39;utf8&#39;);
			//替换connection头
			header = header.replace(/(proxy\-)?connection\:.+\r\n/ig,&#39;&#39;)
					.replace(/Keep\-Alive\:.+\r\n/i,&#39;&#39;)
					.replace(&quot;\r\n&quot;,&#39;\r\nConnection: close\r\n&#39;);
			//替换网址格式(去掉域名部分)
			if (req.httpVersion == &#39;1.1&#39;)
			{
				var url = req.path.replace(/http\:\/\/[^\/]+/,&#39;&#39;);
				if (url.path != url) header = header.replace(req.path,url);
			}
			buffer = buffer_add(new Buffer(header,&#39;utf8&#39;),buffer.slice(_body_pos));
		}

		//建立到目标服务器的连接
		var server = net.createConnection(req.port,req.host);
		//交换服务器与浏览器的数据
		client.on(&quot;data&quot;, function(data){ server.write(data); });
		server.on(&quot;data&quot;, function(data){ client.write(data); });

		if (req.method == &#39;CONNECT&#39;)
			client.write(new Buffer(&quot;HTTP/1.1 200 Connection established\r\nConnection: close\r\n\r\n&quot;));
		else
			server.write(buffer);
	}
}).listen(local_port);

console.log(&#39;Proxy server running at localhost:&#39;+local_port);

//处理各种错误
process.on(&#39;uncaughtException&#39;, function(err)
{
	console.log(&quot;\nError!!!!&quot;);
	console.log(err);
});

/**
* 从请求头部取得请求详细信息
* 如果是 CONNECT 方法，那么会返回 { method,host,port,httpVersion}
* 如果是 GET/POST 方法，那么返回 { metod,host,port,path,httpVersion}
*/
function parse_request(buffer)
{
	var s = buffer.toString(&#39;utf8&#39;);
	var method = s.split(&#39;\n&#39;)[0].match(/^([A-Z]+)\s/)[1];
	if (method == &#39;CONNECT&#39;)
	{
		var arr = s.match(/^([A-Z]+)\s([^\:\s]+)\:(\d+)\sHTTP\/(\d\.\d)/);
		if (arr &amp;&amp; arr[1] &amp;&amp; arr[2] &amp;&amp; arr[3] &amp;&amp; arr[4])
			return { method: arr[1], host:arr[2], port:arr[3],httpVersion:arr[4] };
	}
	else
	{
		var arr = s.match(/^([A-Z]+)\s([^\s]+)\sHTTP\/(\d\.\d)/);
		if (arr &amp;&amp; arr[1] &amp;&amp; arr[2] &amp;&amp; arr[3])
		{
			var host = s.match(/Host\:\s+([^\n\s\r]+)/)[1];
			if (host)
			{
				var _p = host.split(&#39;:&#39;,2);
				return { method: arr[1], host:_p[0], port:_p[1]?_p[1]:80, path: arr[2],httpVersion:arr[3] };
			}
		}
	}
	return false;
}

/**
* 两个buffer对象加起来
*/
function buffer_add(buf1,buf2)
{
	var re = new Buffer(buf1.length + buf2.length);
	buf1.copy(re);
	buf2.copy(re,buf1.length);
	return re;
}

/**
* 从缓存中找到头部结束标记(&quot;\r\n\r\n&quot;)的位置
*/
function buffer_find_body(b)
{
	for(var i=0,len=b.length-3;i &lt; len;i++)
	{
		if (b[i] == 0x0d &amp;&amp; b[i+1] == 0x0a &amp;&amp; b[i+2] == 0x0d &amp;&amp; b[i+3] == 0x0a)
		{
			return i+4;
		}
	}
	return -1;
}</pre>
</div>
</div>
<p>另外，可以用 “nohup node some.js &gt; /dev/null &amp;” 命令让nodejs程序在后台运行。</p>