---
layout: post
title:  "使用Ruby Oauth连接腾讯微薄时不能获取request_token ... 4 replies"
date:   2011-01-13 11:45:06
author: Latest from ChinaonRails
categories: program
---

## 使用Ruby Oauth连接腾讯微薄时不能获取request_token ... 4 replies
### by Latest from ChinaonRails
### at 2011-01-13 11:45:06
### original <http://chinaonrails.com/topic/view/4774.html>

在获取request_token的时候就碰到了问题,下面是代码：<br>
<br>
require 'rubygems'     <br>
gem 'oauth','0.4.3'<br>
require 'oauth'<br>
<br>
<br>
API_KEY = "9b43a5afaca04afd8eb573e673fce3f9"<br>
API_KEY_SECRET = "dd6b3cef9a6896e373085a83cd8b74f9"<br>
<br>
options = {<br>
  :site =&gt; &quot;https://open.t.qq.com/&quot;,<br>
  :request_token_path =&gt; &quot;/cgi-bin/request_token&quot;,<br>
  :access_token_path =&gt; &quot;/cgi-bin/access_token&quot;,<br>
  :authorize_path =&gt; &quot;/cgi-bin/authorize&quot;,<br>
  :scheme =&gt; :query_string<br>
}<br>
<br>
<br>
@consumer=OAuth::Consumer.new(API_KEY, API_KEY_SECRET, options)<br>
@request_token=@consumer.get_request_token<br>
puts @request_token.inspect<br>
<br>
同样的方式，连接新浪微薄，豆瓣是没有问题的。<br>
求解，谢谢<img src="http://www1.feedsky.com/t1/465637181/RubyonRails_q5tb/feedsky/s.gif?r=http://chinaonrails.com/topic/view/4774.html" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/RubyonRails_q5tb/465637181/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/RubyonRails_q5tb/465637181/art01.gif"></a></p>