---
layout: post
title:  "【外刊IT评论网】为什么我们要学习Haskell这样的编程语言"
date:   2012-04-11 00:03:27
author: Aqee
categories: program
---

## 【外刊IT评论网】为什么我们要学习Haskell这样的编程语言
### by Aqee
### at 2012-04-11 00:03:27
### original <http://www.aqee.net/learn-you-a-haskell-for-great-good/>

<br><p>最近的几个月，我一直在学习一种叫Haskell的编程语言。由于里面有太多的从未遇到的编程概念，整个过程就像是完全重新学习如何编程。在<a href="http://corp.i.tv/">i.TV</a>网站上，我写了很多JavaScript(<a title="Node.js" href="http://nodejs.org/">node.js</a>和前端代码)。虽然有不少的函数式/haskell式的编程模式不能引用进来，但仍有大量的技术思想让我在使用javascript编程语言时受益不少。</p>
<p><span></span></p>
<p>你会发现Haskell库里有能够处理<strong>各种事情</strong>的各种各样的函数。起初我以为这些只是一种技术上的积累，但随后我认识到，这些函数相比起其它语言里的函数，它们能应用到形式更广泛的问题中。这使得它们更有价值，因为我们都不太喜欢对一些常见的问题还不得不自己去写解决方案。</p>
<p>这些函数是可以<strong>相互组合</strong><sup><a rel="footnote" href="http://www.aqee.net/#fn:1">1</a></sup>的：它们能针对性的解决某些问题，而不对你的代码做任何依赖，所以，你可以拼装它们，组合成一个能够解决你的大问题的东西。</p>
<h3>高阶函数(Higher Order Functions)</h3>
<p>在Haskell语言中，最多的被反复使用的函数都是<a title="Higher Order Function" href="http://en.wikipedia.org/wiki/Higher-order_function">高阶函数(higher order functions)</a>——能以函数作为参数、能返回函数的函数。这使得它们具有固有的灵活性。下面是一个不太灵活的函数：它计算一个数组里等于某个值的元素的个数。</p>
<pre><code><span>// 不灵活</span>
<span>function</span> <span>countMatching</span><span>(</span><span>array</span><span>,</span> <span>value</span><span>)</span> <span>{</span>
    <span>var</span> <span>counted</span> <span>=</span> <span>0</span>
    <span>for</span> <span>(</span><span>var</span> <span>i</span> <span>=</span> <span>0</span><span>;</span> <span>i</span> <span>&lt;</span> <span>array</span><span>.</span><span>length</span><span>;</span> <span>i</span><span>++</span><span>)</span> <span>{</span>
        <span>if</span> <span>(</span><span>array</span><span>[</span><span>i</span><span>]</span> <span>==</span> <span>value</span><span>)</span>
            <span>counted</span><span>++</span>
    <span>}</span>
    <span>return</span> <span>counted</span>
<span>}</span>

<span>// == 2</span>
<span>countMatching</span><span>([</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>],</span> <span>3</span><span>)</span>
</code></pre>
<p>它不灵活，因为它只能用来计算一个数组中<strong>精确匹配某个值</strong>的元素的个数。</p>
<p>下面是一个灵活一些的版本，它能接受一个函数，而不是一个值，作为参数。我们可以用它来对任何数据、任何对象进行比较。</p>
<pre><code><span>// more flexible</span>
<span>function</span> <span>count</span><span>(</span><span>array</span><span>,</span> <span>matching</span><span>)</span> <span>{</span>
    <span>var</span> <span>counted</span> <span>=</span> <span>0</span>
    <span>for</span> <span>(</span><span>var</span> <span>i</span> <span>=</span> <span>0</span><span>;</span> <span>i</span> <span>&lt;</span> <span>array</span><span>.</span><span>length</span><span>;</span> <span>i</span><span>++</span><span>)</span> <span>{</span>
        <span>if</span> <span>(</span><span>matching</span><span>(</span><span>array</span><span>[</span><span>i</span><span>]))</span>
            <span>counted</span><span>++</span>
    <span>}</span>
    <span>return</span> <span>counted</span>
<span>}</span>

<span>// == 2, same as first example</span>
<span>count</span><span>([</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>],</span> <span>function</span><span>(</span><span>num</span><span>)</span> <span>{</span>
    <span>return</span> <span>(</span><span>num</span> <span>==</span> <span>3</span><span>)</span>
<span>})</span>

<span>// == 2, now we can use our functions for ANY kind of items or match test!</span>
<span>count</span><span>([{</span><span>name</span><span>:</span><span>"bob"</span><span>},</span> <span>{</span><span>name</span><span>:</span><span>"henry"</span><span>},</span> <span>{</span><span>name</span><span>:</span><span>"jon"</span><span>}],</span> <span>function</span><span>(</span><span>obj</span><span>)</span> <span>{</span>
    <span>return</span> <span>(</span><span>obj</span><span>.</span><span>name</span><span>.</span><span>length</span> <span>&lt;</span> <span>4</span><span>)</span>
<span>})</span>
</code></pre>
<p>因为高阶函数更具灵活性，你就更少有机会去写它们，因为你一旦你写成一个，你可以它应用到各种不同的情况中。</p>
<h3>可重复利用的比较函数</h3>
<p>你可能注意到了，<code>count</code>函数的写法比<code>countMatching</code>更冗长。但是，虽然<code>count</code>函数可复用了，但比较函数<sup><a rel="footnote" href="http://www.aqee.net/#fn:2">2</a></sup>却不可复用。如果是一些简单的情况，这就足够了，但经常，我们会需要更复杂的比较方法的函数。这样的函数不仅仅可用于计数，它们可以用于任何事情上，一但你写成或找到了这样的函数，从长期的角度看，它们会节省你大量的时间和调试功夫。</p>
<p>让我们来定义一个可复用的比较函数，达到我们的目的。<code>==</code>不是一个函数。我们是否可以定义一个<code>eq</code>函数来帮我们完成类似的事情呢？</p>
<pre><code><span>function</span> <span>eq</span><span>(</span><span>a</span><span>,</span> <span>b</span><span>)</span> <span>{</span>
    <span>return</span> <span>(</span><span>a</span> <span>==</span> <span>b</span><span>)</span>
<span>}</span>

<span>count</span><span>([</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>],</span> <span>function</span><span>(</span><span>num</span><span>)</span> <span>{</span>
    <span>return</span> <span>eq</span><span>(</span><span>3</span><span>,</span> <span>num</span><span>)</span>
<span>})</span>
</code></pre>
<p>我们向前迈进了一步：我们用了一个库函数来完成比较任务，而不是使用我们现写的代码。如果<code>eq</code>函数很复杂，我们可以测试它并可以在其它的地方复用它。</p>
<p>但这使代码变得冗长，因为<code>count</code>函数的参数是一个只需要一个参数——数组元素——的函数，而<code>eq</code>函数却需要两个参数。我还是要定义我们自己的匿名函数。让我们来简化一下这些代码。</p>
<pre><code>
<span>function</span> <span>makeEq</span><span>(</span><span>a</span><span>)</span> <span>{</span>
    <span>// countMatchingWith wants a function that takes </span>
    <span>// only 1 argument, just like the one we're returning</span>
    <span>return</span> <span>function</span><span>(</span><span>b</span><span>)</span> <span>{</span>
        <span>return</span> <span>eq</span><span>(</span><span>a</span><span>,</span> <span>b</span><span>)</span>
    <span>}</span>
<span>}</span>

<span>// now it's only on one line!</span>
<span>count</span><span>([</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>],</span> <span>makeEq</span><span>(</span><span>3</span><span>))</span>
</code></pre>
<p>我们写了一个兼容<code>count</code>函数的函数(一个参数——数组元素——返回true或false)。看起来就像是<code>count</code>函数调用的是<code>eq(3, item)</code>。这叫做偏函数用法(partial function application)。</p>
<h3>偏函数用法(Partial Application)</h3>
<p><a title="Partial Application" href="http://en.wikipedia.org/wiki/Partial_application">偏函数用法(Partial Function Application)</a>是指创建一个调用另外一个部分参数已经预置的函数的函数的用法。这样，它就能被别的地方，比如<code>count</code>函数，以更少的参数形式来调用。我们在<code>makeEq</code>函数里已经实现了这些，但是，我们并不想针对我们各种功能开发出各种版本的<code>makeX</code>(比如makeEqt，makeGt，makeLt等)函数。让我们来找一种方法能通用于各种形式的函数。</p>
<pre><code><span>function</span> <span>applyFirst</span><span>(</span><span>f</span><span>,</span> <span>a</span><span>)</span> <span>{</span>
    <span>return</span> <span>function</span><span>(</span><span>b</span><span>)</span> <span>{</span>
        <span>return</span> <span>f</span><span>.</span><span>call</span><span>(</span><span>null</span><span>,</span> <span>a</span><span>,</span> <span>b</span><span>)</span>
    <span>}</span>
<span>}</span>

<span>count</span><span>([</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>],</span> <span>applyFirst</span><span>(</span><span>eq</span><span>,</span> <span>3</span><span>))</span>
</code></pre>
<p>现在我们不再需要一个<code>makeEq</code>函数了。任何2个参数的库函数，我们都可以按这种方式调用。通过偏函数用法，使得定义即使是诸如<code>==</code>这样简单功能的各种函数都变得十分有意义，我们可以在高阶函数中更容易的使用它们。</p>
<p>对那些超过2个参数的函数如何办呢？下面的这一版本<sup><a rel="footnote" href="http://www.aqee.net/#fn:3">3</a></sup>能让我们接受任意多的参数，高阶函数可以自己追加参数。 </p>
<pre><code><span>function</span> <span>apply</span><span>(</span><span>f</span><span>)</span> <span>{</span>
    <span>var</span> <span>args</span> <span>=</span> <span>Array</span><span>.</span><span>prototype</span><span>.</span><span>slice</span><span>.</span><span>call</span><span>(</span><span>arguments</span><span>,</span> <span>1</span><span>)</span>
    <span>return</span> <span>function</span><span>(</span><span>x</span><span>)</span> <span>{</span>
        <span>return</span> <span>f</span><span>.</span><span>apply</span><span>(</span><span>null</span><span>,</span> <span>args</span><span>.</span><span>concat</span><span>(</span><span>x</span><span>))</span>
    <span>}</span>
<span>}</span>

<span>function</span> <span>propertyEquals</span><span>(</span><span>propertyName</span><span>,</span> <span>value</span><span>,</span> <span>obj</span><span>)</span> <span>{</span>
    <span>return</span> <span>(</span><span>obj</span><span>[</span><span>propertyName</span><span>]</span> <span>==</span> <span>value</span><span>)</span>
<span>}</span>

<span>count</span><span>([{</span><span>name</span><span>:</span><span>"bob"</span><span>},{</span><span>name</span><span>:</span><span>"john"</span><span>}],</span> <span>apply</span><span>(</span><span>propertyEquals</span><span>,</span> <span>"name"</span><span>,</span> <span>"bob"</span><span>))</span> <span>// == 1</span>
</code></pre>
<p>我们预置了2个参数，“name” 和 “bob”，<code>count</code>函数补足了最后一个参数来完成整个调用。偏函数用法使我们能接受各样的函数为参数，例如<code>eq</code>，然后把它们用于各样的高阶函数，例如<code>count</code>，以此来解决我们特定的问题。</p>
<h3>配合ES5的 Map 和 Filter 功能函数的偏函数用法</h3>
<p><a href="http://kangax.github.com/es5-compat-table/">ES5</a>里有很多非常好的高阶函数，<a href="http://documentcloud.github.com/underscore/">underscore</a>里的数量更多。让我们看看<code>filter</code>函数——一个接收比较函数、过滤数组内容的函数。</p>
<pre><code><span>// this equals [1,3,3]</span>
<span>[</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>].</span><span>filter</span><span>(</span><span>function</span><span>(</span><span>num</span><span>)</span> <span>{</span>
    <span>return</span> <span>(</span><span>num</span> <span>&lt;</span> <span>4</span><span>)</span>
<span>})</span>
</code></pre>
<p>让我们把它替换成一个可以复用的比较函数<code>lt</code> (less than)。</p>
<pre><code><span>function</span> <span>lt</span><span>(</span><span>a</span><span>,</span> <span>b</span><span>)</span> <span>{</span>
    <span>return</span> <span>(</span><span>a</span> <span>&lt;</span> <span>b</span><span>)</span>
<span>}</span>

<span>[</span><span>1</span><span>,</span><span>3</span><span>,</span><span>3</span><span>,</span><span>4</span><span>,</span><span>5</span><span>].</span><span>filter</span><span>(</span><span>apply</span><span>(</span><span>lt</span><span>,</span> <span>4</span><span>))</span>
</code></pre>
<p>看上去添加这个<code>lt</code>函数的做法有点傻，但是，我们可以使用偏函数用法来创造一个很简练的比较函数，当这个比较函数变的很复杂的时候，我们就能从对它的复用过程中获得好处。</p>
<p><code>map</code>函数能让你把数组里的一个东西变成另外一个东西。</p>
<pre><code><span>var</span> <span>usersById</span> <span>=</span> <span>{</span><span>"u1"</span><span>:</span><span>{</span><span>name</span><span>:</span><span>"bob"</span><span>},</span> <span>"u2"</span><span>:</span><span>{</span><span>name</span><span>:</span><span>"john"</span><span>}}</span>
<span>var</span> <span>user</span> <span>=</span> <span>{</span><span>name</span><span>:</span><span>"sean"</span><span>,</span> <span>friendIds</span><span>:</span> <span>[</span><span>"u1"</span><span>,</span> <span>"u2"</span><span>]}</span>

<span>// == ["bob", "john"]</span>
<span>function</span> <span>friendsNames</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>return</span> <span>user</span><span>.</span><span>friendIds</span><span>.</span><span>map</span><span>(</span><span>function</span><span>(</span><span>id</span><span>)</span> <span>{</span>
        <span>return</span> <span>usersById</span><span>[</span><span>id</span><span>].</span><span>name</span>
    <span>})</span>
<span>}</span>
</code></pre>
<p>我们写一个可以复用的map变换函数，就像之前我们的可复用比较函数一样。让我们写一个叫做<code>lookup</code>的函数。</p>
<pre><code><span>function</span> <span>lookup</span><span>(</span><span>obj</span><span>,</span> <span>key</span><span>)</span> <span>{</span>
    <span>return</span> <span>obj</span><span>[</span><span>key</span><span>]</span>
<span>}</span>

<span>// == [{name:"bob"}, {name:"john"}]</span>
<span>function</span> <span>friends</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>return</span> <span>user</span><span>.</span><span>friendIds</span><span>.</span><span>map</span><span>(</span><span>apply</span><span>(</span><span>lookup</span><span>,</span> <span>usersById</span><span>))</span>
<span>}</span>
</code></pre>
<p>很接近要求，但我们需要的是名称，而不是friend对象本身。如果我们再写一个参数颠倒过来的 <code>lookup</code>函数，通过第二次的map可以把它们的名称取出来。</p>
<pre><code><span>function</span> <span>lookupFlipped</span><span>(</span><span>key</span><span>,</span> <span>obj</span><span>)</span> <span>{</span>
    <span>return</span> <span>lookup</span><span>(</span><span>obj</span><span>,</span> <span>key</span><span>)</span>
<span>}</span>

<span>// == ["bob", "john"]</span>
<span>function</span> <span>friendsNames</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>return</span> <span>friends</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span>
            <span>.</span><span>map</span><span>(</span><span>apply</span><span>(</span><span>lookupFlipped</span><span>,</span> <span>"name"</span><span>))</span>
<span>}</span>
</code></pre>
<p>但是我不想定义这个<code>lookupFlipped</code>函数，这样干有点傻。这样，我们来定义一个函数，它接收参数的顺序是从右到左，而不是从左到右，于是我们就能够复用<code>lookup</code>了。</p>
<pre><code><span>function</span> <span>applyr</span><span>(</span><span>f</span><span>)</span> <span>{</span>
    <span>var</span> <span>args</span> <span>=</span> <span>Array</span><span>.</span><span>prototype</span><span>.</span><span>slice</span><span>.</span><span>call</span><span>(</span><span>arguments</span><span>,</span> <span>1</span><span>)</span>
    <span>return</span> <span>function</span><span>(</span><span>x</span><span>)</span> <span>{</span>
        <span>return</span> <span>f</span><span>.</span><span>apply</span><span>(</span><span>null</span><span>,</span> <span>[</span><span>x</span><span>].</span><span>concat</span><span>(</span><span>args</span><span>))</span>
    <span>}</span>
<span>}</span>

<span>// == ["bob", "john"]</span>
<span>function</span> <span>friendsNames</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>return</span> <span>friends</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span>
            <span>.</span><span>map</span><span>(</span><span>applyr</span><span>(</span><span>lookup</span><span>,</span> <span>"name"</span><span>))</span> <span>// we can use normal lookup!</span>
<span>}</span>
</code></pre>
<p><code>applyr(lookup, "name")</code>函数返回的函数只接受一个参数——那个对象——返回对象的名称。我们不再需要反转任何东西：我们可以按任何顺序接受参数。 </p>
<p>偏函数用法需要对一些常见的功能定义各种不同的函数，就像<code>lt</code>函数，但这正是我们的目的。你可以以偏函数用法把<code>lt</code>函数既用于<code>count</code>函数，也可用于<code>Array.filter</code>函数。它们可以复用，可以组合使用。</p>
<h3><a title="Function Composition" href="http://en.wikipedia.org/wiki/Function_composition_%28computer_science%29">函数组合</a></h3>
<p>在之前的例子中，我们遍历了数组两次，一次用来获取users，一次为了获取names。如果能在一次map映射操作中同时做这两件事情，效率会高很多。</p>
<pre><code><span>function</span> <span>friendsNames</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>return</span> <span>user</span><span>.</span><span>friendIds</span><span>.</span><span>map</span><span>(</span><span>function</span><span>(</span><span>id</span><span>)</span> <span>{</span>
        <span>var</span> <span>friend</span> <span>=</span> <span>lookup</span><span>(</span><span>usersById</span><span>,</span> <span>id</span><span>)</span>
        <span>return</span> <span>lookup</span><span>(</span><span>friend</span><span>,</span> <span>"name"</span><span>)</span>
    <span>})</span>
<span>}</span>
</code></pre>
<p>我们得到首次<code>lookup</code>的结果，把它第二次传入<code>lookup</code>。<a title="Function Composition" href="http://en.wikipedia.org/wiki/Function_composition_%28computer_science%29">函数组合</a>意思是串联多个函数，组成一个新的函数，每一次串联都是把前一个函数的输出当作下一个函数的输入。</p>
<p>让我们来写一个能这样运转的高阶函数，利用它把<code>friendsNames</code>函数重写成一个只需要单次map操作的函数。需要注意的是，函数串联的执行顺序是从右到左的，就跟你写出<code>f(g(x))</code>这样的代码的运行方式一样。</p>
<pre><code><span>function</span> <span>compose</span><span>(</span><span>f</span><span>,</span> <span>g</span><span>)</span> <span>{</span>
    <span>return</span> <span>function</span><span>(</span><span>x</span><span>)</span> <span>{</span>
        <span>return</span> <span>f</span><span>(</span><span>g</span><span>(</span><span>x</span><span>))</span>
    <span>}</span>
<span>}</span>

<span>function</span> <span>friendsNames</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>return</span> <span>user</span><span>.</span><span>friendIds</span><span>.</span><span>map</span><span>(</span><span>compose</span><span>(</span><span>applyr</span><span>(</span><span>lookup</span><span>,</span> <span>"name"</span><span>),</span> <span>apply</span><span>(</span><span>lookup</span><span>,</span> <span>usersById</span><span>)))</span>
<span>}</span>
</code></pre>
<p>对数组的遍历只进行了一次，只使用一次map操作，跟我们头一个例子一样。</p>
<p>我们不能使用我们写出的<code>friends</code>函数，因为它既包含了如何取出一个friend的业务逻辑，也包含了map操作。<strong><code>friends</code>函数是不能复用的，它的职责太多了——它是针对特定事物的</strong>。如果你们再写一个<code>friend</code>函数，让它只map一个friend，写一个<code>name</code>函数，让它返回对象的名称呢？</p>
<pre><code><span>var</span> <span>friend</span> <span>=</span> <span>lookup</span> <span>// lookup 恰巧能干我们想要的事情。</span>
<span>var</span> <span>name</span> <span>=</span> <span>applyr</span><span>(</span><span>lookup</span><span>,</span> <span>"name"</span><span>)</span>

<span>function</span> <span>friendsNames</span><span>(</span><span>usersById</span><span>,</span> <span>user</span><span>)</span> <span>{</span>
    <span>// this line is now more semantic. </span>
    <span>return</span> <span>user</span><span>.</span><span>friends</span><span>.</span><span>map</span><span>(</span><span>compose</span><span>(</span><span>name</span><span>,</span> <span>apply</span><span>(</span><span>friend</span><span>,</span> <span>usersById</span><span>)))</span>
<span>}</span>
</code></pre>
<p>相较于定义一个既包含转换操作，又包含遍历操作的<code>friends</code>函数，我们只定义了一个可做转换操作的<code>friend</code>函数，而我们已经有了<code>map</code>函数为我做变换操作。<code>friend</code>函数比<code>friends</code>函数更具复用性，因为它包含更少的特定业务逻辑，能在更多的情形中使用。</p>
<p>在<a title="Compose: functions as building blocks" href="http://javascriptweblog.wordpress.com/2010/04/14/compose-functions-as-building-blocks/">这里</a>你能找到更多的关于JavaScript里函数组合的信息。</p>
<h3>函数式和功能单一化让你的代码库更整洁</h3>
<p>我发现我的很多的JavaScript代码都是从无到有自己写出来的。这不仅仅是说比起使用现成的程序包要效率低，它还会暗藏更多的bug，更难阅读和维护。使用高阶函数和偏函数用法，我们可以写出可复用的程序库，每个函数都精准的对应解决它们能解决的一部分问题。</p>
<p>随着时间的推移，项目会变得越来越复杂，各部分越来越耦合，如果我们拥有的是一个能够各自独立测试不依赖的程序库，我们的项目会从中受益，变得更健康，更稳定。</p>
<p>
<ol>
<li>
<p>一种宽泛的组合。并不特指函数或对象组合，只是一种你用小东西组建大东西的思想。<a href="http://www.aqee.net/#fnref:1">↩</a></p>
</li>
<li>
<p>“Matching functions”被称作<a title="Predicate" href="http://en.wikipedia.org/wiki/Predicate_%28mathematical_logic%29">predicates</a>，但我这里不想引入新的编程术语。<a href="http://www.aqee.net/#fnref:2">↩</a></p>
</li>
<li>
<p><a title="Partial Application in Javascript" href="http://msdn.microsoft.com/en-us/scriptjunkie/gg575560">这里</a>有更通用的<code>apply</code>实现。<a href="http://www.aqee.net/#fnref:3">↩</a></p>
</li>
</ol>
<hr>本文来自<a href="http://www.aqee.net">外刊IT评论网</a>(<a href="http://www.aqee.net">www.aqee.net</a>)，原始地址：<a href="http://www.aqee.net/learn-you-a-haskell-for-great-good/" rel="bookmark">为什么我们要学习Haskell这样的编程语言</a><br><img src="http://www1.feedsky.com/t1/628090895/aqee-net/feedsky/s.gif?r=http://www.aqee.net/learn-you-a-haskell-for-great-good/" border="0" height="0" width="0"></p>