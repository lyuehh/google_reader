---
layout: post
title:  "JQuery Smart UI (一) 简介 — 纯Htm+Js的ajax开发框架"
date:   2010-11-16 22:57:00
author: Roy Zhang
categories: program
---

## JQuery Smart UI (一) 简介 — 纯Htm+Js的ajax开发框架
### by Roy Zhang
### at 2010-11-16 22:57:00
### original <http://www.cnblogs.com/zhh8077/archive/2010/11/16/1879037.html>

<p><p>　　JQuery Smart UI是基于JQuery的Ajax开发框架，实现前、后台分离、功能和数据分离，UI层全部使用htm+js+json完成，通过一个统一数据接口与服务端进行数据交换。</p>
<p>     主要有三部分组成：</p>
<ul>
<li><strong>一套日常应用的JQuery插件(类似JQuery UI,JQuery EasyUI)，核心基于template.js模板插件,具有强大的自定义功能。</strong></li>
</ul>
<p>    <img src="http://pic002.cnblogs.com/images/2010/10174/2010111622475211.png" alt=""></p>
<ul>
<li><strong>一套前端开发框架（js、css），满足各种基本场景，有着不错的封装、扩展性。系统自动完成动态创建页面，与后台交互，取、赋值等繁琐的操作。</strong></li>
<strong></strong>
</ul>
<p>　　例：查询场景，其中查询条件区域、列表区域，数据格式化都是通过简单的配置和方法动态生成。</p>
<p><img src="http://pic002.cnblogs.com/images/2010/10174/2010111622481647.png" alt=""></p>
<p>　　</p>
<p><img src="http://pic002.cnblogs.com/images/2010/10174/2010111622100684.png" alt=""></p>
<ul>
<li><strong>与Smart UI无缝结合的后台数据框架（.net）。</strong></li>
</ul>
<ol>
<li>统一数据交互接口，便于统一管理和维护。</li>
<li>FnCode的方式，权限控制到每个方法。</li>
<li>在开源框架【NBearLite】的基础上扩展，使用“结构实体”来解析前台的Json数据，避免了ORM中实体的反射转换的性能消耗。</li>
<li>数据库操作实现读写分离，分别用不同类实现；结合“结构实体”自动的绑定回发的数据，使用更加便捷。</li>
<li>多种数据库支持和支持多数据库；</li>
</ol>
<p>   <img src="http://pic002.cnblogs.com/images/2010/10174/2010111622405895.jpg" alt=""></p>
<p> </p>
<p>   几个月时间奋战，JQuery Smart UI终于完成一版，先简单介绍一下，明天放出完整的演示Demo。</p>
<p> </p><img src="http://www.cnblogs.com/zhh8077/aggbug/1879037.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/zhh8077/">Roy Zhang</a> 发表于 2010-11-16 22:57 <a href="http://www.cnblogs.com/zhh8077/archive/2010/11/16/1879037.html">原文链接</a></p><p>评论: 20　<a href="http://www.cnblogs.com/zhh8077/archive/2010/11/16/1879037.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/zhh8077/archive/2010/11/16/1879037.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/81386/">微软将为其社交游戏服务建立统一平台</a><span style="color:gray">(2010-11-17 11:53)</span><br>· <a href="http://news.cnblogs.com/n/81385/">百度生活产品“百度身边”今日公测上线</a><span style="color:gray">(2010-11-17 11:47)</span><br>· <a href="http://news.cnblogs.com/n/81384/">沈南鹏：微博应用平台的三大投资机会</a><span style="color:gray">(2010-11-17 11:29)</span><br>· <a href="http://news.cnblogs.com/n/81383/">《星际争霸II》创造盗版新纪录 - 下载量突破15.77PB</a><span style="color:gray">(2010-11-17 11:27)</span><br>· <a href="http://news.cnblogs.com/n/81382/">版本管理器的发展史</a><span style="color:gray">(2010-11-17 11:26)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/81361/">微软Imagine Cup 2011，用科技解决全球最棘手问题</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>