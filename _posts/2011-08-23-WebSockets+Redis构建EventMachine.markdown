---
layout: post
title:  "WebSockets+Redis构建EventMachine"
date:   2011-08-23 14:37:02
author: nosqlfan
categories: program
---

## WebSockets+Redis构建EventMachine
### by nosqlfan
### at 2011-08-23 14:37:02
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/549276931/6253001/1/item.html>

<p><a href="http://rubyeventmachine.com/">EventMachine</a> 是一个Ruby的事件驱动网络库，本文不是要介绍它，而是要介绍一个以<span><a href="http://blog.nosqlfan.com/tags/redis" title="查看 Redis 的全部文章">Redis</a></span> 的 <span><a href="http://blog.nosqlfan.com/tags/pubsub" title="查看 Pub/Sub 的全部文章">Pub/Sub</a></span> 机制为后端，以<span><a href="http://blog.nosqlfan.com/tags/websocket" title="查看 WebSocket 的全部文章">WebSocket</a></span>s为前端的类<span><a href="http://blog.nosqlfan.com/tags/eventmachine" title="查看 EventMachine 的全部文章">EventMachine</a></span>实现。</p>
<p>前端代码，创建Socket连接到本地8081端口，当有消息push过来的时候，将消息打印到指定的div里：</p>
<pre>&lt;!DOCTYPE html&gt;

&lt;html&gt;
&lt;head&gt;
	&lt;title&gt;Websockets!&lt;/title&gt;
	&lt;script&gt;
	function onMessage(evt) {
		con = document.getElementById(&quot;console&quot;);
		con.innerHTML += evt.data;
		con.innerHTML += &#39;&lt;br /&gt;&#39;;
	}
	websocket = new WebSocket(&quot;ws://localhost:8081&quot;);
	websocket.onmessage = function(evt) { onMessage(evt); };
	&lt;/script&gt;
&lt;/head&gt;

&lt;body&gt;
	&lt;div id=&quot;console&quot;&gt;
	&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
<p>后端代码：</p>
<pre>require &#39;redis&#39;
require &#39;em-websocket&#39;

SOCKETS = []
@redis = Redis.new(:host =&gt; &#39;127.0.0.1&#39;, :post =&gt; 6379)

# Creating a thread for the EM event loop
Thread.new do
  EventMachine.run do
    # Creates a websocket listener
    EventMachine::WebSocket.start(:host =&gt; &#39;0.0.0.0&#39;, :port =&gt; 8081) do |ws|
      ws.onopen do
        # When someone connects I want to add that socket to the SOCKETS array that
        # I instantiated above
        puts &#39;creating socket&#39;
        SOCKETS &lt;&lt; ws
      end

      ws.onclose do
        # Upon the close of the connection I remove it from my list of running sockets
        puts &#39;closing socket&#39;
        SOCKETS.delete ws
      end
    end
  end
end

# Creating a thread for the redis subscribe block
Thread.new do
  @redis.subscribe(&#39;ws&#39;) do |on|
    # When a message is published to &#39;ws&#39;
    on.message do |chan, msg|
     puts &quot;sending message: #{msg}&quot;
     # Send out the message on each open socket
     SOCKETS.each {|s| s.send msg}
    end
  end
end

sleep</pre>
<p>开启8081端口接受连接，同时连到Redis上订阅ws这个key的消息</p>
<p>当前后端都启动并连接上后，你就可以用如下代码往Redis的ws这个key上写消息，页面上就能看到push过来的消息了：</p>
<pre>require &#39;redis&#39;
@redis = Redis.new(:host =&gt; &#39;127.0.0.1&#39;, :post =&gt; 6379)
@redis.publish &#39;ws&#39;, &#39;Something witty&#39;</pre>
<p>来源：<a href="http://jessedearing.com/nodes/4-pubsubbin-with-redis-eventmachine-and-websockets">jessedearing.com</a><br>
<span style="color:#ff0000">have fun！</span></p>
<table cellspacing="0" cellpadding="2" border="0" width="100%" style="clear:both">
    
    <tr>
        <td><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2938.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2902.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">案例：用Redis来存储关注关系</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2925.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2902.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">redis 适用场景与实现</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2799.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2902.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">Redis事件驱动库结构</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F222.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2902.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">Redis 2.0发布,新功能细数</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1675.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2902.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">Redis在.NET上的应用教程</font>
                    </a>
                </td>
            </tr>
    
    <tr>
        <td align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/549276931/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/549276931/6253001/1/item.html" border="0" height="0" width="0">