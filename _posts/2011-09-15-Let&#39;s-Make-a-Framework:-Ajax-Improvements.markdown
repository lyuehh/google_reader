---
layout: post
title:  "Let&#39;s Make a Framework: Ajax Improvements"
date:   2011-09-15 07:00:00
author: 
categories: program
---

## Let&#39;s Make a Framework: Ajax Improvements
### by 
### at 2011-09-15 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/TURKqg_iMqk/framework-80>

<div>
<p><em>Let’s Make a Framework</em> is an ongoing series about building a JavaScript framework from the ground up.</p>
<p>These articles are tagged with <a href="http://dailyjs.com/tags.html#lmaf">lmaf</a>.  The project we’re creating is called <a href="http://github.com/alexyoung/turing.js">Turing</a>.  Documentation is available at <a href="http://turingjs.com/">turingjs.com</a>.</p>
</div>
<h3><span>API</span> Redesign</h3>
<p>As I mentioned last week, I thought it would be interesting to change the <code>turing.net</code> module to have a chained <span>API</span> that looks more like TJ Holowaychuk’s <a href="https://github.com/visionmedia/superagent">Superagent</a> library.  Part of the reason for doing this is to again demonstrate how easy it is to make a simple and consistent chained <span>API</span> with JavaScript.  However, TJ’s reasoning behind Superagent’s design is worth thinking about, because he makes some good points about the inconsistencies found in many modern frameworks.</p>
<p>jQuery’s <span>API</span> generally looks like this:</p>
<div><pre><code><span>$</span><span>.</span><span>get</span><span>(</span><span>&#39;/user/1&#39;</span><span>,</span> <span>function</span><span>(</span><span>data</span><span>,</span> <span>textStatus</span><span>,</span> <span>xhr</span><span>)</span> <span>{</span>
  <span>// Callback manages result</span>
<span>});</span>
</code></pre>
</div><p>TJ argued that reducing the arity in the callback would make it more friendly:</p>
<div><pre><code><span>request</span><span>.</span><span>get</span><span>(</span><span>&#39;/user/1&#39;</span><span>,</span> <span>function</span><span>(</span><span>res</span><span>)</span> <span>{</span>
  <span>// The abstracted `res` object contains status code, result data, etc.</span>
<span>});</span>
</code></pre>
</div><p>The <code>res</code> object neatly encapsulates everything you need to deal with the server’s response.</p>
<p>Another issue is any configuration requires falling back to the full-blown <code>$.ajax()</code> method.  And, much like my initial Turing design, we end up with calls like this:</p>
<div><pre><code><span>$</span><span>.</span><span>ajax</span><span>({</span>
  <span>url</span><span>:</span> <span>&#39;/api/pet&#39;</span><span>,</span>
  <span>type</span><span>:</span> <span>&#39;POST&#39;</span><span>,</span>
  <span>data</span><span>:</span> <span>{</span> <span>name</span><span>:</span> <span>&#39;Manny&#39;</span><span>,</span> <span>species</span><span>:</span> <span>&#39;cat&#39;</span> <span>},</span>
  <span>headers</span><span>:</span> <span>{</span> <span>&#39;X-API-Key&#39;</span><span>:</span> <span>&#39;foobar&#39;</span> <span>}</span>
<span>}).</span><span>success</span><span>(</span><span>function</span><span>(</span><span>res</span><span>)</span> <span>{</span>

<span>}).</span><span>error</span><span>(</span><span>function</span><span>()</span> <span>{</span>

<span>});</span>
</code></pre>
</div><p>Whenever you’re designing a JavaScript <span>API</span> and you find yourself creating large configuration options, it’s often worth sketching out what a chained <span>API</span> would look like.  I agree with TJ’s example that this looks better:</p>
<div><pre><code><span>request</span>
  <span>.</span><span>post</span><span>(</span><span>&#39;/api/pet&#39;</span><span>)</span>
  <span>.</span><span>data</span><span>({</span> <span>name</span><span>:</span> <span>&#39;Manny&#39;</span><span>,</span> <span>species</span><span>:</span> <span>&#39;cat&#39;</span> <span>})</span>
  <span>.</span><span>set</span><span>(</span><span>&#39;X-API-Key&#39;</span><span>,</span> <span>&#39;foobar&#39;</span><span>)</span>
  <span>.</span><span>set</span><span>(</span><span>&#39;Accept&#39;</span><span>,</span> <span>&#39;application/json&#39;</span><span>)</span>
  <span>.</span><span>end</span><span>(</span><span>function</span><span>(</span><span>res</span><span>)</span> <span>{</span>

  <span>});</span>
</code></pre>
</div><p>Rather than providing two callbacks, users of the library can process <code>res</code> however they wish.  They don’t depend on the framework’s interpretation of <span>HTTP</span> status codes.</p>
<h3>Tests</h3>
<p>To convert Turing’s <span>API</span> to a Superagent-inspired chained <span>API</span>, I started by writing tests based on my idealised <span>API</span>:</p>
<div><pre><code><span>$t</span><span>.</span><span>post</span><span>(</span><span>&#39;/post-test&#39;</span><span>)</span>
  <span>.</span><span>data</span><span>({</span> <span>key</span><span>:</span> <span>&#39;value&#39;</span> <span>})</span>
  <span>.</span><span>end</span><span>(</span><span>function</span><span>(</span><span>res</span><span>)</span> <span>{</span>
    <span>assert</span><span>.</span><span>equal</span><span>(</span><span>&#39;value&#39;</span><span>,</span> <span>res</span><span>.</span><span>responseText</span><span>);</span>
  <span>});</span>
</code></pre>
</div><p>Compare that to the old style:</p>
<div><pre><code><span>$t</span><span>.</span><span>post</span><span>(</span><span>&#39;/post-test&#39;</span><span>,</span> <span>{</span>
  <span>postBody</span><span>:</span> <span>{</span> <span>key</span><span>:</span> <span>&#39;value&#39;</span> <span>},</span>
  <span>success</span><span>:</span> <span>function</span><span>(</span><span>r</span><span>)</span> <span>{</span>
    <span>assert</span><span>.</span><span>equal</span><span>(</span><span>&#39;value&#39;</span><span>,</span> <span>r</span><span>.</span><span>responseText</span><span>);</span>
  <span>},</span>
  <span>error</span><span>:</span> <span>function</span><span>()</span> <span>{</span>
    <span>assert</span><span>.</span><span>ok</span><span>(</span><span>false</span><span>);</span>
  <span>}</span>
<span>});</span>
</code></pre>
</div><h3>Chains</h3>
<p>The network methods call and return an internal method, <code>ajax</code>.  This was returning an object for chaining <code>then</code> (the promise <span>API</span>), so I extended it:</p>
<div><pre><code><span>function</span> <span>ajax</span><span>(</span><span>url</span><span>,</span> <span>options</span><span>)</span> <span>{</span>
  <span>var</span> <span>chain</span> <span>=</span> <span>{};</span>

  <span>// All the old ajax stuff goes here</span>

  <span>chain</span> <span>=</span> <span>{</span>
    <span>set</span><span>:</span> <span>function</span><span>(</span><span>key</span><span>,</span> <span>value</span><span>)</span> <span>{</span>
      <span>options</span><span>.</span><span>headers</span><span>[</span><span>key</span><span>]</span> <span>=</span> <span>value</span><span>;</span>
      <span>return</span> <span>chain</span><span>;</span>
    <span>},</span>

    <span>send</span><span>:</span> <span>function</span><span>(</span><span>data</span><span>,</span> <span>callback</span><span>)</span> <span>{</span>
      <span>options</span><span>.</span><span>postBody</span> <span>=</span> <span>net</span><span>.</span><span>serialize</span><span>(</span><span>data</span><span>);</span>
      <span>options</span><span>.</span><span>callback</span> <span>=</span> <span>callback</span><span>;</span>
      <span>send</span><span>();</span>
      <span>return</span> <span>chain</span><span>;</span>
    <span>},</span>

    <span>end</span><span>:</span> <span>function</span><span>(</span><span>callback</span><span>)</span> <span>{</span>
      <span>options</span><span>.</span><span>callback</span> <span>=</span> <span>callback</span><span>;</span>
      <span>send</span><span>();</span>
      <span>return</span> <span>chain</span><span>;</span>
    <span>},</span>

    <span>data</span><span>:</span> <span>function</span><span>(</span><span>data</span><span>)</span> <span>{</span>
      <span>options</span><span>.</span><span>postBody</span> <span>=</span> <span>net</span><span>.</span><span>serialize</span><span>(</span><span>data</span><span>);</span>
      <span>return</span> <span>chain</span><span>;</span>
    <span>},</span>

    <span>then</span><span>:</span> <span>function</span><span>()</span> <span>{</span>
      <span>chain</span><span>.</span><span>end</span><span>();</span>
      <span>if</span> <span>(</span><span>promise</span><span>)</span> <span>promise</span><span>.</span><span>then</span><span>.</span><span>apply</span><span>(</span><span>promise</span><span>,</span> <span>arguments</span><span>);</span>
      <span>return</span> <span>chain</span><span>;</span>
    <span>}</span>
  <span>};</span>

  <span>return</span> <span>chain</span><span>;</span>
<span>}</span>
</code></pre>
</div><p>As with all chained APIs, the trick is to just return an object with the expected methods.  Here I return the same object from each method.  The only other things I had to change were to make <code>respondToReadyState</code> always call <code>options.callback</code> if present (else it looks for success/error callbacks from the promise <span>API</span> or old style <span>API</span>), and I made <code>net.serialize</code> cope with strings.</p>
<h3>Conclusion</h3>
<p>As we’ve seen before in this series, implementing chainable APIs is pretty easy.  In this case, a chainable network <span>API</span> seems a lot cleaner than large configuration objects</p>
<p>These changes can be reviewed in <a href="https://github.com/alexyoung/turing.js/commit/5263c6ca5418761f25b6025cc53229f6591f01ae">commit 5263c6c</a>.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/TURKqg_iMqk" height="1" width="1">