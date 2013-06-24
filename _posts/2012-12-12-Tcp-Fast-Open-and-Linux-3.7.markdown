---
layout: post
title:  "Tcp Fast Open and Linux 3.7"
date:   2012-12-12 18:41:37
author: snnn
categories: program
---

## Tcp Fast Open and Linux 3.7
### by snnn
### at 2012-12-12 18:41:37
### original <http://www.udpwork.com/item/8845.html>

<p>今天<a href="http://www.kernel.org/">Linux</a>3.7发布了,一个引人注目的特性是，包含了完整的对TCP Fast Open（TFO）的支持，client端以及server端。</p>
<p>传统的TCP需要经过3次握手才能建立。假设我们要从服务器上用http协议下载一个图片</p>
<p>C-&gt;S ： SYN</p>
<p>S-&gt;C : SYN+ACK</p>
<p>C-&gt;S : ACK+ http get request</p>
<p>S-&gt;S : http response</p>
<p>所以一个最简单的HTTP请求也需要2个round-trip time才能完成。比如从北京到加州的round-trip time大约是200ms左右，2 round-trip time 就是400ms左右。无论如何增加带宽，都无济于事。</p>
<p>Google给出了一个解决方案，让第一个SYN包就携带数据，于是在一个RTT之内就能得到response。但是，这个看似很小的改动，实际应用起来却很不容易。</p>
<p>首先，目前只有<a href="http://www.kernel.org/">Linux</a>支持了TFO，先给你的<a href="http://www.kernel.org/">Linux</a>升级kernel吧。</p>
<p>其次，server和client的代码都得改。</p>
<p>server在listen之前要设置TFO选项。</p>
<p>int qlen = 5;                           
<br>
setsockopt(sfd, SOL_TCP, TCP_FASTOPEN, &amp;qlen, sizeof(qlen));</p>
<p>client就复杂多了。去掉connect调用，send/write方法要改成sendto或者sendmsg。</p>
<p>sendto(sfd, data, data_len, MSG_FASTOPEN,
<br>
                (struct sockaddr *) &amp;server_addr, addr_len);</p>
<p>看完TFO的paper后，作为一个搞后台开发的程序员，我当时首先想的是，如果<a href="http://www.mysql.com/">MySQL</a>也启用TFO的话……</p>
<p>我不知道你们怎么用MySQL的。JAVA程序员习惯用长连接，而PHP程序员习惯用短连接，我之前在某浪公司工作的时候，那边则是强烈要求使用短连接。短连接的原因是<a href="http://www.mysql.com/">MySQL</a>是按Connection分配内存资源（而非Query),短连接能提高内存利用率。但是另一方面，谁都知道，每次都打开、关闭TCP连接，进行身份认证，设置变量，会造成不必要的消耗，增大延迟。否则你告诉我，JAVA程序员干嘛非得去用连接池？</p>
<p>但是TFO要适用在MySQL上还必须得修改MySQL协议。因为TFO允许因SYN包在传输中被duplicate导致一个request被server处理两次。这对数据库的修改操作来说是不可忍受的。最简洁的解决办法就是在协议的头部加上一串随机数，server端cache一下最近几秒收到过的，进行排重。另外，要是<a href="http://www.mysql.com/">MySQL</a>在协议层也加入Fast Open的考虑就好了。</p>
<p>TFO的意义很深远，让我们可以把TCP当作UDP来用。重新考虑是否要保持长连接的问题。</p>
<p>附: Google的原始论文<a href="http://research.google.com/pubs/pub37517.html" title="http://research.google.com/pubs/pub37517.html">http://research.google.com/pubs/pub37517.html</a></p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/8845.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/8845.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">1</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/8845.html#reviews">评论: 1</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>