---
layout: post
title:  "远程调用服务(RPC)和消息(Message Queue)对比及其适用/不适用场合"
date:   2013-02-11 02:00:17
author: 李 鼎
categories: program
---

## 远程调用服务(RPC)和消息(Message Queue)对比及其适用/不适用场合
### by 李 鼎
### at 2013-02-11 02:00:17
### original <http://rdc.taobao.com/team/jm/archives/2763>

<p>在阿里的平台技术部参与开发了Dubbo（远程调用服务）和Napoli（消息解决方案），又给网站应用支持这2个产品很长一段时间，了解了这2个产品的实现及应用对这两个产品的用法。</p>
<p>大部分情况下，“给定场景下应该使用这两个产品中哪个”这个问题，大家都会容易决定，而且不需要多少讨论。</p>
<p>我为什么要拿出来讨论一下：</p>
<ul>
<li>一些场景会比较模糊，觉得都可以使用。这时需要知道产品缺点，而不是看到优势。</li>
<li>一些新人会觉得产品功能是可以替换的，要给说明一下。</li>
</ul>
<p>这里简单说一下两者的区别。</p>
<h1>系统结构</h1>
<pre><code>RPC系统结构：

+----------+     +----------+
| Consumer | &lt;=&gt; | Provider |
+----------+     +----------+
Consumer调用的Provider提供的服务。

Message Queue系统结构：

+--------+     +-------+     +----------+
| Sender | &lt;=&gt; | Queue | &lt;=&gt; | Receiver |
+--------+     +-------+     +----------+
Sender发送消息给Queue；Receiver从Queue拿到消息来处理。</code></pre>
<h1>功能特点</h1>
<p>在架构上，RPC和Message的差异点是，Message有一个中间结点Message Queue，可以把消息存储。</p>
<h2>消息的特点</h2>
<ul>
<li>Message Queue把请求的压力保存一下，逐渐释放出来，让处理者按照自己的节奏来处理。</li>
<li>Message Queue引入一下新的结点，让系统的可靠性会受Message Queue结点的影响。</li>
<li>Message Queue是<strong>异步</strong><em>单向</em>的消息。发送消息设计成是<em>不需要</em><strong>等待</strong>消息处理的完成。</li>
</ul>
<p>所以对于有同步返回需求，Message Queue则方向。</p>
<h2>PRC的特点</h2>
<ul>
<li>同步调用，对于要等待返回结果/处理结果的场景，RPC是可以非常自然直觉的使用方式。<br>
# RPC也可以是异常调用。</li>
<li>由于等待结果，Consumer（Client）会有线程消耗。</li>
</ul>
<p>如果以异步RPC的方式使用，Consumer（Client）线程消耗可以去掉。但不能做到像消息一样暂存消息/请求，压力会直接传导到服务Provider。</p>
<h1>适用场合说明</h1>
<ul>
<li>希望同步得到结果的场合，RPC合适。</li>
<li>希望使用简单，则RPC；RPC操作基于接口，使用简单，使用方式模拟本地调用。异步的方式编程比较复杂。</li>
<li>不希望发送端（RPC Consumer、Message Sender）受限于处理端（RPC Provider、Message Receiver）的速度时，使用Message Queue。</li>
</ul>
<p>随着业务增长，有的处理端处理量会成为瓶颈，会进行同步调用到异步消息的改造。</p>
<p>这样的改造实际上有调整业务的使用方式。</p>
<p>比如原来一个操作页面提交后就下一个页面会看到处理结果；改造后异步消息后，下一个页面就会变成“操作已提交，完成后会得到通知”。</p>
<h1>不适用场合说明</h1>
<p>RPC同步调用使用Message Queue来传输调用信息。 上面分析可以知道，这样的做法，发送端是在等待，同时占用一个中间点的资源。变得复杂了，但没有对等的收益。</p>
<p>对于返回值是void的调用，可以这样做，因为实际上这个调用业务上往往不需要同步得到处理结果的，只要保证会处理即可。（RPC的方式可以保证调用返回即处理完成，使用消息方式后这一点<strong><em>不能</em></strong>保证了。）</p>
<p>返回值是void的调用，使用消息，效果上是把消息的使用方式Wrap成了服务调用（服务调用使用方式成简单，基于业务接口）。</p>
<p> </p>
<p>PS： 也发在个人博客 <a href="http://oldratlee.com/post/2013-02-01/synchronous-rpc-vs-asynchronous-message">远程调用服务(RPC)和消息(Message Queue)对比及其适用/不适用场合</a></p>