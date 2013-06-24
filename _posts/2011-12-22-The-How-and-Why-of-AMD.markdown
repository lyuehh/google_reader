---
layout: post
title:  "The How and Why of AMD"
date:   2011-12-22 08:00:00
author: 
categories: program
---

## The How and Why of AMD
### by 
### at 2011-12-22 08:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/-Njq5eb0ibc/framework>

<div>
<p><em>Let’s Make a Framework</em> is an ongoing series about building a JavaScript framework from the ground up.</p>
<p>These articles are tagged with <a href="http://dailyjs.com/tags.html#lmaf">lmaf</a>.  The project we’re creating is called <a href="http://github.com/alexyoung/turing.js">Turing</a>.  Documentation is available at <a href="http://turingjs.com/">turingjs.com</a>.</p>
</div>
<p>In <a href="http://dailyjs.com/2011/11/24/framework-88/">Improving Client-Side Modularity</a> I talked about better ways to load modules, and showed how jQuery supports the <a href="https://github.com/amdjs/amdjs-api/wiki/AMD" title="AMD">Asynchronous Module Definition</a> <span>API</span>.  As open source developers, supporting this specification makes it easier for others to reuse our code.  It also helps resource loading frameworks like <a href="http://requirejs.org/">RequireJS</a>.</p>
<p>In the <em>Let’s Make a Framework</em> series, we’ve already covered <a href="http://wiki.commonjs.org/wiki/Modules/1.1">Common JS Modules</a> and seen how they can make our code accessible for Node developers.  However, implementing a convincing client-side version of this is difficult because <code>require</code> is generally implemented as a synchronous statement.  There are projects to wrap this for browser-based development, but <span>AMD</span> offers a native solution that’s easy to work with.  This is where <span>AMD</span> steps in as a “transport format”.</p>
<h3>Conditional <span>AMD</span> Support</h3>
<p>jQuery provides conditional <span>AMD</span> support.  When writing smaller, tightly focused libraries, this is a useful approach because it allows <span>AMD</span> to be supported where available.  jQuery checks for the existence of <code>define</code>, and this approach can be reused:</p>
<div><pre><code><span>var</span> <span>myModule</span> <span>=</span> <span>{</span>
  <span>awesome</span><span>:</span> <span>&#39;not really&#39;</span>
<span>};</span>

<span>if</span> <span>(</span><span>typeof</span> <span>define</span> <span>===</span> <span>&#39;function&#39;</span> <span>&amp;&amp;</span> <span>define</span><span>.</span><span>amd</span><span>)</span> <span>{</span>
  <span>define</span><span>(</span><span>&#39;myModule&#39;</span><span>,</span> <span>[],</span> <span>function</span><span>()</span> <span>{</span>
    <span>return</span> <span>myModule</span><span>;</span>
  <span>});</span>
<span>}</span>
</code></pre>
</div><p>The <code>amd</code> property is explained by the <span>AMD</span> specification:</p>
<blockquote>
<p>To allow a clear indicator that a global define function (as needed for script src browser loading) conforms to the <span>AMD</span> <span>API</span>, any global define function <span>SHOULD</span> have a property called “amd” whose value is an object. This helps avoid conflict with any other existing JavaScript code that could have defined a define() function that does not conform to the <span>AMD</span> <span>API</span>.</p>
</blockquote>
<p>We need to check for both the <code>define</code> function, and that it conforms to the specification.</p>
<h3>Internal <span>AMD</span> Use</h3>
<p>Another approach is to build an entire project around <span>AMD</span>.  <a href="http://dojotoolkit.org/">Dojo</a> was restructured to work this way.  This comment is from Dojo’s source:</p>
<blockquote>
<p>This function defines an <span>AMD</span>-compliant loader that can be configured to operate in either synchronous or asynchronous modes.</p>
</blockquote>
<p>Then modules are wrapped with <code>define</code>:</p>
<div><pre><code><span>define</span><span>([</span><span>&quot;./_base/kernel&quot;</span><span>,</span> <span>&quot;./_base/lang&quot;</span><span>,</span> <span>&quot;./_base/Color&quot;</span><span>,</span> <span>&quot;./_base/array&quot;</span><span>],</span> <span>function</span><span>(</span><span>dojo</span><span>,</span> <span>lang</span><span>,</span> <span>Color</span><span>,</span> <span>ArrayUtil</span><span>)</span> <span>{</span>

<span>});</span>
</code></pre>
</div><p>Notice how dependencies in <span>AMD</span> are the same order as the callback (or “factory function”, as the <span>AMD</span> specification refers to it).  This is explained in the specification:</p>
<blockquote>
<p>[…] the resolved values should be passed as arguments to the factory function with argument positions corresponding to indexes in the dependencies array.</p>
</blockquote>
<p>As I mentioned in <em>Improving Client-Side Modularity</em>, Kris Zyp documented this move to <span>AMD</span> in <a href="http://dojotoolkit.org/features/1.6/async-modules">Asynchronous Modules Come to Dojo 1.6</a>.</p>
<h3>CommonJS Wrapping</h3>
<p>The <span>AMD</span> specification also addresses CommonJS modules with this example:</p>
<div><pre><code><span>define</span><span>(</span><span>function</span><span>(</span><span>require</span><span>,</span> <span>exports</span><span>,</span> <span>module</span><span>)</span> <span>{</span>
  <span>var</span> <span>a</span> <span>=</span> <span>require</span><span>(</span><span>&#39;a&#39;</span><span>),</span>
      <span>b</span> <span>=</span> <span>require</span><span>(</span><span>&#39;b&#39;</span><span>);</span>

  <span>exports</span><span>.</span><span>action</span> <span>=</span> <span>function</span><span>()</span> <span>{};</span>
<span>});</span>
</code></pre>
</div><p>The RequireJS documentation points out that this can help with cases where there are a lot of dependencies:</p>
<div><pre><code><span>define</span><span>([</span> <span>&#39;require&#39;</span><span>,</span> <span>&#39;jquery&#39;</span><span>,</span> <span>&#39;blade/object&#39;</span><span>,</span> <span>&#39;blade/fn&#39;</span><span>,</span> <span>&#39;rdapi&#39;</span><span>,</span>
         <span>&#39;oauth&#39;</span><span>,</span> <span>&#39;blade/jig&#39;</span><span>,</span> <span>&#39;blade/url&#39;</span><span>,</span> <span>&#39;dispatch&#39;</span><span>,</span> <span>&#39;accounts&#39;</span><span>,</span>
         <span>&#39;storage&#39;</span><span>,</span> <span>&#39;services&#39;</span><span>,</span> <span>&#39;widgets/AccountPanel&#39;</span><span>,</span> <span>&#39;widgets/TabButton&#39;</span><span>,</span>
         <span>&#39;widgets/AddAccount&#39;</span><span>,</span> <span>&#39;less&#39;</span><span>,</span> <span>&#39;osTheme&#39;</span><span>,</span> <span>&#39;jquery-ui-1.8.7.min&#39;</span><span>,</span>
         <span>&#39;jquery.textOverflow&#39;</span><span>],</span> <span>// ...</span>
</code></pre>
</div><p>This is difficult to read.  The RequireJS documentation goes on to say that using <code>require</code> is potentially easier to follow.  An <span>AMD</span> module loader has to parse out the <code>require</code> calls and insert the <code>define</code> dependencies transparently.</p>
<p>Since the authors of RequireJS have experience implementing such things, they’re all too aware of browser inconsistencies and limitations:</p>
<blockquote>
<p>Not all browsers give a usable Function.prototype.toString() results. As of October 2011, the PS 3 and older Opera Mobile browsers do not. Those browsers are more likely to need an optimized build of the modules for network/device limitations, so just do a build with an optimizer that knows how to convert these files to the normalized dependency array form, like the RequireJS optimizer.</p>
</blockquote>
<p>This means a pre-compilation step may be necessary to support a broad range of browsers using this approach.</p>
<h3>Conclusion</h3>
<p>If you’ve followed our previous tutorials on building an asynchronous script loader, you’ll know that writing JavaScript APIs for loading scripts isn’t trivial.  Supporting CommonJS isn’t necessarily difficult, but supporting features like parsing <code>require</code> calls is where things get tricky.</p>
<p>This leads us to an important point that must be considered when building our own libraries and frameworks: should we ship our own script loading solution, or conditionally support <span>AMD</span>?  Given the complexity of building a script loader, it may be better to conditionally support <span>AMD</span> as jQuery does.</p>
<p>This doesn’t quite satisfy our needs for Turing, however.  Turing does have one or two modules that are dependent on each other, and <code>define</code> provides a way to express this programatically.  It may be better to use <code>define</code> to wrap modules, as we already do using an <span>IIFE</span> (Immediately-Invoked Function Expression), and effectively no-op it if it isn’t present.</p>
<p>Next week I’ll look at adding <span>AMD</span> to Turing, then test it out with RequireJS.</p>
<h3>References</h3>
<ul>
	<li><a href="http://dojotoolkit.org/features/1.6/async-modules">Asynchronous Modules Come to Dojo 1.6</a></li>
	<li><a href="http://dailyjs.com/2011/11/24/framework-88/">Improving Client-Side Modularity</a></li>
	<li><a href="http://requirejs.org/docs/whyamd.html">RequireJS: Why <span>AMD</span>?</a></li>
	<li><a href="https://github.com/amdjs/amdjs-api/wiki/AMD"><span>AMD</span> Wiki</a></li>
	<li><a href="http://wiki.commonjs.org/wiki/Modules/1.1">CommonJS Modules</a></li>
</ul><img src="http://feeds.feedburner.com/~r/dailyjs/~4/-Njq5eb0ibc" height="1" width="1">