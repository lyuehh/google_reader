---
layout: post
title:  "jQuery Roundup: Smartupdater, vkTemplate, Handpicked jQuery Plugins"
date:   2011-11-29 08:00:00
author: 
categories: program
---

## jQuery Roundup: Smartupdater, vkTemplate, Handpicked jQuery Plugins
### by 
### at 2011-11-29 08:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/euf05zK5PbA/jquery-roundup>

<div>
<p>Note: You can send your plugins and articles in for review through our <a href="http://dailyjs.com/contact.html">contact form</a> or <a href="http://twitter.com/dailyjs">@dailyjs</a>.</p>
</div>
<h3>Smartupdater</h3>
<p><a href="http://www.eslinstructor.net/smartupdater/">Smartupdater</a> (GitHub: <a href="https://github.com/vkiryukhin/Smartupdater">vkiryukhin / Smartupdater</a>, License: <em><span>MIT</span></em> and <em><span>GPL</span></em>) by Vadim Kiryukhin is a plugin that specialises in Ajax polling, originally featured on DailyJS last year in <a href="http://dailyjs.com/2010/08/17/polling/">jQuery Rolling and Polling</a>.  It has recently been updated to version 4.0 (beta).</p>
<p>The <span>API</span> has been updated to reduce the number of functions the plugin adds to jQuery, so rather than <code>$('#myObject').smartupdaterStop()</code>, <code> $('#myObject').smartupdater("stop")</code> can be used instead.</p>
<h3>vkTemplate</h3>
<p>Vadim Kiryukhin has also released <a href="http://www.eslinstructor.net/vktemplate/">vkTemplate</a> (GitHub: <a href="https://github.com/vkiryukhin/vkTemplate">vkiryukhin / vkTemplate</a>, License: <em><span>MIT</span></em> and <em><span>GPL</span></em>).  This is a small (less than 1.5kb) templating plugin that’s designed to be used with Ajax.  Given URLs for a template and date, a template can be rendered like this:</p>
<div><pre><code><span>$</span><span>(</span><span>&#39;#subcontainer&#39;</span><span>).</span><span>vkTemplate</span><span>(</span><span>&#39;/user.tmpl&#39;</span><span>,</span> <span>&#39;/users/1.json&#39;</span><span>);</span>

<span>// JSON can be passed as well</span>
<span>$</span><span>(</span><span>&#39;#subcontainer&#39;</span><span>).</span><span>vkTemplate</span><span>(</span><span>&#39;/user.tmpl&#39;</span><span>,</span> <span>{</span> <span>name</span><span>:</span> <span>&#39;Kurt&#39;</span><span>,</span> <span>email</span><span>:</span> <span>&#39;kurt@thepope.org&#39;</span> <span>});</span>
</code></pre>
</div><h3>Handpicked jQuery Plugins</h3>
<p><img src="http://dailyjs.com/images/posts/handpicked.png" alt=""></p>
<p>The Handpicked jQuery Plugins Repo is a list of jQuery plugins curated by <a href="http://davidhiggins.me/">David Higgins</a>.  It’s actually a repository on GitHub, and he invites people to fork it here: <a href="https://github.com/higgo/handpicked.jquery.plugins.repo">higgo / handpicked.jquery.plugins.repo</a>.</p>
<p>David has also made a site where the plugins can be browsed, at <a href="http://iwantaneff.in/repo/">iwantaneff.in/repo/</a>.  And, if you’re not comfortable with git he’s even uploaded the plugins as files that can be downloaded from <a href="http://iwantaneff.in/repo/browser/">iwantaneff.in/repo/browser/</a>.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/euf05zK5PbA" height="1" width="1">