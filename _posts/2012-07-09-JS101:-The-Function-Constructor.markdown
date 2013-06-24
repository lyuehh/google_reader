---
layout: post
title:  "JS101: The Function Constructor"
date:   2012-07-09 07:00:00
author: 
categories: program
---

## JS101: The Function Constructor
### by 
### at 2012-07-09 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/AQ_3atrfnhQ/function-2>

<p><a href="http://dailyjs.com/2012/07/02/function">Last week</a> we took a cursory glance at functions, and this week we’ll look at the <code>Function</code> object itself.</p>

<h3>Creating Functions with the <code>Function</code> Constructor</h3>

<p>Functions can be instantiated with zero or more arguments. The last argument is always the function body:</p>
<div><pre><code><span>var</span> <span>sum</span> <span>=</span> <span>new</span> <span>Function</span><span>(</span><span>&#39;a&#39;</span><span>,</span> <span>&#39;b&#39;</span><span>,</span> <span>&#39;return a + b&#39;</span><span>);</span>
<span>sum</span><span>(</span><span>1</span><span>,</span> <span>1</span><span>);</span>
<span>// 2</span>
</code></pre>
</div>
<p>The <code>length</code> property will return the number of arguments:</p>
<div><pre><code><span>sum</span><span>.</span><span>length</span>
<span>// 2</span>
</code></pre>
</div>
<p>This property is not writeable. The function body will have access to <code>arguments</code>, so supporting <a href="http://es5.github.com/#x15.3.5.1">variable arguments is still possible</a>:</p>

<blockquote>
<p>The behaviour of a function when invoked on a number of arguments other than the number specified by its length property depends on the function.</p>
</blockquote>

<p>Other properties include <a href="http://dailyjs.com/2012/06/25/this-binding/">call, apply, and bind</a>. There’s also a <code>toString</code> method, which will return a string containing the function’s body. It won’t be exactly the same as the source supplied to the <code>Function</code> constructor.</p>

<h3>Scope</h3>

<p>To all intents and purposes, functions created this way are indistinguishable from any other function. There is a difference, however, and that lies in the scope binding. The <a href="http://es5.github.com/#x10.2.3">Global Environment</a> is passed as the scope for the new function, which isn’t necessarily intuitive. This isn’t a bug and is covered by the specification, but it’s worth being aware of the behaviour.</p>

<p>For example, this will fail because <code>a</code> isn’t in scope for the <code>Function</code> instances:</p>
<div><pre><code><span>function</span> <span>container</span><span>()</span> <span>{</span>
  <span>var</span> <span>a</span> <span>=</span> <span>1</span>
    <span>,</span> <span>b</span> <span>=</span> <span>1</span>
    <span>,</span> <span>sum</span> <span>=</span> <span>new</span> <span>Function</span><span>(</span><span>&#39;return a + b&#39;</span><span>)</span>
    <span>,</span> <span>sum2</span> <span>=</span> <span>new</span> <span>Function</span><span>(</span><span>&#39;sum()&#39;</span><span>);</span>
  <span>sum</span><span>();</span>
  <span>sum2</span><span>();</span>
<span>}</span>

<span>container</span><span>();</span>
</code></pre>
</div>
<p>The amazing thing is, an <code>eval</code> would have access to <code>a</code> and <code>b</code> because <a href="http://es5.github.com/#x10.4.2">entering eval code</a> sets up the lexical and variable environments the usual way.</p>

<p>This difference is useful, because <code>new Function</code> offers a way to execute arbitrary code without providing access to local (perhaps considered “internal”) variables. jQuery’s JSON parser uses this distinction to <a href="https://github.com/jquery/jquery/blob/f5fd41252e3ae48a655c5da4a0b2910bb897b6ed/src/core.js#L501">parse JSON</a>:</p>
<div><pre><code><span>if</span> <span>(</span><span>rvalidchars</span><span>.</span><span>test</span><span>(</span> <span>data</span><span>.</span><span>replace</span><span>(</span> <span>rvalidescape</span><span>,</span> <span>&quot;&quot;</span> <span>)</span>
    <span>.</span><span>replace</span><span>(</span> <span>rvalidtokens</span><span>,</span> <span>&quot;]&quot;</span> <span>)</span>
    <span>.</span><span>replace</span><span>(</span> <span>rvalidbraces</span><span>,</span> <span>&quot;&quot;</span><span>))</span> <span>)</span> <span>{</span>

  <span>return</span> <span>(</span> <span>new</span> <span>Function</span><span>(</span> <span>&quot;return &quot;</span> <span>+</span> <span>data</span> <span>)</span> <span>)();</span>
<span>}</span>
</code></pre>
</div>
<p>In this way we can see <code>eval</code> and <code>new Function</code> are related, but not entirely the same. However, some well-known JavaScript professionals <a href="http://javascript.crockford.com/code.html">strongly warn against using both</a>:</p>

<blockquote>
<p><code>eval</code> has aliases. Do not use the <code>Function</code> constructor. Do not pass strings to <code>setTimeout</code> or <code>setInterval</code>.</p>
</blockquote>

<h3>Metaprogramming</h3>

<p>The <code>Function</code> constructor is occasionally used for <a href="http://en.wikipedia.org/wiki/Metaprogramming">metaprogramming</a>. It’s used to generate new methods in the <a href="https://github.com/LearnBoost/mongoose/blob/35d8eea943ef8f3ca8706ad39ab6ea2e74a166d0/lib/types/buffer.js#L147-151">Mongoose MongoDB module for Node</a>:</p>
<div><pre><code><span>MongooseBuffer</span><span>.</span><span>prototype</span><span>[</span><span>method</span><span>]</span> <span>=</span> <span>new</span> <span>Function</span><span>(</span>
  <span>&#39;var ret = Buffer.prototype.&#39;</span><span>+</span><span>method</span><span>+</span><span>&#39;.apply(this, arguments);&#39;</span> <span>+</span>
  <span>&#39;this._markModified();&#39;</span> <span>+</span>
  <span>&#39;return ret;&#39;</span>
<span>)</span>
</code></pre>
</div>
<p>In this case, it would be trivial to refactor out the <code>Function</code> constructor.</p>

<p>There are more specific cases where the <code>Function</code> constructor is used for more devious metaprogramming. The Jade template language <a href="https://github.com/visionmedia/jade/blob/e805f6a2d5eb80c680e7bbddd3ea4390b2808c2e/lib/jade.js#L143-160">compiles templates into functions</a>. Dojo also has a few places relating to templates where it’s used as well.</p>

<h3>Conclusion</h3>

<p>The <code>Function</code> constructor has some scope behaviour that takes a bit of getting used to, but it’s exploited by a very specific class of libraries. In general, most code doesn’t really need to use <code>new Function</code>.</p>

<h3>References</h3>

<ul>
<li><a href="http://es5.github.com/">Annotated ES5</a></li>

<li><a href="https://github.com/jquery/jquery/">jQuery’s source</a></li>

<li><a href="https://github.com/LearnBoost/mongoose">Mongoose’s source</a></li>

<li><a href="https://github.com/visionmedia/jade">Jade’s source</a></li>
</ul><img src="http://feeds.feedburner.com/~r/dailyjs/~4/AQ_3atrfnhQ" height="1" width="1">