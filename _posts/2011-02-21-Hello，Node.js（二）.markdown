---
layout: post
title:  "Hello，Node.js（二）"
date:   2011-02-21 10:48:00
author: dreamhead
categories: program
---

## Hello，Node.js（二）
### by dreamhead
### at 2011-02-21 10:48:00
### original <http://dreamhead.blogbus.com/logs/106188588.html>

<p>从<a href="http://dreamhead.blogbus.com/logs/105599686.html">上一篇的介绍</a>
里面，我们知道了Node.js可以用来编写Server端应用。但实际上，Node.js带来的可不这些，其实，我们可以把它视为一个独立的编程平台，通过它，我们可以像使用Ruby、Python一样使用JavaScript。<br>
<br>
所以，学习Node.js和学习其它编程平台可以等同起来，事实上，人们也是这样来对待Node.js的。比如，在Ruby中，我们通过RubyGems管理各种软件包进行管理，而在Node.js中，与之对应的是<a href="http://npmjs.org/">npm</a>
。<br>
<br>
安装npm，非常简单，只要执行下面的命令即可。<br>
$ curl http://npmjs.org/install.sh | sh<br>
<br>
有了npm，我们就可以利用它安装软件包了，欲知有哪些软件包可以安装，npm为我们提供了<a href="http://search.npmjs.org/">一个可以搜寻的地方</a>
。<br>
<br>
如果你熟悉常见的包管理器，用npm安装软件包和它们如出一辙。以Express为例：<br>
$ npm install express<br>
<br>
<a href="http://expressjs.com/">Express</a>
是一个web开发框架，不过，它并不是<a href="http://rubyonrails.org/">Rails</a>
那样的全功能框架，而更多的把精力集中在web层，如果你熟悉Ruby世界的开发框架，它更像<a href="http://www.sinatrarb.com/">Sinatra</a>
。<br>
<br>
下面就是Express版的“hello, world”：<br>
var app = require('express').createServer();<br>
<br>
app.get('/', function(req, res){<br>
    res.send(&#39;hello world&#39;);<br>
});<br>
<br>
app.listen(3000);<br>
<br>
把它保存在hello_express.js中，我们就可以运行它了：<br>
$ node hello_express.js<br>
<br>
好，打开浏览器，访问http://localhost:3000/，我们就可以看到来自express的问候了。</p>