---
layout: post
title:  "Instagram的实时图片Demo：Node.js, Redis 加 Web Sockets"
date:   2011-06-16 11:53:33
author: nosqlfan
categories: program
---

## Instagram的实时图片Demo：Node.js, Redis 加 Web Sockets
### by nosqlfan
### at 2011-06-16 11:53:33
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/523316283/6253001/1/item.html>

<p><span><a href="http://blog.nosqlfan.com/tags/instagram" title="查看 Instagram 的全部文章">Instagram</a></span>刚刚公布其<a href="http://instagram.com/developer/realtime/">实时图片API</a>的演示网站（<a href="http://demo.instagram.com/">http://demo.instagram.com/</a>）的源码，这个网站是用Node.js、<span><a href="http://blog.nosqlfan.com/tags/redis" title="查看 Redis 的全部文章">Redis</a></span>加<span><a href="http://blog.nosqlfan.com/tags/web-sockets" title="查看 Web Sockets 的全部文章">Web Sockets</a></span>来做的，主要使用了<span><a href="http://blog.nosqlfan.com/tags/redis" title="查看 Redis 的全部文章">Redis</a></span>的pub/sub机制来进行消息推送。这里是instagram<a href="https://github.com/Instagram/Realtime-Demo">官方的源码</a>，但是这位同学发现上面的代码可能由于一些版本更新已经很难再用了。于是他又好心的搞了一个新的版本，在<a href="https://github.com/asalant/Realtime-Demo">这里</a>。</p>
<p><a href="http://blog.nosqlfan.com/wp-content/uploads/2011/06/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2011-06-16-%E4%B8%8A%E5%8D%8811.48.24.jpg"><img title="屏幕快照 2011-06-16 上午11.48.24" src="http://blog.nosqlfan.com/wp-content/uploads/2011/06/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2011-06-16-%E4%B8%8A%E5%8D%8811.48.24.jpg" alt="" width="609" height="303"></a></p>
<p>相关链接：<a href="http://blog.carbonfive.com/2011/06/14/instagram-realtime-demo-with-node-js-redis-and-web-sockets/">Instagram Realtime Demo with Node.js, Redis and Web Sockets</a>
<p style="font-weight:bold">觉得文章还不错？快分享给更多的人吧！<a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html&amp;text=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/ga3Zw.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/b6giQ.png"></a><a href="http://v.t.qq.com/share/share.php?title=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/HMY8k.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/6hVj7.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html&amp;title=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/SB2B.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html&amp;title=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/3CHaH.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/1yDki.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html&amp;title=Instagram%E7%9A%84%E5%AE%9E%E6%97%B6%E5%9B%BE%E7%89%87Demo%EF%BC%9ANode.js,%20Redis%20%E5%8A%A0%20Web%20Sockets%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/ftAKQ.png"></a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="REDIS TO GO：一个Redis存储服务" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1940.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/14/12130942.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">REDIS TO GO：一个Redis存储服务</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Redis.io-Redis的新家" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F946.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11642415.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis.io-Redis的新家</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="6379-为何Redis选择它作为默认端口号？" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F790.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/14/12228787.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">6379-为何Redis选择它作为默认端口号？</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="redisfs – Redis＋FUSE 构建的文件系统" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1471.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/10/11715176.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">redisfs – Redis＋FUSE 构建的文件系统</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Redis Web界面管理工具" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F471.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2007.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11641555.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis Web界面管理工具</font>
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
</table><img src="http://www1.feedsky.com/t1/523316283/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/523316283/6253001/1/item.html" border="0" height="0" width="0"></p>