---
layout: post
title:  "MongoLive：在Chrome里进行MongoDB实时监控"
date:   2011-06-14 17:42:55
author: nosqlfan
categories: program
---

## MongoLive：在Chrome里进行MongoDB实时监控
### by nosqlfan
### at 2011-06-14 17:42:55
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/523316288/6253001/1/item.html>

<p><a href="https://chrome.google.com/webstore/detail/apgglicbkgjcfnohdcgbcobengnkcjef?hl=zh-CN#">Mongo Live </a>是一个Chrome<span><a href="http://blog.nosqlfan.com/tags/%e6%8f%92%e4%bb%b6" title="查看 插件 的全部文章">插件</a></span>，安装后你可以指定你 <span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span> 的RESTFul接口的地址，然后它会画出实时的<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span> 的操作操作执行曲线，非常方便。</p>
<p>唯一需要注意的是你需要在启动MongoDB时指定–<span><a href="http://blog.nosqlfan.com/tags/rest" title="查看 rest 的全部文章">rest</a></span>参数以开启RESTFul访问接口。</p>
<p><a href="http://blog.nosqlfan.com/wp-content/uploads/2011/06/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2011-06-14-%E4%B8%8B%E5%8D%8805.40.43.jpg"><img title="屏幕快照 2011-06-14 下午05.40.43" src="http://blog.nosqlfan.com/wp-content/uploads/2011/06/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7-2011-06-14-%E4%B8%8B%E5%8D%8805.40.43.jpg" alt="" width="531" height="494"></a></p>
<p style="font-weight:bold">觉得文章还不错？快分享给更多的人吧！<a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html&amp;text=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/ga3Zw.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/b6giQ.png"></a><a href="http://v.t.qq.com/share/share.php?title=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/HMY8k.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/6hVj7.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html&amp;title=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/SB2B.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html&amp;title=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/3CHaH.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/1yDki.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html&amp;title=MongoLive%EF%BC%9A%E5%9C%A8Chrome%E9%87%8C%E8%BF%9B%E8%A1%8CMongoDB%E5%AE%9E%E6%97%B6%E7%9B%91%E6%8E%A7%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/ftAKQ.png"></a><table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="MongoDB管理工具" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F213.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11638867.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB管理工具</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="mtop – 针对 MongoDB 的 top 命令" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1425.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">mtop – 针对 MongoDB 的 top 命令</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="监控你的MongoDB（MongoSV 精华摘录）" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1022.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">监控你的MongoDB（MongoSV 精华摘录）</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="简单的MongoDB" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F219.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/9MymB.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">简单的MongoDB</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB系列教程" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F348.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1964.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11640559.gif" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB系列教程</font>
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
</table><img src="http://www1.feedsky.com/t1/523316288/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/523316288/6253001/1/item.html" border="0" height="0" width="0"></p>