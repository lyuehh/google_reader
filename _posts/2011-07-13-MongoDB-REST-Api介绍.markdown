---
layout: post
title:  "MongoDB REST Api介绍"
date:   2011-07-13 00:36:19
author: nosqlfan
categories: program
---

## MongoDB REST Api介绍
### by nosqlfan
### at 2011-07-13 00:36:19
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/535603143/6253001/1/item.html>

<p><span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>默认会开启一个<span><a href="http://blog.nosqlfan.com/tags/http" title="查看 HTTP 的全部文章">HTTP</a></span>协议的端口提供REST的服务，这个端口是你Server端口加上1000，比如你的Server端口为27017，那么这个HTTP端口就是28017，默认的HTTP端口功能是有限的，你可以通过添加–<span><a href="http://blog.nosqlfan.com/tags/rest" title="查看 rest 的全部文章">rest</a></span>参数启动更多功能。下面是在这个端口通过其<span><a href="http://blog.nosqlfan.com/tags/restful" title="查看 RESTFul 的全部文章">RESTFul</a></span> 的API操作MongoDB数据的几个例子，来源是<a href="http://www.mongodb.org/display/DOCS/Http+Interface">MongoDB官方文档</a>。</p>
<p>下面是直接通过浏览器访问相应端口的HTTP服务时的页面，页面上显示了很多Server相关的信息</p>
<p><a href="http://blog.nosqlfan.com/wp-content/uploads/2011/07/console.png"><img title="console" src="http://blog.nosqlfan.com/wp-content/uploads/2011/07/console.png" alt="" width="598" height="435"></a></p>
<p>下面是一系列操作数据的方法：</p>
<h3>列出databaseName数据库中的collectionName集合下的所有数据：</h3>
<pre>http://127.0.0.1:28017/databaseName/collectionName/</pre>
<h3>给上面的数据集添加一个limit参数限制返回10条</h3>
<pre>http://127.0.0.1:28017/databaseName/collectionName/?limit=-10</pre>
<h3>给上面的数据加上一个skip参数设定跳过5条记录</h3>
<pre>http://127.0.0.1:28017/databaseName/collectionName/?skip=5</pre>
<h3>同时加上limit限制和skip限制</h3>
<pre>http://127.0.0.1:28017/databaseName/collectionName/?skip=5&amp;limit=10</pre>
<h3>按条件{a:1}进行结果筛选（在关键字filter后面接上你的字段名）</h3>
<pre>http://127.0.0.1:28017/databaseName/collectionName/?filter_a=1</pre>
<h3>加条件的同时再加上limit限制返回条数</h3>
<pre>http://127.0.0.1:28017/databaseName/collectionName/?filter_a=1&amp;limit=-10</pre>
<h3>执行任意命令</h3>
<p>如果你要执行特定的命令，可以通过在admin.$cmd上面执行find命令，同样的你也可以在REST API里实现，如下，执行{listDatabase:1}命令：</p>
<pre>http://localhost:28017/admin/$cmd/?filter_listDatabases=1&amp;limit=1</pre>
<p style="font-weight:bold"><span style="padding-top:5px;float:left">技术传播，需要你我共同努力！</span><a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html&amp;text=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/custom.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/custom.png"></a><a href="http://v.t.qq.com/share/share.php?title=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/custom.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/custom.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html&amp;title=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/custom.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html&amp;title=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/custom.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/custom.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html&amp;title=MongoDB%20REST%20Api%E4%BB%8B%E7%BB%8D%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/custom.png"></a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Neo4j REST API使用教程" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2360.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/07/08/16659799.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Neo4j REST API使用教程</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoLive：在Chrome里进行MongoDB实时监控" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/14/12233210.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoLive：在Chrome里进行MongoDB实时监控</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB 与 CouchDB 全方位对比" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1519.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB 与 CouchDB 全方位对比</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Webdis – 为 Redis 提供 HTTP 接口" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1031.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Webdis – 为 Redis 提供 HTTP 接口</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB数据缓存刷新机制" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2409.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2391.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB数据缓存刷新机制</font>
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
</table><img src="http://www1.feedsky.com/t1/535603143/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/535603143/6253001/1/item.html" border="0" height="0" width="0">