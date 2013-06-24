---
layout: post
title:  "CouchDB，反Redis的产物？－Redis与CouchDB特性对比"
date:   2011-08-09 23:33:20
author: nosqlfan
categories: program
---

## CouchDB，反Redis的产物？－Redis与CouchDB特性对比
### by nosqlfan
### at 2011-08-09 23:33:20
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/544836997/6253001/1/item.html>

<p>本文是一篇转载翻译文章，原文标题是 Is <span><a href="http://blog.nosqlfan.com/tags/couchdb" title="查看 CouchDB 的全部文章">CouchDB</a></span> The Anti-<span><a href="http://blog.nosqlfan.com/tags/redis" title="查看 Redis 的全部文章">Redis</a></span>? 作者在对比了Redis和CouchDB之后得出这样一个结论，这两家伙是反着来的，当然，这个反着来没有对和错，只是适合的应用场景不同，本人觉得其评价还是比较中肯，下面是对其主要内容的摘录和翻译，希望对你有用。</p>
<p><a href="http://blog.nosqlfan.com/wp-content/uploads/2011/08/couchdbredis.jpg"><img title="couchdbredis" src="http://blog.nosqlfan.com/wp-content/uploads/2011/08/couchdbredis.jpg" alt="" width="491" height="111"></a></p>
<p>相比来看，CouchDB 的长处正是Redis的短处：存储大量的不易变但会被经常查询的数据。Redis的长处正是CouchDB的短处：存储小量的常变数据。</p>
<p>以一个博客系统为例，CouchDB作为一个文档型数据库，可以用来存储文章，评论，模板及附件等，而Redis以其丰富的数据类型的数据结构，更适合用来存储评论列表，网站实时状态，过滤spam，用户session信息以及页面缓存。</p>
<p>作为一个内存数据库，Redis提供了快速对其数据结构进行复杂操作的功能，另外通过一份顺序的日志来保证其数据可靠性。</p>
<p>CouchDB使用了一种<span><a href="http://blog.nosqlfan.com/tags/append-only" title="查看 append-only 的全部文章">append-only</a></span>的数据模型，不仅在数据库数据存储上，包括其B-tree和R-tree索引都是append-only的，所以如果你的数据修改操作太多（比如计数器应用），那么CouchDB的数据文件会飞速膨胀。</p>
<p>Redis采用定时将内存数据Flush成RDB文件的方法来实现数据的持久化，而CouchDB的数据需要定时做数据压缩以缩减数据文件的大小，这一过程会把数据文件读入，压缩后再写成新的文件。是一个非常耗时的过程。</p>
<p>Redis提供了简单的索引机制和复杂的数据结构，而CouchDB提供的是复杂的索引和简单的数据结构。Redis适合用来存储实时数据，而CouchDB适合用来存储大量的文档型数据。</p>
<p>下面是一个更详细的各方面对比表格：</p>
<table border="1" cellspacing="0" cellpadding="0" width="620">
<tbody>
<tr height="20">
<td width="210"></td>
<td width="210">Couchdb</td>
<td width="210">Redis</td>
</tr>
<tr height="20">
<td height="20">Written in</td>
<td>Erlang</td>
<td>C</td>
</tr>
<tr height="20">
<td height="20">License</td>
<td>Apache</td>
<td>BSD</td>
</tr>
<tr height="20">
<td height="20">Release</td>
<td>1.1.0, 2.0 preview</td>
<td>2.2.12, 2.4.0RC5</td>
</tr>
<tr height="20">
<td height="20">API</td>
<td>REST</td>
<td>Telnet-style</td>
</tr>
<tr height="20">
<td height="20">API Speed</td>
<td>Slow</td>
<td>Fast</td>
</tr>
<tr height="20">
<td height="20">Data</td>
<td>JSON documents, binary attachments</td>
<td>Text, binary, hash, list, set, sorted set</td>
</tr>
<tr height="20">
<td height="20">Indexes</td>
<td>B-tree, R-tree, Full-text (with Lucene), any combination of data types via map/reduce</td>
<td>Hash only</td>
</tr>
<tr height="20">
<td height="20">Queries</td>
<td>Predefined view/list/show model, ad-hoc queries require table scans</td>
<td>Individual keys</td>
</tr>
<tr height="20">
<td height="20">Storage</td>
<td>Append-only on disk</td>
<td>In-memory, append-only log</td>
</tr>
<tr height="20">
<td height="20">Updates</td>
<td>MVCC</td>
<td>In-place</td>
</tr>
<tr height="20">
<td height="20">Transactions</td>
<td>Yes, all-or-nothing batches</td>
<td>Yes, with conditional commands</td>
</tr>
<tr height="20">
<td height="20">Compaction</td>
<td>File rewrite</td>
<td>Snapshot</td>
</tr>
<tr height="20">
<td height="20">Threading</td>
<td>Many threads</td>
<td>Single-threaded, forks for snapshots</td>
</tr>
<tr height="20">
<td height="20">Multi-Core</td>
<td>Yes</td>
<td>No</td>
</tr>
<tr height="20">
<td height="20">Memory</td>
<td>Tiny</td>
<td>Large (all data)</td>
</tr>
<tr height="20">
<td height="20">SSD-Friendly</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr height="20">
<td height="20">Robust</td>
<td>Yes</td>
<td>Yes</td>
</tr>
<tr height="20">
<td height="20">Backup</td>
<td>Just copy the files</td>
<td>Just copy the files</td>
</tr>
<tr height="20">
<td height="20">Replication</td>
<td>Master-master, automatic</td>
<td>Master-slave, automatic</td>
</tr>
<tr height="20">
<td height="20">Scaling</td>
<td>Clustering (BigCouch)</td>
<td>Clustering (Redis cluster*)</td>
</tr>
<tr height="20">
<td height="20">Scripting</td>
<td>JavaScript, Erlang, others via plugin</td>
<td>Lua*</td>
</tr>
<tr height="20">
<td height="20">Files</td>
<td>One per database</td>
<td>One per database</td>
</tr>
<tr height="20">
<td height="20">Virtual Files</td>
<td>Attachments</td>
<td>No</td>
</tr>
<tr height="20">
<td height="20">Other</td>
<td>Changes feed, Standalone applications</td>
<td>Pub/Sub, Key expiry</td>
</tr>
</tbody>
</table>
<p>来源：<a href="http://ai.mee.nu/is_couchdb_the_anti-redis">ai.mee.nu</a>
<p style="font-weight:bold"><span style="padding-top:5px;float:left">技术传播，需要你我共同努力！</span><a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html&amp;text=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVKEJk/custom.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html" title="新浪微博" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVKrzm/custom.png"></a><a href="http://v.t.qq.com/share/share.php?title=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html" title="腾讯微博" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJX6o/custom.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html" title="QQ空间" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJSKI/custom.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html&amp;title=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJrri/custom.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html&amp;title=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVJ4v4/custom.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html" title="人人网" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVI86k/custom.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html&amp;title=CouchDB%EF%BC%8C%E5%8F%8DRedis%E7%9A%84%E4%BA%A7%E7%89%A9%EF%BC%9F%EF%BC%8DRedis%E4%B8%8ECouchDB%E7%89%B9%E6%80%A7%E5%AF%B9%E6%AF%94%20%E6%9D%A5%E8%87%AA%EF%BC%9A@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px;border:none"><img style="border:none;padding:0px" src="http://pic.yupoo.com/iammutex/B8hVHr67/custom.png"></a>    </p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Redis事件驱动库结构" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2799.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/08/14/22784581.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis事件驱动库结构</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="OSCON2011精华摘录：Learning CouchDB" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2680.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/08/02/20570253.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">OSCON2011精华摘录：Learning CouchDB</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Redis4You：Redis云存储服务商" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2789.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis4You：Redis云存储服务商</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="CouchDB 歌曲大串烧" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2748.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/08/08/21646183.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">CouchDB 歌曲大串烧</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Reds：一个Redis加Node.js的全文搜索引擎" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2676.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2754.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/08/02/20527286.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Reds：一个Redis加Node.js的全文搜索引擎</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="5" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/544836997/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/544836997/6253001/1/item.html" border="0" height="0" width="0"></p>