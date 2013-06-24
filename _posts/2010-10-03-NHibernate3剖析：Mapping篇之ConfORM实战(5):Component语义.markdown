---
layout: post
title:  "NHibernate3剖析：Mapping篇之ConfORM实战(5):Component语义"
date:   2010-10-03 13:46:00
author: 李永京
categories: program
---

## NHibernate3剖析：Mapping篇之ConfORM实战(5):Component语义
### by 李永京
### at 2010-10-03 13:46:00
### original <http://www.cnblogs.com/lyj/archive/2010/10/03/inside-nh3-conform-component.html>

<p><a href="http://www.cnblogs.com/lyj/"><img src="http://pic.cnblogs.com/face/u20012.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/lyj/">李永京</a> 发表于 2010-10-03 13:46 <a href="http://www.cnblogs.com/lyj/archive/2010/10/03/inside-nh3-conform-component.html">原文链接</a> 阅读: 597 评论: 3</p>
<p><em><strong>本节内容</strong></em></p>
<ul>
    <li><a href="http://www.cnblogs.com/rss#insidenh3">系列引入</a></li>
    <li><a href="http://www.cnblogs.com/rss#introduction">ConfORM概述</a></li>
	<li><a href="http://www.cnblogs.com/rss#component">Component语义</a></li>
	<li><a href="http://www.cnblogs.com/rss#summary">结语</a></li>
	<li><a href="http://www.cnblogs.com/rss#reference">参考资料</a></li>
</ul>
<h1><a name="insidenh3">系列引入</a></h1>
<p><strong>NHibernate3剖析系列</strong>分别从Configuration篇、Mapping篇、Session篇、Core篇、Tool篇、Practice篇、Extension篇等方面全面揭示NHibernate3版本内容、特性及其应用，完全基于NHibernte3版本。</p>
<ul>
	<li>NHibernate专题：<a href="http://kb.cnblogs.com/zt/nhibernate/">http://kb.cnblogs.com/zt/nhibernate/</a></li>
	<li>NHibernate官方站点：<a href="http://nhforge.org/">http://nhforge.org/</a></li>
	<li>NHibernate参考文档：<a href="http://nhforge.org/doc/nh/en/">http://nhforge.org/doc/nh/en/</a></li>
	<li>获取NHibernate地址：<a href="http://sourceforge.net/projects/nhibernate/files/">http://sourceforge.net/projects/nhibernate/files/</a> </li>
</ul>
<h1><a name="introduction">ConfORM概述</a></h1>
<p>ConfORM实战系列：</p>
<ul>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/04/21/inside-nh3-conform-introduction.html">ConfORM实战(1):概览</a>：ConfORM简单使用</li>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/09/09/inside-nh3-conform-theory.html">ConfORM实战(2):原理</a>：ConfORM的基本实现原理</li>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/09/10/inside-nh3-conform-onetoone.html">ConfORM实战(3):OneToOne语义</a>：使用ConfORM“映射”一对一关联</li>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/09/26/inside-nh3-conform-manytomany.html">ConfORM实战(4):ManyToMany语义</a>：使用ConfORM“映射”多对多关联</li>
</ul>
<p>如果你不熟悉ConfORM请查看前几篇文章，你可以到<a href="http://code.google.com/p/codeconform/">http://code.google.com/p/codeconform/</a>获取ConfORM最新版本。</p>
<h1><a name="component">Component语义</a></h1>
<p>使用ConfORM“映射”组件，我们无需特别设置，ConfORM内部会根据Domain定义来判定组件，一般而言，没有主键的类就是组件。</p>
<pre><span>[<span style="color:#2b91af">Test</span>]
<span style="color:blue">public void </span>ComponentMappingDemo()
{
    <span style="color:green">//show how work with components and how ConfORM understands OOP
    </span><span style="color:blue">var </span>orm = <span style="color:blue">new </span><span style="color:#2b91af">ObjectRelationalMapper</span>();
    <span style="color:blue">var </span>mapper = <span style="color:blue">new </span><span style="color:#2b91af">Mapper</span>(orm);
    <span style="color:green">//use the definition of table-to-class strategy class by class
    </span>orm.TablePerClass&lt;<span style="color:#2b91af">Person</span>&gt;();
    <span style="color:green">// Show the mapping to the console
    </span><span style="color:blue">var </span>mapping = mapper.CompileMappingFor(<span style="color:blue">new</span>[] { <span style="color:blue">typeof</span>(<span style="color:#2b91af">Person</span>) });
    <span style="color:#2b91af">Console</span>.Write(mapping.AsString());
}</span></pre>
<h2>一些Domain范例</h2>
<p>我们使用各种集合定义Domain：</p>
<p><strong>1.Mapping a class with components</strong></p>
<p>Person实体有两个组件：</p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">public int </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Name </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Address </span>Address { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}

<span style="color:blue">public class </span><span style="color:#2b91af">Name
</span>{
    <span style="color:blue">public string </span>First { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Last { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}
<span style="color:blue">public class </span><span style="color:#2b91af">Address
</span>{
    <span style="color:blue">public string </span>Street { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public int </span>CivicNumber { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-ClassWithComponents.png">
<img alt="ClassWithComponents" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-ClassWithComponents.png"></a></p>
<p><strong>2.Mapping a class with double usage of same component</strong></p>
<p>在上面Domain的基础上新增一个Name类型的属性：</p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">public int </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Name </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Name </span>ShowBusinessAlias { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Address </span>Address { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-ClassWithDoubleSameComponents.png">
<img alt="ClassWithDoubleSameComponents" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-ClassWithDoubleSameComponents.png"></a></p>
<p><strong>3.Collection of components</strong></p>
<p>使用一个集合，而不是单一组件：</p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">private </span>Iesi.Collections.Generic.<span style="color:#2b91af">ISet</span>&lt;<span style="color:#2b91af">Address</span>&gt; addresses;
    <span style="color:blue">public </span>Person()
    {
        addresses = <span style="color:blue">new </span>Iesi.Collections.Generic.<span style="color:#2b91af">HashedSet</span>&lt;<span style="color:#2b91af">Address</span>&gt;();
    }
    <span style="color:blue">public int </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Name </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">ICollection</span>&lt;<span style="color:#2b91af">Address</span>&gt; Addresses { <span style="color:blue">get </span>{ <span style="color:blue">return </span>addresses; } }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-CollectionOfComponents.png">
<img alt="CollectionOfComponents" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-CollectionOfComponents.png"></a></p>
<p><strong>4.Bidirectional relation</strong></p>
<p>实现实体与组件的双向关联关系：</p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">public int </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Name </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Name </span>ShowBusinessAlias { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">Address </span>Address { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}
<span style="color:blue">public class </span><span style="color:#2b91af">Name
</span>{
    <span style="color:blue">public </span><span style="color:#2b91af">Person </span>Person { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>First { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Last { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-BidirectionalRelation.png">
<img alt="BidirectionalRelation" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-BidirectionalRelation.png"></a></p>
<h1><a name="summary">结语</a></h1>
<p>这篇文章展示ConfORM的Components语义应用，映射了一些Domain示例。接下来继续介绍ConfORM。Are you ConfORM?</p>
<h1><a name="reference">参考资料</a></h1>
<p>Fabio Maulo：<a href="http://fabiomaulo.blogspot.com/2010/03/conform-mapping-components.html">ConfORM:“Mapping” Components</a></p>
<p>希望本文对你有所帮助。</p><img src="http://www.cnblogs.com/lyj/aggbug/1841589.html?type=1" width="1" height="1" alt=""><p>评论: 3　<a href="http://www.cnblogs.com/lyj/archive/2010/10/03/inside-nh3-conform-component.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/lyj/archive/2010/10/03/inside-nh3-conform-component.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76351/">传雅虎前高管戴维加盟Zynga负责移动业务</a><span style="color:gray">(2010-10-05 07:03)</span><br>· <a href="http://news.cnblogs.com/n/76350/">Twitter联合创始人威廉姆斯辞去CEO一职</a><span style="color:gray">(2010-10-05 07:01)</span><br>· <a href="http://news.cnblogs.com/n/76349/">传Skype即将任命新CEO 或为其年底IPO铺路</a><span style="color:gray">(2010-10-05 07:00)</span><br>· <a href="http://news.cnblogs.com/n/76348/">Gmail 开设官方 twitter 帐号</a><span style="color:gray">(2010-10-05 06:58)</span><br>· <a href="http://news.cnblogs.com/n/76347/">Google TV 迷你网站发布，透露新平台细节[视频]</a><span style="color:gray">(2010-10-05 06:56)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/76302/">2010年10月编程语言排行榜：Java的混乱之治</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>