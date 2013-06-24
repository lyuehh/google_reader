---
layout: post
title:  "Python调用MongoDB使用心得"
date:   2011-09-07 11:33:11
author: nosqlfan
categories: program
---

## Python调用MongoDB使用心得
### by nosqlfan
### at 2011-09-07 11:33:11
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/555157232/6253001/1/item.html>

<p>本文是一个<span><a href="http://blog.nosqlfan.com/tags/python" title="查看 Python 的全部文章">Python</a></span> 使用<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>的简单教程，将使用<span><a href="http://blog.nosqlfan.com/tags/pymongo" title="查看 pymongo 的全部文章">pymongo</a></span>对MongoDB进行的各种操作进行了简单的汇总，NoSQLFan进行了简单整理，使用Python的同学可以看一看。</p>
<blockquote><p>下载相应平台的版本，解压即可。为方便使用，将bin路径添加到系统path环境变量里。其中mongod是服务器，mongo是客户shell，然后创建数据文件目录：在c盘下创建data文件夹，里面创建db文件夹。</p>
<p>基本使用：</p>
<p><img title="pymongo" src="http://pic.yupoo.com/iammutex/BlM6njVR/kPjgU.png" alt="" width="487" height="139"></p>
<p>安装对应语言的Driver，Python 安装 pymongo</p>
<pre>$ easy_install pymongo</pre>
<p><span><a href="http://blog.nosqlfan.com/tags/%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95" title="查看 使用方法 的全部文章">使用方法</a></span>总结，摘自官方教程</p>
<p>创建连接</p>
<pre>&gt;&gt;&gt; import pymongo
&gt;&gt;&gt; connection=pymongo.Connection(&#39;localhost&#39;,27017)</pre>
<p>切换数据库</p>
<pre>&gt;&gt;&gt; db = connection.test_database</pre>
<p>获取collection</p>
<pre>&gt;&gt;&gt; collection = db.test_collection</pre>
<p>db和collection都是延时创建的，在添加Document时才真正创建</p>
<p>文档添加，_id自动创建</p>
<pre>&gt;&gt;&gt; import datetime
&gt;&gt;&gt; post = {&quot;author&quot;: &quot;Mike&quot;,
...         &quot;text&quot;: &quot;My first blog post!&quot;,
...         &quot;tags&quot;: [&quot;mongodb&quot;, &quot;python&quot;, &quot;pymongo&quot;],
...         &quot;date&quot;: datetime.datetime.utcnow()}
&gt;&gt;&gt; posts = db.posts
&gt;&gt;&gt; posts.insert(post)
ObjectId(&#39;...&#39;)</pre>
<p>批量插入</p>
<pre>&gt;&gt;&gt; new_posts = [{&quot;author&quot;: &quot;Mike&quot;,
...               &quot;text&quot;: &quot;Another post!&quot;,
...               &quot;tags&quot;: [&quot;bulk&quot;, &quot;insert&quot;],
...               &quot;date&quot;: datetime.datetime(2009, 11, 12, 11, 14)},
...              {&quot;author&quot;: &quot;Eliot&quot;,
...               &quot;title&quot;: &quot;MongoDB is fun&quot;,
...               &quot;text&quot;: &quot;and pretty easy too!&quot;,
...               &quot;date&quot;: datetime.datetime(2009, 11, 10, 10, 45)}]
&gt;&gt;&gt; posts.insert(new_posts)
[ObjectId(&#39;...&#39;), ObjectId(&#39;...&#39;)]</pre>
<p>获取所有collection（相当于SQL的show tables）</p>
<pre>&gt;&gt;&gt; db.collection_names()
[u&#39;posts&#39;, u&#39;system.indexes&#39;]</pre>
<p>获取单个文档</p>
<pre>&gt;&gt;&gt; posts.find_one()
{u&#39;date&#39;: datetime.datetime(...), u&#39;text&#39;: u&#39;My first blog post!&#39;, u&#39;_id&#39;: ObjectId(&#39;...&#39;), u&#39;author&#39;: u&#39;Mike&#39;, u&#39;tags&#39;: [u&#39;mongodb&#39;, u&#39;python&#39;, u&#39;pymongo&#39;]}</pre>
<p>查询多个文档</p>
<pre>&gt;&gt; for post in posts.find():
...   post
...
{u&#39;date&#39;: datetime.datetime(...), u&#39;text&#39;: u&#39;My first blog post!&#39;, u&#39;_id&#39;: ObjectId(&#39;...&#39;), u&#39;author&#39;: u&#39;Mike&#39;, u&#39;tags&#39;: [u&#39;mongodb&#39;, u&#39;python&#39;, u&#39;pymongo&#39;]}
{u&#39;date&#39;: datetime.datetime(2009, 11, 12, 11, 14), u&#39;text&#39;: u&#39;Another post!&#39;, u&#39;_id&#39;: ObjectId(&#39;...&#39;), u&#39;author&#39;: u&#39;Mike&#39;, u&#39;tags&#39;: [u&#39;bulk&#39;, u&#39;insert&#39;]}
{u&#39;date&#39;: datetime.datetime(2009, 11, 10, 10, 45), u&#39;text&#39;: u&#39;and pretty easy too!&#39;, u&#39;_id&#39;: ObjectId(&#39;...&#39;), u&#39;author&#39;: u&#39;Eliot&#39;, u&#39;title&#39;: u&#39;MongoDB is fun&#39;}</pre>
<p>加条件的查询</p>
<pre>&gt;&gt;&gt; posts.find_one({&quot;author&quot;: &quot;Mike&quot;})</pre>
<p>高级查询</p>
<pre>&gt;&gt;&gt; posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;)</pre>
<p>统计数量</p>
<pre>&gt;&gt;&gt; posts.count()
3</pre>
<p>加索引</p>
<pre>&gt;&gt;&gt; from pymongo import ASCENDING, DESCENDING
&gt;&gt;&gt; posts.create_index([(&quot;date&quot;, DESCENDING), (&quot;author&quot;, ASCENDING)])
u&#39;date_-1_author_1&#39;</pre>
<p>查看查询语句的性能</p>
<pre>&gt;&gt;&gt; posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;).explain()[&quot;cursor&quot;]
u&#39;BtreeCursor date_-1_author_1&#39;
&gt;&gt;&gt; posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;).explain()[&quot;nscanned&quot;]
2</pre>
<p>附自己总结的一点小心得，仅供参考</p>
<p><strong>缺点</strong></p>
<ul>
<li>不是全盘取代传统数据库（NoSQLFan：是否能取代需要看应用场景）</li>
<li>不支持复杂事务（NoSQLFan：MongoDB只支持对单个文档的原子操作）</li>
<li>文档中的整个树，不易搜索，4MB限制？（NoSQLFan：1.8版本已经修改为16M）</li>
</ul>
<p><strong>特点</strong>（NoSQLFan：作者在这里列举的很多只是一些表层的特点）：</p>
<ul>
<li>文档型数据库，表结构可以内嵌</li>
<li>没有模式，避免空字段开销（Schema Free）</li>
<li>分布式支持</li>
<li>查询支持正则</li>
<li>动态扩展架构</li>
<li>32位的版本最多只能存储2.5GB的数据（NoSQLFan：最大文件尺寸为2G，生产环境推荐64位）</li>
</ul>
<p><strong>名词对应</strong></p>
<ul>
<li>一个数据项叫做 Document（NoSQLFan：对应MySQL中的单条记录）</li>
<li>一个文档嵌入另一个文档（comment 嵌入 post）叫做 Embed</li>
<li>储存一系列文档的地方叫做 Collections（NoSQLFan：对应MySQL中的表）</li>
<li>表间关联，叫做 Reference</li>
</ul>
</blockquote>
<p>来源链接已经404了～暂无法附来源。</p>
<table cellspacing="0" cellpadding="2" border="0" width="100%" style="clear:both">
    
    <tr>
        <td><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">写给Python程序员的MongoDB介绍</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2195.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">又一篇给Python程序员的MongoDB教程</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F383.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">Mongodb Mapreduce 初窥</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F231.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">MongoDB 1.4.3 发布</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F172.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">MongoDB 1.4.2 (stable) Released</font>
                    </a>
                </td>
            </tr>
    
    <tr>
        <td align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/555157232/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/555157232/6253001/1/item.html" border="0" height="0" width="0">