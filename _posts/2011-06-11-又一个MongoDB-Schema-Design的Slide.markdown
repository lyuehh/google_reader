---
layout: post
title:  "又一个MongoDB Schema Design的Slide"
date:   2011-06-11 11:49:25
author: nosqlfan
categories: program
---

## 又一个MongoDB Schema Design的Slide
### by nosqlfan
### at 2011-06-11 11:49:25
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/522009618/6253001/1/item.html>

<p>此Slide的作者是10gen的工程师<a href="http://twitter.com/hwaet">Kyle Banker</a>，来自 MongoNYC 会议。主要关于MongoDB的文档式存储模式介绍和在MongoDB存储下的Schema 设计，内容比较丰富，值得一看。</p>
<div style="width:595px"><strong><a title="MongoDB Schema Design - MongoDB NYC Meetup" href="http://www.slideshare.net/kbanker/mongodb-meetup">MongoDB Schema Design – MongoDB NYC Meetup</a></strong> <iframe src="http://reader.googleusercontent.com/reader/embediframe?src=http://static.slidesharecdn.com/swf/ssplayer2.swf?doc%3Dmongodb-meetup-schema-design-100921151002-phpapp02%26stripped_title%3Dmongodb-meetup%26userName%3Dkbanker&amp;width=595&amp;height=497" width="595" height="497"></iframe>
<div style="padding:5px 0 12px">View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/kbanker">Kyle Banker</a></div>
</div>
<p style="font-weight:bold">觉得文章还不错？快分享给更多的人吧！<a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html&amp;text=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/ga3Zw.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/b6giQ.png"></a><a href="http://v.t.qq.com/share/share.php?title=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/HMY8k.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/6hVj7.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html&amp;title=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/SB2B.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html&amp;title=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/3CHaH.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/1yDki.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html&amp;title=%E5%8F%88%E4%B8%80%E4%B8%AAMongoDB%20Schema%20Design%E7%9A%84Slide%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/ftAKQ.png"></a><table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="MongoDB Schema Design" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F798.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB Schema Design</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MySQL到MongoDB的同步工具" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1688.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11639050.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MySQL到MongoDB的同步工具</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="在 VMware CloudFoundry 平台上构建 MongoDB ＋ Rails 应用" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1635.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/10/11691672.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">在 VMware CloudFoundry 平台上构建 MongoDB ＋ Rails 应用</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB 的数据可靠性，单机可靠性有望在1.8版本后增强" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1066.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11642170.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB 的数据可靠性，单机可靠性有望在1.8版本后增强</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB 文档阅读笔记 —— 优雅的 NoSQL" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1369.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11640641.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB 文档阅读笔记 —— 优雅的 NoSQL</font>
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
</table><img src="http://www1.feedsky.com/t1/522009618/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/522009618/6253001/1/item.html" border="0" height="0" width="0"></p>