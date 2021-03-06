---
layout: post
title:  "jQuery Roundup: Notificon, stickySectionHeaders, jQuery.suggest, smartTruncation"
date:   2011-10-11 07:00:00
author: 
categories: program
---

## jQuery Roundup: Notificon, stickySectionHeaders, jQuery.suggest, smartTruncation
### by 
### at 2011-10-11 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/P2_pDQr4zOU/jquery-roundup>

<div>
<p>Note: You can send your plugins and articles in for review through our <a href="http://dailyjs.com/contact.html">contact form</a> or <a href="http://twitter.com/dailyjs">@dailyjs</a>.</p>
</div>
<h3>Notificon</h3>
<p><a href="http://makeable.github.com/Notificon/">Notificon</a> (GitHub: <a href="https://github.com/makeable/Notificon">makeable / Notificon</a>, License: <em><span>BSD</span></em>) by Matt Williams dynamically changes a site’s favicon to include a notification value.  This works by generating <code>link</code> elements with data URIs using a <code>canvas</code>:</p>
<div><pre><code><span>var</span> <span>changeFavicon</span> <span>=</span> <span>function</span> <span>changeFavicon</span><span>(</span><span>canvas</span><span>)</span> <span>{</span>
  <span>var</span> <span>link</span> <span>=</span> <span>document</span><span>.</span><span>createElement</span><span>(</span><span>&#39;link&#39;</span><span>);</span>
  <span>link</span><span>.</span><span>type</span> <span>=</span> <span>&#39;image/x-icon&#39;</span><span>;</span>
  <span>link</span><span>.</span><span>rel</span> <span>=</span> <span>&#39;icon notificon&#39;</span><span>;</span>
  <span>link</span><span>.</span><span>href</span> <span>=</span> <span>canvas</span><span>.</span><span>toDataURL</span><span>(</span><span>&quot;image/png&quot;</span><span>);</span>
  <span>removeNotificon</span><span>();</span>
  <span>document</span><span>.</span><span>getElementsByTagName</span><span>(</span><span>&#39;head&#39;</span><span>)[</span><span>0</span><span>].</span><span>appendChild</span><span>(</span><span>link</span><span>);</span>
<span>};</span>
</code></pre>
</div><p>It’s a cool hack, and it made me wonder about Internet Explorer support.  IE8 apparently supports <code>link</code> elements with base64 encoded data, so perhaps something like <a href="http://code.google.com/p/explorercanvas/">ExplorerCanvas</a> could be used to get broader browser support?</p>
<h3>stickySectionHeaders</h3>
<p><a href="http://polarblau.github.com/stickySectionHeaders/">stickySectionHeaders</a> (GitHub: <a href="https://github.com/polarblau/stickySectionHeaders">polarblau / stickySectionHeaders</a>, License: <em><span>GPL</span> and <span>MIT</span></em>) by Polarblau is a jQuery plugin that can help create iOS-style sticky headers in a list.  All that’s needed is a simple call:</p>
<div><pre><code><span>$</span><span>(</span><span>&#39;#sticky-list&#39;</span><span>).</span><span>stickySectionHeaders</span><span>({</span>
  <span>stickyClass</span>     <span>:</span> <span>&#39;sticky&#39;</span><span>,</span>
  <span>headlineSelector</span><span>:</span> <span>&#39;strong&#39;</span>
<span>});</span>
</code></pre>
</div><p>The author has provided some iOS-style <span>CSS</span> as well.</p>
<h3>jQuery.suggest</h3>
<p><a href="http://polarblau.github.com/suggest/">jQuery.suggest</a> (GitHub: <a href="https://github.com/polarblau/suggest">polarblau / suggest</a>, License: <em><span>GPL</span> and <span>MIT</span></em>) also by Polarblau is an autocomplete plugin that works a little bit like native elements, with greyed out text.</p>
<p><img src="http://dailyjs.com/images/posts/jquerysuggest.png" alt=""></p>
<p>While it’s true there’s a lot of autocomplete plugins out there, how many of them are written this cleanly and come with tests?</p>
<h3>smartTruncation</h3>
<p>Finally, <a href="http://polarblau.github.com/smarttruncation/">smartTruncation</a> (GitHub: <a href="https://github.com/polarblau/smarttruncation">polarblau / smarttruncation</a>, License: <em><span>GPL</span> and <span>MIT</span></em>) is another plugin by Polarblau.  This one truncates text within its parent element.  It can also truncate from the centre: “Hello World” becomes “Hel…rld”.</p>
<p>Text truncation in client-side code is a source of fascination for me because I’ve attempted it a few times without a huge amount of success.  Polarblau’s algorithm creates a cache of font size values and then adds characters to an element until it has filled the desired size.  It also takes file name extensions into account.</p>
<p>It’s pretty easy to use:</p>
<div><pre><code><span>$</span><span>(</span><span>&#39;.files li&#39;</span><span>).</span><span>smartTruncation</span><span>({</span>
  <span>&#39;protectExtensions&#39;</span> <span>:</span> <span>true</span> <span>// &quot;myimagefile.jpg&quot; -&gt; &quot;myimagef...jpg&quot;</span>
<span>});</span>
</code></pre>
</div><p>It’s worth noting that each of these plugins by Polarblau is clearly licensed, includes tests, and has its own project web site (hosted by GitHub).  This is how you distribute plugins!</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/P2_pDQr4zOU" height="1" width="1">