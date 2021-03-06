---
layout: post
title:  "[原]关于Erlang Socket的三种消息接收模式"
date:   2013-04-11 10:51:21
author: Skyman
categories: program
---

## [原]关于Erlang Socket的三种消息接收模式
### by Skyman
### at 2013-04-11 10:51:21
### original <http://www.udpwork.com/item/9655.html>

<p>转载请注明，来自：<a href="http://blog.csdn.net/skyman_2001">http://blog.csdn.net/skyman_2001</a></p>
<p>erlang的socket有3种消息接收模式：active、passive和active once，可以在gen_tcp:connect/3或gen_tcp:listen/2里设置{active, true | false |once}来实现，也可以用inet:setopts/2来动态设置。这3种模式的区别是：</p>
<p>1. active（主动消息接收）：非阻塞。当数据到达时系统会向控制进程发送{tcp, Socket, Data}消息。控制进程无法控制消息流；</p>
<p>2. passive（被动消息接收）：阻塞。控制进程必须主动调用recv()来接收消息。可以控制消息流；</p>
<p>3. active once（混合消息接收）：半阻塞。这种模式是主动的但仅针对一个消息，在控制进程收到一个消息后，必须显式调用inet:setopts(Socket, [{active, once}])来重新</p>
<p>激活以接收下一个消息。可以进行流量控制。这种模式相对于被动模式来说，有个优点是可以同时等待多个socket的数据。</p>
<p>
<br>
</p>
<p>Erlang官方推荐使用active once模式：</p>
<p>Active, once is the recommended way to implement a server in both UDP and
<br>
TCP.</p>
<p>The use of active once is superior to the other alternatives from a
<br>
programmatical standpoint, it is clean and robust. I don't recommend the
<br>
use of active true or active false just for performance reasons, because it
<br>
will cause other problems instead.
<br>

<br>
We are currently working with improvements regarding active once for TCP
<br>
since it obviously seems to be slower than necessary. Most probably it is
<br>
the same with UDP and we will look into that as well.
<br>

<br>
We don't think there need any significant performance difference between
<br>
active once and the other alternatives and hope to have a solution
<br>
confirming that soon (meaning r15b02 or 03)
<br>

<br>
Regards Kenneth Erlang/OTP, Ericsson
<br>
</p>
<p>详见：<a href="http://erlang.org/pipermail/erlang-questions/2012-April/065991.html">http://erlang.org/pipermail/erlang-questions/2012-April/065991.html</a>
<br>
</p>
<p>
<br>
</p>
<p>R15B02对active once进行了优化，降低延迟，最大吞吐量提高4~6倍。详见：<a href="https://github.com/erlang/otp/commit/689ba6e5657867eb3efbcb9a2dbec4aa98ddac5d">https://github.com/erlang/otp/commit/689ba6e5657867eb3efbcb9a2dbec4aa98ddac5d</a></p>
<div>作者：skyman_2001 发表于2013-4-11 10:51:21<a href="http://blog.csdn.net/skyman_2001/article/details/8786684">原文链接</a></div>
<div>阅读：106 评论：0<a href="http://blog.csdn.net/skyman_2001/article/details/8786684#comments">查看评论</a></div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/9655.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/9655.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">udpwork.com</a> 聚合
					|
					<a href="http://www.udpwork.com/item/9655.html#reviews">评论: 0</a>
					|
					<a href="http://www.jikenow.com/">要! 要! 即刻! Now!</a>
				</p>
			</div>