---
layout: post
title:  "Riak与MongoDB的对比"
date:   2011-08-06 00:01:29
author: nosqlfan
categories: program
---

## Riak与MongoDB的对比
### by nosqlfan
### at 2011-08-06 00:01:29
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/543661997/6253001/1/item.html>

<p>本文来自<span><a href="http://blog.nosqlfan.com/tags/riak" title="查看 Riak 的全部文章">Riak</a></span>所属的<a href="http://basho.com">Basho</a>公司的技术WiKi，文章从几个方面对Riak和<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>进行了对比，这不是一篇PK文章，NoSQLFan翻译给大家，希望本文能让您对Riak和MongoDB有更多的了解。</p>
<p>来源地址：<a href="http://wiki.basho.com/Riak-Compared-to-MongoDB.html">wiki.basho.com</a></p>
<p><a href="http://blog.nosqlfan.com/wp-content/uploads/2011/08/mongodbriak.jpg"><img title="mongodbriak" src="http://blog.nosqlfan.com/wp-content/uploads/2011/08/mongodbriak.jpg" alt="" width="483" height="90"></a></p>
<h3>机制与概念上的异同</h3>
<p>Riak和MongoDB在使用特性上有下面几个相同点：</p>
<ul>
<li>都是文档型的数据模型</li>
<li>具体存储方式都不是以文档型进行存储</li>
<li>写性能及写吞吐都很高</li>
</ul>
<p>虽然上面几点看起来二者挺像，但在内部实现上两者却是相去甚远。比如Riak是一个分布式的存储，而MongoDB可以理解为是一个单一的数据库系统，同时加上了Replication和Sharding功能。MongoDB的内部数据结构上还是文档，而Riak是不用关心存储内容的二进制。MongoDB提供GridFS机制来存储二进制内容，而Riak的二进制内容与普通内容存储方式一样。MongoDB的写入方式是 in-place方式，修改一个文档是原子性的，而Riak是通过quormNRW的机制保证写入操作安全性的。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Home">http://www.mongodb.org/display/DOCS/Home</a></li>
<li><a href="http://blog.mongodb.org/post/248614779/fast-updates-with-mongodb-update-in-place">http://blog.mongodb.org/post/248614779/fast-updates-with-mongodb-update-in-place</a></li>
<li><a href="http://www.mongodb.org/display/DOCS/Updating#Updating-Update">http://www.mongodb.org/display/DOCS/Updating#Updating-Update</a></li>
</ul>
<h3>复制备份及横向扩展</h3>
<p>Riak主要通过<span><a href="http://blog.nosqlfan.com/tags/%e4%b8%80%e8%87%b4%e6%80%a7hash" title="查看 一致性hash 的全部文章">一致性hash</a></span>算法来实现其数据的复制及分片，一致性hash机制是Riak的核心思想之一。在Riak中，每个节点都是对等的，所以其不存在单点故障。</p>
<ul>
<li><a href="http://wiki.basho.com/Basic-Cluster-Setup.html#Add-a-Second-Node-to-Your-Cluster">Add Nodes to Riak</a></li>
<li><a href="http://wiki.basho.com/Riak-Glossary.html#Consistent-Hashing">Consistent Hashing</a></li>
</ul>
<p>而MongoDB在1.6版本后也推出了强有力的复制备份功能</p>
<h4>1.主从复制</h4>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Master+Slave">http://www.mongodb.org/display/DOCS/Master+Slave</a></li>
</ul>
<h4>2.<span><a href="http://blog.nosqlfan.com/tags/replica-sets" title="查看 Replica Sets 的全部文章">Replica Sets</a></span></h4>
<p>Replica Sets是MongoDB的重头功能之一，它让几个节点组成一个集合，在这个集合中的节点中有一个主机提供写入，其它节点会从主机上备份数据，主机故障后会自动在从机中选取产生新的主机。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Replica+Sets">http://www.mongodb.org/display/DOCS/Replica+Sets</a></li>
</ul>
<p>而在数据分片上，MongoDB提供了一种叫<span><a href="http://blog.nosqlfan.com/tags/auto-sharding" title="查看 auto-sharding 的全部文章">auto-sharding</a></span>的机制，使数据在多个节点间可以均匀分布，提供动态添加删除节点的功能。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Sharding">http://www.mongodb.org/display/DOCS/Sharding</a></li>
<li><a href="http://www.mongodb.org/display/DOCS/Sharding+Introduction">http://www.mongodb.org/display/DOCS/Sharding+Introduction</a></li>
<li><a href="http://en.wikipedia.org/wiki/Sharding">http://en.wikipedia.org/wiki/Sharding</a></li>
</ul>
<h3>数据分片的自动调整</h3>
<p>Riak基于一致性hash策略，在有节点从hash环上移除后，其数据会自动分摊整个环上的其它节点上。其负载也就被均匀分摊了。而MongoDB也支持在Sharding中摘除节点后的自动数据迁移，具体见此文：</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Configuring+Sharding#ConfiguringSharding-Removingashard">http://www.mongodb.org/display/DOCS/Configuring+Sharding#ConfiguringSharding-Removingashard</a></li>
</ul>
<h3>性能对比</h3>
<p>Riak的存储引擎本身是作为插件的形式挂载的，Riak支持BitCask，InnoDB和LevelDB等存储引擎，使用默认的BitCask引擎，你可以在性能和数据持久化的选择上进行调节。相比之下，MongoDB由于采用了mmap机制，如果索引和热数据能被内存完全装下，那么其操作基本上相当于内存操作，所以MongoDB的当机性能是相当高的。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Durability+and+Repair">http://www.mongodb.org/display/DOCS/Durability+and+Repair</a></li>
<li><a href="http://blog.mongodb.org/post/381927266/what-about-durability">http://blog.mongodb.org/post/381927266/what-about-durability</a></li>
</ul>
<h3>数据模型</h3>
<p>Riak的数据存储没有特定的格式需求，它允许你存储不同体积的文档型数据，另外Riak还可以在数据间创建link来为数据建立关联。</p>
<ul>
<li><a href="http://wiki.basho.com/An-Introduction-to-Riak.html#Data-Storage">Data Storage in Riak</a></li>
</ul>
<p>MongoDB的数据是以BSON格式存储的，你可以在MongoDB中存储任意JSON格式的文档，在存储时会被转成BSON进行存储，另外二进制数据也可以转换成相应的一种BSON数据类型进行存储，GridFS正是基于这种类型来实现的。</p>
<h3>查询语句及分布式操作</h3>
<p>Riak只提供key-value式的数据操作接口，它支持key-value数据的各种操作，也支持link-walking和MapReduce操作，像二级索引这种东西，在Riak里是不存在的，因为Riak根本不关心它存的数据是什么样的，value对它来说只是一串数据。</p>
<ul>
<li><em><a href="https://wiki.basho.com/display/RIAK/MapReduce">https://wiki.basho.com/display/RIAK/MapReduce</a></em></li>
</ul>
<p>MongoDB提供与关系型数据库类似的各种数据操作（除了关联查询），其索引机制更是与关系型数据库几乎一模一样。同时MongoDB也提供MapReduce的操作接口，用以处理一些批量任务。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Indexes">http://www.mongodb.org/display/DOCS/Indexes</a></li>
<li><a href="http://www.mongodb.org/display/DOCS/Querying">http://www.mongodb.org/display/DOCS/Querying</a></li>
<li><a href="http://www.mongodb.org/display/DOCS/MapReduce">http://www.mongodb.org/display/DOCS/MapReduce</a></li>
</ul>
<h3>冲突解决策略</h3>
<p>Riak使用vector-clock机制来进行冲突检测，所以其冲突解决的选择权是留给应用层来做的。应用层可以决定两个用户对同一行数据的更新哪一个会胜出。</p>
<ul>
<li><a href="http://wiki.basho.com/Riak-Glossary.html#Vector-Clock">Vector Clocks</a></li>
</ul>
<p>MongoDB使用的是最近更新者胜出的方式，相对来说更简单直接。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Atomic+Operations">http://www.mongodb.org/display/DOCS/Atomic+Operations</a></li>
</ul>
<h3>API</h3>
<p>Riak提供给非Erlang的客户端两种操作方式</p>
<ul>
<li>1. <a href="http://wiki.basho.com/HTTP-API.html">HTTP</a></li>
<li>2. <a href="http://wiki.basho.com/PBC-API.html">Protocol Buffers</a></li>
</ul>
<p>MongoDB的协议是自己制定的一套特有协议，其客户端由其所属的<a href="http://10gen.com">10gen</a>公司开发并维护，基本主流的语言都有相应的官方客户端。</p>
<ul>
<li><a href="http://www.mongodb.org/display/DOCS/Mongo+Wire+Protocol">http://www.mongodb.org/display/DOCS/Mongo+Wire+Protocol</a></li>
</ul>
<p><span style="color:#008000"><strong><span style="color:#ff0000">最后，七夕看这篇文章的都是尼玛折翼的梁山好汉，各位七夕快乐。</span></strong></span>
<p style="font-weight:bold"><span style="padding-top:5px;float:left">技术传播，需要你我共同努力！</span><a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html&amp;text=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVKEJk/custom.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html" title="新浪微博" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVKrzm/custom.png"></a><a href="http://v.t.qq.com/share/share.php?title=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html" title="腾讯微博" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJX6o/custom.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html" title="QQ空间" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJSKI/custom.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html&amp;title=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJrri/custom.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html&amp;title=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJ4v4/custom.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html" title="人人网" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVI86k/custom.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2705.html&amp;title=Riak%E4%B8%8EMongoDB%E7%9A%84%E5%AF%B9%E6%AF%94%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVHr67/custom.png"></a>    </p><img src="http://www1.feedsky.com/t1/543661997/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/543661997/6253001/1/item.html" border="0" height="0" width="0"></p>