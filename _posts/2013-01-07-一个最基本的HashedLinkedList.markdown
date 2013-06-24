---
layout: post
title:  "一个最基本的HashedLinkedList"
date:   2013-01-07 23:52:50
author: 老赵
categories: program
---

## 一个最基本的HashedLinkedList
### by 老赵
### at 2013-01-07 23:52:50
### original <http://blog.zhaojie.me/2013/01/a-basic-hashed-linked-list.html>

<p><a href="https://github.com/JeffreyZhao/Tmc">Tmc</a>的初衷是补充一些常用的数据结构，例如<a href="http://blog.zhaojie.me/2012/12/nullablekey-nullable-key-in-dictionary.html">对<code>null</code>作为字典键的支持</a>，以及带有一个<a href="http://blog.zhaojie.me/2013/01/reading-dotnet-source-code.html">额外<code>Remove</code>方法的<code>HashDictionary</code></a>。但是，其实我创建Tmc项目的“初衷”却是<code>HashedLinkedList</code>。.NET BCL中已经有一个<code>LinkedList</code>，这是一个双向链表。说起来，我之前在面试中经常会提出一系列数据结构的基础问题，其中便包含<code>LinkedList</code>，我会问各个操作的时间复杂度，以及如何改进它们。例如，如何将它的<code>Remove</code>操作优化成O(1)的时间复杂度？最容易想到的做法便是使用一个字典来记录元素到特定<code>LinkedListNode</code>的映射关系。这种模式实在过于常见，所以便有了个特别的名称，叫做<code>HashedLinkedList</code>。</p>

<p>好吧，其实在<a href="http://www.itu.dk/research/c5/">C5</a>中已经有了一个<a href="https://github.com/sestoft/C5/blob/master/C5/linkedlists/HashedLinkedList.cs"><code>HashedLinkedList</code>类</a>，但是有重大缺陷，例如没有暴露出<code>LinkedListNode</code>这个类型。我们来看下.NET BCL中<code>LinkedList</code>的一些方法：</p>

<pre><span style="color:blue">public class </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt;
{
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedList</span>&lt;T&gt; List { <span style="color:blue">get</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; Next { <span style="color:blue">get</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; Previous { <span style="color:blue">get</span>; }
    <span style="color:blue">public </span>T Value { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}

<span style="color:blue">public class </span><span style="color:#2b91af">LinkedList</span>&lt;T&gt;
{
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; AddFirst(T value);
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; AddLast(T value);
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; AddBefore(<span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; node, T value);
    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; AddAfter(<span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; node, T value);
    <span style="color:green">// ...
</span>}</pre>

<p>.NET BCL的<code>LinkedList</code>会暴露出配套的<code>LinkedListNode</code>，这样我们便可以保留<code>LinkedListNode</code>对象，并执行O(1)实现复杂度的快速插入。此外，我们还可以根据<code>LinkedListNode</code>的<code>Next</code>和<code>Previous</code>进行前后遍历，这才是真正的“链表”。之前有人问我，为什么<code>LinkedList</code>不实现<code>IList</code>接口呢？我的看法就是<code>LinkedList</code>和<code>List</code>在表现上的区别实在太大，而<code>IList</code>的含义是后者，因此就放过<code>LinkedList</code>了。</p>

<p>其实，完全从外部实现一个<code>HashedLinkedList</code>（的部分功能）也不难，我们只要封装一个<code>LinkedList</code>和一个<code>Dictionary</code>即可。例如：</p>

<pre><span style="color:blue">public class </span><span style="color:#2b91af">HashedLinkedList</span>&lt;T&gt; {
    <span style="color:blue">private readonly </span><span style="color:#2b91af">LinkedList</span>&lt;T&gt; _list = <span style="color:blue">new </span><span style="color:#2b91af">LinkedList</span>&lt;T&gt;();
    <span style="color:blue">private readonly </span><span style="color:#2b91af">Dictionary</span>&lt;T, <span style="color:#2b91af">LinkedListNode</span>&lt;T&gt;&gt; _nodes = <span style="color:blue">new </span><span style="color:#2b91af">Dictionary</span>&lt;T, <span style="color:#2b91af">LinkedListNode</span>&lt;T&gt;&gt;();

    <span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; AddLast(T value) {
        <span style="color:blue">var </span>node = _list.AddLast(value);
        _nodes.Add(value, node);

        <span style="color:blue">return </span>node;
    } 

    <span style="color:blue">public void </span>Remove(T value) {
        <span style="color:blue">var </span>node = _nodes[value];
        _nodes.Remove(value);
        _list.Remove(node);
    }
}</pre>

<p>当然，这种“粗制滥造”的<code>HashedLinkedList</code>类完全不能作为通用的数据结构来使用，但假如是普通项目需要，这么做往往也够了。而且事实上一个“通用”的<code>HashLinkedList</code>的确也会遇到很多决策问题，例如：是否支持相同元素？我们知道.NET BCL中的<code>LinkedList</code>是支持相同元素的（事实上<code>LinkedListNode</code>的<code>Value</code>属性可读可写），但显然上面这个粗糙的<code>HashedLinkedList</code>便不支持。</p>

<p>有人可能会说，支持相同元素很容易呀，只要用<code>Dictionary&lt;T, List&lt;LinkedListNode&gt;&gt;</code>或类似的结构，将一个值映射到多个<code>LinkedListNode</code>就行了。这可能会解决一部分问题，但事情远没有那么简单。例如.NET BCL的<code>LinkedList</code>还有<code>Find(T value)</code>和<code>FindLast(T value)</code>这两个方法，分别用来找出“第一个”和“最后一个”与<code>value</code>相等的<code>LinkedListNode</code>。没错，<code>LinkedList</code>是有顺序的，假如我们要保留<code>Find</code>和<code>FindLast</code>的语义不变，就没法提供O(1)的时间复杂度的实现——不信你试试看？</p>

<p>假如我们还是用遍历的方法来实现<code>Find</code>和<code>FindLast</code>，这便失去了<code>HashedLinkedList</code>的意义。但是反过来说，在很多时候，我们可以确定不需要其支持相同元素，或者无所谓<code>Find</code>得到的是其中的哪个节点呐。我估计这也是.NET BCL中不提供<code>HashedLinkedList</code>的缘故吧，既然没法“通用”，那么就由开发人员自己根据需要来组合吧。</p>

<p>目前在Tmc中的<a href="https://github.com/JeffreyZhao/Tmc/blob/2e451733fe20bc57ea1c194ceba6323c4c3d4671/src/Tmc/HashedLinkedList.cs"><code>HashedLinkedList</code></a>便不支持保存相同元素，它只是将.NET BCL的<code>LinkedList</code>几乎原封不动地复制过来，然后增加一些简单的功能。.NET BCL的<code>LinkedList</code>的实现为“双向循环链表”，不同于某些数据结构书上使用<code>head</code>和<code>tail</code>来保存首尾节点，“双向循环列表”只需要记录头节点，而尾节点只需要简单的访问“头节点”的“前一个节点”即可：</p>

<pre><span style="color:blue">private </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; _head;

<span style="color:blue">public </span><span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; Last
{
    <span style="color:blue">get </span>{ <span style="color:blue">return </span>_head == <span style="color:blue">null </span>? <span style="color:blue">null </span>: _head._prev; }
}</pre>

<p>使用“双向循环列表”的好处在于实现特别简单，各种修改操作都能统一为三个方法：</p>

<pre><span style="color:blue">private void </span>InsertNodeBefore(<span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; node, <span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; newNode);
<span style="color:blue">private void </span>InsertNodeToEmptyList(<span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; newNode);
<span style="color:blue">private void </span>RemoveNode(<span style="color:#2b91af">LinkedListNode</span>&lt;T&gt; node);</pre>

<p>因此，在此基础上实现的<code>HashedLinkedList</code>也只需要修改这三个方法即可，几乎是瞬间的事情。当然，假如只是这样的<code>HashedLinkedList</code>，我将其放入Tmc项目的意义也不大。因此，不久的将来我会删除这个类，并提供额外的数据结构来应对不同的需求：</p>

<ol>
  <li>不支持相同元素。 </li>

  <li>支持相同元素，但不保证顺序，效率比前者略低。 </li>

  <li>支持相同元素，并保证顺序，效率比前两者低，但肯定要高于.NET BCL中自带的<code>LinkedList</code>。 </li>
</ol>

<p>这种数据结构（亦或是三种？），我就称之为<code>IndexedLinkedList</code>吧。假如是你，你会怎么做呢？</p><p><a href="http://blog.zhaojie.me/2013/01/a-basic-hashed-linked-list.html">继续阅读 »</a></p>