---
layout: post
title:  "AngularJS: Adding Dependencies"
date:   2013-05-30 07:00:00
author: 
categories: program
---

## AngularJS: Adding Dependencies
### by 
### at 2013-05-30 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/jjiDymnwJ5U/angularjs-6>

<ul>
  <li><a href="http://dailyjs.com/2013/04/11/angularjs-1/">Part 1: Google, Twitter, and AngularJS</a></li>
  <li><a href="http://dailyjs.com/2013/04/18/angularjs-2/">Part 2: Let's Make a Feed Reader</a></li>
  <li><a href="http://dailyjs.com/2013/04/25/angularjs-3/">Part 3: Rendering Feeds</a></li>
  <li><a href="http://dailyjs.com/2013/05/09/angularjs-4/">Part 4: Managing Feeds</a></li>
  <li><a href="http://dailyjs.com/2013/05/16/angularjs-5/">Part 5: Tests</a></li>
  <li><a href="http://dailyjs.com/2013/05/30/angularjs-6/"><strong>Part 6: Adding Dependencies</strong></a></li>
  <li><a href="http://dailyjs.com/2013/06/06/angularjs-7/">Part 7: Form Validation</a></li>
</ul>
<h3>Adding Dependencies with Bower</h3>

<p>This tutorial is really about <a href="http://yeoman.io/">Yeoman</a>, Bower, and Grunt, because I still feel like it’s worth exploring the build system that I introduced for this AngularJS project. I appreciate that the number of files installed by Yeoman is a little bit bewildering, so we’re going to take a step back from AngularJS and look at how dependencies work and how to add new dependencies to a project.</p>

<p>Although Yeoman helps get a new project off the ground, it takes a fair amount of digging to figure out how everything is laid out. For example: let’s say we want to add <a href="https://github.com/jlong/sass-twitter-bootstrap.git">sass-bootstrap</a> to <a href="https://github.com/alexyoung/djsreader">djsreader</a> – how exactly do we do this?</p>

<p>Yeoman uses <a href="http://bower.io/">Bower</a> for managing dependencies, and Bower uses <code>component.json</code> (or <code>bower.json</code> by default in newer versions). To add <code>sass-bootstrap</code> to the project, open <code>component.json</code> and add <code>&quot;sass-bootstrap&quot;: &quot;2.3.x&quot;</code> to the <code>dependencies</code> property:</p>
<div><pre><code><span>{</span>
  <span>&quot;name&quot;</span><span>:</span> <span>&quot;djsreader&quot;</span><span>,</span>
  <span>&quot;version&quot;</span><span>:</span> <span>&quot;0.0.0&quot;</span><span>,</span>
  <span>&quot;dependencies&quot;</span><span>:</span> <span>{</span>
    <span>&quot;angular&quot;</span><span>:</span> <span>&quot;~1.0.5&quot;</span><span>,</span>
    <span>&quot;json3&quot;</span><span>:</span> <span>&quot;~3.2.4&quot;</span><span>,</span>
    <span>&quot;es5-shim&quot;</span><span>:</span> <span>&quot;~2.0.8&quot;</span><span>,</span>
    <span>&quot;angular-resource&quot;</span><span>:</span> <span>&quot;~1.0.5&quot;</span><span>,</span>
    <span>&quot;angular-cookies&quot;</span><span>:</span> <span>&quot;~1.0.5&quot;</span><span>,</span>
    <span>&quot;angular-sanitize&quot;</span><span>:</span> <span>&quot;~1.0.5&quot;</span><span>,</span>
    <span>&quot;sass-bootstrap&quot;</span><span>:</span> <span>&quot;2.3.x&quot;</span>
  <span>},</span>
  <span>&quot;devDependencies&quot;</span><span>:</span> <span>{</span>
    <span>&quot;angular-mocks&quot;</span><span>:</span> <span>&quot;~1.0.5&quot;</span><span>,</span>
    <span>&quot;angular-scenario&quot;</span><span>:</span> <span>&quot;~1.0.5&quot;</span>
  <span>}</span>
<span>}</span>
</code></pre>
</div>
<p>Next run <code>bower install</code> to install the dependencies to <code>app/components</code>. If you look inside <code>app/components</code> you should see <code>sass-bootstrap</code> in there.</p>

<p>Now the package is installed, how do we actually use it with our project? The easiest way is to create a suitable Grunt task.</p>

<h3>Grunt</h3>

<p><a href="http://gruntjs.com/">Grunt</a> runs the djsreader development server and compiles production builds that can be dropped onto a web server. <code>Gruntfile.js</code> is mostly configuration – it has the various settings needed to drive Grunt tasks so they can build our project. One task is <code>compass</code> – if you search the file for <code>compass</code> you should see a property that defines some options for compiling Sass files.</p>

<p>The convention for Grunt task configuration is <code>taskName: { argument: options }</code>. We want to add a new argument to the <code>compass</code> task for building the Bootstrap Sass files. We know the files are in <code>app/components/sass-bootstrap</code>, so we just need to tell it to compile the files in there.</p>

<p>Add a new property to <code>compass</code> called <code>bootstrap</code>. It should be on line 143:</p>
<div><pre><code><span>compass</span><span>:</span> <span>{</span>
  <span>// options/dist/server</span>
  <span>bootstrap</span><span>:</span> <span>{</span>
    <span>options</span><span>:</span> <span>{</span>
      <span>sassDir</span><span>:</span> <span>&#39;&lt;%= yeoman.app %&gt;/components/sass-bootstrap/lib&#39;</span><span>,</span>
      <span>cssDir</span><span>:</span> <span>&#39;.tmp/styles&#39;</span>
    <span>}</span>
  <span>}</span>
<span>}</span>
</code></pre>
</div>
<p>Near the bottom of the file add an entry for <code>compass:bootstrap</code> to <code>grunt.registerTask(&#39;server&#39;, [</code> and <code>grunt.registerTask(&#39;build&#39;, [</code>:</p>
<div><pre><code><span>grunt</span><span>.</span><span>registerTask</span><span>(</span><span>&#39;server&#39;</span><span>,</span> <span>[</span>
  <span>&#39;clean:server&#39;</span><span>,</span>
  <span>&#39;coffee:dist&#39;</span><span>,</span>
  <span>&#39;compass:server&#39;</span><span>,</span>
  <span>&#39;compass:bootstrap&#39;</span><span>,</span> <span>/* This one! */</span>
  <span>&#39;livereload-start&#39;</span><span>,</span>
  <span>&#39;connect:livereload&#39;</span><span>,</span>
  <span>&#39;open&#39;</span><span>,</span>
  <span>&#39;watch&#39;</span>
<span>]);</span>
</code></pre>
</div>
<p>This causes the Bootstrap <code>.scss</code> files to be compiled whenever a server is started.</p>

<p>Now open <code>app/index.html</code> and add <code>styles/bootstrap.css</code>:</p>
<div><pre><code><span>&lt;link</span> <span>rel=</span><span>&quot;stylesheet&quot;</span> <span>href=</span><span>&quot;styles/bootstrap.css&quot;</span><span>&gt;</span>
<span>&lt;link</span> <span>rel=</span><span>&quot;stylesheet&quot;</span> <span>href=</span><span>&quot;styles/main.css&quot;</span><span>&gt;</span>
</code></pre>
</div>
<h3>Conclusion</h3>

<p><img src="http://dailyjs.com/images/posts/angularboot.png" alt="Angular/Bootstrap"></p>

<p>The settings files Yeoman created for us makes managing dependencies easy – there’s a world of cool things you can find with <code>bower search</code> and try out.</p>

<p>This week’s code is in <a href="https://github.com/alexyoung/djsreader/tree/005d1be3ec20100ac5044cb2e05489c8a7b5fa8d">commit 005d1be</a>.</p>
   <img src="http://feeds.feedburner.com/~r/dailyjs/~4/jjiDymnwJ5U" height="1" width="1">