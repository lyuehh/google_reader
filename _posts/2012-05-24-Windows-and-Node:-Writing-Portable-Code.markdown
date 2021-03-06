---
layout: post
title:  "Windows and Node: Writing Portable Code"
date:   2012-05-24 07:00:00
author: 
categories: program
---

## Windows and Node: Writing Portable Code
### by 
### at 2012-05-24 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/vybA2RF6F1o/windows-and-node-4>

<p>I’ve been surveying popular Node modules and the <a href="https://groups.google.com/forum/?fromgroups#!forum/nodejs">nodejs Google Group</a> to find common portability issues people have found when testing modules in Windows.</p>

<p>For the most part, Node code seems very portable – there are only a few problem areas that seem to crop up frequently. Let’s take a look at these problems and the solutions so we can write code that runs everywhere.</p>

<h3>Platform-Specific Code</h3>

<p>Despite Node code’s inherent portability, there are times when platform-specific code is required. This is dealt with in Node’s core modules like this:</p>
<div><pre><code><span>var</span> <span>isWindows</span> <span>=</span> <span>process</span><span>.</span><span>platform</span> <span>===</span> <span>&#39;win32&#39;</span><span>;</span>

<span>if</span> <span>(</span><span>isWindows</span><span>)</span> <span>{</span>
  <span>// Windows-specific code</span>
<span>}</span>
</code></pre>
</div>
<p>This example is based on <a href="https://github.com/joyent/node/blob/master/lib/path.js">path.js</a>.</p>

<p>For more detailed information on the operating system, the <a href="http://nodejs.org/docs/latest/api/all.html#all_os">os</a> module can come in handy.</p>

<p><img alt="Nodes OS module in Windows" src="http://dailyjs.com/images/posts/win4/1_cmd_os.png"></p>

<h3>File System</h3>

<p>Windows can actually accept backslashes or forward slashes as a path separator. This means you don’t need to change all of your <code>require</code> calls to use different slashes; most things should just work. There are a few cases where we need to be careful, however, particularly if a path name is absolute or it’s going to be displayed somewhere.</p>

<p>One common issue I’ve found is where the developer has made certain assumptions about the structure of absolute paths. In a commit to Express, <a href="https://github.com/visionmedia/express/commit/cbf330c3db48e2a3e93c34a2e1e32c56a31bea7a">Fixed absolute path checking on windows</a>, we can see where the authors have adapted a method called <code>isAbsolute</code> to support Windows:</p>
<div><pre><code><span>exports</span><span>.</span><span>isAbsolute</span> <span>=</span> <span>function</span><span>(</span><span>path</span><span>){</span>
  <span>if</span> <span>(</span><span>&#39;/&#39;</span> <span>==</span> <span>path</span><span>[</span><span>0</span><span>])</span> <span>return</span> <span>true</span><span>;</span>
  <span>if</span> <span>(</span><span>&#39;:&#39;</span> <span>==</span> <span>path</span><span>[</span><span>1</span><span>]</span> <span>&amp;&amp;</span> <span>&#39;\\&#39;</span> <span>==</span> <span>path</span><span>[</span><span>2</span><span>])</span> <span>return</span> <span>true</span><span>;</span>
<span>};</span>
</code></pre>
</div>
<p>Isaac Schlueter <a href="http://dailyjs.com/%5BBest">recommends</a> using <a href="http://nodejs.org/docs/latest/api/all.html#all_path_resolve_from_to">path.resolve</a> to make relative paths absolute in a cross-platform way.</p>

<p>When dealing with fragments of paths, using <code>path.join</code> will automatically insert the correct slash based on platform. For example, the <a href="https://github.com/joyent/node/blob/master/lib/path.js#L195">Windows version</a> will insert backslashes:</p>
<div><pre><code><span>var</span> <span>joined</span> <span>=</span> <span>paths</span><span>.</span><span>join</span><span>(</span><span>&#39;\\&#39;</span><span>);</span>
</code></pre>
</div>
<p>Notice that JavaScript strings require two backslashes because one acts as an escape, so when working with Windows path names don’t be surprised if there are lot of double slashes.</p>

<p>Another big source of Windows issues is <a href="http://nodejs.org/docs/latest/api/all.html#all_fs_watch_filename_options_listener">fs.watch</a>. This module is routinely used by programs that watch for file system changes. Node’s documentation makes it clear that the API isn’t portable, so the slower but more compatible <a href="http://nodejs.org/docs/latest/api/all.html#all_fs_watchfile_filename_options_listener">fs.watchFile</a> can be used instead.</p>

<p>In <a href="https://github.com/cjblomqvist/derby/commit/deeb28b68d02366844ef156d694dc95379f80258">this patch for the Derby web framework</a>, we can see where the developers opted to branch based on <code>process.platform</code> to use <code>fs.watchFile</code> in Windows, but <code>fs.watch</code> elsewhere.</p>

<h3>Text Interfaces</h3>

<p>Be aware that not everybody has a super-fancy UTF-8 terminal that supports colours. Certain programs depend on text output, but people may have trouble seeing it correctly if your program relies on symbols their terminal or font doesn’t support.</p>

<p>Mocha is a good example of such a program, and in the issue <a href="https://github.com/visionmedia/mocha/issues/294">Ability to configure passed/failed checkmarks for the spec reporter</a>, we can see where someone has struggled to read the output with <code>cmd.exe</code>.</p>

<h3>Environment</h3>

<p>Assuming certain environmental variables will exist (or mean the same thing) on every platform is a good way to create portability headaches.</p>

<p>James Halliday’s <a href="https://github.com/substack/node-browserify">Browserify</a> had its fair share of Windows issues, which was problematic due to several other popular modules depending on it.</p>

<p><a href="https://github.com/bennage/node-browserify/commit/9dd2fae0f9647463b6e9bfef713c8741f3cdecff">This commit</a> to Browserify demonstrates a fix Christopher Bennage submitted that replaces calls to <code>process.env.HOME</code> with the following:</p>
<div><pre><code><span>var</span> <span>home</span> <span>=</span> <span>(</span><span>process</span><span>.</span><span>env</span><span>.</span><span>HOME</span> <span>||</span> <span>process</span><span>.</span><span>env</span><span>.</span><span>USERPROFILE</span><span>);</span>
</code></pre>
</div>
<p>I tried this in Windows 7 and found <code>process.env.HOME</code> wasn’t set, but <code>process.env.USERPROFILE</code> worked as expected.</p>

<h3>Sockets</h3>

<p>Node’s TCP sockets are portable, but Unix domain sockets are not. However, Windows has <a href="http://en.wikipedia.org/wiki/Named_pipe">named pipes</a>. The following code is almost exactly the same as the Unix equivalent, it just has a different path to the named pipe:</p>
<div><pre><code><span>var</span> <span>net</span> <span>=</span> <span>require</span><span>(</span><span>&#39;net&#39;</span><span>);</span>

<span>net</span><span>.</span><span>createServer</span><span>(</span><span>function</span><span>(</span><span>socket</span><span>)</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;Connected&#39;</span><span>);</span>
<span>}).</span><span>listen</span><span>(</span><span>&#39;\\\\.\\pipe\\named-pipe-test&#39;</span><span>);</span>
</code></pre>
</div>
<p>Notice the escaped backslashes – forgetting to insert them here will raise a confusing <code>EACCESS</code> error. In <a href="https://github.com/joyent/node/blob/master/test/common.js">node/test/common.js</a>, there’s a branch based on platform to set the name of the pipe so it works in Windows and Unix:</p>
<div><pre><code><span>if</span> <span>(</span><span>process</span><span>.</span><span>platform</span> <span>===</span> <span>&#39;win32&#39;</span><span>)</span> <span>{</span>
  <span>exports</span><span>.</span><span>PIPE</span> <span>=</span> <span>&#39;\\\\.\\pipe\\libuv-test&#39;</span><span>;</span>
<span>}</span> <span>else</span> <span>{</span>
  <span>exports</span><span>.</span><span>PIPE</span> <span>=</span> <span>exports</span><span>.</span><span>tmpDir</span> <span>+</span> <span>&#39;/test.sock&#39;</span><span>;</span>
<span>}</span>
</code></pre>
</div>
<h3>References</h3><img src="http://feeds.feedburner.com/~r/dailyjs/~4/vybA2RF6F1o" height="1" width="1">