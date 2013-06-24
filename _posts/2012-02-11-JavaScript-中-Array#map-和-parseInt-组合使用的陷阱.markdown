---
layout: post
title:  "JavaScript 中 Array#map 和 parseInt 组合使用的陷阱"
date:   2012-02-11 13:15:05
author: Xueqiao Xu
categories: program
---

## JavaScript 中 Array#map 和 parseInt 组合使用的陷阱
### by Xueqiao Xu
### at 2012-02-11 13:15:05
### original <http://typedef.me/2012/02/11/javascript-array-map-parseint-pitfall>

<p>将一个形如 [&#39;1&#39;, &#39;2&#39;, &#39;3&#39;] 的数组转换为 [1, 2, 3] ，很多人也许会不加思索的写出下面的代码：

</p>
<div><pre><span>[</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;2&#39;</span><span>,</span> <span>&#39;3&#39;</span><span>].</span><span>map</span><span>(</span><span>parseInt</span><span>);</span>
</pre></div>

<p>上面这个代码看上去十分正确，而且非常优美，没有任何多余的噪音。但其返回结果却令人吃惊：

</p>
<div><pre><span>&gt;</span> <span>[</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;2&#39;</span><span>,</span> <span>&#39;3&#39;</span><span>].</span><span>map</span><span>(</span><span>parseInt</span><span>);</span>
<span>[</span> <span>1</span><span>,</span> <span>NaN</span><span>,</span> <span>NaN</span> <span>]</span>
</pre></div>

<blockquote><p>“WTF ！！！！为什么第一个元素是正确的，而后面的会是 <code>NaN</code> ？”

</p></blockquote>
<p>然后继续实验：

</p>
<div><pre><span>&gt;</span> <span>[</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;1&#39;</span><span>,</span> <span>&#39;1&#39;</span><span>,</span> <span>&#39;1&#39;</span><span>].</span><span>map</span><span>(</span><span>parseInt</span><span>);</span>
<span>[</span> <span>1</span><span>,</span> <span>NaN</span><span>,</span> <span>1</span><span>,</span> <span>1</span> <span>]</span>
</pre></div>

<blockquote><p>“喂喂，中间的那位 <code>NaN</code> 同学，你走错片场了吧。”

</p></blockquote>
<p>究其原因，其实是由两部分组成的：

</p>
<ol><li><code>map</code> 的回调函数的第二个参数为数组下标</li><li><code>parseInt</code> 的第二个参数为转换进制 </li></ol>
<p>看到上面这两点，熟练 js 的同学应该就会恍然大悟了。如果还没明白的话也没关系，下面是详细的解释：

</p>
<h3>Array#map</h3>
<p>首先，map 方法所期待的回调函数在调用时接受三个参数，分别为：

</p>
<ol><li>当前元素的值</li><li>当前元素的索引（下标）</li><li>被遍历的数组 </li></ol>
<p>一个典型的 map 实现如下：

</p>
<div><pre><span>Array</span><span>.</span><span>prototype</span><span>.</span><span>map</span> <span>=</span> <span>function</span> <span>map</span><span>(</span><span>fun</span><span>)</span> <span>{</span>
  <span>var</span> <span>length</span> <span>=</span> <span>this</span><span>.</span><span>length</span> <span>&gt;&gt;&gt;</span> <span>0</span><span>;</span>
  <span>var</span> <span>result</span> <span>=</span> <span>Array</span><span>(</span><span>length</span><span>);</span>
  <span>var</span> <span>thisp</span> <span>=</span> <span>arguments</span><span>[</span><span>1</span><span>];</span>

  <span>for</span> <span>(</span><span>var</span> <span>i</span> <span>=</span> <span>0</span><span>;</span> <span>i</span> <span>&lt;</span> <span>length</span><span>;</span> <span>i</span><span>++</span><span>)</span> <span>{</span>
    <span>if</span> <span>(</span><span>i</span> <span>in</span> <span>this</span><span>)</span>
      <span>result</span><span>[</span><span>i</span><span>]</span> <span>=</span> <span>fun</span><span>.</span><span>call</span><span>(</span><span>thisp</span><span>,</span> <span>this</span><span>[</span><span>i</span><span>],</span> <span>i</span><span>,</span> <span>this</span><span>);</span>
  <span>}</span>
  <span>return</span> <span>result</span><span>;</span>
<span>};</span>
</pre></div>

<p>调用示例：

</p>
<div><pre><span>[</span><span>&#39;a&#39;</span><span>,</span> <span>&#39;b&#39;</span><span>,</span> <span>&#39;c&#39;</span><span>].</span><span>map</span><span>(</span><span>function</span><span>(</span><span>v</span><span>,</span> <span>i</span><span>,</span> <span>a</span><span>)</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>v</span><span>,</span> <span>i</span><span>,</span> <span>a</span><span>);</span>
<span>});</span>
<span>// prints</span>
<span>// a 0 [ &#39;a&#39;, &#39;b&#39;, &#39;c&#39; ]</span>
<span>// b 1 [ &#39;a&#39;, &#39;b&#39;, &#39;c&#39; ]</span>
<span>// c 2 [ &#39;a&#39;, &#39;b&#39;, &#39;c&#39; ]</span>
</pre></div>

<h3>parseInt</h3>
<p><code>parseInt</code> 可以接受两个参数，其中第二个参数（可选）为转换的进制

</p>
<p>例如，以 8 进制解析 12

</p>
<div><pre><span>&gt;</span> <span>parseInt</span><span>(</span><span>&#39;12&#39;</span><span>,</span> <span>8</span><span>)</span>
<span>10</span>
</pre></div>

<p>而当第二个参数不存在或为<code>0</code>时：

</p>
<ol><li>如果字符串以 <code>0x</code> 或 <code>0X</code> 开头，则以 16 进制解析</li><li>如果以 <code>0</code> 开头，则以 8 进制解析</li><li>否则以 10 进制解析</li><li>解析失败返回 NaN </li></ol>
<p>因此，当我们组合使用 <code>map</code> 和 <code>parseInt</code> 的时候，每个元素的下标成为了解析它时的进制：

</p>
<div><pre><span>[</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;2&#39;</span><span>,</span> <span>&#39;3&#39;</span><span>].</span><span>map</span><span>(</span><span>function</span><span>(</span><span>v</span><span>,</span> <span>i</span><span>)</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;parseInt(&quot;&#39;</span> <span>+</span> <span>v</span> <span>+</span> <span>&#39;&quot;, &#39;</span> <span>+</span> <span>i</span> <span>+</span> <span>&#39;) gives &#39;</span> <span>+</span> <span>parseInt</span><span>(</span><span>v</span><span>,</span> <span>i</span><span>));</span>
<span>});</span>
<span>// prints</span>
<span>// parseInt(&quot;1&quot;, 0) gives 1</span>
<span>// parseInt(&quot;2&quot;, 1) gives NaN</span>
<span>// parseInt(&quot;3&quot;, 2) gives NaN</span>
</pre></div>

<p>注意上面结果的最后两项，以1进制来解析 <code>&quot;2&quot;</code> 和以2进制来解析 <code>&quot;3&quot;</code> 都是失败的，因此返回 <code>NaN</code>。

</p>
<p>正确的做法应当为：

</p>
<div><pre><span>[</span><span>&#39;1&#39;</span><span>,</span> <span>&#39;2&#39;</span><span>,</span> <span>&#39;3&#39;</span><span>].</span><span>map</span><span>(</span><span>function</span><span>(</span><span>v</span><span>)</span> <span>{</span> <span>// 只传入元素的值</span>
  <span>return</span> <span>parseInt</span><span>(</span><span>v</span><span>,</span> <span>10</span><span>);</span> <span>// 指定以 10 为底，这是一个好习惯</span>
<span>})</span>
</pre></div>

<p>至此，原因应当相当清晰了。同时，也当记住，除了 <code>parseInt</code> 外，任何可以接受两个或两个以上参数的函数在配合 <code>map</code> 使用的时候都应当多加注意。</p>