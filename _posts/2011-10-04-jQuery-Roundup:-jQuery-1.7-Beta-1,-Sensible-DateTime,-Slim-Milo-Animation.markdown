---
layout: post
title:  "jQuery Roundup: jQuery 1.7 Beta 1, Sensible DateTime, Slim Milo Animation"
date:   2011-10-04 07:00:00
author: 
categories: program
---

## jQuery Roundup: jQuery 1.7 Beta 1, Sensible DateTime, Slim Milo Animation
### by 
### at 2011-10-04 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/DI5qu725XHU/jquery-roundup>

<div>
<p>Note: You can send your plugins and articles in for review through our <a href="http://dailyjs.com/contact.html">contact form</a> or <a href="http://twitter.com/dailyjs">@dailyjs</a>.</p>
</div>
<h3>jQuery 1.7 Beta 1</h3>
<p><a href="http://blog.jquery.com/2011/09/28/jquery-1-7-beta-1-released/">jQuery 1.7 beta 1</a> has been released, and this version looks significant with a new events <span>API</span> and major underlying event code changes to improve Internet Explorer support.</p>
<p>The new events <span>API</span> is pretty simple on the surface: <code>$.on</code> and <code>$.off</code> have been added, which attach or remove events.  However, the intention behind this is to effectively combine the existing APIs for <code>.bind</code>, <code>.live</code>. and <code>.delegate</code>.  The rationale behind this is to remove some “surprising interactions” caused when mixing <code>bind</code> and <code>live</code> events.  This is fully explained on the jQuery blog’s 1.7 announcement.</p>
<h3>Sensible DateTime</h3>
<p><a href="http://crossbreeze.github.com/jquery-sensible-datetime/">Sensible DateTime</a> (GitHub: <a href="https://github.com/crossbreeze/jquery-sensible-datetime">crossbreeze / jquery-sensible-datetime</a>, License: <em><span>MIT</span></em>) by Jaewoong Kim is a plugin for formatting <span>ISO</span> times.  Formatting strings are supported, as well as natural language relative times — for example, “2 minutes ago” rather than a date and time.</p>
<p>To use it, call <code>$('.datetime').sensible()</code> on an element with either a <code>datetime</code> or <code>title</code> attribute, and make sure it contains a <a href="http://en.wikipedia.org/wiki/ISO_8601">ISO8601</a> datetime string.</p>
<p>The last time I solved this problem I’m fairly sure I used an ancient scrap of code by John Resig.  This library looks neatly written and has some fixes to support older browsers like IE6, so I’m going to use it the next time I’m dealing with client-side dates.</p>
<h3>The Slim Milo Affair</h3>
<p><a href="http://slimmilo.com/blog/index.php/2011/09/27/the-slim-milo-affair/">The Slim Milo Affair</a> is a tutorial with jQuery source that details the creation of a Pablo Ferro-inspired animation.  The author wanted to employ a similar effect to Ferro’s animation used by the <a href="http://www.youtube.com/watch?v=ELgjuHTbT3o">Thomas Crown Affair title sequence</a>.</p>
<blockquote>
<p>The basic concept of the library is that you have a single <span>DOM</span> element that you want to animate with various “stages”. Each stage consists of a main image shown in full, a set of horizontal and vertical bars, and a set of sub images put into the “slots” defined by the bars.</p>
</blockquote>
<p>It’s interesting to read through the reasoning behind this type of animation design, because we often use animation libraries without much thought to how they actually work.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/DI5qu725XHU" height="1" width="1">