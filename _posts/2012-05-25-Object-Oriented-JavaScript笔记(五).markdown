---
layout: post
title:  "Object-Oriented JavaScript笔记(五)"
date:   2012-05-25 13:05:49
author: 枯藤昏鸦
categories: program
---

## Object-Oriented JavaScript笔记(五)
### by 枯藤昏鸦
### at 2012-05-25 13:05:49
### original <http://ioio.name/object-oriented-javascript-note-5.html>

<p>Object-Oriented JavaScript 笔记<br>
<a href="http://ioio.name/object-oriented-javascript-note.html">Object-Oriented JavaScript笔记(一)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-2.html">Object-Oriented JavaScript笔记(二)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-3.html">Object-Oriented JavaScript笔记(三)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-4.html">Object-Oriented JavaScript笔记(四)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-5.html">Object-Oriented JavaScript笔记(五)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-6.html">Object-Oriented JavaScript笔记(六)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-7.html">Object-Oriented JavaScript笔记(七)</a></p>
<p><strong>第六章 继承</strong></p>
<p>空的构造函数—new F()<br>
用于解决原型链上的属性被覆盖的问题。</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">var</span> extend <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>origin<span style="color:#339933">,</span> target<span style="color:#009900">)</span><span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> F <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
     F.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> origin.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
     target.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">new</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
     target.<span style="color:#660066">prototype</span>.<span style="color:#660066">constructor</span> <span style="color:#339933">=</span> target<span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span>
 
<span style="color:#003366;font-weight:bold">var</span> A <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
A.<span style="color:#660066">prototype</span>.<span style="color:#000066">name</span> <span style="color:#339933">=</span> <span style="color:#3366cc">'A'</span><span style="color:#339933">;</span>
A.<span style="color:#660066">prototype</span>.<span style="color:#660066">toString</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span> <span style="color:#000066;font-weight:bold">return</span> <span style="color:#000066;font-weight:bold">this</span>.<span style="color:#000066">name</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
 
<span style="color:#003366;font-weight:bold">var</span> B <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
extend<span style="color:#009900">(</span>A<span style="color:#339933">,</span>B<span style="color:#009900">)</span><span style="color:#339933">;</span>
B.<span style="color:#660066">prototype</span>.<span style="color:#000066">name</span> <span style="color:#339933">=</span> <span style="color:#3366cc">'B'</span><span style="color:#339933">;</span>
 
<span style="color:#006600;font-style:italic">/*
var a = new A();
var b = new B();
b instanceof B
true
b instanceof A
true
a instanceof A
true
a instanceof B
false
*/</span></pre></div></div>

<p>Uber 访问父方法。</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">var</span> extend <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>origin<span style="color:#339933">,</span> target<span style="color:#009900">)</span><span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> F <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
     F.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> origin.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
     target.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">new</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
     target.<span style="color:#660066">prototype</span>.<span style="color:#660066">constructor</span> <span style="color:#339933">=</span> target<span style="color:#339933">;</span>
     target.<span style="color:#660066">uber</span> <span style="color:#339933">=</span> origin.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span>
 
<span style="color:#003366;font-weight:bold">var</span> A <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
A.<span style="color:#660066">prototype</span>.<span style="color:#000066">name</span> <span style="color:#339933">=</span> <span style="color:#3366cc">'A'</span><span style="color:#339933">;</span>
A.<span style="color:#660066">prototype</span>.<span style="color:#660066">toString</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> result <span style="color:#339933">=</span> <span style="color:#009900">[</span><span style="color:#009900">]</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">this</span>.<span style="color:#660066">constructor</span>.<span style="color:#660066">uber</span><span style="color:#009900">)</span><span style="color:#009900">{</span>
          result<span style="color:#009900">[</span>result.<span style="color:#660066">length</span><span style="color:#009900">]</span> <span style="color:#339933">=</span> <span style="color:#000066;font-weight:bold">this</span>.<span style="color:#660066">constructor</span>.<span style="color:#660066">uber</span>.<span style="color:#660066">toString</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
     <span style="color:#009900">}</span>
     result<span style="color:#009900">[</span>result.<span style="color:#660066">length</span><span style="color:#009900">]</span> <span style="color:#339933">=</span> <span style="color:#000066;font-weight:bold">this</span>.<span style="color:#000066">name</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">return</span> result.<span style="color:#660066">join</span><span style="color:#009900">(</span><span style="color:#3366cc">', '</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span>
A.<span style="color:#660066">prototype</span>.<span style="color:#660066">show</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span>
     console.<span style="color:#660066">log</span><span style="color:#009900">(</span><span style="color:#3366cc">'A show'</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span>
<span style="color:#003366;font-weight:bold">var</span> B <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
extend<span style="color:#009900">(</span>A<span style="color:#339933">,</span>B<span style="color:#009900">)</span><span style="color:#339933">;</span>
B.<span style="color:#660066">prototype</span>.<span style="color:#000066">name</span> <span style="color:#339933">=</span> <span style="color:#3366cc">'B'</span><span style="color:#339933">;</span>
B.<span style="color:#660066">prototype</span>.<span style="color:#660066">show</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span>
     <span style="color:#000066;font-weight:bold">this</span>.<span style="color:#660066">constructor</span>.<span style="color:#660066">uber</span>.<span style="color:#660066">show</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
     console.<span style="color:#660066">log</span><span style="color:#009900">(</span><span style="color:#3366cc">'B show'</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span>
<span style="color:#006600;font-style:italic">/*
var a = new A();
var b = new B();
b instanceof B
true
b instanceof A
true
a instanceof A
true
a instanceof B
false
*/</span></pre></div></div>

<p>用函数来实现继承 （注：实际上上面的两个示例的extend已经是被我封装过的了）</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">var</span> exntend <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>Child<span style="color:#339933">,</span> Parent<span style="color:#009900">)</span><span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> F <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
     F.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> Parent.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
     Child.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">new</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
     Child.<span style="color:#660066">prototype</span>.<span style="color:#660066">constructor</span> <span style="color:#339933">=</span> Child<span style="color:#339933">;</span>
     Child.<span style="color:#660066">uber</span> <span style="color:#339933">=</span> Parent.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span><span style="color:#339933">;</span></pre></div></div>

<p>复制属性</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> extend2<span style="color:#009900">(</span>Child<span style="color:#339933">,</span> Parent<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> p <span style="color:#339933">=</span> Parent.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
     <span style="color:#003366;font-weight:bold">var</span> c <span style="color:#339933">=</span> Child.<span style="color:#660066">prototype</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#003366;font-weight:bold">var</span> i <span style="color:#000066;font-weight:bold">in</span> p<span style="color:#009900">)</span> <span style="color:#009900">{</span>
          c<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">=</span> p<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
     <span style="color:#009900">}</span>
     c.<span style="color:#660066">uber</span> <span style="color:#339933">=</span> p<span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p>当复制引用带来的问题</p>
<p>修改引用对象会导致原型的也被修改。</p>
<p>从对象中继承：</p>
<p>浅拷贝</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> extendCopy<span style="color:#009900">(</span>p<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> c <span style="color:#339933">=</span> <span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#003366;font-weight:bold">var</span> i <span style="color:#000066;font-weight:bold">in</span> p<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     c<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">=</span> p<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
     <span style="color:#009900">}</span>
     c.<span style="color:#660066">uber</span> <span style="color:#339933">=</span> p<span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">return</span> c<span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p>深拷贝：</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> deepCopy<span style="color:#009900">(</span>p<span style="color:#339933">,</span> c<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> c <span style="color:#339933">=</span> c <span style="color:#339933">||</span> <span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#003366;font-weight:bold">var</span> i <span style="color:#000066;font-weight:bold">in</span> p<span style="color:#009900">)</span> <span style="color:#009900">{</span>
          <span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">typeof</span> p<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">===</span> <span style="color:#3366cc">'object'</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
               c<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">=</span> <span style="color:#009900">(</span>p<span style="color:#009900">[</span>i<span style="color:#009900">]</span>.<span style="color:#660066">constructor</span> <span style="color:#339933">===</span> Array<span style="color:#009900">)</span> <span style="color:#339933">?</span> <span style="color:#009900">[</span><span style="color:#009900">]</span> <span style="color:#339933">:</span> <span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">;</span>
               deepCopy<span style="color:#009900">(</span>p<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">,</span> c<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
          <span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#009900">{</span>
               c<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">=</span> p<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
          <span style="color:#009900">}</span>
     <span style="color:#009900">}</span>
     <span style="color:#000066;font-weight:bold">return</span> c<span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p>object()构造起，老道提议的形式。</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> object<span style="color:#009900">(</span>o<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">function</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#009900">}</span>
     F.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> o<span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">return</span> <span style="color:#003366;font-weight:bold">new</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p>混合原型拷贝和属性继承（很多时候需要继承一个对象，并给它添加一些属性用）</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> objectPlus<span style="color:#009900">(</span>o<span style="color:#339933">,</span> stuff<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> n<span style="color:#339933">;</span>
     <span style="color:#003366;font-weight:bold">function</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#009900">}</span>
     F.<span style="color:#660066">prototype</span> <span style="color:#339933">=</span> o<span style="color:#339933">;</span>
     n <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">new</span> F<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
     n.<span style="color:#660066">uber</span> <span style="color:#339933">=</span> o<span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#003366;font-weight:bold">var</span> i <span style="color:#000066;font-weight:bold">in</span> stuff<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     n<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">=</span> stuff<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
     <span style="color:#009900">}</span>
     <span style="color:#000066;font-weight:bold">return</span> n<span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p>多重继承，说白了就是按顺序进行属性复制，后出现的覆盖已经存在的。</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> multi<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#003366;font-weight:bold">var</span> n <span style="color:#339933">=</span> <span style="color:#009900">{</span><span style="color:#009900">}</span><span style="color:#339933">,</span> stuff<span style="color:#339933">,</span> j <span style="color:#339933">=</span> <span style="color:#cc0000">0</span><span style="color:#339933">,</span> len <span style="color:#339933">=</span> arguments.<span style="color:#660066">length</span><span style="color:#339933">;</span>
     <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span>j <span style="color:#339933">=</span> <span style="color:#cc0000">0</span><span style="color:#339933">;</span> j <span style="color:#339933">&lt;</span> len<span style="color:#339933">;</span> j<span style="color:#339933">++</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
          stuff <span style="color:#339933">=</span> arguments<span style="color:#009900">[</span>j<span style="color:#009900">]</span><span style="color:#339933">;</span>
          <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#003366;font-weight:bold">var</span> i <span style="color:#000066;font-weight:bold">in</span> stuff<span style="color:#009900">)</span> <span style="color:#009900">{</span>
               n<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">=</span> stuff<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
          <span style="color:#009900">}</span>
     <span style="color:#009900">}</span>
     <span style="color:#000066;font-weight:bold">return</span> n<span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p>-EOF-</p>
<hr><h2>Related posts:</h2><ul><li><a href="http://ioio.name/object-oriented-javascript-note-4.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(四)">Object-Oriented JavaScript笔记(四)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-2.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(二)">Object-Oriented JavaScript笔记(二)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-7.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(七)">Object-Oriented JavaScript笔记(七)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(一)">Object-Oriented JavaScript笔记(一)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-3.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(三)">Object-Oriented JavaScript笔记(三)</a></li></ul><hr><small>Copyright © 2005~2011 | <a href="http://ioio.name/object-oriented-javascript-note-5.html" title="Permalink">Permalink</a> | <a href="http://ioio.name/object-oriented-javascript-note-5.html#comments">1 Comments</a> | <a href="http://closetou.com" title="Close To U">Close To U</a> <br>
<a href="http://feeds.feedburner.com/miss">订阅</a> <a href="https://twitter.com/tearnon">Twitter</a> <a href="http://ioio.name/godaddy">域名优惠码</a> <a href="http://ioio.name/mt">Media Temple空间</a>
<a href="http://mdiatemple.com/" title="Domain Sale! $6.89 .com at GoDaddy">主机/域名优惠码</a>
</small> )