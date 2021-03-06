---
layout: post
title:  "推荐NHibernate新书：NHibernate 3.0 CookBook"
date:   2010-10-11 08:43:00
author: 李永京
categories: program
---

## 推荐NHibernate新书：NHibernate 3.0 CookBook
### by 李永京
### at 2010-10-11 08:43:00
### original <http://www.cnblogs.com/lyj/archive/2010/10/11/nhibernate-3-0-cookbook.html>

<p><a href="http://www.cnblogs.com/lyj/"><img src="http://pic.cnblogs.com/face/u20012.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/lyj/">李永京</a> 发表于 2010-10-11 08:43 <a href="http://www.cnblogs.com/lyj/archive/2010/10/11/nhibernate-3-0-cookbook.html">原文链接</a> 阅读: 1823 评论: 41</p><p>NHibernate 3.0 CookBook这本书在2010年10月4号出版，出版后NHibernate的Lead：<a href="http://www.cnblogs.com/fabiomaulo.blogspot.com">Fabio 
Maulo</a>赠送我一份免费优惠券，我花了几天时间阅读了这本电子书，以下是我阅读NHibernate 3.0 CookBook这本书的读书心得分享一下。</p>
<h1>NHibernate近况</h1>
<p>目前我们很难对NHibernate整个技术体系有个完整的把握。其原因是官方文档不全，关于NHibernate的书也没几本。NHibernate 3.0 
CookBook这本书可以算是弥补了NHibernate文档很多空白。</p>
<p>
在国内更是资料寥寥无几。甚至可以说是空白。但是从今年的NHibernate下载量<a href="http://bit.ly/90JuFh">官方统计</a>来看，中国居然是全球第一，占24%，高达4万之多，从这么大的数字来看，不知道有多少人真正的把NHibernate使用地“灵活自如”呢，这的确是个未知数。</p>
<h1>这本书概述</h1>
<a href="https://www.packtpub.com/nhibernate-3-0-cookbook/book">
<img alt="NHibernate 3.0 CookBook" height="431" src="https://www.packtpub.com/sites/default/files/3043OS_MockupCover_Cookbook.jpg" width="350" style="float:right"></a>
<p>从全部的内容来看，这本书几乎涵盖了NHibernate 3版本的全部内容，基本上对NHibernate有了一个整体的把握：</p>
<ul>
	<li><strong>NHibernate功能介绍</strong><ul>
		<li><strong>Mapping</strong></li>
		<li><strong>Configuration</strong></li>
		<li><strong>Query</strong></li>
		<li><strong>Test</strong></li>
		<li><strong>Extending</strong></li>
	</ul>
	</li>
	<li><strong>NHibernate贡献项目介绍</strong></li>
	<li><strong>各种环境(ASP.NET MVC、ASP.NET、WPF、WinForms)里的最佳实践</strong></li>
</ul>
<p>现在我们使用到的技术在这本书都有一个比较全面的介绍。</p>
<p>每一个章节都是采用真实实战的方式去写的，基本上有四步：</p>
<ol>
	<li><strong>准备工作(Getting ready)</strong>：为这个知识点准备下实体。</li>
	<li><strong>如何实现(How to do it...)</strong>：进行实际编码完成功能。</li>
	<li><strong>分析原理(How it works...)</strong>：详细描述了为什么这样做以及这样做的原因。</li>
	<li><strong>必要扩展(There&#39;s more...)</strong>：对这个知识点进行适当扩展。</li>
</ol>
<p>在完成之后你也基本上对这节有了一个清晰的把握了。而且可以仔细阅读源代码实际运行看结果。</p>
<h1>这本书分析</h1>
<p><strong>第一章：Models and Mappings</strong></p>
<p>本书把映射作为出发点，介绍了三种映射方式：XML、Fluent、ConfORM。然后分析一些映射实例：类继承映射、一对多映射、版本映射、枚举映射、组件映射。通过这一章基本上已经熟悉了映射，但是有一些内容没有介绍，
例如数据库对象(DatabaseObjects)、筛选定义(FilterDefinitions)、结果集(ResultSets)、类型定义(TypeDefinitions)等等。</p>
<p><strong>第二章：Configuration and Schema</strong></p>
<p>我们使用NHibernate，必须先配置NH。这一章作者使用了App.config、hibernate.cfg.xml、Loquacious、FluentNH方式配置NHibernate，并使用NHibernate架构生成工具创建数据库架构。这一章是必不可少的基础内容，往往很多初学者在这里过不去，我想看看这一章就完全清楚了。</p>
<p><strong>第三章：Sessions and Transactions</strong></p>
<p>个人觉得这一章写的最好了，介绍了Session接口和几种Session策略，实例分析ASP.NET Web Forms和ASP.NET MVC程序中session per web request策略应用并对其扩展。</p>
<p><strong>第四章：Queries</strong></p>
<p>NHibernate3版本里所有的查询方式，应该是7种：HQL(NHibernate查询语言)、Criteria(条件查询)、QueryOver(Lambda表达式查询)、LINQ(语言集成查询)、H-SQL(NHibernate特定SQL查询)、SQL(原生SQL查询)、Custom DSL(自定义特定领域语言查询)。
不过作者在这一章介绍了常见的几种。每种查询都是举了一些实际例子。</p>
<p><strong>第五章：Testing</strong></p>
<p>作者写个测试环境，并介绍NHibernate Profiler、SQLite和重影。推荐使用这些可以有效的提高工作效率。</p>
<p><strong>第六章：Data Access Layer</strong></p>
<p>我们在程序中NH相关查询代码是写在数据访问层的，作者在这一章介绍了各种查询的写法并阐述了几种模式（Repository、LINQ specifications）在数据访问层的应用。</p>
<p><strong>第七章：Extending NHibernate</strong></p>
<p>扩展NHibernate，这个比较高级，但是在企业级应用里必不可少。作者在这一章介绍的几种扩展都是挺实用的。</p>
<p><strong>第八章：NHibernate Contribution Projects</strong></p>
<p>这一章介绍了NHibernate贡献项目，对于我们使用这些贡献项目有个参考了。</p>
<p>最后作者结合ASP.NET MVC、ASP.NET、WPF、WinForms环境对全书内容整理了一个学习、应用思路。</p>
<h1>推荐链接</h1>
<p>
最后觉得应该把这本书推荐给所有NHibernate开发人员。从这本书中你肯定可以学到很多你不知道的东西。如果有的公司使用NHibernate，推荐采购一本实体书作为手上的参考书。以后我也会结合这本书的内容适当的补充写写博客。当然前提你读过了这本书了。</p>
<p>书名：<a href="https://www.packtpub.com/nhibernate-3-0-cookbook/book">NHibernate 3.0 Cookbook</a></p>
<p>作者：<a href="http://www.jasondentler.com/">Jason Dentler</a></p>
<p>购买地址：<a href="https://www.packtpub.com/nhibernate-3-0-cookbook/book">https://www.packtpub.com/nhibernate-3-0-cookbook/book</a></p>
<p>出版社：<a href="http://www.packtpub.com/">Packt Publishing</a></p>
<h1>这本书评价</h1>
<p>Fabio Maulo：<a href="http://nhforge.org/blogs/nhibernate/archive/2010/10/05/nhibernate-3-0-cookbook.aspx">NHibernate 3.0 Cookbook</a>(今年底购买输入优惠码NHIBCBK20(区分大小写)享受20%优惠)</p>
<p>Mogens Heller Grabe：<a href="http://mookid.dk/oncode/archives/1596">Book review: NHibernate 3.0 Cookbook</a></p>
<p>希望本文对你有所帮助。</p>
<img src="http://www.cnblogs.com/lyj/aggbug/1847579.html?type=1" width="1" height="1" alt=""><p>评论: 41　<a href="http://www.cnblogs.com/lyj/archive/2010/10/11/nhibernate-3-0-cookbook.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/lyj/archive/2010/10/11/nhibernate-3-0-cookbook.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76932/">MeeGo 和 Symbian——开源死到临头？</a><span style="color:gray">(2010-10-11 23:11)</span><br>· <a href="http://news.cnblogs.com/n/76931/">LG发布首款Windows Phone 7手机</a><span style="color:gray">(2010-10-11 22:46)</span><br>· <a href="http://news.cnblogs.com/n/76930/">图文直播：微软Windows phone 7发布会</a><span style="color:gray">(2010-10-11 22:45)</span><br>· <a href="http://news.cnblogs.com/n/76929/">微软将斥资4亿美元推广Windows Phone 7</a><span style="color:gray">(2010-10-11 21:28)</span><br>· <a href="http://news.cnblogs.com/n/76928/">业界传言称微软WP7系统将用于平板电脑</a><span style="color:gray">(2010-10-11 21:07)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/76823/">PHP将死，何以为继？</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>