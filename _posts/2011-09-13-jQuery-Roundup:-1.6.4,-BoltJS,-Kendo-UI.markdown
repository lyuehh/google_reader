---
layout: post
title:  "jQuery Roundup: 1.6.4, BoltJS, Kendo UI"
date:   2011-09-13 07:00:00
author: 
categories: program
---

## jQuery Roundup: 1.6.4, BoltJS, Kendo UI
### by 
### at 2011-09-13 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/WCWkJ_ikGXw/jquery-roundup>

<div>
<p>Note: You can send your plugins and articles in for review through our <a href="http://dailyjs.com/contact.html">contact form</a> or <a href="http://twitter.com/dailyjs">@dailyjs</a>.</p>
</div>
<h3>jQuery 1.6.4</h3>
<p><a href="http://blog.jquery.com/2011/09/12/jquery-1-6-4-released/">jQuery 1.6.4</a> is out already!  This version fixes issues discovered in the last release:</p>
<ul>
	<li><a href="http://bugs.jquery.com/ticket/10194">#10194</a> – Data attribute names with single dash-surrounded letters cannot be accessed by the camel-case name</li>
	<li><a href="http://bugs.jquery.com/ticket/10208">#10208</a> – <code>$("form").live("submit", fn)</code> not fired from <code>&lt;button type=&quot;submit&quot;&gt;</code> in IE8</li>
	<li><a href="http://bugs.jquery.com/ticket/10197">#10197</a> – Bug with mime-type <code>application/xhtml+xml</code> in jquery 1.6.3</li>
</ul>
<h3>BoltJS</h3>
<p><a href="http://shaneosullivan.github.com/boltjs/">BoltJS</a> (GitHub: <a href="https://github.com/shaneosullivan/boltjs">shaneosullivan / boltjs</a>, License: <em><span>MIT</span></em>) by Shane O’Sullivan (and Facebook apparently) is a client-side framework built on <a href="http://javelinjs.com">Javelin</a> and CommonJS modules.  It makes it possible to compose complex widgets from other widgets, then synchronise their data models.  To do this, several core modules are used:</p>
<ul>
	<li><a href="http://shaneosullivan.github.com/boltjs/view_building.html">Builder</a> – for managing views based on <span>JSON</span> data</li>
	<li><a href="http://shaneosullivan.github.com/boltjs/view_layouts.html">Layout Views</a> – includes horizontal and vertical view abstractions, implemented with the <a href="http://www.w3.org/TR/css3-flexbox"><span>CSS</span> Flex Box Model</a></li>
	<li><a href="http://shaneosullivan.github.com/boltjs/model_binding.html">Binding Models To Views</a> – a “data binding” framework</li>
</ul>
<h3>Kendo UI</h3>
<p><img src="http://dailyjs.com/images/posts/kendoui.png" alt=""></p>
<p><a href="http://www.kendoui.com/">Kendo UI</a> (<a href="http://www.kendoui.com/download/licenseagreement.aspx?skuId=436">License</a>) by Telerik Inc. is a new HTML5 UI framework that’s built on jQuery.  The core framework includes a <code>DataSource</code> component for using local or remote data that supports <span>CRUD</span> operations, and can be bound to UI widgets.  Templates are another core feature, and it’s claimed they’re extremely fast.  Kendo UI also supports a strong set of UI widgets and tools for building mobile web apps.</p>
<p>The <a href="http://demos.kendoui.com/treeview/index.html">TreeView</a>, <a href="http://demos.kendoui.com/upload/index.html">Upload</a>, and <a href="http://demos.kendoui.com/splitter/index.html">Splitter</a> widgets caught my eye as I looked through the documentation — particularly the Splitter, as it’s something I’ve often been tasked to build and felt was missing from jQuery UI.</p>
<p>Kendo UI’s widgets are instantiated and configured the same way as any jQuery plugin.  The “widget client object” can be accessed using <code>data()</code>:</p>
<div><pre><code><span>var</span> <span>grid</span> <span>=</span> <span>$</span><span>(</span><span>&#39;#grid&#39;</span><span>).</span><span>data</span><span>(</span><span>&#39;kendoGrid&#39;</span><span>);</span>
</code></pre>
</div><p>Events can be bound through widget initialization or using <code>bind</code>:</p>
<div><pre><code><span>$</span><span>(</span><span>&#39;#search&#39;</span><span>).</span><span>kendoAutoComplete</span><span>({</span>
  <span>dataSource</span><span>:</span> <span>data</span><span>,</span>
  <span>change</span><span>:</span> <span>onChange</span><span>,</span>
  <span>close</span><span>:</span> <span>onClose</span><span>,</span>
  <span>open</span><span>:</span> <span>onOpen</span>
<span>});</span>

<span>var</span> <span>autoComplete</span> <span>=</span> <span>$</span><span>(</span><span>&#39;#search&#39;</span><span>).</span><span>data</span><span>(</span><span>&#39;kendoAutoComplete&#39;</span><span>);</span>
<span>autoComplete</span><span>.</span><span>bind</span><span>(</span><span>&#39;change&#39;</span><span>,</span> <span>onChange</span><span>);</span>
</code></pre>
</div><img src="http://feeds.feedburner.com/~r/dailyjs/~4/WCWkJ_ikGXw" height="1" width="1">