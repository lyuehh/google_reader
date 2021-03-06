---
layout: post
title:  "Unix and Node: Interfaces"
date:   2012-04-05 07:00:00
author: 
categories: program
---

## Unix and Node: Interfaces
### by 
### at 2012-04-05 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/8DQwxBbqODw/node-unix-interfaces>

<p>Earlier in this series I covered <a href="http://dailyjs.com/2012/03/01/unix-node-arguments/">command-line arguments</a>, which are well-supported in Node. There are times when a more interactive interface is required, however.</p>

<p>Fortunately, various node modules give us the tools to create command-line Unix programs with many different console-based interfaces, from Read-Eval-Print-Loops to GUI-like terminal control libraries.</p>

<h3>REPL</h3>

<p>Node’s Read-Eval-Print-Loop (REPL) is available as a module, and can be used to create interactive JavaScript shells. <a href="http://nodejs.org/docs/latest/api/all.html#all_repl">Node’s documentation</a> has a cool example that uses a TCP server, so clients can connect with <code>telnet</code>.</p>

<p>The documentation is currently slightly inaccurate with regard to <code>REPL.start</code> – the callback method actually takes four arguments and won’t work as advertised. This example should work with the current version of Node 0.6:</p>
<div><pre><code><span>var</span> <span>repl</span> <span>=</span> <span>require</span><span>(</span><span>&#39;repl&#39;</span><span>)</span>
  <span>,</span> <span>vm</span> <span>=</span> <span>require</span><span>(</span><span>&#39;vm&#39;</span><span>);</span>

<span>repl</span><span>.</span><span>start</span><span>(</span><span>&#39;&gt; &#39;</span><span>,</span> <span>process</span><span>,</span> <span>function</span><span>(</span><span>code</span><span>,</span> <span>context</span><span>,</span> <span>file</span><span>,</span> <span>callback</span><span>)</span> <span>{</span>
  <span>var</span> <span>result</span>
    <span>,</span> <span>err</span><span>;</span>

  <span>try</span> <span>{</span>
    <span>result</span> <span>=</span> <span>vm</span><span>.</span><span>runInThisContext</span><span>(</span><span>code</span><span>,</span> <span>file</span><span>);</span>
  <span>}</span> <span>catch</span> <span>(</span><span>err</span><span>)</span> <span>{</span>
    <span>console</span><span>.</span><span>error</span><span>(</span><span>&#39;Error:&#39;</span><span>,</span> <span>err</span><span>);</span>
  <span>}</span>
  <span>callback</span><span>(</span><span>err</span><span>,</span> <span>result</span><span>);</span>
<span>});</span>
</code></pre>
</div>
<p>The <code>process</code> global is passed as the <code>stream</code> argument to make the REPL read and write to <code>stdin</code> and <code>stdout</code>. The callback method can do anything that’s required. For example, you could allow access to your database objects and methods in a web application, or provide an interactive administration interface to a daemon.</p>

<h3>Readline</h3>

<p>The <code>repl</code> module works well when a JavaScript shell is required, but what about a custom REPL? Node actually includes a Readline module, which is perfect for this:</p>
<div><pre><code><span>var</span> <span>readline</span> <span>=</span> <span>require</span><span>(</span><span>&#39;readline&#39;</span><span>)</span>
  <span>,</span> <span>rl</span><span>;</span>

<span>rl</span> <span>=</span> <span>readline</span><span>.</span><span>createInterface</span><span>(</span><span>process</span><span>.</span><span>stdin</span><span>,</span> <span>process</span><span>.</span><span>stdout</span><span>,</span> <span>null</span><span>);</span>

<span>rl</span><span>.</span><span>setPrompt</span><span>(</span><span>&#39;➜&#39;</span><span>);</span>

<span>rl</span><span>.</span><span>on</span><span>(</span><span>&#39;line&#39;</span><span>,</span> <span>function</span><span>(</span><span>cmd</span><span>)</span> <span>{</span>
  <span>if</span> <span>(</span><span>cmd</span> <span>===</span> <span>&#39;quit&#39;</span><span>)</span> <span>{</span>
    <span>rl</span><span>.</span><span>question</span><span>(</span><span>&#39;Are you sure? (y/n) &#39;</span><span>,</span> <span>function</span><span>(</span><span>answer</span><span>)</span> <span>{</span>
      <span>if</span> <span>(</span><span>answer</span> <span>===</span> <span>&#39;y&#39;</span><span>)</span> <span>{</span>
        <span>rl</span><span>.</span><span>close</span><span>();</span>
      <span>}</span> <span>else</span> <span>{</span>
        <span>rl</span><span>.</span><span>prompt</span><span>();</span>
      <span>}</span>
    <span>});</span>
  <span>}</span> <span>else</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;You typed:&#39;</span><span>,</span> <span>cmd</span><span>);</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;Type &quot;quit&quot; to exit&#39;</span><span>);</span>
  <span>}</span>

  <span>rl</span><span>.</span><span>prompt</span><span>();</span>
<span>});</span>

<span>rl</span><span>.</span><span>on</span><span>(</span><span>&#39;close&#39;</span><span>,</span> <span>function</span><span>()</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;Bye&#39;</span><span>);</span>
  <span>process</span><span>.</span><span>exit</span><span>();</span>
<span>});</span>

<span>rl</span><span>.</span><span>prompt</span><span>();</span>
</code></pre>
</div>
<p>Here I’ve used the <code>readline</code> module to create an interface, then listen for <code>line</code> events which denote a line of text was typed. The <code>question</code> method will display a prompt and invoke the callback with the response.</p>

<p>By using simple string matching, a completely customised command-line interface can be created. The <code>readline</code> module also has some useful built-in features like command history.</p>

<h3>ncurses</h3>

<p>The <a href="https://github.com/mscdex/node-ncurses">ncurses</a> module by Brian White provides bindings to the ncurses library. This is a popular method for creating text-based user interfaces. If your application needs things like windows, menus, and more elaborate widgets such as a calendar, then ncurses is a good solution.</p>

<p>These bindings require a level of familiarisation with the original ncurses API. One freely available resource for learning ncurses is the <a href="http://tldp.org/HOWTO/NCURSES-Programming-HOWTO/">NCURSES Programming HOWTO</a> – combined with the <a href="https://github.com/mscdex/node-ncurses">ncurses-node README</a> it’s possible to work out how to apply these techniques to a Node project.</p>

<p>Brian has also written some reusable widgets that come with the node-ncurses module:</p>
<div><pre><code><span>var</span> <span>ncurses</span> <span>=</span> <span>require</span><span>(</span><span>&#39;ncurses&#39;</span><span>)</span>
  <span>,</span> <span>widgets</span> <span>=</span> <span>require</span><span>(</span><span>&#39;ncurses/lib/widgets&#39;</span><span>)</span>
  <span>,</span> <span>win</span> <span>=</span> <span>new</span> <span>ncurses</span><span>.</span><span>Window</span><span>();</span>

<span>widgets</span><span>.</span><span>InputBox</span><span>(</span><span>&#39;Enter your name:&#39;</span><span>,</span> <span>{</span>
    <span>pos</span><span>:</span> <span>&#39;center&#39;</span><span>,</span>
    <span>style</span><span>:</span> <span>{</span>
      <span>colors</span><span>:</span> <span>{</span>
        <span>bg</span><span>:</span> <span>&#39;blue&#39;</span><span>,</span>
        <span>input</span><span>:</span> <span>{</span>
          <span>fg</span><span>:</span> <span>&#39;red&#39;</span><span>,</span>
          <span>bg</span><span>:</span> <span>&#39;black&#39;</span>
        <span>}</span>
      <span>}</span>
    <span>}</span>
  <span>},</span> <span>function</span><span>(</span><span>input</span><span>)</span> <span>{</span>
    <span>if</span> <span>(</span><span>!</span><span>input</span><span>)</span> <span>{</span>
      <span>input</span> <span>=</span> <span>&#39;nothing&#39;</span><span>;</span>
    <span>}</span>
    <span>win</span><span>.</span><span>centertext</span><span>(</span><span>0</span><span>,</span> <span>&#39;You entered: &#39;</span> <span>+</span> <span>input</span><span>);</span>
    <span>win</span><span>.</span><span>refresh</span><span>();</span>
    <span>setTimeout</span><span>(</span><span>function</span><span>()</span> <span>{</span> <span>win</span><span>.</span><span>close</span><span>();</span> <span>},</span> <span>1000</span><span>);</span>
<span>});</span>
</code></pre>
</div>
<p>I’ve adapted this example from Brian’s code – it should work if you install the relevant module with <code>npm install ncurses</code>. The result looks like this:</p>

<p><img src="http://dailyjs.com/images/posts/node-ncurses.png" alt="node-ncurses screenshot"></p>

<h3>Alternatives</h3>

<p>There are simpler alternatives to ncurses. Libraries we’ve covered before, like <a href="http://visionmedia.github.com/commander.js/">Commander.js</a> have prompts and dialogs. Then there’s <a href="https://github.com/TooTallNate/ansi.js">ansi.js</a> (License: <em>MIT</em>, npm: <em>ansi</em>) which makes working with 256 ANSI colours relatively painless, particularly for web developers who are familiar with hex colours:</p>
<div><pre><code><span>cursor</span><span>.</span><span>hex</span><span>(</span><span>&#39;#660000&#39;</span><span>).</span><span>bold</span><span>().</span><span>underline</span><span>();</span>
</code></pre>
</div>
<p><a href="https://github.com/jocafa/node-term-ui">TermUI</a> (npm: <em>node-term-ui</em>) by Josh Faul has a chainable event-based API that can move the cursor around and output text with various colours:</p>
<div><pre><code><span>TermUI</span>
  <span>.</span><span>pos</span><span>(</span><span>10</span><span>,</span><span>20</span><span>)</span>
  <span>.</span><span>fg</span><span>(</span><span>TermUI</span><span>.</span><span>C</span><span>.</span><span>w</span><span>)</span>
  <span>.</span><span>bg</span><span>(</span><span>TermUI</span><span>.</span><span>C</span><span>.</span><span>w</span><span>)</span>
  <span>.</span><span>out</span><span>(</span><span>&#39;Hello, world!&#39;</span><span>);</span>
</code></pre>
</div>
<p>There’s even a tab completion module: <a href="https://github.com/hij1nx/complete">complete</a> (License: <em>MIT</em>, npm: <em>complete</em>) by hij1nx. It’s actually designed to work with bash completion, and will write to <code>.bashrc</code> or <code>.bash_profile</code>:</p>
<div><pre><code><span># Node Completion - Auto-generated, do not touch.</span>
<span>shopt</span> -s progcomp
<span>for </span>f in <span>$(</span><span>command </span>ls ~/.node-completion<span>)</span>; <span>do</span>
<span>  </span><span>f</span><span>=</span><span>&quot;$HOME/.node-completion/$f&quot;</span>
  <span>test</span> -f <span>&quot;$f&quot;</span> <span>&amp;&amp;</span> . <span>&quot;$f&quot;</span>
<span>done</span>
</code></pre>
</div>
<p>There are dozens more interesting command-line UI libraries out there. If you’ve written something that you’d like us to feature in a Node Roundup post, then please <a href="http://dailyjs.com/contact.html">get in touch</a>!</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/8DQwxBbqODw" height="1" width="1">