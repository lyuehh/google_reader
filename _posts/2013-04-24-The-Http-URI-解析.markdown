---
layout: post
title:  "The Http URI 解析"
date:   2013-04-24 21:30:00
author: sunchangming
categories: program
---

## The Http URI 解析
### by sunchangming
### at 2013-04-24 21:30:00
### original <http://www.udpwork.com/item/9718.html>

<p>今天我在对照着<a href="http://www.ietf.org/rfc/rfc3986.txt">rfc3986</a>看java.net.URI的实现。URL无处不在，摆在我们面前的一个核心问题就是，如何解析它。</p>
<p>URI的全称是Uniform Resource Identifiers。URL的全称是Uniform Resource Locator，它是URI的子集。意思是说，我们是通过资源的primary access mechanism的一种表示方式来标识这个资源，而不是通过名字、或其它属性的方式标识。</p>
<p>Http request的第一行中，method后面是Request-URI。</p>
<p>如： </p>
<p>GET /index.html HTTP/1.1</p>
<p>其中”/index.html”就是 Request-URI。</p>
<p>Request-URI有4种形式：</p>
<ol><li>*    就一个星号。 如 “OPTIONS * HTTP/1.0”</li>
<li>absoluteURI 。下面详细说</li>
<li>abs_path： 即”/” path_segments [ “?” query ]</li>
<li>authority。 给Connect方法用</li>
</ol>
<p>absoluteURI 有三种形式：</p>
<p>net_path 模式： scheme “://” authority [ abs_path ] [ “?” query ]</p>
<p>abs_path 模式： scheme “:/” path_segments [ “?” query ]</p>
<p>opaque 模式： scheme “:”  authority uric_no_slash *uric</p>
<p>平时用的主要都是第一种。
<br>
</p>
<p>schema不分大小写。schema的第一个字符必须是[a-zA-Z]，后面的字符必须是[a-zA-Z0-9]和+ - . 这三个字符。拿到input之后首先找”:/?#”这4个字符，如果首先找到的是冒号，那么就说明input是含有scheme的。java通过这样把absoluteURI和abs_path分开。通常来说，absoluteURI用于发给http proxy，而abs_path发给普通的http server。http标准要求所有支持http/1.1的server都得支持absoluteURI形式的请求。也就是说，下面这样发给www.baidu.com是合法的。</p>
<p>GET<a href="http://www.baidu.com/index.html">http://www.baidu.com/index.html</a>HTTP/1.1</p>
<p>此时要求从absoluteURI中提取Hostname，它优先于headers中的Host。解析完schema之后，如果冒号后面不是“/”，那么它就是第三种模式，opaque模式。</p>
<p>如果是net_path，在跳过了”://”之后，搜索”/?#”这3个字符中的一个，这时的位置就是authority的past-the-end。所以此时有三种情况：
<br>
</p>
<p>1.找到了，且authority不为空。</p>
<p>2.找到了，但是authority为空。例如”file:///test.txt”。rfc认为这是非法的，但是java出于自己的考虑要支持这种格式的URI。</p>
<p>3.没有找到。格式非法。</p>
<p>然后继续往后解析path即可。</p>
<p>如果是abs_path，就不用关心authority，跳过“:/”往后解析path即可。</p>
<p>解析path时，首先查找”?#”这两个字符。它们之后的是query和fragment。这中间的部分就是path。</p>
<p>至此，URI已经被拆成scheme、authority、path、query、fragment这么几部分。拆的过程中用到的分割符只是”:/?#”这4种字符，属于必须被转义的字符，所以对于如ascii兼容的编码（如utf-8），URI中有中文字符一样可被正确解析。如果只是想提取出path（这是http server最关心的），那么不必做decode，直接parse即可。</p>
<p>在encoding URI的时候要注意，</p>
<p>保留字符： ”;” | “/” | “?” | “:” | “@” | “&amp;” | “=” | “+” | “$” | “,”</p>
<p>这些只能用来做分割符，不能用在其它地方。</p>
<p>Unreserved Characters：</p>
<p>大写字母|小写字母 | 数字 | “-” | “_” | “.” | “!” | “~” | “*” | “’” | “(” | “)”</p>
<p>这些属于可转义也可不转义。把这样的字符替换成转义序列后，含义不变，与原来的等价。Unreserved Characters之外的都该转义。</p>
<p>转义的规则是先把character按照某种编码(如utf-8、gbk）进行编码。然后把编码后的每个8位的字节，替换成% hex hex这样的。比如”@”对应%40，等号”=“ 对应%3d。如果百分号之后只有一个数字，如%3，应当写成%03，如果写成%3就是错的。这时候java就会抛异常，拒绝继续解析下去，但是实际上我们的parser在有时候是应该越宽容越好，比如日志分析程序中的url parser。</p>
<p>#后面的是fragment，fragment不属于URI，不应该被发给服务器。但是服务器给客户端的答复中，如Headers中的Location的值，其实应允许包含#的。我也不清楚浏览器现在如何实现的，待测试。</p>
<p> </p>
<p>（待续）</p>
<p> </p>
<p></p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/9718.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/9718.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">udpwork.com</a> 聚合
					|
					<a href="http://www.udpwork.com/item/9718.html#reviews">评论: 0</a>
					|
					<a href="http://www.jikenow.com/">要! 要! 即刻! Now!</a>
				</p>
			</div>