---
layout: post
title:  "jQuery Roundup: Publish Subscribe, Transparency, slabText"
date:   2012-01-24 08:00:00
author: 
categories: program
---

## jQuery Roundup: Publish Subscribe, Transparency, slabText
### by 
### at 2012-01-24 08:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/5XkrT05nhuk/jquery-roundup>

<div>
Note: You can send your plugins and articles in for review through our <a href="http://dailyjs.com/contact.html">contact form</a> or <a href="http://twitter.com/dailyjs">@dailyjs</a>.
</div>
<h3>Publish Subscribe Plugin</h3>

<p><a href="http://www.joezimjs.com/projects/publish-subscribe-jquery-plugin/">Joseph Zimmerman’s Publish Subscribe Plugin</a> (License: <em>GPL</em>) is an implementation of the aforementioned messaging pattern aimed at browser-based JavaScript:</p>
<div><pre><code><span>var</span> <span>handle</span> <span>=</span> <span>$</span><span>.</span><span>subscribe</span><span>(</span><span>&#39;foo&#39;</span><span>,</span> <span>function</span><span>(</span><span>topic</span><span>,</span> <span>data</span><span>)</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>data</span><span>,</span> <span>topic</span><span>);</span>
<span>});</span>

<span>$</span><span>.</span><span>publish</span><span>(</span><span>&#39;foo bar&#39;</span><span>,</span> <span>&#39;This is some data&#39;</span><span>);</span>

<span>$</span><span>.</span><span>unsubscribe</span><span>(</span><span>handle</span><span>);</span>
</code></pre>
</div>
<p>The author has implemented it with core jQuery methods like <code>$.type</code> and <code>$.each</code> so the source is readily understandable.</p>

<h3>Transparency</h3>

<p><a href="http://leonidas.github.com/transparency/">Transparency</a> (GitHub: <a href="https://github.com/leonidas/transparency">leonidas / transparency</a>, License: <em>MIT</em>) by Jarno Keskikangas is a template engine for jQuery that maps JSON to DOM elements using a <code>$.render</code> method:</p>
<div><pre><code><span>var</span> <span>hello</span> <span>=</span> <span>{</span>
  <span>hello</span><span>:</span>   <span>&#39;Hello&#39;</span><span>,</span>
  <span>goodbye</span><span>:</span> <span>&#39;Goodbye!&#39;</span>
<span>};</span>

<span>$</span><span>(</span><span>&#39;.container&#39;</span><span>).</span><span>render</span><span>(</span><span>hello</span><span>);</span>
</code></pre>
</div>
<p>This example would write the values to tags matching the selectors <code>.hello</code> and <code>.goodbye</code>:</p>

<blockquote>
<p>Transparency relies on convention over configuration and requires you to have 1:1 match between CSS classes and JSON objects. The idea is to minimize the cognitive noise you have to deal with. Just call <code>$(&#39;.container&#39;).render(data)</code> and move on.</p>
</blockquote>

<p>There’s a detailed blog post about Transparency here: <a href="https://github.com/leonidas/codeblog/blob/master/2012/2012-01-13-implementing-semantic-anti-templating-with-jquery.md">Implementing Semantic Anti-Templating With jQuery</a>.</p>

<h3>slabText</h3>

<p><img src="http://dailyjs.com/images/posts/slabtext.png" alt="slabText example"></p>

<p><a href="http://www.frequency-decoder.com/demo/slabText/">slabText</a> (GitHub: <a href="https://github.com/freqDec/slabText/">freqdec / slabText</a>, License: <em>MIT/GPLv2</em>) by Brian McAllister splits headlines into rows, and resizes them to fill the available space. This works as the browser viewport changes.</p>

<p>Brian notes that this is based on Erik Loyer’s <a href="http://erikloyer.com/index.php/blog/the_slabtype_algorithm_part_1_background/">slabtype algorithm</a>, which is interesting reading for those inspired by <a href="http://fittextjs.com/">FitText</a>.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/5XkrT05nhuk" height="1" width="1">