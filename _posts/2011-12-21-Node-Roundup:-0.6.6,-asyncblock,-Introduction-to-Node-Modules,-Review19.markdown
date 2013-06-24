---
layout: post
title:  "Node Roundup: 0.6.6, asyncblock, Introduction to Node Modules, Review19"
date:   2011-12-21 08:00:00
author: 
categories: program
---

## Node Roundup: 0.6.6, asyncblock, Introduction to Node Modules, Review19
### by 
### at 2011-12-21 08:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/f9uQWc0uPY8/node-roundup>

<h3>Node 0.6.6</h3>
<p><a href="http://blog.nodejs.org/2011/12/15/node-v0-6-6/">Node 0.6.6</a> is out, which updates npm, fixes some bugs, and adds pause/resume semantics to stdin.  Node also has a new website that is greener than ever, but remains clean and features a new corporate-friendly edge.</p>
<p>There’s also a post on the official Node blog entitled <a href="http://blog.nodejs.org/2011/12/15/growing-up/">Growing up</a> by Ryan Dahl that discusses Windows Azure support:</p>
<blockquote>
<p>The overarching goal of the port was to expand our user base to the largest number of developers. Happily, this has paid off in the form of being a first class citizen on Azure.</p>
</blockquote>
<h3>asyncblock</h3>
<p><a href="https://github.com/scriby/asyncblock">asyncblock</a> (License: <em><span>MIT</span></em>, npm: <em>asyncblock</em>) is a fork of <a href="https://github.com/axkibe/node-green-light">node-green-light</a>.  It works like a flow control library combined with <a href="https://github.com/laverdet/node-fibers">node-fibers</a>.</p>
<p>The author compares a ‘pure Node’ example with his own library’s code:</p>
<div><pre><code><span>function</span> <span>example</span><span>(</span><span>callback</span><span>){</span>
    <span>var</span> <span>finishedCount</span> <span>=</span> <span>0</span><span>;</span>
    <span>var</span> <span>fileContents</span> <span>=</span> <span>[];</span>

    <span>var</span> <span>continuation</span> <span>=</span> <span>function</span><span>(){</span>
        <span>if</span><span>(</span><span>finishedCount</span> <span>&lt;</span> <span>2</span><span>){</span>
            <span>return</span><span>;</span>
        <span>}</span>

        <span>fs</span><span>.</span><span>writeFile</span><span>(</span><span>&#39;path3&#39;</span><span>,</span> <span>fileContents</span><span>[</span><span>0</span><span>],</span> <span>function</span><span>(</span><span>err</span><span>)</span> <span>{</span>
            <span>if</span><span>(</span><span>err</span><span>)</span> <span>{</span>
                <span>throw</span> <span>new</span> <span>Error</span><span>(</span><span>err</span><span>);</span>
            <span>}</span>

            <span>fs</span><span>.</span><span>readFile</span><span>(</span><span>&#39;path3&#39;</span><span>,</span> <span>&#39;utf8&#39;</span><span>,</span> <span>function</span><span>(</span><span>err</span><span>,</span> <span>data</span><span>){</span> 
                <span>console</span><span>.</span><span>log</span><span>(</span><span>data</span><span>);</span>
                <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;all done&#39;</span><span>);</span>
            <span>});</span>
        <span>});</span>
    <span>};</span>

    <span>fs</span><span>.</span><span>readFile</span><span>(</span><span>&#39;path1&#39;</span><span>,</span> <span>&#39;utf8&#39;</span><span>,</span> <span>function</span><span>(</span><span>err</span><span>,</span> <span>data</span><span>)</span> <span>{</span>
        <span>if</span><span>(</span><span>err</span><span>)</span> <span>{</span>
            <span>throw</span> <span>new</span> <span>Error</span><span>(</span><span>err</span><span>);</span>
        <span>}</span>

        <span>fnishedCount</span><span>++</span><span>;</span>
        <span>fileContents</span><span>[</span><span>0</span><span>]</span> <span>=</span> <span>data</span><span>;</span>

        <span>continuation</span><span>();</span>
    <span>});</span>

    <span>fs</span><span>.</span><span>readFile</span><span>(</span><span>&#39;path2&#39;</span><span>,</span> <span>&#39;utf8&#39;</span><span>,</span> <span>function</span><span>(</span><span>err</span><span>,</span> <span>data</span><span>)</span> <span>{</span>
        <span>if</span><span>(</span><span>err</span><span>)</span> <span>{</span>
            <span>throw</span> <span>new</span> <span>Error</span><span>(</span><span>err</span><span>);</span>
        <span>}</span>

        <span>fnishedCount</span><span>++</span><span>;</span>
        <span>fileContents</span><span>[</span><span>1</span><span>]</span> <span>=</span> <span>data</span><span>;</span>

        <span>continuation</span><span>();</span>
    <span>});</span>
<span>}</span>
</code></pre>
</div><p>This is the equivalent source using asyncblock and fibers:</p>
<div><pre><code><span>var</span> <span>asyncblock</span> <span>=</span> <span>require</span><span>(</span><span>&#39;asyncblock&#39;</span><span>);</span>

<span>asyncblock</span><span>(</span><span>function</span><span>(</span><span>flow</span><span>){</span>
    <span>fs</span><span>.</span><span>readFile</span><span>(</span><span>&#39;path1&#39;</span><span>,</span> <span>&#39;utf8&#39;</span><span>,</span> <span>flow</span><span>.</span><span>add</span><span>(</span><span>&#39;first&#39;</span><span>));</span>
    <span>fs</span><span>.</span><span>readFile</span><span>(</span><span>&#39;path2&#39;</span><span>,</span> <span>&#39;utf8&#39;</span><span>,</span> <span>flow</span><span>.</span><span>add</span><span>(</span><span>&#39;second&#39;</span><span>));</span>

    <span>var</span> <span>fileContents</span> <span>=</span> <span>flow</span><span>.</span><span>wait</span><span>();</span>

    <span>fs</span><span>.</span><span>writeFile</span><span>(</span><span>&#39;path3&#39;</span><span>,</span> <span>fileContents</span><span>.</span><span>first</span><span>,</span> <span>flow</span><span>.</span><span>add</span><span>());</span>
    <span>flow</span><span>.</span><span>wait</span><span>();</span>

    <span>fs</span><span>.</span><span>readFile</span><span>(</span><span>&#39;path3&#39;</span><span>,</span> <span>&#39;utf8&#39;</span><span>,</span> <span>flow</span><span>.</span><span>add</span><span>());</span>
    <span>var</span> <span>data</span> <span>=</span> <span>flow</span><span>.</span><span>wait</span><span>();</span>

    <span>console</span><span>.</span><span>log</span><span>(</span><span>data</span><span>);</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;all done&#39;</span><span>);</span>
<span>});</span>
</code></pre>
</div><p>Error handling is addressed by including <code>flow.errorCallback</code>.  There are examples of how to use this in the project’s documentation.</p>
<h3>Introduction to Node Modules</h3>
<p>Robert Kuzelj has been writing a series of posts about Node modules:</p>
<ul>
	<li><a href="http://robkuz-blog.blogspot.com/2011/12/introduction-into-node-modules-part-i.html">Introduction to Node Modules Part 1</a></li>
	<li><a href="http://robkuz-blog.blogspot.com/2011/12/npm-modules-step-by-step-part-ii.html">Introduction to Node Modules Part 2</a></li>
	<li><a href="http://robkuz-blog.blogspot.com/2011/12/intro-into-node-modules-part-iii-cli.html">Introduction to Node Modules Part 3</a></li>
</ul>
<p>He demonstrates how to make packages, and how to take advantage of npm’s major features.</p>
<h3>Review19</h3>
<p><img src="http://dailyjs.com/images/posts/review19.png" alt=""></p>
<p>In other Node-related DailyJS articles I’ve invited readers to share their own apps with the community.  <a href="http://review19.com/">Review19</a> by <a href="http://srirangan.net/about">Srirangan</a> is one such project: a kanban and scrum project management tool.</p>
<p>It’s built with <a href="http://socket.io/">Socket.IO</a> to provide a real-time view of a project, and uses simple agile-inspired records that should be familiar to users of Jira or Pivotal Tracker.</p>
<p>Sign up is free, so give it a try.  The author has a Google Group to support the community at <a href="https://groups.google.com/forum/#!forum/review19-community">Review19 Community</a>.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/f9uQWc0uPY8" height="1" width="1">