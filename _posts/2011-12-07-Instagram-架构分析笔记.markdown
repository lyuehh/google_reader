---
layout: post
title:  "Instagram 架构分析笔记"
date:   2011-12-07 15:11:44
author: dbanotes@gmail.com(Fenng)
categories: program
---

## Instagram 架构分析笔记
### by dbanotes@gmail.com(Fenng)
### at 2011-12-07 15:11:44
### original <http://www.dbanotes.net/arch/instagram.html>

<p>by <a href="http://www.dbanotes.net">Fenng</a>@<a href="http://www.dbanotes.net/">dbanotes.net</a> 

<p>Updated: 2012 年4月10日凌晨消息，Instagram 被 Facebook 以10亿美金收购。团队规模：13 人。</p>

<p><a href="http://instagr.am">Instagram</a> 团队上个月才迎来第 7 名员工，是的，7个人的团队。作为 iPhone 上最火爆的图片类工具，instagram 用户数量已经超过 1400 万，图片数量超过 1.5 亿张。不得不说，这真他妈是个业界奇迹。</p>

<p>几天前，只有三个人的 Instagram 工程师团队发布了一篇文章：<a href="http://instagram-engineering.tumblr.com/post/13649370142/what-powers-instagram-hundreds-of-instances-dozens-of">What Powers Instagram: Hundreds of Instances, Dozens of Technologies</a>，披露了 Instagram 架构的一些信息，足够勾起大多数人的好奇心。读罢做点笔记，各种线索还是有一定参考价值的。能打开原文的建议直接读原文。</p>

<p><img alt="Instragram.png" src="http://www.dbanotes.net/Images/Instragram.png" width="302" height="84"></p>

<p>Instagram 开发团队<strong>奉行</strong>的三个核心原则：</p>

<ul>
	<li>Keep it very simple (极简主义) </li>
	<li>Don't re-invent the wheel (不重复发明轮子) </li>
	<li>Go with proven and solid technologies when you can(能用就用靠谱的技术)</li>
</ul>

<p><strong>OS/主机</strong><br>
<p>操作系统的选择，在Amazon EC2上跑 Ubuntu Linux 11.04 (Natty Narwhal) ，这个版本经过验证在 EC2 上够稳定。因为只有三名工程师，只有三名工程师，所以自己部署机器到 IDC 是不靠谱的事情。幸好有亚马逊。</p></p>

<p><strong>负载均衡</strong><br>
<p>此前曾用过两台 Nginx 做 DNS 轮询承载前端请求，这样做会有副作用，现在已经迁移到Amazon的ELB(Elastic Load Balancer)，起了三个 Nginx 实例，在 ELB 层停掉了 SSL , 以缓解 CPU 压力。DNS 服务使用 Amazon Route53 服务。 </p></p>

<p><strong>应用服务器</strong><br>
<p>启用了 25 个  Django 实例，运行在 High-CPU Extra-Large 类型的服务器实例上，之所以用 High-CPU Extra-Large 实例是因为应用请求是 CPU 密集型而非 IO 密集型。</p></p>

<p>使用 <a href="http://gunicorn.org/">Gunicorn</a> 作为 WSGI 服务器。过去曾用过 Apache 下的 mod_wsgi 模块，不过发现 Gunicorn 更容易配置并且节省 CPU 资源。使用 <a href="http://fabric.readthedocs.org/en/1.3.3/index.html">Fabric</a> 加速部署。</p>

<p><strong>数据存储</strong></p>

<p>用户信息、图片元数据、标签等大部分数据存储在 PostgreSQL 中。主要的 Shard 数据库集群有 12个节点。</p>

<p>实践中发现 Amazon 的网络磁盘系统单位时间内寻道能力不行，所以有必要将数据尽量放到内存中。创建了软 RAID 以提升 IO 能力，使用的 <a href="http://en.wikipedia.org/wiki/Mdadm">Mdadm</a> 工具进行 RAID 管理。</p>

<p>管理内存中的数据，<a href="http://hoytech.com/vmtouch/vmtouch.c">vmtouch</a> 这个小工具值得推荐。</p>

<p>PostgreSQL 设置为 Master-Replica 方式，流复制模式。利用 EBS 的快照进行数据库备份。使用 XFS 文件系统，以便和快照服务充分配合。 使用 <a href="https://github.com/greg2ndQuadrant/repmgr">repmgr</a> 这个小工具做 PostgreSQL 复制管理器器。</p>

<p>连接池管理，用了 <a href="http://pgfoundry.org/projects/pgbouncer/">Pgbouncer</a>。<a href="http://thebuild.com/blog/">Christophe Pettus</a> 的文章包含了不少 <a href="http://thebuild.com/blog/">PostgreSQL</a> 数据库的信息。</p>

<p>TB 级别的海量图片存储在 Amazon S3 上，CDN 采用的也是 Amazon 的服务，CloudFront。</p>

<p>Instagram 也是 Redis 的重度用户，Feed 以及 Session 信息都用 Redis 处理，Redis 也是以 Master-Replica 方式部署。在 Replica 节点上进行数据备份。</p>

<p>使用了 Apache Solr 承担  Geo-search API 的工作，Solr 简单的 JSON 接口也不错。</p>

<p>缓存使用了 6 个 Memcached 实例，库使用  pylibmc 和 libmemcached。亚马逊也提供缓存服务－Elastic Cache service ，Instagram 也有尝试，不过不便宜。</p>

<p><strong>任务队列/发布通知</strong></p>

<p>队列服务使用  <a href="http://gearman.org/">Gearman</a> ，通知系统则使用<a href="https://github.com/samuraisam/pyapns"> pyapns</a> 来实现。</p>

<p><strong>监控</strong></p>

<p>前面提及的服务器实例数量加起来，的确有100多个，有效的监控是相当有必要的。使用  Munin 作为主要监控工具 , 也写了不少定制插件，外部监控用 <a href="http://pingdom.com/">Pingdom</a> 的服务。通知服务使用  <a href="http://www.pagerduty.com/">PagerDuty</a>。</p>

<p>对于 Python 的错误报告，使用 Disqus 团队开源的  <a href="http://pypi.python.org/pypi/django-sentry">Sentry</a> 来处理。</p>

<p><strong>几个感想</strong></p>

<p>0）轻装上阵说起来容易，做起来非常难。这也是 Instagram 团队目前最令人着迷的地方；</p>

<p>1）Python 社区已经足够成熟，各个环节上都已经有不错的解决方案了。</p>

<p>2）如果要问我最大的一个感慨，我要说：<strong>Amazon 真是一家伟大的公司，甚至比 Google 还伟大</strong>。</p>

<p>--EOF--</p></p>
<hr>
<p><strong>最近文章|Recent Articles</strong></p>
   <ul>
    
      <li><a href="http://www.dbanotes.net/mylife/Patients-like-Fenng.html">丁香医生，发现健康</a></li>
    
      <li><a href="http://www.dbanotes.net/review/Big_Data_Myth.html">大数据的商业前景被过分夸大</a></li>
    
      <li><a href="http://www.dbanotes.net/review/Sina_Weibo_How_Make_Money.html">谈谈新浪微博的收费问题</a></li>
    
      <li><a href="http://www.dbanotes.net/geek/Big_Questions.html">适合中国大公司的招聘题</a></li>
    
   </ul>
<p>本站赞助商：<a href="http://www.douban.com/">豆瓣网(Douban.com)</a></p>
<p><strong> 评论数(12)|<a href="http://www.dbanotes.net/arch/instagram.html#comments" title="Comment on: Instagram 架构分析笔记">添加评论</a></strong> | 最近作者还说了什么? Follow <a href="http://www.twitter.com/fenng">Fenng@Twitter</a></p>
<p>DBA Notes 理念: 用简约的技术取得最大的收益...</p>



<div name="ClickComments"></div>

<div>
<a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/webarch?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/webarch?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/webarch?i=ESDGLIzNUnA:v9IrI5z8vdw:F7zBnMyn0Lo" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:V_sGLiPBpWU"><img src="http://feeds.feedburner.com/~ff/webarch?i=ESDGLIzNUnA:v9IrI5z8vdw:V_sGLiPBpWU" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:mqyYa2mfVbY"><img src="http://feeds.feedburner.com/~ff/webarch?d=mqyYa2mfVbY" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:I9og5sOYxJI"><img src="http://feeds.feedburner.com/~ff/webarch?d=I9og5sOYxJI" border="0"></a> <a href="http://feeds.feedburner.com/~ff/webarch?a=ESDGLIzNUnA:v9IrI5z8vdw:bcOpcFrp8Mo"><img src="http://feeds.feedburner.com/~ff/webarch?d=bcOpcFrp8Mo" border="0"></a>
</div>