---
layout: post
title:  "JavaScript and DOM API fuss"
date:   2011-08-29 05:42:14
author: James
categories: program
---

## JavaScript and DOM API fuss
### by James
### at 2011-08-29 05:42:14
### original <http://james.padolsey.com/javascript/javascript-and-dom-api-fuss/>

<p>You know what the DOM is, and if you don’t it’s <a href="https://developer.mozilla.org/en/DOM">easy enough to find out</a>. What isn’t so easily discovered is the answer to this: how should your code communicate with the DOM API? Should it be treated as just another API? Is it no more deserving than, for example, a JSON web service or the web storage API?</p>

<p>What I’ve been seeing a lot of is the DOM becoming incredibly intertwined with the logic, i.e. the program becomes indifferentiable to the DOM. It all ends up being the same:</p>


<div><div><pre style="font-family:monospace"><span>var</span> counter <span>=</span> document.<span>createElement</span><span>(</span><span>'div'</span><span>)</span><span>;</span>
counter.<span>innerHTML</span> <span>=</span> <span>'0'</span><span>;</span>
document.<span>body</span>.<span>appendChild</span><span>(</span>counter<span>)</span><span>;</span>
 
<span>function</span> add<span>(</span><span>)</span> <span>{</span>
    counter.<span>innerHTML</span><span>++;</span>
<span>}</span></pre></div></div>




<p>Where does the logic stop and the DOM representation of that logic begin?</p>

<p>This is a rudimentary example, and probably something that few of us would be caught doing, but I think similar things are done every day, and not just by beginners. This is normal code. It isn’t some beginner’s hack.</p>

<p>It wears a different mask wherever it rears its head. Here, for example, it’s under the guise of jQuery’s data API:</p>


<div><div><pre style="font-family:monospace"><span>var</span> alphabetList <span>=</span> <span>{</span>
    giveHigher<span>:</span> <span>function</span><span>(</span>dom1<span>,</span> dom2<span>)</span> <span>{</span>
        <span>return</span> $<span>(</span>dom1<span>)</span>.<span>data</span><span>(</span><span>'word'</span><span>)</span> <span>&gt;</span> $<span>(</span>dom2<span>)</span>.<span>data</span><span>(</span><span>'word'</span><span>)</span> <span>?</span> dom1 <span>:</span> dom2<span>;</span>
    <span>}</span><span>,</span>
    <span>//...</span>
<span>}</span><span>;</span></pre></div></div>




<p>A shockingly high majority of <code>jQuery.data</code> uses seem to be making the mistake that I’ve demonstrated.</p>

<p>It’s subtle, not easily spotted. Right now, I’m wondering whether or not it’s acceptable.</p>

<p>For me, it goes against what I’ve picked up in programming thus far. I’m talking about best practices like separating your concerns and maintaining an OO approach, writing clean APIs to access your program’s logic (etc.). To me, the DOM is yet another representation of an object’s data. The data shouldn’t be <em>in</em> the DOM. This is how I would do it:</p>


<div><div><pre style="font-family:monospace"><span>function</span> Counter<span>(</span><span>)</span> <span>{</span>
    <span>this</span>.<span>dom</span> <span>=</span> document.<span>createElement</span><span>(</span><span>'div'</span><span>)</span><span>;</span>
    <span>this</span>.<span>n</span> <span>=</span> <span>0</span><span>;</span>
<span>}</span>
 
Counter.<span>prototype</span>.<span>add</span> <span>=</span> <span>function</span><span>(</span><span>)</span> <span>{</span>
    <span>this</span>.<span>dom</span>.<span>innerHTML</span> <span>=</span> <span>++</span><span>this</span>.<span>n</span><span>;</span>
<span>}</span><span>;</span></pre></div></div>




<p>The thing is, the approach here is so ridiculously similar to <code>foo.innerHTML++</code> that I really can’t tell what I’m making such a fuss over.</p>

<p>The feeling I get when I see <code>foo.innerHTML++</code> is the same as when I see:</p>


<div><div><pre style="font-family:monospace">jQuery<span>(</span><span>'div'</span><span>)</span>.<span>click</span><span>(</span><span>function</span><span>(</span><span>)</span><span>{</span>
 
    <span>if</span> <span>(</span>$<span>(</span><span>this</span><span>)</span>.<span>data</span><span>(</span><span>'active'</span><span>)</span><span>)</span> <span>{</span>
        <span>// do x</span>
    <span>}</span> <span>else</span> <span>{</span>
        <span>// do y</span>
    <span>}</span>
 
    $<span>(</span><span>this</span><span>)</span>.<span>data</span><span>(</span><span>'active'</span><span>,</span> <span>!</span>$<span>(</span><span>this</span><span>)</span>.<span>data</span><span>(</span><span>'active'</span><span>)</span><span>)</span><span>;</span>
 
<span>}</span><span>)</span><span>;</span></pre></div></div>




<p>… instead of:</p>


<div><div><pre style="font-family:monospace">jQuery<span>(</span><span>'div'</span><span>)</span>.<span>each</span><span>(</span><span>function</span><span>(</span><span>)</span><span>{</span>
 
    <span>var</span> active <span>=</span> <span>false</span><span>;</span>
 
    $<span>(</span><span>this</span><span>)</span>.<span>click</span><span>(</span><span>function</span><span>(</span><span>)</span><span>{</span>
 
        <span>if</span> <span>(</span>active<span>)</span> <span>{</span>
            <span>// do x</span>
        <span>}</span> <span>else</span> <span>{</span>
            <span>// do y</span>
        <span>}</span>
 
        active <span>=</span> <span>!</span>active<span>;</span>
 
    <span>}</span><span>)</span><span>;</span>
 
<span>}</span><span>)</span><span>;</span></pre></div></div>




<p>Opinions welcome.</p>