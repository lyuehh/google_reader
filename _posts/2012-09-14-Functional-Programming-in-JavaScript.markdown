---
layout: post
title:  "Functional Programming in JavaScript"
date:   2012-09-14 07:00:00
author: 
categories: program
---

## Functional Programming in JavaScript
### by 
### at 2012-09-14 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/Mqy4xiukJfU/functional-programming>

<div><div><div></div></div></div><br>
     <p>JavaScript has two parents: Scheme and Self. We can thank Self for all of the object-orientedness of JavaScript and indeed we do in our code and our tutorials. However, Scheme played just as important a role in the language’s design, and we would do ourselves ill to overlook JavaScript’s functional heritage.</p>

<p>What exactly does it mean for JavaScript to be functional? “Functional” merely describes a collection of traits a given language may or may not have. A language like Haskell has all of them: immutable variables, pattern matching, first class functions, and others. Some languages hardly have any, like C. While JavaScript certainly doesn’t have immutable variables or pattern matching it does have a strong emphasis on first class functions; mutating, combining, and using these function objects for cleaner and more succinct code is the purpose of this tutorial.</p>

<h3>Partial Application</h3>

<p>Partial application is a technique for taking a function <em>f</em> and binding it against one or more arguments to produce a new function <em>g</em> with those arguments applied. We’ll demonstrate this operation by adding a helper function <em>p</em> to Function’s prototype.</p>
<div><pre><code><span>Function</span><span>.</span><span>prototype</span><span>.</span><span>p</span> <span>=</span> <span>function</span><span>()</span> <span>{</span>
  <span>// capture the bound arguments</span>
  <span>var</span> <span>args</span> <span>=</span> <span>Array</span><span>.</span><span>prototype</span><span>.</span><span>slice</span><span>.</span><span>call</span><span>(</span><span>arguments</span><span>);</span>
  <span>var</span> <span>f</span> <span>=</span> <span>this</span><span>;</span>
  <span>// construct a new function</span>
  <span>return</span> <span>function</span><span>()</span> <span>{</span>
    <span>// prepend argument list with the closed arguments from above</span>
    <span>var</span> <span>inner_args</span> <span>=</span> <span>Array</span><span>.</span><span>prototype</span><span>.</span><span>slice</span><span>.</span><span>call</span><span>(</span><span>arguments</span><span>);</span>
    <span>return</span> <span>f</span><span>.</span><span>apply</span><span>(</span><span>this</span><span>,</span> <span>args</span><span>.</span><span>concat</span><span>(</span><span>inner_args</span><span>))</span>
  <span>};</span>
<span>};</span>

<span>var</span> <span>plus_two</span> <span>=</span> <span>function</span><span>(</span><span>x</span><span>,</span><span>y</span><span>)</span> <span>{</span> <span>return</span> <span>x</span><span>+</span><span>y</span><span>;</span> <span>};</span>
<span>var</span> <span>add_three</span> <span>=</span> <span>plus_two</span><span>.</span><span>p</span><span>(</span><span>3</span><span>);</span>
<span>add_three</span><span>(</span><span>4</span><span>);</span> <span>// 7</span>
</code></pre>
</div>
<h3>Composition</h3>

<p>Composition is an operation that produces a new function <em>z</em> by nesting functions <em>f</em> and <em>g</em>. You can think of it in this way: <em>z(x) == f(g(x))</em>. Let’s add a helper like we did for partial application.</p>
<div><pre><code><span>Function</span><span>.</span><span>prototype</span><span>.</span><span>c</span> <span>=</span> <span>function</span><span>(</span><span>g</span><span>)</span> <span>{</span>
  <span>// preserve f</span>
  <span>var</span> <span>f</span> <span>=</span> <span>this</span><span>;</span>
  <span>// construct function z</span>
  <span>return</span> <span>function</span><span>()</span> <span>{</span>
    <span>var</span> <span>args</span> <span>=</span> <span>Array</span><span>.</span><span>prototype</span><span>.</span><span>slice</span><span>.</span><span>call</span><span>(</span><span>arguments</span><span>);</span>
    <span>// when called, nest g&#39;s return in a call to f</span>
    <span>return</span> <span>f</span><span>.</span><span>call</span><span>(</span><span>this</span><span>,</span> <span>g</span><span>.</span><span>apply</span><span>(</span><span>this</span><span>,</span> <span>args</span><span>));</span>
  <span>};</span>
<span>};</span>

<span>var</span> <span>greet</span> <span>=</span> <span>function</span><span>(</span><span>s</span><span>)</span> <span>{</span> <span>return</span> <span>&#39;hi, &#39;</span> <span>+</span> <span>s</span><span>;</span> <span>};</span>
<span>var</span> <span>exclaim</span> <span>=</span> <span>function</span><span>(</span><span>s</span><span>)</span> <span>{</span> <span>return</span> <span>s</span> <span>+</span> <span>&#39;!&#39;</span><span>;</span> <span>};</span>
<span>var</span> <span>excited_greeting</span> <span>=</span> <span>greet</span><span>.</span><span>c</span><span>(</span><span>exclaim</span><span>);</span>
<span>excited_greeting</span><span>(</span><span>&#39;Pickman&#39;</span><span>)</span> <span>// hi, Pickman!</span>
</code></pre>
</div>
<h3>Flipping</h3>

<p>Flipping at first seems like a scary and arbitrary thing to do to a poor Function. However, it is useful when one desires to use partial application to bind arguments other than the first. To perform a flip we take function <em>f</em> which takes parameters <em>(a,b)</em> and construct a function <em>g</em> which takes parameters <em>(b,a)</em>.</p>
<div><pre><code><span>Function</span><span>.</span><span>prototype</span><span>.</span><span>f</span> <span>=</span> <span>function</span><span>()</span> <span>{</span>
  <span>// preserve f</span>
  <span>var</span> <span>f</span> <span>=</span> <span>this</span><span>;</span>
  <span>// construct g</span>
  <span>return</span> <span>function</span><span>()</span> <span>{</span>
    <span>var</span> <span>args</span> <span>=</span> <span>Array</span><span>.</span><span>prototype</span><span>.</span><span>slice</span><span>.</span><span>call</span><span>(</span><span>arguments</span><span>);</span>
    <span>// flip arguments when called</span>
    <span>return</span> <span>f</span><span>.</span><span>apply</span><span>(</span><span>this</span><span>,</span> <span>args</span><span>.</span><span>reverse</span><span>());</span>
  <span>};</span>
<span>};</span>

<span>var</span> <span>div</span> <span>=</span> <span>function</span><span>(</span><span>x</span><span>,</span><span>y</span><span>)</span> <span>{</span> <span>return</span> <span>x</span> <span>/</span> <span>y</span><span>;</span> <span>};</span>
<span>div</span><span>(</span><span>1</span><span>,</span> <span>2</span><span>)</span> <span>// 0.5</span>
<span>div</span><span>.</span><span>f</span><span>()(</span><span>1</span><span>,</span><span>2</span><span>)</span> <span>// 2</span>
</code></pre>
</div>
<h3>Point-Free Style</h3>

<p>Point-free programming is a style of coding that one doesn’t see much outside of languages like Haskell or OCaml. However, it can help drastically reduce the use of the rather verbose function declaration syntax omnipresent in JavaScript code. Programming in a point-free style is made possible by our helpers above, and we’ll combine them to illustrate this concept.</p>
<div><pre><code><span>// We&#39;ll start by solving the following problem in a non point-free way.</span>
<span>// Produce a function which, given a list, returns the same list with</span>
<span>// every number made negative.</span>

<span>// First, declare some helpers:</span>
<span>var</span> <span>negate</span> <span>=</span> <span>function</span><span>(</span><span>x</span><span>)</span> <span>{</span> <span>return</span> <span>-</span><span>1</span> <span>*</span> <span>x</span><span>;</span> <span>};</span>
<span>var</span> <span>abs</span> <span>=</span> <span>function</span><span>(</span><span>x</span><span>)</span> <span>{</span> <span>return</span> <span>Math</span><span>.</span><span>abs</span><span>(</span><span>x</span><span>);</span> <span>};</span>
<span>var</span> <span>map</span> <span>=</span> <span>function</span><span>(</span><span>a</span><span>,</span> <span>f</span><span>)</span> <span>{</span> <span>return</span> <span>a</span><span>.</span><span>map</span><span>(</span><span>f</span><span>);</span> <span>};</span>
<span>var</span> <span>numbers</span> <span>=</span> <span>[</span><span>-</span><span>1</span><span>,</span> <span>2</span><span>,</span> <span>0</span><span>,</span> <span>-</span><span>2</span><span>,</span> <span>3</span><span>,</span> <span>4</span><span>,</span> <span>-</span><span>6</span><span>]</span>

<span>var</span> <span>negate_all</span> <span>=</span> <span>function</span><span>(</span><span>array</span><span>)</span> <span>{</span> <span>return</span> <span>map</span><span>(</span><span>array</span><span>,</span> <span>function</span><span>(</span><span>x</span><span>)</span> <span>{</span> <span>return</span> <span>negate</span><span>(</span><span>abs</span><span>(</span><span>x</span><span>))</span> <span>};</span>
<span>negate_all</span><span>(</span><span>numbers</span><span>);</span> <span>// [-1, -2, 0, -2, -3, -4, -6]</span>

<span>// That solves it; but we can do better:</span>

<span>var</span> <span>negate_all</span> <span>=</span> <span>map</span><span>.</span><span>f</span><span>().</span><span>p</span><span>(</span><span>negate</span><span>.</span><span>c</span><span>(</span><span>abs</span><span>));</span>
<span>negate_all</span><span>(</span><span>numbers</span><span>);</span> <span>// [-1, -2, 0, -2, -3, -4, -6]</span>
</code></pre>
</div>
<p>What did we do here? First, we flipped <code>map</code>’s signature to be <em>(f,a)</em>; this allows us to then partially apply a function to map and turn it into a function that takes only a single parameter: the array we wish to negate. But what function do we want to bind to our map? The result of <code>negate.c(abs)</code>, which represents a function that does <code>negate(abs(x))</code>. We’ve produced the same function in the end and solved our problem. In the former attempt, we declare a new function to imperatively do what we wish; in the latter we construct a new function based on functions we already have.</p>

<p>What makes this point-free? Note the redundancy of the <code>array</code> argument in the former declaration. We already have a function that knows how to produce a new array from an existing one; why not convert that function into a new one to do what we want? We cut characters by 37% and, for many, achieve better readability.</p>

<h3>Conclusions</h3>

<p>In the end, functional programming is a matter of taste. For some it is a thing of subtle beauty and for others a wild nest of parentheses. This tutorial is a suggestion of styles that might be and is in no way a ‘Functional is better’ argument. If this has piqued the reader’s interest, she or he may be interested in the following resources:</p>

<ul>
<li><a href="http://osteele.com/sources/javascript/functional/">Oliver Steele’s Functional library</a>. A library which provides the above operators and much more.</li>

<li><a href="http://learnyouahaskell.com/">Learn You a Haskell For Great Good</a>. Doubles as a great introduction to functional programming.</li>

<li><a href="http://www.crockford.com/javascript/little.html">The Little JavaScripter</a>. A take on The Little Schemer by Douglas Crockford.</li>

<li><a href="http://en.wikipedia.org/wiki/Tacit_programming">Point-free (or Tacit) Programming</a>. From Wikipedia.</li>
</ul>
   <img src="http://feeds.feedburner.com/~r/dailyjs/~4/Mqy4xiukJfU" height="1" width="1">