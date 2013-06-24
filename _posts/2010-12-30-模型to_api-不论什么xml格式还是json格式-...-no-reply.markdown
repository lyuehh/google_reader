---
layout: post
title:  "模型to_api 不论什么xml格式还是json格式 ... no reply"
date:   2010-12-30 09:29:20
author: Latest from ChinaonRails
categories: program
---

## 模型to_api 不论什么xml格式还是json格式 ... no reply
### by Latest from ChinaonRails
### at 2010-12-30 09:29:20
### original <http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/kfzchzU_fRU/4736.html>

<a href="https://github.com/atomicobject/to_api" rel="nofollow external">https://github.com/atomicobject/to_api</a><br>
A lightweight, format-agnostic API transformation gem.<br>
<br>
<a href="http://spin.atomicobject.com/2010/12/28/to_api-simple-api-transformations" rel="nofollow external">http://spin.atomicobject.com/2010/12/28/to_api-simple-api-transformations</a><br>
<br>
Models shouldn't know about formats. Controllers shouldn't know what attributes your domain objects have. It should be trivial to serve the same data in multiple formats.<br>
<br>
Enter to_api. Instead of to_xml and as_json, simply implement to_api. Return an unformatted hash. Let the model understand the data. Let the controller understand the format. In Rails, the controller can simply call render :xml =&gt; model.to_api or render :json =&gt; model.to_api.<br>
<br>
调用 to_api method<br>
 You'll get back a hash of arrays, hashes, strings, numbers, and nil. All of these are very easily converted to a format of your choice.<br>
<br>
 模型定义属性漏给某个API，模型说了算<br>
<br>
<img src="http://news.sznews.com/images/attachement/jpg/site3/20101222/001422474e250e7c18662e.jpg" border="0">
<p><a href="http://feedads.g.doubleclick.net/~a/MoN12YO0nKp7Dp5YnRBYighQiAY/0/da"><img src="http://feedads.g.doubleclick.net/~a/MoN12YO0nKp7Dp5YnRBYighQiAY/0/di" border="0" ismap></a><br>
<a href="http://feedads.g.doubleclick.net/~a/MoN12YO0nKp7Dp5YnRBYighQiAY/1/da"><img src="http://feedads.g.doubleclick.net/~a/MoN12YO0nKp7Dp5YnRBYighQiAY/1/di" border="0" ismap></a></p><img src="http://feeds.feedburner.com/~r/LatestFromChinaonrails/~4/kfzchzU_fRU" height="1" width="1"><img src="http://www1.feedsky.com/t1/457753407/RubyonRails_q5tb/feedsky/s.gif?r=http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/kfzchzU_fRU/4736.html" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/RubyonRails_q5tb/457753407/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/RubyonRails_q5tb/457753407/art01.gif"></a></p>