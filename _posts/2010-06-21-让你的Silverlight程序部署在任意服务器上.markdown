---
layout: post
title:  "让你的Silverlight程序部署在任意服务器上"
date:   2010-06-21 06:38:00
author: jv9
categories: program
---

## 让你的Silverlight程序部署在任意服务器上
### by jv9
### at 2010-06-21 06:38:00
### original <http://www.cnblogs.com/jv9/archive/2010/06/21/1761671.html>

<p>作者: <a href="http://www.cnblogs.com/jv9/">jv9</a> 发表于 2010-06-21 06:38 <a href="http://www.cnblogs.com/jv9/archive/2010/06/21/1761671.html">原文链接</a> 阅读: 1779 评论: 6</p><p>今天在CSDN上逛，看到一篇不错的教程贴，“让你的SilverLight程序部署在任意服务器上”，转到园子里，希望更多朋友受益。</p>
<p> </p>
<p>
<p>即使是免费的只支持HTML的空间，同样可以部署SilverLight应用。<br>众所周知，SilverLight的部署问题其实就是.xap文件名是否能被服务器支持的问题。解决的方法无非就是添加MIME类型。<br>但是大多数时候我们并没有操作IIS的权限（比如刚刚申请的免费空间）。<br>好了，下面开始进入正题吧。<br>我的朋友聪明的把.xap文件名改为.zip（原因大家应该知道，xap文件其实就是zip）。但是zip也可能不会被服务器支持，因为这些免费的空间只能显示HTML！因此我把.xap改成了.html（我就是这么改的！），SilverLight程序就可以神奇的显示在HTML页面里了。</p>
<p> </p>
<p>这是个不错的展示Silverlight项目方法，但是局限性只能展示简单Silverlight项目，对于复杂Silverlight项目不能支持跨域，数据库，OOB等功能支持。<br></p>
<p> </p> <img src="http://www.cnblogs.com/jv9/aggbug/1761671.html?type=1" width="1" height="1" alt=""><p>评论: 6　<a href="http://www.cnblogs.com/jv9/archive/2010/06/21/1761671.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/jv9/archive/2010/06/21/1761671.html#commentform">发表评论</a></p><p><a href="http://a4.yeshj.com/rd/35721/">众里寻你千百度，百度期待您的加盟</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/66685/">微软发布iPhone版Windows Live Messenger</a><span style="color:gray">(2010-06-21 16:57)</span><br>· <a href="http://news.cnblogs.com/n/66683/">上海网民每天平均上网3.63小时</a><span style="color:gray">(2010-06-21 16:53)</span><br>· <a href="http://news.cnblogs.com/n/66682/">门户世界杯广告数据爆出 腾讯网广告量质超群</a><span style="color:gray">(2010-06-21 16:50)</span><br>· <a href="http://news.cnblogs.com/n/66681/">meego也疯狂：MeeGo下试玩QQ</a><span style="color:gray">(2010-06-21 16:47)</span><br>· <a href="http://news.cnblogs.com/n/66680/">Google改进VP8编解码器</a><span style="color:gray">(2010-06-21 16:26)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/topic/53/">[热点话题]编程语言之争</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>