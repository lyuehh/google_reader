---
layout: post
title:  "函数加里化(Currying)和偏函数应用(Partial Application)的比较"
date:   2013-04-24 18:16:03
author: 
categories: program
---

## 函数加里化(Currying)和偏函数应用(Partial Application)的比较
### by 
### at 2013-04-24 18:16:03
### original <http://www.aqee.net/currying-partial-application/>

<img src="http://ittopic.gotoip1.com/qee/wordpress/wp-content/uploads/2013/04/f001-620x350.jpg" alt="函数式编程"><br><p><em>【名词解释】Currying：因为是美国数理逻辑学家哈斯凯尔·加里(Haskell Curry)发明了这种函数使用技巧，所以这样用法就以他的名字命名为Currying，中文翻译为“加里化”。</em></p><p>我感觉很多人都对函数加里化(Currying)和偏函数应用(Partial Application)之间的区别搞不清楚，尤其是在相似的上下文环境中它们同时出现的时候。</p><p>偏函数解决这样的问题：如果我们有函数是多个参数的，我们希望能固定其中某几个参数的值。</p><p>几乎所有编程语言中都有非常明显的偏函数应用。在C语言中：</p><div><pre><code><span>int</span> <span>foo</span><span>(</span><span>int</span> <span>a</span><span>,</span> <span>int</span> <span>b</span><span>,</span> <span>int</span> <span>c</span><span>)</span> <span>{</span>

  <span>return</span> <span>a</span> <span>+</span> <span>b</span> <span>+</span> <span>c</span><span>;</span>
<span>}</span>

<span>int</span> <span>foo23</span><span>(</span><span>int</span> <span>a</span><span>,</span> <span>int</span> <span>c</span><span>)</span> <span>{</span>
  <span>return</span> <span>foo</span><span>(</span><span>a</span><span>,</span> <span>23</span><span>,</span> <span>c</span><span>);</span>

<span>}</span>
</code></pre></div><p><code>foo23</code>函数实际上就是一个<code>foo</code>函数的偏函数应用，参数<code>b</code>的值被固定为23。</p><p>当然，像这样明显的偏函数并没有太大的用处；我们通常会希望编程语言能提供我们某些偏函数特征。</p><p>例如，在Python语言中，我们可以这样做：</p><div><pre><code><span>from</span> <span>functools</span> <span>import</span> <span>partial</span>

<span>def</span> <span>foo</span><span>(</span><span>a</span><span>,</span><span>b</span><span>,</span><span>c</span><span>):</span>

  <span>return</span> <span>a</span> <span>+</span> <span>b</span> <span>+</span> <span>c</span>

<span>foo23</span> <span>=</span> <span>partial</span><span>(</span><span>foo</span><span>,</span> <span>b</span><span>=</span><span>23</span><span>)</span>

<span>foo23</span><span>(</span><span>a</span> <span>=</span> <span>1</span><span>,</span> <span>c</span> <span>=</span> <span>3</span><span>)</span>  <span># =&gt; 27</span>

</code></pre>&lt;</div>