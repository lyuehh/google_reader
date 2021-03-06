---
layout: post
title:  "The Chain Gang"
date:   2010-10-25 09:42:28
author: John Nunemaker
categories: program
---

## The Chain Gang
### by John Nunemaker
### at 2010-10-25 09:42:28
### original <http://feedproxy.google.com/~r/railstips/~3/NMKB85YznZI/>

<p>Chain-able interfaces are all the rage — jQuery, ARel, etc. The thing a lot of people do not realize is how easy they are to create. Lets say we want to make the following work:</p>
<div><div>
  <div><pre><span>User</span>.where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>)
<span>User</span>.sort(<span>:age</span>)
<span>User</span>.where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>).sort(<span>:age</span>)
<span>User</span>.sort(<span>:age</span>).where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>)</pre></div>
</div>
</div><p>First, we need to have a class method for both where and sort because we want to allow either one of them to be called and chained on each other.</p>
<div><div>
  <div><pre><span>class</span> <span>User</span>
  <span>def</span> <span>self</span>.<span>where</span>(hash)
  <span>end</span>

  <span>def</span> <span>self</span>.<span>sort</span>(field)
  <span>end</span>
<span>end</span>

<span>User</span>.where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>)
<span>User</span>.sort(<span>:age</span>)</pre></div>
</div>
</div><p>Now we can call where or sort and we do not get errors, but we still cannot chain. In order to make this magic happen, lets make a query class. The query needs to know what model, what where conditions and what field to sort by.</p>
<div><div>
  <div><pre><span>class</span> <span>Query</span>
  <span>def</span> <span>initialize</span>(model)
    <span>@model</span> = model
  <span>end</span>

  <span>def</span> <span>where</span>(hash)
    <span>@where</span> = hash
  <span>end</span>

  <span>def</span> <span>sort</span>(field)
    <span>@sort</span> = field
  <span>end</span>
<span>end</span></pre></div>
</div>
</div><p>Now that we have this, lets create new query objects when the User class methods are called and pass the arguments through.</p>
<div><div>
  <div><pre><span>class</span> <span>User</span>
  <span>def</span> <span>self</span>.<span>where</span>(hash)
    <span>Query</span>.new(<span>self</span>).where(hash)
  <span>end</span>

  <span>def</span> <span>self</span>.<span>sort</span>(field)
    <span>Query</span>.new(<span>self</span>).sort(field)
  <span>end</span>
<span>end</span></pre></div>
</div>
</div><p>We might think we are done at this point, but the sauce that makes this all work is still missing. If you try our initial example, you end up with a cryptic error message.</p>
<pre>ArgumentError: wrong number of arguments (1 for 0)</pre>
<p>The reason is that in order for this to be chainable, we have to return self in Query#where and Query#sort.</p>
<div><div>
  <div><pre><span>class</span> <span>Query</span>
  <span>def</span> <span>initialize</span>(model)
    <span>@model</span> = model
  <span>end</span>

  <span>def</span> <span>where</span>(hash)
    <span>@where</span> = hash
    <span>self</span>
  <span>end</span>

  <span>def</span> <span>sort</span>(field)
    <span>@sort</span> = field
    <span>self</span>
  <span>end</span>
<span>end</span></pre></div>
</div>
</div><p>Now, if we put it all together, you can see that this is the basics of creating a chain-able interface. Simply, do what you need to do and return self.</p>
<div><div>
  <div><pre><span>class</span> <span>Query</span>
  <span>def</span> <span>initialize</span>(model)
    <span>@model</span> = model
  <span>end</span>

  <span>def</span> <span>where</span>(hash)
    <span>@where</span> = hash
    <span>self</span>
  <span>end</span>

  <span>def</span> <span>sort</span>(field)
    <span>@sort</span> = field
    <span>self</span>
  <span>end</span>
<span>end</span>

<span>class</span> <span>User</span>
  <span>def</span> <span>self</span>.<span>where</span>(hash)
    <span>Query</span>.new(<span>self</span>).where(hash)
  <span>end</span>

  <span>def</span> <span>self</span>.<span>sort</span>(field)
    <span>Query</span>.new(<span>self</span>).sort(field)
  <span>end</span>
<span>end</span>

puts <span>User</span>.where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>).inspect
puts <span>User</span>.sort(<span>:age</span>).inspect
puts <span>User</span>.where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>).sort(<span>:age</span>).inspect
puts <span>User</span>.sort(<span>:age</span>).where(<span>:first_name</span> =&gt; <span><span>'</span><span>John</span><span>'</span></span>).inspect

<span># #&lt;Query:0x101020268 @model=User, @where={:first_name=&gt;&quot;John&quot;}&gt;</span>
<span># #&lt;Query:0x101020060 @model=User, @sort=:age&gt;</span>
<span># #&lt;Query:0x10101fe30 @model=User, @where={:first_name=&gt;&quot;John&quot;}, @sort=:age&gt;</span>
<span># #&lt;Query:0x10101fbb0 @model=User, @where={:first_name=&gt;&quot;John&quot;}, @sort=:age&gt;</span></pre></div>
</div>
</div><h2>Conclusion</h2>
<p>From here, all we need to do is define kickers, such as all, first, last, etc. that actually assemble and perform the query and return results. Hope this adds a little something to your repertoire next time you are building an interface. It does not work in every situation, but when applied correctly it can improve the usability of a library.</p>
<p>If you are interested in more on this, feel free to peak at the innards of <a href="http://github.com/jnunemaker/plucky">Plucky</a>, which provides a chain-able interface for querying MongoDB.</p><div>
<a href="http://feeds.feedburner.com/~ff/railstips?a=NMKB85YznZI:jKLyYK-dBPk:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/railstips?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/railstips?a=NMKB85YznZI:jKLyYK-dBPk:dnMXMwOfBR0"><img src="http://feeds.feedburner.com/~ff/railstips?d=dnMXMwOfBR0" border="0"></a> <a href="http://feeds.feedburner.com/~ff/railstips?a=NMKB85YznZI:jKLyYK-dBPk:7Q72WNTAKBA"><img src="http://feeds.feedburner.com/~ff/railstips?d=7Q72WNTAKBA" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/railstips/~4/NMKB85YznZI" height="1" width="1">