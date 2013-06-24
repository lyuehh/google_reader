---
layout: post
title:  "Object-Oriented JavaScript笔记(一)"
date:   2012-02-09 13:18:17
author: 枯藤昏鸦
categories: program
---

## Object-Oriented JavaScript笔记(一)
### by 枯藤昏鸦
### at 2012-02-09 13:18:17
### original <http://ioio.name/object-oriented-javascript-note.html>

<p><strong>第二章 基本数据类型、数组、循环和条件语句</strong></p>
<p>变量是大小写敏感的</p>
<p><strong>基本类型</strong></p>
<p>基本类型（5类） Number、String、Boolean、Undefined、Null<br>
除了基本类型外的，都是object，特例<br>
&gt;&gt;&gt; typeof null<br>
“object”</p>
<p>Infinity是number类型的<br>
&gt;&gt;&gt; typeof Infinity<br>
“number”</p>
<p>JS中最大的数是1.7976931348623157e+308 最小的分数是5e-324</p>
<p>非0数除以0得到Infinity，0除以0得到NaN<br>
1/0 = Infinity<br>
0/0 = NaN</p>
<p>Infinity 与非NaN运算值为Infinity<br>
Infinity – 1 = Infinity</p>
<p>NaN 与任何number运算值为NaN</p>
<p>一个String与一个变量做除’+&#39;(加)以外的运算，都会转换成做数学运算<br>
&gt;&gt;&gt; var s = ’3′; s = 3 * s; typeof s;<br>
“number”</p>
<p>&gt;&gt;&gt; var s = ’3′; s = 3 + s; typeof s;<br>
“string”</p>
<p>两次逻辑非操作会做强制布尔转换。大部分情况都是为真，除非：<br>
1.空字符串 ”<br>
2.null<br>
3.undefined<br>
4.数 0<br>
5.数 NaN<br>
6.布尔值 false</p>
<p>利用惰性求值(Lazy Evaluation)做保护伞并不是处处受用<br>
var mynumber = mynumber || 10;</p>
<p>如果预先定义的  mynumber =0 ，这里就不是我们所预料的了。</p>
<p>=== 值与类型比较<br>
!== 不含类型转换的不等于比较</p>
<p>NaN不与任何值相等，包含其自身<br>
&gt;&gt;&gt; NaN == NaN<br>
false</p>
<p>null值并不是由js环境给分配的，而是由代码给分配的<br>
&gt;&gt;&gt; var value = null;<br>
&gt;&gt;&gt; typeof value<br>
“object”</p>
<p>undefined与null还是大差别虽然微小，但是不容忽视<br>
&gt;&gt;&gt; var i = 1 + undefined;<br>
&gt;&gt;&gt; i<br>
NaN</p>
<p>&gt;&gt;&gt; var i = 1 + null;<br>
&gt;&gt;&gt; i<br>
1</p>
<p>这种差异发生在类型转换上<br>
&gt;&gt;&gt; var i = 1*undefined;<br>
&gt;&gt;&gt; i<br>
NaN</p>
<p>&gt;&gt;&gt; var i = 1*null;<br>
&gt;&gt;&gt; i<br>
0</p>
<p><strong>数组</strong></p>
<p>数组新添加元素索引跟数组最大索引之间有空隙的话，会被undefined值填充<br>
&gt;&gt;&gt; var abc = [1];<br>
&gt;&gt;&gt; abc[3] = 5;<br>
&gt;&gt;&gt; abc<br>
[1, undefined, undefined, 5]</p>
<p>删除数组中的元素并不会改变数组大小，仅将删除位置为undefined<br>
&gt;&gt;&gt; var a = [1, 2, 3];<br>
&gt;&gt;&gt; delete a[2];<br>
&gt;&gt;&gt; a<br>
[1, 2, undefined]</p>
<p>可以使用数组下标的形式访问字符串<br>
&gt;&gt;&gt; var s = ‘one’;<br>
&gt;&gt;&gt; s[0]<br>
“o”</p>
<p><strong>循环和条件语句</strong></p>
<p>通常情况下我们会把变量放if条件中做检查，但这样做并不一定妥当，变量可能存在但值为false<br>
&gt;&gt;&gt; var result = ”;&gt;&gt;&gt; if (somevar){result = ‘yes’;}<br>
￼somevar is not defined<br>
&gt;&gt;&gt; result;<br>
“”<br>
 一种更好的方式<br>
&gt;&gt;&gt; if (typeof somevar !== “undefined”){result = ‘yes’;}<br>
&gt;&gt;&gt; result;<br>
“”</p>
<p>default是可以放在switch里任意位置的，并不一定是最后</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">var</span> a <span style="color:#339933">=</span> <span style="color:#cc0000">4</span><span style="color:#339933">;</span>switch<span style="color:#009900">(</span>a<span style="color:#009900">)</span><span style="color:#009900">{</span>
     <span style="color:#000066;font-weight:bold">case</span> <span style="color:#cc0000">1</span><span style="color:#339933">:</span>
          console.<span style="color:#660066">log</span><span style="color:#009900">(</span><span style="color:#cc0000">1</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
          <span style="color:#000066;font-weight:bold">break</span><span style="color:#339933">;</span>
     <span style="color:#003366;font-weight:bold">default</span><span style="color:#339933">:</span>
          console.<span style="color:#660066">log</span><span style="color:#009900">(</span><span style="color:#3366cc">'d'</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
          <span style="color:#000066;font-weight:bold">break</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">case</span> <span style="color:#cc0000">6</span><span style="color:#339933">:</span>
          console.<span style="color:#660066">log</span><span style="color:#009900">(</span><span style="color:#cc0000">5</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
          <span style="color:#000066;font-weight:bold">break</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span></pre></div></div>

<p>-EOF-</p>
<hr><h2>Related posts:</h2><ul><li><a href="http://ioio.name/object-oriented-javascript-note-4.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(四)">Object-Oriented JavaScript笔记(四)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-2.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(二)">Object-Oriented JavaScript笔记(二)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-3.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(三)">Object-Oriented JavaScript笔记(三)</a></li><li><a href="http://ioio.name/core-javascript-pitfalls.html" rel="bookmark" title="Permanent Link: JavaScript性能陷阱">JavaScript性能陷阱</a></li><li><a href="http://ioio.name/javascript-get-up-guide.html" rel="bookmark" title="Permanent Link: JavaScript 入门指南">JavaScript 入门指南</a></li></ul><hr><small>Copyright © 2005~2011 | <a href="http://ioio.name/object-oriented-javascript-note.html" title="Permalink">Permalink</a> | <a href="http://ioio.name/object-oriented-javascript-note.html#comments">3 Comments</a> | <a href="http://closetou.com" title="Close To U">Close To U</a> <br>
<a href="http://feeds.feedburner.com/miss">订阅</a> <a href="https://twitter.com/tearnon">Twitter</a> <a href="http://ioio.name/godaddy">域名优惠码</a> <a href="http://ioio.name/mt">Media Temple空间</a>
<a href="http://mdiatemple.com/" title="Domain Sale! $6.89 .com at GoDaddy">主机/域名优惠码</a>
</small> )