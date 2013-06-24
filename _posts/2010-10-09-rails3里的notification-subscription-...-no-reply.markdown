---
layout: post
title:  "rails3里的notification/subscription ... no reply"
date:   2010-10-09 08:35:33
author: Latest from ChinaonRails
categories: program
---

## rails3里的notification/subscription ... no reply
### by Latest from ChinaonRails
### at 2010-10-09 08:35:33
### original <http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/HTmMTHSuUTc/4519.html>

Instrument Anything in Rails 3<br>
<br>
<a href="http://gist.github.com/566725" rel="nofollow external">http://gist.github.com/566725</a><br>
<br>
 Rails 3, logging 抽象到ActiveSupport::Notifications 管发布logging事件<br>
<img src="http://i0.reflectornetwork.com/images-4/dWkudGhvdWdodGJvdC5jb20vYXNzZXRzL2Zyb2d3aW5lLmpwZw--.png" border="0"><br>
 ActiveSupport::LogSubscriber, 则消费 events 并负责logs 输出结果<br>
<br>
抽象的好处就是可以让更多的实体来发布事件，任何实体都能被量度了<br>
<br>
 Rails 3's notifications 开放出更大的可能性来做publishing 发布 subscribing 订阅 events事件。<br>
<br>
文章中队改进对自定义log事件的记录响应，以SolrInstrumentation 为例<br>
得出自己的的log，包含了查询的时间<br>
<br>
<div>Completed 200 OK in 562ms (Views: 507.9ms | ActiveRecord: 406.3ms | Solr: 52.2ms)</div>
<br>
另外的着力点在目前广泛采用的nosql上，比如MongoDB or CouchDB 替代 ActiveRecord ，关注它们的效率而产生的log需求。
<p><a href="http://feedads.g.doubleclick.net/~a/KvTV5sAyNkqmSTJ3vF84BNz7eLE/0/da"><img src="http://feedads.g.doubleclick.net/~a/KvTV5sAyNkqmSTJ3vF84BNz7eLE/0/di" border="0" ismap></a><br>
<a href="http://feedads.g.doubleclick.net/~a/KvTV5sAyNkqmSTJ3vF84BNz7eLE/1/da"><img src="http://feedads.g.doubleclick.net/~a/KvTV5sAyNkqmSTJ3vF84BNz7eLE/1/di" border="0" ismap></a></p><img src="http://feeds.feedburner.com/~r/LatestFromChinaonrails/~4/HTmMTHSuUTc" height="1" width="1"><img src="http://www1.feedsky.com/t1/421444148/RubyonRails_q5tb/feedsky/s.gif?r=http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/HTmMTHSuUTc/4519.html" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/RubyonRails_q5tb/421444148/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/RubyonRails_q5tb/421444148/art01.gif"></a></p>