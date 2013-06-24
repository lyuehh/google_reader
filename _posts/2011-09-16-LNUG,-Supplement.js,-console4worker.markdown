---
layout: post
title:  "LNUG, Supplement.js, console4worker"
date:   2011-09-16 07:00:00
author: 
categories: program
---

## LNUG, Supplement.js, console4worker
### by 
### at 2011-09-16 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/TVoGvFn9qvI/lnug-supplementjs-console4worker>

<h3><span>LNUG</span></h3>
<p>The <a href="http://lnug.org/">London Node.js User Group</a> occurs on the last Wednesday of every month, in Camden.  This month’s event is on the 28th of September at 6pm, and has four talks scheduled:</p>
<ul>
	<li>Tom Hall – Webscaling With Node.js</li>
	<li>Andy Kent – Streaming Analytics and Node.js</li>
	<li>Rob Tweed – The Globals Database: It’s Significance To Developers</li>
	<li>Garren Smith – These are the ORMs you are looking for</li>
</ul>
<p>There’s a Google Group for keeping up to date with the group: <a href="https://groups.google.com/group/lnug?pli=1"><span>LNUG</span> Google Group</a>.</p>
<h3>Supplement.js</h3>
<p><a href="http://supplementjs.com/">Supplement.js</a> (GitHub: <a href="https://github.com/olivernn/supplement.js">olivernn / supplement.js</a>) by Oliver Nightingale adds extensions to built-in types:</p>
<blockquote>
<p>[Supplement.js is] a small collection of utility functions to make working with JavaScript that much sweeter and more expressive.  Responsibly adds extensions to built in types.</p>
</blockquote>
<p>There are some nice functional style methods, like <code>Array.prototype.head</code>, <code>Array.prototype.tail</code>, <code>Array.prototype.take</code>, and a lot of functions you probably use from <a href="http://documentcloud.github.com/underscore/">Underscore.js</a>.</p>
<p>The <code>Function</code> extensions are interesting too: <code>Function.prototype.throttle</code> for example returns a function that will only execute once every <code>n</code> milliseconds.</p>
<h3>console4Worker</h3>
<p><a href="http://blog.jetienne.com/blog/2011/09/12/console4worker/">console4Worker</a> (GitHub: <a href="https://github.com/jeromeetienne/console4Worker">jeromeetienne / console4Worker</a>, License: <em><span>MIT</span></em>) by Jerome Etienne helps debug WebWorkers by making <code>console.log</code> (and other <code>console</code> methods) work inside workers in browsers that don’t yet support this.</p>
<p>Some configuration within each worker is required first:</p>
<div><pre><code><span>importScripts</span><span>(</span><span>&#39;console4Worker-worker.js&#39;</span><span>);</span>
<span>console</span><span>.</span><span>log</span><span>(</span><span>&quot;console call made from inside a webworker&quot;</span><span>);</span>
</code></pre>
</div><p>Then on the actual page:</p>
<div><pre><code><span>// init the worker</span>
<span>var</span> <span>worker</span>  <span>=</span> <span>new</span> <span>Worker</span><span>(</span><span>&quot;worker.js&quot;</span><span>);</span>
<span>// bind the console4Worker to get console API from worker</span>
<span>console4Worker</span><span>.</span><span>bind</span><span>(</span><span>worker</span><span>);</span>

<span>worker</span><span>.</span><span>addEventListener</span><span>(</span><span>&#39;message&#39;</span><span>,</span> <span>function</span><span>(</span><span>event</span><span>)</span> <span>{</span>
  <span>// filter this event if it is from console4Worker</span>
  <span>if</span> <span>(</span><span>console4Worker</span><span>.</span><span>filterEvent</span><span>(</span><span>event</span><span>))</span> <span>{</span>
    <span>return</span><span>;</span>
  <span>}</span>

  <span>// ... here handle your own events</span>
<span>},</span> <span>false</span><span>);</span>
</code></pre>
</div><img src="http://feeds.feedburner.com/~r/dailyjs/~4/TVoGvFn9qvI" height="1" width="1">