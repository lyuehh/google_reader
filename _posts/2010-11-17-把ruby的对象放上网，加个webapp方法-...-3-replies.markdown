---
layout: post
title:  "把ruby的对象放上网，加个webapp方法 ... 3 replies"
date:   2010-11-17 14:52:17
author: Latest from ChinaonRails
categories: program
---

## 把ruby的对象放上网，加个webapp方法 ... 3 replies
### by Latest from ChinaonRails
### at 2010-11-17 14:52:17
### original <http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/6__ggJjAJEo/4622.html>

创意来自 最近的ruby大会上，受到启发 -@JEG2's talk at Rubyconf<br>
<br>
<a href="https://gist.github.com/675667" rel="nofollow external">https://gist.github.com/675667</a><br>
<br>
运行代码<br>
<div>require 'rubygems'<br>
require 'rack'<br>
<br>
class Object<br>
  def webapp<br>
    class &lt;&lt; self<br>
       define_method :call do |env|<br>
         func, *attrs = env[&#39;PATH_INFO&#39;].split(&#39;/&#39;).reject(&amp;:empty?)<br>
         [200, {}, send(func, *attrs)]<br>
       end<br>
    end<br>
    self<br>
  end<br>
end<br>
<br>
Rack::Handler::Mongrel.run [].webapp, :Port =&gt; 9292</div>
<br>
在浏览器里操作Array <br>
<br>
# http://localhost:9292/push/1 -&gt; 1<br>
# http://localhost:9292/push/2 -&gt; 12<br>
# http://localhost:9292/push/3 -&gt; 123<br>
<br>
# http://localhost:9292/to_a -&gt; 123<br>
<br>
# http://localhost:9292/pop -&gt; 3<br>
# http://localhost:9292/shift -&gt; 1<br>
<br>
其它语言版本<br>
# Node.js: https://gist.github.com/700995<br>
# Groovy: https://gist.github.com/702337<br>
# Python: https://gist.github.com/702001<br>
<br>
原理解释：<br>
<a href="http://stackoverflow.com/questions/4198883/exposing-any-ruby-object-over-the-web" rel="nofollow external">http://stackoverflow.com/questions/4198883/exposing-any-ruby-object-over-the-web</a><br>
<br>
下面link，这个？？ 你如果没弄明白，可别干<br>
http://localhost:9292/instance_eval/exec("rm -rf /")
<p><a href="http://feedads.g.doubleclick.net/~a/QMrVW4f4Gc-LWds1dXUAtkeywf0/0/da"><img src="http://feedads.g.doubleclick.net/~a/QMrVW4f4Gc-LWds1dXUAtkeywf0/0/di" border="0" ismap></a><br>
<a href="http://feedads.g.doubleclick.net/~a/QMrVW4f4Gc-LWds1dXUAtkeywf0/1/da"><img src="http://feedads.g.doubleclick.net/~a/QMrVW4f4Gc-LWds1dXUAtkeywf0/1/di" border="0" ismap></a></p><img src="http://feeds.feedburner.com/~r/LatestFromChinaonrails/~4/6__ggJjAJEo" height="1" width="1"><img src="http://www1.feedsky.com/t1/438107841/RubyonRails_q5tb/feedsky/s.gif?r=http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/6__ggJjAJEo/4622.html" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/RubyonRails_q5tb/438107841/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/RubyonRails_q5tb/438107841/art01.gif"></a></p>