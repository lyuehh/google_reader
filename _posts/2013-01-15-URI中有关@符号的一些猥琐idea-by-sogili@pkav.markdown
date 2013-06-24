---
layout: post
title:  "URI中有关@符号的一些猥琐idea by sogili@pkav"
date:   2013-01-15 14:01:02
author: only_guest
categories: program
---

## URI中有关@符号的一些猥琐idea by sogili@pkav
### by only_guest
### at 2013-01-15 14:01:02
### original <http://pkav.net/2013/01/332.html>

<p>“@”在URI中是个奇特的字符,在URI中用于分隔userinfo与host.<br>
使用”@”符hack的优点:<br>
1.在host前面.<br>
2.大多数情况不影响访问逻辑.<br>
3.有两个可控部分:username,password. </p>
<p>1.正常含有userinfo的URI:<br>
<code>http://username@mmme.me<br>
ftp://username:pwd@mmme.me</code><br>
ftp,http,https都是URI,只要是URI就支持userinfo(即username:password).<br>
userinfo在URI规范中的定义 </p>
<p>2.十进制hack<br>
<code>http://mmme.me@1945096730</code><br>
1945096730是十进制IP,实际上是访问到baidu,这个trick只具有视觉欺骗效果,但实际攻击效果往往还不错,因为小白不懂什么@符,看上去像就OK.<br>
case:<br>
@only_guest WooYun: 猎豹浏览器存在URL欺骗问题 </p>
<p>3.空password<br>
<code>http://qq.com:@mmme.me </code></p>
<p>http://:@qq.com:@mmme.me</p>
<p>当userinfo的password部分为空的时候,一些应用的匹配userinfo正则可能匹配不到,而采用匹配port的规则,这时候我们便达到了绕过的目的.<br>
case:<br>
@0x0F 我是如何bypass腾讯的消息链接安全检测的<br>
@p.z 一次SWF XSS挖掘和利用 </p>
<p>4.尴尬的反斜杠<br>
<code>http://qq.com\mmme.me</code><br>
反斜杠”\”在浏览器URL中一直是个奇葩,因为win主机解析”\”,所以一般浏览器在处理资源调用URL时会将”\”将换会”/”,但firefox并不买单,这种差异也导致了开发人员在检测URL域时的尴尬.<br>
case:<br>
一个google图片调用demo </p>
<p>5.username编码绕过<br>
<code>http://qq.com%2f@mmme.me </code></p>
<p>http://qq.com%5c@mmme.me</p>
<p>这里开发人员错误的将userinfo中的信息进行解码导致绕过,这个问题普遍存在于腾讯的业务线中.另外safari的location污染也是这个原理.<br>
WooYun: Safari location污染漏洞 </p>
<p>http://www.wooyun.org/bugs/wooyun-2013-017325</p>
<p>6.规则解析顺序绕过<br>
<code>http://mmme.me#@qq.com </code></p>
<p>http://mmme.me:#@qq.com</p>
<p>开发人员在这里错误的将这个url中的冒号解析为userinfo规则的冒号将会导致bypass,本来是个腾讯信任域绕过漏洞的,写完文章后发现正好补了. </p>