---
layout: post
title:  "ECMAScript 6 and Block Scope"
date:   2013-05-24 12:16:20
author: Ariya Hidayat
categories: program
---

## ECMAScript 6 and Block Scope
### by Ariya Hidayat
### at 2013-05-24 12:16:20
### original <http://ariya.ofilabs.com/2013/05/es6-and-block-scope.html>

<p>Until today, JavaScript comes with a function-level scope for variables and functions. This quirk often trips beginners who are already familiar with other curly braces language. With ECMAScript 6, the situation will change with the availability of the well-understood <strong>block scope</strong>.</p>
<p><a href="http://dailyjs.com/2012/07/23/js101-scope/">Function-level scope</a> leads to a situation called <a href="http://net.tutsplus.com/tutorials/javascript-ajax/quick-tip-javascript-hoisting-explained/">hoisting</a>. For example, for this code:</p>

<div><table><tr><td><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">function</span> f<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
  doSomething<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
  <span style="color:#000066;font-weight:bold">var</span> a <span style="color:#339933">=</span> <span style="color:#cc0000">1</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></td></tr></table></div>

<p>what <a href="http://www.2ality.com/2013/05/quirk-variable-scope.html">really happens</a> is something like:</p>

<div><table><tr><td><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">function</span> f<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
  <span style="color:#000066;font-weight:bold">var</span> a<span style="color:#339933">;</span>
  doSomething<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
  a <span style="color:#339933">=</span> <span style="color:#cc0000">1</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></td></tr></table></div>

<p>Often, the knowledge about hoisting (or <a href="http://dmitrysoshnikov.com/ecmascript/es5-chapter-3-2-lexical-environments-ecmascript-implementation">lexical environment</a> in general) is used in a quiz or an interview question. In the following fragment, those who don’t possess the understanding will be left puzzled:</p>

<div><table><tr><td><pre style="font-family:monospace">console.<span style="color:#660066">log</span><span style="color:#009900">(</span><span style="color:#3366cc">'foo'</span> <span style="color:#000066;font-weight:bold">in</span> window<span style="color:#009900">)</span><span style="color:#339933">;</span> <span style="color:#006600;font-style:italic">// true</span>
<span style="color:#000066;font-weight:bold">var</span> foo<span style="color:#339933">;</span></pre></td></tr></table></div>

<p>For programmers familiar with C/C++/Java, the use of <code>var</code> in a wrong place can trigger <a href="http://www.2ality.com/2011/02/javascript-variable-scoping-and-its.html">many pitfalls</a>, among others <em>variable leaking</em>. Whether this is intentional or not is often not cleared from the code itself. Ambiguity like that may lead to a bug and other hard-to-trace annoyances:</p>

<div><table><tr><td><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">var</span> i <span style="color:#339933">=</span> <span style="color:#cc0000">0</span><span style="color:#339933">;</span> i <span style="color:#339933">&lt;</span> <span style="color:#cc0000">3</span><span style="color:#339933">;</span> i<span style="color:#339933">++</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
  <span style="color:#000066;font-weight:bold">var</span> j <span style="color:#339933">=</span> i <span style="color:#339933">*</span> i<span style="color:#339933">;</span>
  console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span>
console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#009900">)</span><span style="color:#339933">;</span> <span style="color:#006600;font-style:italic">//  4</span></pre></td></tr></table></div>

<p>With the upcoming ECMAScript 6, we can solve the issue by using <strong>Let and Const Declarations</strong>, see <a href="http://teramako.github.io/ECMAScript/ecma6th_syntax.html#12.2.1">Section 12.2.1</a> of the latest <a href="http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts">specification draft</a>. If we rewrite the above example to use <code>let</code>, it will look like the following:</p>


<div><table><tr><td><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">var</span> i <span style="color:#339933">=</span> <span style="color:#cc0000">0</span><span style="color:#339933">;</span> i <span style="color:#339933">&lt;</span> <span style="color:#cc0000">3</span><span style="color:#339933">;</span> i<span style="color:#339933">++</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
  let j <span style="color:#339933">=</span> i <span style="color:#339933">*</span> i<span style="color:#339933">;</span>
  console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span>
console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#009900">)</span><span style="color:#339933">;</span> <span style="color:#006600;font-style:italic">// will throw</span></pre></td></tr></table></div>

<p>then running it will give an error instead:</p>
<blockquote><p>ReferenceError: j is not defined</p></blockquote>
<p>This is the typical programmer's expectation, <code>j</code> is confined to that particular curly-braced block.</p>
<p>Its partner-in-crime, <code>const</code>, behaves pretty much the same except it must be <em>initialized</em> and only <em>once</em>. This is perfect to store an immutable object. With both <code>let</code> and <code>const</code>, an optimistic static code analyzer can work much smarter to detect patterns which may cause problems and warn the user ahead of time.</p>
<p>When can we can start using <code>let</code> and <code>const</code>? Fortunately, we can already use it <a href="http://net.tutsplus.com/articles/news/ecmascript-6-today/">today</a>. Firefox already has <a href="https://developer.mozilla.org/en-US/docs/JavaScript/ECMAScript_6_support_in_Mozilla">implemented</a> some supports for ECMAScript 6, including this block scope. With Chrome, you can enable V8 <a href="http://erik.eae.net/archives/2011/12/29/00.00.29/">experimental features</a> by toggling the switch via <code>chrome://flags</code>.</p>
<p>Note: with V8, at least for the time being, you can use let with strict mode only, otherwise it will complain <em>SyntaxError: Illegal let declaration outside extended mode</em>.</p>
<p>What about other browsers? Until they start supporting this block scope feature, you need to fall back to the solution of <em>converting</em> the code (also known as <strong>transpiling</strong>) into some construct which can be executed by today's browsers. Using a generic transpiler such as <a href="https://code.google.com/p/traceur-compiler/">Google Traceur</a> is often the recommended way.</p>
<p>An alternative solution is by using <a href="https://github.com/olov/defs">defs.js</a> from <a href="http://blog.lassus.se">Olov Lassus</a>. The idea is to transform block scoped declarations into normal variable statements, obviously taking into account the scope of each declaration. Given the code, defs.js will use <a href="http://esprima.org">Esprima</a> to parse the code, walk the syntax tree, and apply the transformation whenever necessary. As an example, this code fragment:</p>


<div><table><tr><td><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">function</span> f<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
  let j <span style="color:#339933">=</span> data.<span style="color:#660066">length</span><span style="color:#339933">;</span>
  console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#339933">,</span> <span style="color:#3366cc">'items'</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
  <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span>let i <span style="color:#339933">=</span> <span style="color:#cc0000">0</span><span style="color:#339933">;</span> i <span style="color:#339933">&lt;</span> j<span style="color:#339933">;</span> <span style="color:#339933">++</span>i<span style="color:#009900">)</span> <span style="color:#009900">{</span>
    let j <span style="color:#339933">=</span> data<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">*</span> data<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
    console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#009900">)</span><span style="color:#339933">;</span> <span style="color:#006600;font-style:italic">// squares</span>
  <span style="color:#009900">}</span>
<span style="color:#009900">}</span></pre></td></tr></table></div>

<p>will be transformed into:</p>


<div><table><tr><td><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">function</span> f<span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
  <span style="color:#000066;font-weight:bold">var</span> j <span style="color:#339933">=</span> data.<span style="color:#660066">length</span><span style="color:#339933">;</span>
  console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j<span style="color:#339933">,</span> <span style="color:#3366cc">'items'</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
  <span style="color:#000066;font-weight:bold">for</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">var</span> i <span style="color:#339933">=</span> <span style="color:#cc0000">0</span><span style="color:#339933">;</span> i <span style="color:#339933">&lt;</span> j<span style="color:#339933">;</span> <span style="color:#339933">++</span>i<span style="color:#009900">)</span> <span style="color:#009900">{</span>
    <span style="color:#000066;font-weight:bold">var</span> j$0 <span style="color:#339933">=</span> data<span style="color:#009900">[</span>i<span style="color:#009900">]</span> <span style="color:#339933">*</span> data<span style="color:#009900">[</span>i<span style="color:#009900">]</span><span style="color:#339933">;</span>
    console.<span style="color:#660066">log</span><span style="color:#009900">(</span>j$0<span style="color:#009900">)</span><span style="color:#339933">;</span> <span style="color:#006600;font-style:italic">// squares</span>
  <span style="color:#009900">}</span>
<span style="color:#009900">}</span></pre></td></tr></table></div>

<p>Look how defs.js recognizes the right scope for <code>j</code> and therefore masquerade the innermost <code>j</code> with another name, <code>j$0</code>. The transformation itself is <strong>non-destructive</strong>, defs.js does not bother with anything other than <code>let</code> and <code>const</code> declaration. You can see how a comment is left untouched and the coding style is still exactly the same.</p>
<p>Obviously, there's much more to defs.js than my simple example above, refer to Olov's <a href="http://blog.lassus.se/2013/05/defsjs.html">blog post</a> for more details.</p>
<p>How do you plan to use <code>let</code> and <code>const</code>?</p>