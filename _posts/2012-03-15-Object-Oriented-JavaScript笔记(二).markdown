---
layout: post
title:  "Object-Oriented JavaScript笔记(二)"
date:   2012-03-15 23:48:47
author: 枯藤昏鸦
categories: program
---

## Object-Oriented JavaScript笔记(二)
### by 枯藤昏鸦
### at 2012-03-15 23:48:47
### original <http://ioio.name/object-oriented-javascript-note-2.html>

<p>Object-Oriented JavaScript 笔记<br>
<a href="http://ioio.name/object-oriented-javascript-note.html">Object-Oriented JavaScript笔记(一)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-2.html">Object-Oriented JavaScript笔记(二)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-3.html">Object-Oriented JavaScript笔记(三)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-4.html">Object-Oriented JavaScript笔记(四)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-5.html">Object-Oriented JavaScript笔记(五)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-6.html">Object-Oriented JavaScript笔记(六)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-7.html">Object-Oriented JavaScript笔记(七)</a></p>
<p><strong>第三章 函数</strong></p>
<p>JavaScript会给定义了但是没有传值的参数赋值为undefined，若是参数多于定义的，会被忽略。</p>
<p>没有函数都会带有一个名为arguments的类数组变量，包含所有传入的参数信息。</p>
<p><strong>JavaScript内置函数（Pre-defined Functions）：</strong><br>
parseInt()<br>
parseFloat()<br>
isNaN()<br>
isFinite()<br>
encodeURI()<br>
decodeURI()<br>
encodeURIComponent()<br>
decodeURIComponent()<br>
eval()</p>
<p>使用parseInt时最好每次都明确指定第二个（基数）参数，如 parseInt(‘FF’, 16)</p>
<p>parseFloat只接受一个参数，并能够识别指数表示法，如 parseFloat(’123e2′)</p>
<p>escape() 与 unescape() 已经被弃用，不应该再被使用。</p>
<p>alert并不是JavaScript的一部分，并且它会阻塞浏览器线程，在富ajax应用中不应该使用它。</p>
<p>未使用var 声明的变量会被分配到全局作用域<br>
&gt;&gt;&gt; var f = function(){test = 2;};<br>
&gt;&gt;&gt; test<br>
ReferenceError: test is not defined<br>
&gt;&gt;&gt; f();<br>
&gt;&gt;&gt; test<br>
2</p>
<p>函数中声明的局部变量与全局变量同名会覆盖同名全局变量，即使是先使用后声明。</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">var</span> a <span style="color:#339933">=</span> <span style="color:#cc0000">123</span><span style="color:#339933">;</span>
<span style="color:#003366;font-weight:bold">function</span> f<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
     console.<span style="color:#660066">log</span><span style="color:#009900">(</span>a<span style="color:#009900">)</span><span style="color:#339933">;</span>
     <span style="color:#003366;font-weight:bold">var</span> a <span style="color:#339933">=</span> <span style="color:#cc0000">1</span><span style="color:#339933">;</span>
     console.<span style="color:#660066">log</span><span style="color:#009900">(</span>a<span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span>
f<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span></pre></div></div>

<p><strong>匿名函数</strong></p>
<p>不仅是字符串，一些常量写在代码中也是可以的</p>

<div><div><pre style="font-family:monospace"><span style="color:#3366cc">&quot;test&quot;</span><span style="color:#339933">;</span> <span style="color:#009900">[</span><span style="color:#cc0000">1</span><span style="color:#339933">,</span><span style="color:#cc0000">2</span><span style="color:#339933">,</span><span style="color:#cc0000">3</span><span style="color:#009900">]</span><span style="color:#339933">;</span> undefined<span style="color:#339933">;</span> <span style="color:#003366;font-weight:bold">null</span><span style="color:#339933">;</span> <span style="color:#cc0000">1</span><span style="color:#339933">;</span></pre></div></div>

<p>ECMAScript 5严格模式（Strict Mode）便是利用该特性</p>

<div><div><pre style="font-family:monospace"><span style="color:#3366cc">'use strict'</span><span style="color:#339933">;</span></pre></div></div>

<p>Lexical Scope 函数工作在定义时的作用域，而非执行作用域。它仅能访问自己的作用域和全局作用域。</p>
<p>&gt;&gt;&gt; function f1(){var a = 1; f2();}<br>
&gt;&gt;&gt; function f2(){return a;}<br>
&gt;&gt;&gt; f1();<br>
a is not defined</p>
<p><strong>闭包Closure的常用场景</strong><br>
如：循环、get/set访问器、迭代器<br>
-EOF-</p>
<hr><h2>Related posts:</h2><ul><li><a href="http://ioio.name/object-oriented-javascript-note-4.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(四)">Object-Oriented JavaScript笔记(四)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-7.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(七)">Object-Oriented JavaScript笔记(七)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(一)">Object-Oriented JavaScript笔记(一)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-5.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(五)">Object-Oriented JavaScript笔记(五)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-3.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(三)">Object-Oriented JavaScript笔记(三)</a></li></ul><hr><small>Copyright © 2005~2011 | <a href="http://ioio.name/object-oriented-javascript-note-2.html" title="Permalink">Permalink</a> | <a href="http://ioio.name/object-oriented-javascript-note-2.html#comments">1 Comments</a> | <a href="http://closetou.com" title="Close To U">Close To U</a> <br>
<a href="http://feeds.feedburner.com/miss">订阅</a> <a href="https://twitter.com/tearnon">Twitter</a> <a href="http://ioio.name/godaddy">域名优惠码</a> <a href="http://ioio.name/mt">Media Temple空间</a>
<a href="http://mdiatemple.com/" title="Domain Sale! $6.89 .com at GoDaddy">主机/域名优惠码</a>
</small> )