---
layout: post
title:  "写给Python程序员的MongoDB介绍"
date:   2011-06-23 16:15:30
author: nosqlfan
categories: program
---

## 写给Python程序员的MongoDB介绍
### by nosqlfan
### at 2011-06-23 16:15:30
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/526722288/6253001/1/item.html>

<p>下面的Slide来自<a href="http://ep2011.europython.eu/">EuroPython</a>大会，<span><a href="http://blog.nosqlfan.com/tags/europython" title="查看 EuroPython 的全部文章">EuroPython</a></span>是一个在欧洲举行的python开发大会，slide作者以《<span><a href="http://blog.nosqlfan.com/tags/python" title="查看 Python 的全部文章">Python</a></span> and <span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>  The perfect Match》为题，表达了自己对<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>的喜爱。</p>
<p>Slide比较长，前面部分主要对<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>做了系统的介绍，后面介绍了使用PyMongo扩展操作MongoDB的方法，后面还推荐了 <a href="http://flask.pocoo.org/docs/patterns/mongokit/">MongoKit</a> 和 <a href="http://mongoengine.org/">MongoEngine</a> 两个<span><a href="http://blog.nosqlfan.com/tags/python" title="查看 Python 的全部文章">Python</a></span>下的MongoDB ORM封装。</p>
<div style="width:595px"><strong><a title="Python mongo db-training-europython-2011" href="http://www.slideshare.net/ajung/python-mongo-dbtrainingeuropython2011">Python mongo db-training-europython-2011</a></strong> <iframe src="http://reader.googleusercontent.com/reader/embediframe?src=http://static.slidesharecdn.com/swf/ssplayer2.swf?doc%3Dpython-mongodb-training-europython-2011-110620230438-phpapp02%26stripped_title%3Dpython-mongo-dbtrainingeuropython2011%26userName%3Dajung&amp;width=595&amp;height=497" width="595" height="497"></iframe>
<div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/ajung">Andreas Jung</a></div>
</div>
<p style="font-weight:bold"><span style="padding-top:10px;float:left">觉得文章还不错？快分享给更多的人吧！</span><a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html&amp;text=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/ga3Zw.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/b6giQ.png"></a><a href="http://v.t.qq.com/share/share.php?title=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/HMY8k.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/6hVj7.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html&amp;title=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/SB2B.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html&amp;title=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/3CHaH.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/1yDki.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html&amp;title=%E5%86%99%E7%BB%99Python%E7%A8%8B%E5%BA%8F%E5%91%98%E7%9A%84MongoDB%E4%BB%8B%E7%BB%8D%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/ftAKQ.png"></a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="又一篇给Python程序员的MongoDB教程" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2195.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">又一篇给Python程序员的MongoDB教程</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB:下一代MySQL？" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2215.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/27/14609183.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB:下一代MySQL？</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="从 Google Trends 看 MongoDB，北京关注度居首位" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2206.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/27/14592753.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">从 Google Trends 看 MongoDB，北京关注度居首位</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="mtop – 针对 MongoDB 的 top 命令" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1425.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">mtop – 针对 MongoDB 的 top 命令</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB介绍PPT分享" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F778.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2153.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB介绍PPT分享</font>
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
</table><img src="http://www1.feedsky.com/t1/526722288/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/526722288/6253001/1/item.html" border="0" height="0" width="0">