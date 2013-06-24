---
layout: post
title:  "MongoDB-Log：基于MongoDB的日志系统"
date:   2011-09-19 22:29:39
author: nosqlfan
categories: program
---

## MongoDB-Log：基于MongoDB的日志系统
### by nosqlfan
### at 2011-09-19 22:29:39
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/560677710/6253001/1/item.html>

<p><span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span> 的 Capped Collection是一个天生的<span><a href="http://blog.nosqlfan.com/tags/%e6%97%a5%e5%bf%97%e7%b3%bb%e7%bb%9f" title="查看 日志系统 的全部文章">日志系统</a></span>，MongoDB自己的oplog就是用它来存储的，Capped Collection的特点是可以指定Collection的大小，当记录总大小超过设定大小后，老的数据会被自动抹掉用于存储新的内容。</p>
<p>本文介绍的是一个名叫mongodb-log的开源项目，它是一个基于MongoDB的<span><a href="http://blog.nosqlfan.com/tags/python" title="查看 Python 的全部文章">Python</a></span>日志系统。</p>
<p>使用它，首先你需要创建一个Capped Collection，当然，大小你可以自己设定。</p>
<pre>&gt; use mongolog
&gt; db.createCollection(&#39;log&#39;, {capped:true, size:100000})</pre>
<p>创建完成后通过 collection的 stats命令可以看到如下输出：</p>
<pre>&gt; db.log.stats()
{ &quot;ns&quot; : &quot;mongolog.log&quot;, &quot;count&quot; : 0, &quot;size&quot; : 0, &quot;storageSize&quot; :
100096, &quot;numExtents&quot; : 1, &quot;nindexes&quot; : 0, &quot;lastExtentSize&quot; : 100096,
&quot;paddingFactor&quot; : 1, &quot;flags&quot; : 0, &quot;totalIndexSize&quot; : 0, &quot;indexSizes&quot; : {
}, &quot;capped&quot; : 1, &quot;max&quot; : 2147483647, &quot;ok&quot; : 1 }</pre>
<p>然后你在你的Python程序里面就可以通过下面的方式记录日志了：</p>
<pre>import logging
from mongolog.handlers import MongoHandler
log = logging.getLogger('demo')
log.setLevel(logging.DEBUG)
log.addHandler(MongoHandler.to(db='mongolog', collection='log'))
log.debug('Some message')</pre>
<p>项目地址：<a href="https://github.com/andreisavu/mongodb-log">github.com</a></p><img src="http://www1.feedsky.com/t1/560677710/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/560677710/6253001/1/item.html" border="0" height="0" width="0">