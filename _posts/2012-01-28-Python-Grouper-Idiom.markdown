---
layout: post
title:  "Python Grouper Idiom"
date:   2012-01-28 22:50:36
author: Xueqiao Xu
categories: program
---

## Python Grouper Idiom
### by Xueqiao Xu
### at 2012-01-28 22:50:36
### original <http://typedef.me/2012/01/28/python-grouper-idiom>

<p>在 StackOverflow 上有人问了<a href="http://stackoverflow.com/questions/9020843/how-to-convert-a-mac-number-to-mac-string">这样一个问题</a>：

</p>
<blockquote><p>在 Python 中，如何将形如 &quot;00163e2fbab7&quot; 这样的一个 MAC 地址转换成 &quot;00:16:3e:2f:ba:b7&quot; 的形式。

</p></blockquote>
<p>面对这个简单的问题，普通青年给出的答案或许是：

</p>
<div><pre><span>&#39;:&#39;</span><span>.</span><span>join</span><span>([</span><span>address</span><span>[</span><span>i</span><span>:</span><span>i</span><span>+</span><span>2</span><span>]</span> <span>for</span> <span>i</span> <span>in</span> <span>range</span><span>(</span><span>0</span><span>,</span> <span>12</span><span>,</span> <span>2</span><span>)])</span>
</pre></div>

<p>但是，文艺青年 <a href="http://stackoverflow.com/users/190597/unutbu">@unutbu</a> 却给出了<a href="http://stackoverflow.com/a/9020930/707911">一个不同寻常的解法</a>：

</p>
<div><pre><span>&#39;:&#39;</span><span>.</span><span>join</span><span>(</span><span>&#39;&#39;</span><span>.</span><span>join</span><span>(</span><span>pair</span><span>)</span> <span>for</span> <span>pair</span> <span>in</span> <span>zip</span><span>(</span><span>*</span><span>[</span><span>iter</span><span>(</span><span>addr</span><span>)]</span><span>*</span><span>2</span><span>))</span>
</pre></div>

<p>这个解法第一眼看上去似乎不是那么好理解，但是一旦细细琢磨进去，发现了其中的巧妙之处，就不由得大呼精彩。

</p>
<p>这个答案中总共有两点精彩之处，分别为：

</p>
<ol><li>Generator Expression</li><li>Grouper Idiom </li></ol>
<h3>Generator Expression</h3>
<p>也许很多人第一眼看到那个答案，疑惑的一点是那个列表解析两旁怎么没有方括号<code>[]</code>。 
其实这是利用了一个从 Python2.4 开始就存在，却经常为人所忽略的技巧： <a href="http://www.python.org/dev/peps/pep-0289/">Generator Expression</a> (生成器表达式)。

</p>
<ul><li>在语法层面，生成器表达式的与列表解析相同，唯一的不同之处在于其是被<code>()</code>括起来而不是<code>[]</code>。</li><li>在底层的实现层面，生成器表达式不像列表解析那般需要在内存中生成完整的列表，而是使用生成器的形式按需访问元素。 </li></ul>
<p>以求平方和的语句为例：

</p>
<div><pre><span>sum</span><span>([</span><span>x</span> <span>*</span> <span>x</span> <span>for</span> <span>x</span> <span>in</span> <span>range</span><span>(</span><span>10</span><span>)])</span>
</pre></div>

<p>在上面这个例子中，普通的列表解析版本会先生成一个完整的平方数列表： <code>[1, 4, 9, 16 ...]</code>，然后再将 <code>sum</code> 函数应用在这个列表上。


</p>
<p>生成器表达式的版本是这样:

</p>
<div><pre><span>sum</span><span>(</span><span>x</span> <span>*</span> <span>x</span> <span>for</span> <span>x</span> <span>in</span> <span>range</span><span>(</span><span>10</span><span>))</span>
</pre></div>

<p>在这个版本中，<code>sum</code> 括号里面产生的是一个生成器，而非一个列表。<code>sum</code> 函数使用这个生成器来逐个获取元素并进行求和。

</p>
<p>我们也可以直接对这个生成器进行操作，如下：

</p>
<div><pre><span>&gt;&gt;&gt;</span> <span>g</span> <span>=</span> <span>(</span><span>x</span> <span>*</span> <span>x</span> <span>for</span> <span>x</span> <span>in</span> <span>range</span><span>(</span><span>10</span><span>))</span>
<span>&gt;&gt;&gt;</span> <span>g</span>
<span>&lt;</span><span>generator</span> <span>object</span> <span>&lt;</span><span>genexpr</span><span>&gt;</span> <span>at</span> <span>0x92d1324</span><span>&gt;</span>
<span>&gt;&gt;&gt;</span> <span>g</span><span>.</span><span>next</span><span>()</span>
<span>0</span>
<span>&gt;&gt;&gt;</span> <span>g</span><span>.</span><span>next</span><span>()</span>
<span>1</span>
<span>&gt;&gt;&gt;</span> <span>g</span><span>.</span><span>next</span><span>()</span>
<span>4</span>
<span>&gt;&gt;&gt;</span> <span>g</span><span>.</span><span>next</span><span>()</span>
<span>9</span>
</pre></div>

<p>生成器会记住上次返回时的位置，在每次调用 <code>next</code> 方法后继续计算下一个需要的值。

</p>
<p>因此，当数据规模很大时，使用列表解析将产生不可忽视的内存开销。而此时使用生成器表达式则可以避免这个问题。

</p>
<h3>Grouper Idiom</h3>
<p>关于一开始所说的字符串问题，其核心便在于如何将原始的字符串按照两个字符一组的分开来，Grouper Idiom 的应用则是这个答案的精华所在。

</p>
<p>再看一眼一开始时的代码：

</p>
<div><pre><span>addr</span> <span>=</span> <span>&quot;00163e2fbab7&quot;</span>
<span>&#39;:&#39;</span><span>.</span><span>join</span><span>(</span><span>&#39;&#39;</span><span>.</span><span>join</span><span>(</span><span>pair</span><span>)</span> <span>for</span> <span>pair</span> <span>in</span> <span>zip</span><span>(</span><span>*</span><span>[</span><span>iter</span><span>(</span><span>addr</span><span>)]</span><span>*</span><span>2</span><span>))</span>
</pre></div>

<p>我们将这个代码中 <code>zip</code> 的那段提取出来：

</p>
<div><pre><span>zip</span><span>(</span><span>*</span><span>[</span><span>iter</span><span>(</span><span>addr</span><span>)]</span><span>*</span><span>2</span><span>)</span>
</pre></div>

<p>它产生的是这样的一个列表，完美的解决了分组问题：

</p>
<div><pre><span>[(</span><span>&#39;0&#39;</span><span>,</span> <span>&#39;0&#39;</span><span>),</span> <span>(</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;6&#39;</span><span>),</span> <span>(</span><span>&#39;3&#39;</span><span>,</span> <span>&#39;e&#39;</span><span>),</span> <span>(</span><span>&#39;2&#39;</span><span>,</span> <span>&#39;f&#39;</span><span>),</span> <span>(</span><span>&#39;b&#39;</span><span>,</span> <span>&#39;a&#39;</span><span>),</span> <span>(</span><span>&#39;b&#39;</span><span>,</span> <span>&#39;7&#39;</span><span>)]</span>
</pre></div>

<p>那这一步到底是如何工作的呢？

</p>
<p>对于熟悉函数式编程的同学肯定对 <code>zip</code> 不会陌生，它可以将一个或多个可迭代的容器像拉链一般组合：

</p>
<div><pre><span>&gt;&gt;&gt;</span> <span>zip</span><span>([</span><span>1</span><span>,</span><span>2</span><span>,</span><span>3</span><span>],</span> <span>[</span><span>4</span><span>,</span><span>5</span><span>,</span><span>6</span><span>],</span> <span>[</span><span>7</span><span>,</span><span>8</span><span>,</span><span>9</span><span>])</span>
<span>[(</span><span>1</span><span>,</span> <span>4</span><span>,</span> <span>7</span><span>),</span> <span>(</span><span>2</span><span>,</span> <span>5</span><span>,</span> <span>8</span><span>),</span> <span>(</span><span>3</span><span>,</span> <span>6</span><span>,</span> <span>9</span><span>)]</span>
</pre></div>

<p>而括号内部的第一个星号<code>*</code>用于分拆一个列表(unpack a list)。当作用于函数参数上时，其效果类似于函数式编程里面的<code>apply</code>，亦即 <code>f(*[a,b,c])</code> 与 <code>f(a,b,c)</code> 等价。

</p>
<pre><code>&gt;&gt;&gt; zip(*[[1,2,3], [4,5,6], [7,8,9]])
[(1, 4, 7), (2, 5, 8), (3, 6, 9)]</code></pre>
<p>于是，<code>zip(*[it]*2)</code> 等价于 <code>zip(*[it, it])</code> 等价于 <code>zip(it, it)</code>

</p>
<div><pre><span>&gt;&gt;&gt;</span> <span>it</span> <span>=</span> <span>iter</span><span>(</span><span>&quot;00163e2fbab7&quot;</span><span>)</span>
<span>&gt;&gt;&gt;</span> <span>zip</span><span>(</span><span>it</span><span>,</span> <span>it</span><span>)</span>
<span>[(</span><span>&#39;0&#39;</span><span>,</span> <span>&#39;0&#39;</span><span>),</span> <span>(</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;6&#39;</span><span>),</span> <span>(</span><span>&#39;3&#39;</span><span>,</span> <span>&#39;e&#39;</span><span>),</span> <span>(</span><span>&#39;2&#39;</span><span>,</span> <span>&#39;f&#39;</span><span>),</span> <span>(</span><span>&#39;b&#39;</span><span>,</span> <span>&#39;a&#39;</span><span>),</span> <span>(</span><span>&#39;b&#39;</span><span>,</span> <span>&#39;7&#39;</span><span>)]</span>
</pre></div>

<p>我们先要了解一个事实：<code>zip</code> 从两个列表中取值时，是同真的“拉链”类似，按需取值，亦即每次分别从两个列表取一个值加到一起生成一个元组，再分别从两个列表取一个值生成一个元组，如此下去。

</p>
<p>再注意此处 <code>zip</code> 的两个参数都是同一个迭代器，因此这里 <code>zip</code> 的时候每次会取出相邻的两个值。

</p>
<p>关于这里<code>zip</code>和迭代器的组合用法，我们可以推而广之，提炼出来一个通用的过程：


</p>
<pre><code>先用 `iter` 函数生成原始容器的一个迭代器，将这个迭代器放入一个列表中，并将列表变为原先的 n 倍(n 为每个分组的长度)，然后分拆列表并将其应用于`zip`函数上，用于生成分组。</code></pre>
<p>这便是 Grouper Idiom。我们再将其变成 Python 代码，如下（取自 <a href="http://docs.python.org/library/itertools.html#recipes">itertools文档</a>）：

</p>
<div><pre><span>from</span> <span>itertools</span> <span>import</span> <span>*</span>

<span>def</span> <span>grouper</span><span>(</span><span>n</span><span>,</span> <span>iterable</span><span>,</span> <span>fillvalue</span><span>=</span><span>None</span><span>):</span>
    <span>args</span> <span>=</span> <span>[</span><span>iter</span><span>(</span><span>iterable</span><span>)]</span> <span>*</span> <span>n</span>
    <span>return</span> <span>izip_longest</span><span>(</span><span>fillvalue</span><span>=</span><span>fillvalue</span><span>,</span> <span>*</span><span>args</span><span>)</span>
</pre></div>

<p>这里使用 <code>izip_longest</code> 而不是 <code>zip</code>，使其可以设置默认填充值，并返回迭代器而不是列表。

</p>
<p>示例：将 &quot;ABCDEFG&quot; 这个字符串以每组长度为 3 分组，不足时填充 &#39;x&#39; ：

</p>
<div><pre><span>&gt;&gt;&gt;</span> <span>list</span><span>(</span><span>grouper</span><span>(</span><span>3</span><span>,</span> <span>&#39;ABCDEFG&#39;</span><span>,</span> <span>&#39;x&#39;</span><span>))</span>
<span>[(</span><span>&#39;A&#39;</span><span>,</span> <span>&#39;B&#39;</span><span>,</span> <span>&#39;C&#39;</span><span>),</span> <span>(</span><span>&#39;D&#39;</span><span>,</span> <span>&#39;E&#39;</span><span>,</span> <span>&#39;F&#39;</span><span>),</span> <span>(</span><span>&#39;G&#39;</span><span>,</span> <span>&#39;x&#39;</span><span>,</span> <span>&#39;x&#39;</span><span>)]</span>
</pre></div>