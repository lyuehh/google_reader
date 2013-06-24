---
layout: post
title:  "Let&#39;s Make a Framework: Ajax Improvements"
date:   2011-09-08 07:00:00
author: 
categories: program
---

## Let&#39;s Make a Framework: Ajax Improvements
### by 
### at 2011-09-08 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/WDr1bdF2WBM/framework-79>

<div>
<p><em>Let’s Make a Framework</em> is an ongoing series about building a JavaScript framework from the ground up.</p>
<p>These articles are tagged with <a href="http://dailyjs.com/tags.html#lmaf">lmaf</a>.  The project we’re creating is called <a href="http://github.com/alexyoung/turing.js">Turing</a>.  Documentation is available at <a href="http://turingjs.com/">turingjs.com</a>.</p>
</div>
<p>I was impressed by the <a href="https://github.com/visionmedia/superagent">Superagent</a> <span>HTTP</span> library, so I decided to see what improvements I could make to <code>turing.net</code> based on it.  Along the way I found some IE compatibility issues that I hadn’t spotted before, and improved the functional testing script.</p>
<h3>Response and Requests</h3>
<p>Both <a href="http://expressjs.com/">Express</a> and Superagent pass abstracted response and request objects back to callbacks.  I found the need for this arose when I noticed IE6 didn’t like me adding properties to the <code>ActiveXObject</code> it uses to support <code>XMLHttpRequest</code>.  To fix this I decided to take some inspiration from these projects and provide an abstracted response object to callbacks:</p>
<div><pre><code><span>var</span> <span>response</span> <span>=</span> <span>{};</span>

<span>response</span><span>.</span><span>status</span> <span>=</span> <span>request</span><span>.</span><span>status</span><span>;</span>
<span>response</span><span>.</span><span>responseText</span> <span>=</span> <span>request</span><span>.</span><span>responseText</span><span>;</span>
<span>if</span> <span>(</span><span>/json/</span><span>.</span><span>test</span><span>(</span><span>contentType</span><span>))</span> <span>{</span>
  <span>response</span><span>.</span><span>responseJSON</span> <span>=</span> <span>net</span><span>.</span><span>parseJSON</span><span>(</span><span>request</span><span>.</span><span>responseText</span><span>);</span>
<span>}</span> <span>else</span> <span>if</span> <span>(</span><span>/xml/</span><span>.</span><span>test</span><span>(</span><span>contentType</span><span>))</span> <span>{</span>
  <span>response</span><span>.</span><span>responseXML</span> <span>=</span> <span>net</span><span>.</span><span>parseXML</span><span>(</span><span>request</span><span>.</span><span>responseText</span><span>);</span>
<span>}</span>

<span>if</span> <span>(</span><span>successfulRequest</span><span>(</span><span>request</span><span>))</span> <span>{</span>
  <span>if</span> <span>(</span><span>options</span><span>.</span><span>success</span><span>)</span> <span>options</span><span>.</span><span>success</span><span>(</span><span>response</span><span>,</span> <span>request</span><span>);</span>
  <span>if</span> <span>(</span><span>promise</span><span>)</span> <span>promise</span><span>.</span><span>resolve</span><span>(</span><span>response</span><span>,</span> <span>request</span><span>);</span>
<span>}</span> <span>else</span> <span>{</span>
  <span>if</span> <span>(</span><span>options</span><span>.</span><span>error</span><span>)</span> <span>options</span><span>.</span><span>error</span><span>(</span><span>response</span><span>,</span> <span>request</span><span>);</span>
  <span>if</span> <span>(</span><span>promise</span><span>)</span> <span>promise</span><span>.</span><span>reject</span><span>(</span><span>response</span><span>,</span> <span>request</span><span>);</span>
<span>}</span>
</code></pre>
</div><p>Now the callbacks get a more friendly response object, as well as the original <code>XMLHttpRequest</code> request object.  The <span>JSON</span> and <span>XML</span> tests were also added here — previously only <span>JSON</span> was parsed, but I added support for <span>XML</span> as well.</p>
<h3>Parsing <span>XML</span></h3>
<p>In <a href="https://developer.mozilla.org/en/Parsing_and_serializing_XML">Parsing and serializing <span>XML</span></a> on <span>MDN</span>, the following fragment is suggested for parsing <span>XML</span>:</p>
<div><pre><code><span>var</span> <span>theString</span><span>=</span><span>&#39;&lt;a id=&quot;a&quot;&gt;&lt;b id=&quot;b&quot;&gt;hey!&lt;/b&gt;&lt;/a&gt;&#39;</span><span>;</span>
<span>var</span> <span>parser</span> <span>=</span> <span>new</span> <span>DOMParser</span><span>();</span>
<span>var</span> <span>dom</span> <span>=</span> <span>parser</span><span>.</span><span>parseFromString</span><span>(</span><span>theString</span><span>,</span> <span>&quot;text/xml&quot;</span><span>);</span>
<span>// print the name of the root element or error message</span>
<span>dump</span><span>(</span><span>dom</span><span>.</span><span>documentElement</span><span>.</span><span>nodeName</span> <span>==</span> <span>&quot;parsererror&quot;</span> <span>?</span> <span>&quot;error while parsing&quot;</span> <span>:</span> <span>dom</span><span>.</span><span>documentElement</span><span>.</span><span>nodeName</span><span>);</span>
</code></pre>
</div><p>The resulting <span>XML</span> isn’t parsed into a plain JavaScript <code>Object</code> but a <code>Document</code> instead, which means methods and properties like <code>nodeName</code> are available.</p>
<p>Microsoft’s approach is again to use <code>ActiveXObject</code>:</p>
<div><pre><code><span>// Instantiate a DOM object at run time.</span>
<span>var</span> <span>dom</span> <span>=</span> <span>new</span> <span>ActiveXObject</span><span>(</span><span>&quot;msxml2.DOMDocument.6.0&quot;</span><span>);</span>
<span>dom</span><span>.</span><span>async</span> <span>=</span> <span>false</span><span>;</span>
<span>dom</span><span>.</span><span>resolveExternals</span> <span>=</span> <span>false</span><span>;</span>
<span>dom</span><span>.</span><span>loadXML</span><span>(</span><span>&quot;&lt;a&gt;A&lt;/a&gt;&quot;</span><span>);</span>
</code></pre>
</div><p>This is from <a href="http://msdn.microsoft.com/en-us/library/ms764708(v=vs.85).aspx">InstantiateDOM.js at <span>MSDN</span></a>.</p>
<p>I decided to define a <code>parseXML</code> method once based on browser support:</p>
<div><pre><code>  <span>/**</span>
<span>    * Parses XML represented as a string.</span>
<span>    *</span>
<span>    * @param {String} string The original string</span>
<span>    * @returns {Object} A JavaScript object</span>
<span>    */</span>
  <span>if</span> <span>(</span><span>window</span><span>.</span><span>DOMParser</span><span>)</span> <span>{</span>
    <span>net</span><span>.</span><span>parseXML</span> <span>=</span> <span>function</span><span>(</span><span>text</span><span>)</span> <span>{</span>
      <span>return</span> <span>new</span> <span>DOMParser</span><span>().</span><span>parseFromString</span><span>(</span><span>text</span><span>,</span> <span>&#39;text/xml&#39;</span><span>);</span>
    <span>};</span>
  <span>}</span> <span>else</span> <span>{</span>
    <span>net</span><span>.</span><span>parseXML</span> <span>=</span> <span>function</span><span>(</span><span>text</span><span>)</span> <span>{</span>
      <span>var</span> <span>xml</span> <span>=</span> <span>new</span> <span>ActiveXObject</span><span>(</span><span>&#39;Microsoft.XMLDOM&#39;</span><span>);</span>
      <span>xml</span><span>.</span><span>async</span> <span>=</span> <span>&#39;false&#39;</span><span>;</span>
      <span>xml</span><span>.</span><span>loadXML</span><span>(</span><span>text</span><span>);</span>
      <span>return</span> <span>xml</span><span>;</span>
    <span>};</span>
  <span>}</span>
</code></pre>
</div><p>To test this, I wrote the following:</p>
<div><pre><code><span>&#39;test xml parsing&#39;</span><span>:</span> <span>function</span><span>()</span> <span>{</span>
  <span>$t</span><span>.</span><span>post</span><span>(</span><span>&#39;/give-me-xml&#39;</span><span>,</span> <span>{</span>
    <span>contentType</span><span>:</span> <span>&#39;application/xml&#39;</span><span>,</span>
    <span>success</span><span>:</span> <span>function</span><span>(</span><span>r</span><span>)</span> <span>{</span>
      <span>assert</span><span>.</span><span>equal</span><span>(</span><span>&#39;key&#39;</span><span>,</span> <span>r</span><span>.</span><span>responseXML</span><span>.</span><span>documentElement</span><span>.</span><span>nodeName</span><span>);</span>
    <span>}</span>
  <span>});</span>
<span>}</span>
</code></pre>
</div><p>This runs through an Express app, in <a href="https://github.com/alexyoung/turing.js/blob/master/test/functional/ajax.js">test/functional/ajax.js</a>.</p>
<h3>Promises</h3>
<p>As I mentioned, IE doesn’t like modifying the <code>XMLHttpRequest</code> object it provides through ActiveX.  I was setting a <code>then</code> property on request objects to support <a href="http://dailyjs.com/2011/06/02/framework-65/">promises</a> which was only used because the <code>XMLHttpRequest</code> was being returned from the network-related methods.  To get around the IE issue, I decided just to return an empty object with a <code>then</code> property, so this is still possible:</p>
<div><pre><code><span>$t</span><span>.</span><span>get</span><span>(</span><span>&#39;/get-test&#39;</span><span>).</span><span>then</span><span>(</span>
  <span>function</span><span>(</span><span>r</span><span>)</span> <span>{</span> <span>assert</span><span>.</span><span>equal</span><span>(</span><span>&#39;Sample text&#39;</span><span>,</span> <span>r</span><span>.</span><span>responseText</span><span>);</span> <span>},</span>
  <span>function</span><span>(</span><span>r</span><span>)</span> <span>{</span> <span>assert</span><span>.</span><span>ok</span><span>(</span><span>false</span><span>);</span> <span>}</span>
<span>);</span>
</code></pre>
</div><p>I made the documentation slightly ambiguous about what the network methods return because I suspect returning the current <code>turing</code> object to allow other chaining might be more useful.  It currently reads <code>@returns {Object} An object for further chaining with promises</code>, which is what I intended it to do in the first place.</p>
<h3>Conclusion</h3>
<p>After all that, I never really got to use any of TJ’s ideas from Superagent, other than abstracting response objects.  The most important thing to remember when creating cross-browser code is to be careful about extending native objects.  That’s a good rule of thumb for JavaScript in general, and I feel doubly embarrassed about doing it in the first place because I’ve been preaching this for a while!</p>
<p>You can get this week’s code in <a href="https://github.com/alexyoung/turing.js/commit/93a5d75338a8422e5a48fa2b5aa80049b1565b27">commit 93a5d75</a>.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/WDr1bdF2WBM" height="1" width="1">