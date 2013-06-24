---
layout: post
title:  "JS101: Prototypes"
date:   2012-05-21 07:00:00
author: 
categories: program
---

## JS101: Prototypes
### by 
### at 2012-05-21 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/HyjjQmO_wY8/js101-prototype>

<div>
  JS101 is a tutorial series aimed at beginners.  Each post is a bite-sized chunk aimed to elucidate JavaScript fundamentals.  To read previous posts, view the <a href="http://dailyjs.com/tags.html#js101">js101</a> tag.
</div>
<p>After spending years studying object oriented programming, adapting to JavaScript can be frustrating. In particular, the lack of a <code>class</code> keyword is a source of confusion. However, JavaScript’s design needn’t be a hindrance – mastering its prototype-based inheritance will improve your understanding of the language.</p>

<p>The first thing to realise is there should be a distinction between <em>object</em>-oriented programming and <em>class</em>-oriented. JavaScript gives us the tools we need to do most of the things languages with classes can do – we just need to learn how to use it properly.</p>

<p>Let’s take a brief look at the <code>prototype</code> property to see how it can deepen our knowledge of JavaScript.</p>

<h3>The <code>prototype</code> Property</h3>

<p>The <code>prototype</code> property is an internal property, and it’s designed to be used to implement inheritance. What we mean by “inheritance” here is a specific form of inheritance. Because both state and methods are carried by objects, then we can say that structure, behaviour, and state are all inherited (<a href="http://es5.github.com/#x4.2.1">ES5: Objects</a>). This is in contrast to class-based languages, where state is carried by instances and methods are carried by classes.</p>

<p>A constructor is a function that has a property named <code>prototype</code>:</p>
<div><pre><code><span>function</span> <span>Animal</span><span>()</span> <span>{</span>
<span>}</span>

<span>console</span><span>.</span><span>log</span><span>(</span><span>Animal</span><span>.</span><span>prototype</span><span>);</span>
</code></pre>
</div>
<p>This displays <code>{}</code> – the <code>Animal</code> object has a <code>prototype</code> property, but there’s nothing user-defined in it yet. We’re free to add values and methods as we please:</p>
<div><pre><code><span>function</span> <span>Animal</span><span>()</span> <span>{</span>
<span>}</span>

<span>Animal</span><span>.</span><span>prototype</span><span>.</span><span>type</span> <span>=</span> <span>&#39;Unknown&#39;</span><span>;</span>
<span>Animal</span><span>.</span><span>prototype</span><span>.</span><span>weight</span> <span>=</span> <span>0</span><span>;</span>
<span>Animal</span><span>.</span><span>prototype</span><span>.</span><span>weightUnits</span> <span>=</span> <span>&#39;kg&#39;</span><span>;</span>

<span>Animal</span><span>.</span><span>prototype</span><span>.</span><span>toString</span> <span>=</span> <span>function</span><span>()</span> <span>{</span>
  <span>return</span> <span>this</span><span>.</span><span>type</span> <span>+</span> <span>&#39;, &#39;</span> <span>+</span> <span>this</span><span>.</span><span>weight</span> <span>+</span> <span>this</span><span>.</span><span>weightUnits</span><span>;</span>
<span>};</span>

<span>var</span> <span>molly</span> <span>=</span> <span>new</span> <span>Animal</span><span>();</span>
<span>molly</span><span>.</span><span>type</span> <span>=</span> <span>&#39;Dog&#39;</span><span>;</span>
<span>molly</span><span>.</span><span>weight</span> <span>=</span> <span>28</span><span>;</span>

<span>console</span><span>.</span><span>log</span><span>(</span><span>molly</span><span>.</span><span>toString</span><span>());</span>
</code></pre>
</div>
<p>This would display “Dog, 28kg”. We can group these assignments together by using an object literal:</p>
<div><pre><code><span>function</span> <span>Animal</span><span>()</span> <span>{</span>
<span>}</span>

<span>Animal</span><span>.</span><span>prototype</span> <span>=</span> <span>{</span>
  <span>type</span><span>:</span> <span>&#39;Unknown&#39;</span><span>,</span>
  <span>weight</span><span>:</span> <span>0</span><span>,</span>
  <span>weightUnits</span><span>:</span> <span>&#39;kg&#39;</span><span>,</span>

  <span>toString</span><span>:</span> <span>function</span><span>()</span> <span>{</span>
    <span>return</span> <span>this</span><span>.</span><span>type</span> <span>+</span> <span>&#39;, &#39;</span> <span>+</span> <span>this</span><span>.</span><span>weight</span> <span>+</span> <span>this</span><span>.</span><span>weightUnits</span><span>;</span>
  <span>}</span>
<span>};</span>
</code></pre>
</div>
<p>This shouldn’t look too different from the classes you might be more familiar with.</p>

<h3>Dynamic Prototypes</h3>

<p>Properties can be added to objects dynamically, simply by assigning values:</p>
<div><pre><code><span>var</span> <span>molly</span> <span>=</span> <span>new</span> <span>Animal</span><span>()</span>
  <span>,</span> <span>harley</span> <span>=</span> <span>new</span> <span>Animal</span><span>();</span>

<span>molly</span><span>.</span><span>type</span> <span>=</span> <span>&#39;Dog&#39;</span><span>;</span>
<span>molly</span><span>.</span><span>weight</span> <span>=</span> <span>28</span><span>;</span>

<span>harley</span><span>.</span><span>type</span> <span>=</span> <span>&#39;Dog&#39;</span><span>;</span>
<span>harley</span><span>.</span><span>weight</span> <span>=</span> <span>38</span><span>;</span>
<span>harley</span><span>.</span><span>name</span> <span>=</span> <span>&#39;Harley&#39;</span><span>;</span>

<span>console</span><span>.</span><span>log</span><span>(</span><span>molly</span><span>);</span>
<span>console</span><span>.</span><span>log</span><span>(</span><span>harley</span><span>);</span>

<span>// { type: &#39;Dog&#39;, weight: 28 }</span>
<span>// { type: &#39;Dog&#39;, weight: 38, name: &#39;Harley&#39; }</span>
</code></pre>
</div>
<p>Adding the <code>name</code> property here only affects that instance. However, the constructor’s prototype can be changed, and this will affect objects made with that prototype:</p>
<div><pre><code><span>Animal</span><span>.</span><span>prototype</span><span>.</span><span>weightUnits</span> <span>=</span> <span>&#39;oz&#39;</span><span>;</span>

<span>console</span><span>.</span><span>log</span><span>(</span><span>molly</span><span>.</span><span>toString</span><span>())</span>
<span>// Now displays &#39;Dog, 28oz&#39;</span>
</code></pre>
</div>
<p>This is why people boast that their libraries don’t touch native prototypes, or only do so safely – it’s entirely possible to change the expected built-in functionality of objects like <code>String</code> to do unsafe things:</p>
<div><pre><code><span>String</span><span>.</span><span>prototype</span><span>.</span><span>match</span> <span>=</span> <span>function</span><span>()</span> <span>{</span>
  <span>return</span> <span>true</span><span>;</span>
<span>};</span>

<span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;alex&#39;</span><span>.</span><span>match</span><span>(</span><span>/1234/</span><span>));</span>
</code></pre>
</div>
<p>This returns <code>true</code>, so I’ve now succeeded in breaking a fundamental method that many JavaScript programs rely on.</p>

<p>Modifying native prototypes isn’t always bad; people use it for useful things like patching support for more modern versions of ECMAScript in older browsers.</p>

<p>What happens if we replace the <code>prototype</code> property?</p>
<div><pre><code><span>var</span> <span>molly</span> <span>=</span> <span>new</span> <span>Animal</span><span>()</span>
  <span>,</span> <span>harley</span><span>;</span>

<span>molly</span><span>.</span><span>type</span> <span>=</span> <span>&#39;Dog&#39;</span><span>;</span>
<span>molly</span><span>.</span><span>weight</span> <span>=</span> <span>28</span><span>;</span>

<span>Animal</span><span>.</span><span>prototype</span> <span>=</span> <span>{</span>
  <span>toString</span><span>:</span> <span>function</span><span>()</span> <span>{</span>
    <span>return</span> <span>&#39;...&#39;</span><span>;</span>
  <span>}</span>
<span>};</span>

<span>harley</span> <span>=</span> <span>new</span> <span>Animal</span><span>;</span>
<span>harley</span><span>.</span><span>type</span> <span>=</span> <span>&#39;Dog&#39;</span><span>;</span>
<span>harley</span><span>.</span><span>weight</span> <span>=</span> <span>38</span><span>;</span>

<span>console</span><span>.</span><span>log</span><span>(</span><span>molly</span><span>.</span><span>toString</span><span>());</span>
<span>console</span><span>.</span><span>log</span><span>(</span><span>harley</span><span>.</span><span>toString</span><span>());</span>

<span>// Dog, 28kg</span>
<span>// ...</span>
</code></pre>
</div>
<p>Despite the fact changing a prototype affects all of the instances, replacing a constructor’s prototype entirely does not affect older instances. Why? Well, instances have a <em>reference</em> to the prototype rather than a discrete copy. Think of it like this: each instance created by the <code>new</code> keyword is <em>connected</em> to the original prototype.</p>

<h3>Next Week</h3>

<p>In the spirit of keeping this post bite-sized, I’ll finish here. Next week we’ll look at prototype chains and inheritance in more detail.</p>

<h3>References</h3>

<ul>
<li><a href="http://es5.github.com/">Annotated ECMAScript 5.1</a></li>

<li><a href="https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Function/prototype">MDN Prototype</a></li>

<li><a href="http://www.youtube.com/watch?v=3zpQ3bnnLUI">Comsat Angels - Will You Stay Tonight</a></li>
</ul><img src="http://feeds.feedburner.com/~r/dailyjs/~4/HyjjQmO_wY8" height="1" width="1">