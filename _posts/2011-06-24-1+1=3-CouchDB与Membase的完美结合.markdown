---
layout: post
title:  "1+1=3 CouchDB与Membase的完美结合"
date:   2011-06-24 11:03:28
author: nosqlfan
categories: program
---

## 1+1=3 CouchDB与Membase的完美结合
### by nosqlfan
### at 2011-06-24 11:03:28
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/526891885/6253001/1/item.html>

<p><span><a href="http://blog.nosqlfan.com/tags/couchdb" title="查看 CouchDB 的全部文章">CouchDB</a></span>是文档型NoSQL中的出色代表，也是目前对移动设备最友好的NoSQL系统（<a href="http://www.readwriteweb.com/hack/2011/03/couchbase-ios-developer-preview.php">iOS</a>,<a href="http://blog.nosqlfan.com/html/1163.html">Android</a>），<span><a href="http://blog.nosqlfan.com/tags/membase" title="查看 Membase 的全部文章">Membase</a></span>是Memcached的加强版，提供商业化的集群管理工具，是非常完善的分布式缓存系统。</p>
<p>而前段时间两者合并成为Couchbase。本文就是对Couchbase的一个讲解，讲述了Couchbase是如何实现1＋1＝3的。</p>
<div style="width:595px"><strong><a title="Couchdb + Membase = Couchbase" href="http://www.slideshare.net/iammutex/couchdb-membase-couchbase">Couchdb + Membase = Couchbase</a></strong> <iframe src="http://reader.googleusercontent.com/reader/embediframe?src=http://static.slidesharecdn.com/swf/ssplayer2.swf?doc%3Ddaleharveybb2011-110623214902-phpapp02%26stripped_title%3Dcouchdb-membase-couchbase%26userName%3Diammutex&amp;width=595&amp;height=497" width="595" height="497"></iframe>
<div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/iammutex">iammutex</a></div>
</div>
<p style="font-weight:bold"><span style="padding-top:10px;float:left">觉得文章还不错？快分享给更多的人吧！</span><a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html&amp;text=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/ga3Zw.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/b6giQ.png"></a><a href="http://v.t.qq.com/share/share.php?title=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/HMY8k.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/6hVj7.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html&amp;title=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/SB2B.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html&amp;title=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/3CHaH.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/1yDki.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html&amp;title=1+1=3%20CouchDB%E4%B8%8EMembase%E7%9A%84%E5%AE%8C%E7%BE%8E%E7%BB%93%E5%90%88%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/ftAKQ.png"></a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="node.js、CouchDB搭建博客全教程" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2222.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/27/14636405.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">node.js、CouchDB搭建博客全教程</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="CouchBase创新应用赢大奖" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1842.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">CouchBase创新应用赢大奖</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Couchbase2.0 性能将大幅提升" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2033.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/16/12678253.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Couchbase2.0 性能将大幅提升</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="CouchDB与我：CouchDB之父讲述CouchDB背后的故事" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2022.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/16/12650931.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">CouchDB与我：CouchDB之父讲述CouchDB背后的故事</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Couchbase发布Membase Server1.7版本：集群、监控、管理功能增强" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1858.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2160.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Couchbase发布Membase Server1.7版本：集群、监控、管理功能增强</font>
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
</table><img src="http://www1.feedsky.com/t1/526891885/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/526891885/6253001/1/item.html" border="0" height="0" width="0">