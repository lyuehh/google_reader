---
layout: post
title:  "NHibernate3剖析：Mapping篇之ConfORM实战(4):ManyToMany语义"
date:   2010-09-26 21:03:00
author: 李永京
categories: program
---

## NHibernate3剖析：Mapping篇之ConfORM实战(4):ManyToMany语义
### by 李永京
### at 2010-09-26 21:03:00
### original <http://www.cnblogs.com/lyj/archive/2010/09/26/inside-nh3-conform-manytomany.html>

<p><a href="http://www.cnblogs.com/lyj/"><img src="http://pic.cnblogs.com/face/u20012.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/lyj/">李永京</a> 发表于 2010-09-26 21:03 <a href="http://www.cnblogs.com/lyj/archive/2010/09/26/inside-nh3-conform-manytomany.html">原文链接</a> 阅读: 568 评论: 11</p><p><em><strong>本节内容</strong></em></p>
<ul>
    <li><a href="http://www.cnblogs.com/rss#insidenh3">系列引入</a></li>
    <li><a href="http://www.cnblogs.com/rss#introduction">ConfORM概述</a></li>
	<li><a href="http://www.cnblogs.com/rss#manytomany">Many-To-Many语义</a></li>
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
</ul>
<p>如果你不熟悉ConfORM请查看前几篇文章，你可以到<a href="http://code.google.com/p/codeconform/">http://code.google.com/p/codeconform/</a>获取ConfORM最新版本。</p>
<p>在Domain设计中经常使用集合，在.Net中的集合有四种：Iesi.Collections.Generic.ISet&lt;T&gt;、System.Collections.Generic.ICollection&lt;T&gt;、System.Collections.Generic.IList&lt;T&gt;、System.Collections.Generic.IDictionary&lt;TKey,TValue&gt;，NHibernate分别使用Set、Bag、List、Map映射来映射这些集合，有关这些集合映射基础可以参考下面四篇文章，手动编写简单的Mapping，从此杜绝啥啥啥生成工具(生成的东西太糟蹋了)：</p>
<ul>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/05/08/inside-nh3-mapping-set.html">NHibernate3剖析：Mapping篇之集合映射基础(1):Set映射</a></li>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/05/10/inside-nh3-mapping-bag.html">NHibernate3剖析：Mapping篇之集合映射基础(2):Bag映射</a></li>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/05/12/inside-nh3-mapping-list.html">NHibernate3剖析：Mapping篇之集合映射基础(3):List映射</a></li>
	<li><a href="http://www.cnblogs.com/lyj/archive/2010/05/12/inside-nh3-mapping-map.html">NHibernate3剖析：Mapping篇之集合映射基础(4):Map映射</a></li>
</ul>
<p>这篇我们使用ConfORM“映射”多对多关联。关联关系有单向关联和双向关联两种，所以分为单向多对多关联(Unidirectional many-to-many)、双向多对多关联(Bidirectional many-to-many)。</p>
<h1><a name="manytomany">Many-To-Many语义</a></h1>
<p>使用ConfORM“映射”多对多关联，无论是单向关联还是双向关联，我们只需要使用ObjectRelationalMapper类中的ManyToMany方法即可(下面示例代码的黑体)。ConfORM会根据Domain解析集合成员进行配置。</p>
<pre><span>[<span style="color:#2b91af">Test</span>]
<span style="color:blue">public void </span>ManyToManyMappingDemo()
{
    <span style="color:green">//show how work with many-to-many and how ConfORM understands OOP
    </span><span style="color:blue">var </span>orm = <span style="color:blue">new </span><span style="color:#2b91af">ObjectRelationalMapper</span>();
    <span style="color:blue">var </span>mapper = <span style="color:blue">new </span><span style="color:#2b91af">Mapper</span>(orm);
    <span style="color:blue">var </span>entities = <span style="color:blue">new</span>[] { <span style="color:blue">typeof</span>(<span style="color:#2b91af">Person</span>), <span style="color:blue">typeof</span>(<span style="color:#2b91af">Address</span>) };
    <span style="color:green">//use the definition of table-to-class strategy class by class
    </span>orm.TablePerClass(entities);
    <span style="color:green">// Defining relations</span>
    <strong>orm.ManyToMany&lt;<span style="color:#2b91af">Person</span>, <span style="color:#2b91af">Address</span>&gt;();</strong>
    <span style="color:green">// Show the mapping to the console
    </span><span style="color:blue">var </span>mapping = mapper.CompileMappingFor(entities);
    <span style="color:#2b91af">Console</span>.Write(mapping.AsString());
}</span></pre>
<h2>单向多对多关联(Unidirectional many-to-many)</h2>
<p>我们使用各种集合定义Domain：</p>
<p><strong>1.Unidirectional using Set&lt;T&gt;</strong></p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">private </span><span style="color:#2b91af">ISet</span>&lt;<span style="color:#2b91af">Address</span>&gt; addresses;
    <span style="color:blue">public </span>Person()
    {
        addresses = <span style="color:blue">new </span><span style="color:#2b91af">HashedSet</span>&lt;<span style="color:#2b91af">Address</span>&gt;();
    }
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">ICollection</span>&lt;<span style="color:#2b91af">Address</span>&gt; Addresses
    {
        <span style="color:blue">get </span>{ <span style="color:blue">return </span>addresses; }
    }
}
<span style="color:blue">public class </span><span style="color:#2b91af">Address
</span>{
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Street { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public int </span>CivicNumber { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>上面测试输出HbmMapping的映射字符串，如果你使用<a href="http://www.jetbrains.com/resharper/">ReSharper</a>或者<a href="http://www.testdriven.net/">TestDriven.Net</a>工具测试，你可以看见下面输出：</p>
<p>注意红色块是关键代码，只是按其要求配置了必需标签，其中access是指访问策略，ConfORM根据Domain定义选择了一个正确的访问策略。</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingSet.png">
<img alt="UnidirectionalManyToManyMappingSet" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingSet.png"></a></p>
<p><strong>2.Unidirectional using Bag&lt;T&gt;</strong></p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">private </span><span style="color:#2b91af">ICollection</span>&lt;<span style="color:#2b91af">Address</span>&gt; addresses;
    <span style="color:blue">public </span>Person()
    {
        addresses = <span style="color:blue">new </span><span style="color:#2b91af">List</span>&lt;<span style="color:#2b91af">Address</span>&gt;();
    }
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">ICollection</span>&lt;<span style="color:#2b91af">Address</span>&gt; Addresses
    {
        <span style="color:blue">get </span>{ <span style="color:blue">return </span>addresses; }
    }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingBag.png">
<img alt="UnidirectionalManyToManyMappingBag" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingBag.png"></a></p>
<p><strong>3.Unidirectional using List&lt;T&gt;</strong></p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">private </span><span style="color:#2b91af">IList</span>&lt;<span style="color:#2b91af">Address</span>&gt; addresses;
    <span style="color:blue">public </span>Person()
    {
        addresses = <span style="color:blue">new </span><span style="color:#2b91af">List</span>&lt;<span style="color:#2b91af">Address</span>&gt;();
    }
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">ICollection</span>&lt;<span style="color:#2b91af">Address</span>&gt; Addresses
    {
        <span style="color:blue">get </span>{ <span style="color:blue">return </span>addresses; }
    }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingList.png">
<img alt="UnidirectionalManyToManyMappingList" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingList.png"></a></p>
<p><strong>4.Unidirectional using Map&lt;TK,TV&gt;</strong></p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">Person
</span>{
    <span style="color:blue">private </span><span style="color:#2b91af">IDictionary</span>&lt;<span style="color:blue">string</span>, <span style="color:#2b91af">Address</span>&gt; addresses;
    <span style="color:blue">public </span>Person()
    {
        addresses = <span style="color:blue">new </span><span style="color:#2b91af">Dictionary</span>&lt;<span style="color:blue">string</span>, <span style="color:#2b91af">Address</span>&gt;();
    }
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">IDictionary</span>&lt;<span style="color:blue">string</span>, <span style="color:#2b91af">Address</span>&gt; Addresses
    {
        <span style="color:blue">get </span>{ <span style="color:blue">return </span>addresses; }
    }
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingMap.png">
<img alt="UnidirectionalManyToManyMappingMap" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-UnidirectionalManyToManyMappingMap.png"></a></p>
<h2>双向多对多关联(Bidirectional many-to-many)</h2>
<p><strong>Domain</strong></p>
<pre><span><span style="color:blue">public class </span><span style="color:#2b91af">User
</span>{
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">ISet</span>&lt;<span style="color:#2b91af">Role</span>&gt; Roles { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}

<span style="color:blue">public class </span><span style="color:#2b91af">Role
</span>{
    <span style="color:blue">public </span><span style="color:#2b91af">Guid </span>Id { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public string </span>Name { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
    <span style="color:blue">public </span><span style="color:#2b91af">ISet</span>&lt;<span style="color:#2b91af">User</span>&gt; Users { <span style="color:blue">get</span>; <span style="color:blue">set</span>; }
}</span></pre>
<p><strong>ConfORM</strong></p>
<pre><span><span style="color:blue">public void </span>DomainDefinition(<span style="color:#2b91af">ObjectRelationalMapper </span>orm)
{
    orm.TablePerClass(<span style="color:blue">new</span>[] { <span style="color:blue">typeof</span>(<span style="color:#2b91af">User</span>), <span style="color:blue">typeof</span>(<span style="color:#2b91af">Role</span>) });
    orm.ManyToMany&lt;<span style="color:#2b91af">User</span>, <span style="color:#2b91af">Role</span>&gt;();
}</span></pre>
<p><strong>Mapping</strong></p>
<p>输出HbmMapping的映射字符串结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-BidirectionalManyToManyMapping.png">
<img alt="BidirectionalManyToManyMapping" src="http://images.cnblogs.com/cnblogs_com/lyj/NH3/ConfOrm-BidirectionalManyToManyMapping.png"></a></p>
<h1><a name="summary">结语</a></h1>
<p>这篇文章展示ConfORM的Many-To-Many语义应用，映射了两种Many-To-Many映射。大家同时也注意到了，上面列名的命名规则感觉有点别扭，这是ConfORM按照默认的模式适配器配置了，我们可以增加自定义模式适配器或者使用通用定制化、特定定制化达到自定义的目的，这些内容接下来的文章中介绍。</p>
<h1><a name="reference">参考资料</a></h1>
<p>Fabio Maulo：<a href="http://fabiomaulo.blogspot.com/2010/03/conform-mapping-many-to-many.html">ConfORM: “Mapping” Many-To-Many</a></p>
<p>希望本文对你有所帮助。</p>
<img src="http://www.cnblogs.com/lyj/aggbug/1836265.html?type=1" width="1" height="1" alt=""><p>评论: 11　<a href="http://www.cnblogs.com/lyj/archive/2010/09/26/inside-nh3-conform-manytomany.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/lyj/archive/2010/09/26/inside-nh3-conform-manytomany.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/75633/">分析称诺基亚官僚文化扼杀创新</a><span style="color:gray">(2010-09-27 11:35)</span><br>· <a href="http://news.cnblogs.com/n/75632/">评论：李开复的盗梦空间</a><span style="color:gray">(2010-09-27 11:31)</span><br>· <a href="http://news.cnblogs.com/n/75631/">Groupon给创业公司的经验：自我营销化繁为简</a><span style="color:gray">(2010-09-27 11:28)</span><br>· <a href="http://news.cnblogs.com/n/75630/">德国专家设想给网络信息设“失效期”</a><span style="color:gray">(2010-09-27 11:26)</span><br>· <a href="http://news.cnblogs.com/n/75629/">凡客诚品宣布全面进军运动服市场</a><span style="color:gray">(2010-09-27 11:13)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/75617/">Google今日涂鸦：庆祝12岁生日</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>