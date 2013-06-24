---
layout: post
title:  "JS101: Call, Apply, and Bind"
date:   2012-06-25 07:00:00
author: 
categories: program
---

## JS101: Call, Apply, and Bind
### by 
### at 2012-06-25 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/2sgZ3kU5xbc/this-binding>

<p><a href="http://dailyjs.com/2012/06/18/js101-this/">Last week</a> we looked at <code>this</code>, and how it can be assigned and manipulated. The example I gave showed that a function inside another function (or method) will resolve <code>this</code> to the global object, and I used <code>var self = this</code> to reference the intended object.</p>

<p>Reader n1k0 pointed out another solution: <code>Function.prototype.call</code>:</p>
<div><pre><code><span>Shape</span><span>.</span><span>prototype</span> <span>=</span> <span>{</span>
  <span>move</span><span>:</span> <span>function</span><span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>x</span> <span>+=</span> <span>x</span><span>;</span>
    <span>this</span><span>.</span><span>y</span> <span>+=</span> <span>y</span><span>;</span>

    <span>function</span> <span>checkBounds</span><span>()</span> <span>{</span>
      <span>if</span> <span>(</span><span>this</span><span>.</span><span>x</span> <span>&gt;</span> <span>100</span><span>)</span> <span>{</span>
        <span>console</span><span>.</span><span>error</span><span>(</span><span>&#39;Warning: Shape out of bounds&#39;</span><span>);</span>
      <span>}</span>
    <span>}</span>

    <span>checkBounds</span><span>.</span><span>call</span><span>(</span><span>this</span><span>);</span>
  <span>}</span>
<span>};</span>
</code></pre>
</div>
<h3>The <code>call</code> and <code>apply</code> Methods</h3>

<p>In <a href="http://dailyjs.com/2012/06/04/js101-object-create/">JS101: Object.create</a>, I mentioned how <code>Function.prototype.call</code> can be used to chain constructors. Let’s take a deeper look at these methods of <code>Function</code>.</p>

<p>The difference between the two is we generally use <code>call</code> when we know what a function’s arguments are, because they’re supplied as arguments. Conversely, <code>apply</code> expects an array of parameters:</p>
<div><pre><code><span>Shape</span><span>.</span><span>prototype</span> <span>=</span> <span>{</span>
  <span>move</span><span>:</span> <span>function</span><span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>x</span> <span>+=</span> <span>x</span><span>;</span>
    <span>this</span><span>.</span><span>y</span> <span>+=</span> <span>y</span><span>;</span>

    <span>function</span> <span>checkBounds</span><span>(</span><span>min</span><span>,</span> <span>max</span><span>)</span> <span>{</span>
      <span>if</span> <span>(</span><span>this</span><span>.</span><span>x</span> <span>&lt;</span> <span>min</span> <span>||</span> <span>this</span><span>.</span><span>x</span> <span>&gt;</span> <span>max</span><span>)</span> <span>{</span>
        <span>console</span><span>.</span><span>error</span><span>(</span><span>&#39;Warning: Shape out of bounds&#39;</span><span>);</span>
      <span>}</span>
    <span>}</span>

    <span>checkBounds</span><span>.</span><span>call</span><span>(</span><span>this</span><span>,</span> <span>0</span><span>,</span> <span>100</span><span>);</span>
    <span>checkBounds</span><span>.</span><span>apply</span><span>(</span><span>this</span><span>,</span> <span>[</span><span>0</span><span>,</span> <span>100</span><span>]);</span>
  <span>}</span>
<span>};</span>
</code></pre>
</div>
<p>These methods have been around since ECMAScript 3rd Edition, and you’ll see them used a lot. Take a look at the source for any popular JavaScript project and you’ll find one of these methods being used:</p>

<ul>
<li><a href="https://github.com/jquery/jquery/blob/master/src/core.js">jQuery: core.js</a></li>

<li><a href="https://github.com/visionmedia/express/blob/master/lib/application.js">Express: application.js</a></li>

<li><a href="https://github.com/documentcloud/backbone/blob/master/backbone.js">Backbone.js</a></li>
</ul>

<p>Why are these methods so popular? Basically it comes down to functions being a first class citizen in JavaScript. I’ve been talking about the importance of objects for weeks, but learning how to manipulate functions is the key to mastering JavaScript.</p>

<p>JavaScript allows us to pass functions around, then execute them in different contexts by taking advantage of <code>call</code> and <code>apply</code>.</p>

<p>This is an example from the beginning of <a href="http://api.jquery.com/jQuery/">jQuery’s documentation</a>:</p>
<div><pre><code><span>$</span><span>(</span><span>&#39;div.foo&#39;</span><span>).</span><span>click</span><span>(</span><span>function</span><span>()</span> <span>{</span>
  <span>$</span><span>(</span><span>this</span><span>).</span><span>slideUp</span><span>();</span>
<span>});</span>
</code></pre>
</div>
<p>Here, <code>this</code> refers to the relevant DOM object. We’ve supplied our own callback, but <code>this</code> points to something that makes sense within the context of jQuery’s API.</p>

<h3>Binding</h3>

<p>In the comments for the previous post, Andres Descalzo pointed out that <a href="https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Function/bind">Function.prototype.bind</a> can also be used to change <code>this</code>. The <code>bind</code> method of <code>Function</code> was introduced in ECMAScript 5th Edition, and it returns a new function that’s <em>bound</em> to a different <code>this</code>:</p>
<div><pre><code><span>Shape</span><span>.</span><span>prototype</span> <span>=</span> <span>{</span>
  <span>move</span><span>:</span> <span>function</span><span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>x</span> <span>+=</span> <span>x</span><span>;</span>
    <span>this</span><span>.</span><span>y</span> <span>+=</span> <span>y</span><span>;</span>

    <span>function</span> <span>checkBounds</span><span>(</span><span>min</span><span>,</span> <span>max</span><span>)</span> <span>{</span>
      <span>if</span> <span>(</span><span>this</span><span>.</span><span>x</span> <span>&lt;</span> <span>min</span> <span>||</span> <span>this</span><span>.</span><span>x</span> <span>&gt;</span> <span>max</span><span>)</span> <span>{</span>
        <span>console</span><span>.</span><span>error</span><span>(</span><span>&#39;Warning: Shape out of bounds&#39;</span><span>);</span>
      <span>}</span>
    <span>}</span>

    <span>var</span> <span>checkBoundsThis</span> <span>=</span> <span>checkBounds</span><span>.</span><span>bind</span><span>(</span><span>this</span><span>);</span>
    <span>checkBoundsThis</span><span>(</span><span>0</span><span>,</span> <span>100</span><span>);</span>
  <span>}</span>
<span>};</span>
</code></pre>
</div>
<p>Notice that a new variable has to be created for the newly bound function. <a href="http://dailyjs.com/2012/06/18/js101-this/#comment-561090342">The comment Andres posted</a> uses an anonymous function to remove the extra variable:</p>
<div><pre><code><span>Shape</span><span>.</span><span>prototype</span> <span>=</span> <span>{</span>
  <span>move</span><span>:</span> <span>function</span><span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>x</span> <span>+=</span> <span>x</span><span>;</span>
    <span>this</span><span>.</span><span>y</span> <span>+=</span> <span>y</span><span>;</span>

    <span>var</span> <span>checkBounds</span> <span>=</span> <span>function</span><span>(</span><span>min</span><span>,</span> <span>max</span><span>)</span> <span>{</span>
      <span>if</span> <span>(</span><span>this</span><span>.</span><span>x</span> <span>&lt;</span> <span>min</span> <span>||</span> <span>this</span><span>.</span><span>x</span> <span>&gt;</span> <span>max</span><span>)</span> <span>{</span>
        <span>console</span><span>.</span><span>error</span><span>(</span><span>&#39;Warning: Shape out of bounds&#39;</span><span>);</span>
      <span>}</span>
    <span>}.</span><span>bind</span><span>(</span><span>this</span><span>);</span>

    <span>checkBounds</span><span>(</span><span>0</span><span>,</span> <span>100</span><span>);</span>
  <span>}</span>
<span>};</span>
</code></pre>
</div>
<p>I like <a href="https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Function/bind">Mozilla’s bind documentation</a>, because it shows how some built-in methods like <code>setTimeout</code> require less syntax when used with methods. The lesson here isn’t really due to the design of <code>setTimeout</code> but more general: using <code>bind</code> is a convenient way to pass a <em>method</em> to another function.</p>

<h3>Summary</h3>

<p>When working with methods, the value of <code>this</code> is important. By using the <code>apply</code>, <code>call</code>, and <code>bind</code> methods of <code>Function</code>, we can:</p>

<ul>
<li>Change <code>this</code> inside a function, enabling flexible APIs to be created as demonstrated by popular projects like jQuery, Backbone.js, and Express</li>

<li>Reduce syntactical overhead when using functions inside methods, and passing methods to other functions</li>

<li>Chain calls to constructors (see <a href="http://dailyjs.com/2012/06/04/js101-object-create/">JS101: Object.create</a>)</li>
</ul><img src="http://feeds.feedburner.com/~r/dailyjs/~4/2sgZ3kU5xbc" height="1" width="1">