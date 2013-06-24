---
layout: post
title:  "探索Google App Engine背后的奥秘(6)- 总结"
date:   2010-06-16 02:48:43
author: dbanotes@gmail.com(Fenng)
categories: program
---

## 探索Google App Engine背后的奥秘(6)- 总结
### by dbanotes@gmail.com(Fenng)
### at 2010-06-16 02:48:43
### original <http://www.dbanotes.net/arch/google_app_engine-summary.html>

<p> By：<a href="http://peopleyun.com/">ikewu</a> posted @ <a href="http://www.dbanotes.net/">dbanotes.net</a>. <a href="http://www.dbanotes.net/index.xml">RSS</a> | Ad.<a href="http://www.infoq.com/cn/vendorcontent/show.action?vcr=1047">Adobe Flash Builder 4 简体中文正式版下载</a>

<blockquote>按：此为客座博文系列。投稿人<a href="http://peopleyun.com/">吴朱华</a>曾在IBM中国研究院从事与云计算相关的研究，现在正致力于研究云计算技术。</blockquote>

<p>本篇是本系列的最终章，将总结一下App Engine在使用方面的注意点，最佳实践和适用场景，最后会谈一下我对App Engine的一些期望。 </p>

<h1>注意点</h1>
<ul>
<li>执行速度偏慢：由于其分布式的设计，所以在速度方面不是最优的，比如普通的Memcache能在几毫秒完成操作，而App Engine的Memcache则大概需要50(毫)秒才能完成操作。 </li>
<li>私有API：其API有很多都是私有，特别是在其服务方面，虽然Google提供了很不错的文档，但是在学习和移植等方面，成本都很高。 </li>
<li>执行会出现失败的情况：根据很多人的实际经验，App Engine会不定时出现执行失败的情况，特别是Datastore和URLFetch这两部分，虽然Google已经将Datastore方面出现错误的几率从原先的0.4降至现在的0.1，但是失败的情况是很难避免的。 </li>
<li>有时会停机：虽然总体而言，停机并不频繁，但是在今年初出现长达136分钟故障导致部分用户的应用无法正常运行，其发生原因来自于其备份数据中心出现了问题。 </li>
<li>无法选择合适的数据中心：比如，你应用所面对的用户主要在欧洲，但是你应用所属App Engine服务器却很有可能是被部署在一个美国的数据中心内，虽然你的应用很有可能在将来移动至欧洲某个数据中心，但是你却无法控制整个过程。 </li>
<li>有时会处理请求超时：虽然能平均在100至200ms之间完成海量的请求，但是有时会出现处理请求超时的情况。 </li>
<li>不支持裸域名：只支持类似CNAME的子域名。 </li>
</ul>

<h1>最佳实践</h1>
<ul>
<li>适应App Engine的数据模型：因为其数据模型，并不是传统的关系模式，而且在性能方面表现也和关系型数据库差别很大，所以如果想要用好非常关键的Datastore，那么理解和适应其数据模型是不可或缺的。 </li>
<li>对应用进行切分：由于App Engine对每个应用都有一定资源限制，而且为了让应用更SOA化和更模块化，可以对一个应用切分多个子应用，比如，可以分成一个用于前端的Web应用和多个用于REST服务的后台应用。 </li>
<li>极可能多地利用Memcache，这样不仅能减少昂贵的Datastore操作，而且能减轻Datastore的压力。 </li>
<li>在上面提到过，由于App Engine在执行某些操作时会出现失败的情况，比如Datastore方面，所以要在设计和实现这两方面做好相应的异常处理工作。 </li>
<li>由于Datastore不是关系型数据库，导致在执行常见的求总数操作时显的有点"捉襟见肘"，所以最好使用Google推荐的<a href="http://code.google.com/appengine/articles/sharding_counters.html">Sharded Counters</a>技术来计算总数。 </li>
<li>由于Blobstore还只是刚走出试验期而已，而且其他模块对静态文件（比如图片等）支持不佳，比如Datastore只支持1MB以内的对象，同时每个应用只能最多上传一千个文件，而且速度不是最优，所以推荐使用其他专业的云存储，比如Amazon的S3或者Google马上就要推出的Google Storage等。 </li>
<li>尽量使用批处理方式，不论是在使用Datastore还是发送邮件等。 </li>
<li>不要手动创建Index：因为App Engine会自动根据你在代码中查询来创建相关的Index。 </li>
</ul>

<h1>适用场景</h1>

<p>现在而言，App Engne主要适用于下面这三个场景：</p>

<ul>
<li>Web Hosting：这是最常见的场景，在App Engine上已经部署了数以十万计的小型网站（其中有很多主要为了学习目的），而且还部署了一些突发流量很大的网站，其中最著名的例子就是美国白宫的"Open For Questions"这个站点，主要用于让美国人民给奥巴马总统提问的，这个站点在短短的几个小时内处理接近百万级别的流量。 </li>
<li>REST服务：这也是在App Engine平台上很常见的场景，最出名的例子就是BuddyPoke，BuddyPoke的客户端就是一个Flash应用，在用户的浏览器上运行，而它的服务器端则是以REST服务的形式放置在App Engine上，每当Flash客户端需要读取和存储数据的时候，它都会发请求给后端的REST服务，来让其执行相关的Datastore操作。 </li>
<li>依赖Google服务的应用：比如应用能够通过App Engine的Email服务来发送大规模的电子邮件。 </li>
</ul>

<h1>未来的期望</h1>
<ul>
<li>更稳定的表现，更少的超时异常和更快的反应速度，特别是在Datastore和Memcached这两方面。 </li>
<li>支持对数据中心的选择，虽然现在App Engine会根据应用的用户群的所在地来调整应用所在的数据中心，但由于整个过程对开发者而言是不可控的，所以希望能在创建应用的时候，能让用户自己选择合适的数据中心。 </li>
<li>SLA，如果App Engine能像S3那样设定一些SLA条款，这样将使用户更放心地在App Engine上部署应用。 </li>
<li>新的语言：比如PHP，但是如果在现有的App Engine架构上添加一门新的语言，整个工作量会非常大的，因为App Engine有接近一半的模块是语言特定的，比如应用服务器和开发环境等，所以短期内我认为不太可能支持新的语言。 </li>
</ul>

<p>总体而言，Google App Engine是Google大战略中一个不可分割的一部分，因为Google希望能通过App Engine来降低Web应用开发的难度，只要难度降低了，那么Web应用替代客户端应用的整体速度将会加快，如果出现这样的情况的话，那么将会对Google今后的发展非常有利。</p>

<p>本系列文章结束。</p>

<p>参考资料：</p>

<ul>
<li><a href="http://perspectives.mvdirona.com/2008/06/25/GooglesDrKaiFuLeeOnCloudComputing.aspx">Google's Dr. Kai-Fu Lee on Cloud Computing</a></li>
<li><a href="http://perspectives.mvdirona.com/2009/10/31/TheCostOfLatency.aspx">The Cost of Latency</a> </li>
<li><a href="http://googleappengine.blogspot.com/">Google App Engine Blog</a></li>
<li><a href="http://labs.google.com/papers/bigtable.html">Bigtable: A Distributed Storage System for Structured Data</a></li>
<li><a href="http://code.google.com/events/io/sessions/FromSparkPlugToDriveTrain.html">From Spark Plug to Drive Train: Life of an App Engine Request</a></li>
<li><a href="http://perspectives.mvdirona.com/2008/07/10/GoogleMegastore.aspx">Google Megastore</a></li>
<li><a href="http://code.google.com/intl/zh-CN/appengine/docs/">Google App Engine官方文档</a></li>
<li><a href="http://highscalability.com/google-architecture">Google Architecture</a></li>
<li><a href="http://highscalability.com/google-appengine-first-look">Google App Engine - a first look</a></li>
<li><a href="http://www.infoq.com/news/2009/08/google-chose-jetty">Google Chose Jetty for App Engine</a></li>
<li><a href="http://www.readwriteweb.com/cloud/2010/02/google-app-engine-is-down---ba.php">Google App Engine is Down - Backup Data Center Having Problems</a></li>
<li><a href="http://www.ibm.com/developerworks/cn/opensource/os-cloud-virtual2/">面向虚拟基础设施的云服务，第 2 部分: Platform as a Service (PaaS) 和 AppScale</a></li>
<li><a href="http://www.google.org.cn/posts/google-working-on-new-filesystem.html">传Google正在开发新的服务器文件系统</a></li>
<li><a href="http://zh.wikipedia.org/zh-cn/Google%E6%AA%94%E6%A1%88%E7%B3%BB%E7%B5%B1">Google File System</a></li>
<li><a href="http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf">Designs, Lessons and Advice from Building Large Distributed Systems</a></li>
<li><a href="http://labs.google.com/papers/chubby.html">The Chubby lock service for loosely-coupled distributed systems</a></li>
<li><a href="http://perspectives.mvdirona.com/2009/01/01/GooglesWillPowerAndDataCenterEfficiency.aspx">Google's Will Power and Data Center Efficiency</a></li>
<li><a href="http://zh.wikipedia.org/zh-cn/MapReduce">MapReduce</a></li>
<li><a href="http://dbanotes.net/labs.google.com/papers/mapreduce-osdi04.pdf%20-">MapReduce的论文</a></li>
<li><a href="http://kimilv.javaeye.com/blog/411092">Protocol Buffer 简介</a></li>
<li><a href="http://www.theregister.co.uk/2008/04/11/google_data_center_map/">Google data centers snub Africa, Oz, and anything near Wyoming Or do they?</a></li>
<li><a href="http://server.ctocio.com.cn/comment/367/8785867.shtml">Google揭秘服务器创新技术 内置电池替换UPS</a></li>
<li><a href="http://www.dbanotes.net/database/database_sharding.html">开源数据库 Sharding 技术（Share Nothing）</a> </li>
<li><a href="http://www.theregister.co.uk/2009/08/12/google_file_system_part_deux/">Google File System II: Dawn of the Multiplying Master Nodes</a></li>
<li><a href="http://snarfed.org/space/transactions_across_datacenters_io.html">Transactions Across Datacenters</a></li>
<li><a href="http://server.51cto.com/IDC-81960.htm">探秘Google全球数据中心与中国机房</a></li>
<li><a href="http://server.51cto.com/NGDC-197298.htm">揭开Google数据中心五大神话</a></li>
<li><a href="http://tech.watchstor.com/storage-systems-112892.htm">俄勒冈州的Google数据中心耗电惊人</a></li>
<li><a href="http://tomuse.com/google-app-engine-java-microblog-development-review/#ixzz0nLlxnZQ2">Google App Engine For Java - Microblogging Case Study</a></li>
<li><a href="http://snarfed.org/space/datastore_talk.html">Under the covers of the App Engine Datastore</a></li>
<li><a href="http://perspectives.mvdirona.com/2008/07/10/GoogleMegastore.aspx">Google Megastore</a></li>
<li><a href="http://hi.baidu.com/knuthocean/blog/item/12bb9f3dea0e400abba1673c.html">Megastore/Bigtable Replication的文章</a></li>
</ul>

<p>--EOF--<br>
</p></p>
<hr>
<p><strong>最近文章|Recent Articles</strong></p>
   <ul>
    
      <li><a href="http://www.dbanotes.net/sitelog/movable_type_wide_character_in_subroutine_entry_bug.html">Movable Type 的 wide character in subroutine entry Bug</a></li>
    
      <li><a href="http://www.dbanotes.net/review/fucking_china_mobile.html">狗日的中国移动</a></li>
    
      <li><a href="http://www.dbanotes.net/review/3par_acquired_by_HP.html">3PAR 争夺战</a></li>
    
      <li><a href="http://www.dbanotes.net/mylife/inception.html">盗梦空间（Inception）</a></li>
    
   </ul>
<p>本站赞助商：<a href="http://www.douban.com/">豆瓣网</a></p>
<p><strong> 评论数(5)|<a href="http://www.dbanotes.net/arch/google_app_engine-summary.html#comments" title="Comment on: 探索Google App Engine背后的奥秘(6)- 总结">添加评论</a></strong> | 最近作者还说了什么? Follow <a href="http://www.twitter.com/fenng">Fenng@Twitter</a><br>本文网址：<a href="http://www.dbanotes.net/arch/google_app_engine-summary.html">http://www.dbanotes.net/arch/google_app_engine-summary.html</a></p>
<p>DBA Notes 理念: 用简约的技术取得最大的收益...</p>



<div name="ClickComments"></div>

<div>
<a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/webarch?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/webarch?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/webarch?i=B_GewJpRogY:cgzvyMG3T9Y:F7zBnMyn0Lo" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:V_sGLiPBpWU"><img src="http://feeds.feedburner.com/~ff/webarch?i=B_GewJpRogY:cgzvyMG3T9Y:V_sGLiPBpWU" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:mqyYa2mfVbY"><img src="http://feeds.feedburner.com/~ff/webarch?d=mqyYa2mfVbY" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:I9og5sOYxJI"><img src="http://feeds.feedburner.com/~ff/webarch?d=I9og5sOYxJI" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=B_GewJpRogY:cgzvyMG3T9Y:bcOpcFrp8Mo"><img src="http://feeds.feedburner.com/~ff/webarch?d=bcOpcFrp8Mo" border="0"></a>
</div>