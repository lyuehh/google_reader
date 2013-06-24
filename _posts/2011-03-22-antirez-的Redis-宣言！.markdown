---
layout: post
title:  "antirez 的Redis 宣言！"
date:   2011-03-22 11:00:06
author: 
categories: program
---

## antirez 的Redis 宣言！
### by 
### at 2011-03-22 11:00:06
### original <http://news.cnblogs.com/n/94911/>

<p>　　Redis 的作者<a href="http://twitter.com/antirez">antirez</a>（Salvatore Sanfilippo）同学最近发表了一篇名为Redis 宣言（<a href="http://antirez.com/post/redis-manifesto.html">Redis Manifesto</a>）的文章，文中列举了Redis 的七个原则，以向大家阐明Redis 的思想。本站翻译如下：</p>
<ol>
<li>Redis 是一个操作数据结构的语言工具，它提供基于TCP的协议以操作丰富的数据结构。在Redis中，数据结构这个词的意义不仅表示在某种数据结构上的操作，更包括了结构本身及这些操作的时间空间复制度。</li>
<li>Redis 定位于一个内存数据库，正是由于内存的快速访问特性，才使得Redis能够有如此高的性能，才使得Redis能够轻松处理大量复杂的数据结构，Redis会尝试其它的存储方面的选择，但是永远不会改变它是一个内存数据库的角色。</li>
<li>Redis 使用基础的API操作基础的数据结构，Redis的API与数据结构一样，都是一些最基础的元素，你几乎可以将任何信息交互使用此API格式表示。作者调侃说，如果有其它非人类的智能生物存在，他们也能理解Redis的API。因为它是如此的基础。</li>
<li>Redis 有着诗一般优美的代码，经常有一些不太了解Redis 原则的人会建议Redis采用一些其它人的代码，以实现一些Redis 未实现的功能，但这对我们来说就像是非要给《红楼梦》接上后四十回一样。（作者此处用了莎士比亚的比喻）</li>
<li>Redis 始终避免复杂化，我们认为设计一个系统的本质，就是与复杂化作战。我们不会为了一个小功能而往源码里添加上千行代码，解决复杂问题的方法就是让复杂问题永远不要提复杂的问题。</li>
<li>Redis 支持两个层成的API，第一个层面包含部分操作API，但它支持用于分布式环境下的Redis。第二个层面的API支持更复杂的multi-key操作。它们各有所长，但是我们不会推出两者都支持的API，但我们希望能够提供实例间数据迁移的命令，并执行multi-key操作。</li>
<li>我们以优化代码为乐，我们相信编码是一件辛苦的工作，唯一对得起这辛苦的就是去享受它。如果我们在编码中失去了乐趣，那最好的解决办法就是停下来。我们决不会选择让Redis不好玩的开发模式。</li>
</ol>
<p>　　原文链接：<a href="http://antirez.com/post/redis-manifesto.html">Redis Manifesto</a></p><p><br>　　本文链接：<a href="http://news.cnblogs.com/n/94911/">http://news.cnblogs.com/n/94911/</a></p><p>　　相关新闻：<br>　　· <a href="http://news.cnblogs.com/n/63097/">Apache Cassandra正式获商业支持</a><span style="color:gray">(2010-04-29)</span><br>　　· <a href="http://news.cnblogs.com/n/58689/">Digg用NoSQL替代MySQL</a><span style="color:gray">(2010-03-14)</span><br>　　· <a href="http://news.cnblogs.com/n/73894/">NoSQL有了Android版本</a><span style="color:gray">(2010-09-11)</span><br>　　· <a href="http://news.cnblogs.com/n/85466/">百万级访问量网站的技术准备工作</a><span style="color:gray">(2010-12-22)</span><br>　　· <a href="http://news.cnblogs.com/n/91693/">Digg 采用Redis作计数存储</a><span style="color:gray">(2011-02-21)</span><br></p><img src="http://news.cnblogs.com/news/rssclick.aspx?id=94911" width="1" height="1" alt="">