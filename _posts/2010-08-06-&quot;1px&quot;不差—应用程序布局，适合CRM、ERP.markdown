---
layout: post
title:  "&quot;1px&quot;不差—应用程序布局，适合CRM、ERP"
date:   2010-08-06 23:31:00
author: Junior Lau
categories: program
---

## &quot;1px&quot;不差—应用程序布局，适合CRM、ERP
### by Junior Lau
### at 2010-08-06 23:31:00
### original <http://www.cnblogs.com/lauyee/archive/2010/08/06/1794474.html>

<p><a href="http://www.cnblogs.com/lauyee/"><img src="http://pic.cnblogs.com/face/u55295.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/lauyee/">Junior Lau</a> 发表于 2010-08-06 23:31 <a href="http://www.cnblogs.com/lauyee/archive/2010/08/06/1794474.html">原文链接</a> 阅读: 2008 评论: 68</p>
<p>      适合CRM、ERP可以用到的框架布局，高度宽度都做到了自适应。右侧的iframe也是高度自适应 。调整了不同浏览器那几个px的差异,多种浏览器表现一模一样。只是使用的标准的简单的CSS代码，代码简单但是还是比较细致的。基本没有用到CSS hack。初学者，高手都只得一看。</p>
<p>     我知道，一般程序员，都比较烦这个，不屑于做这个东西。如果觉得有用拿去用吧，如果觉得没用。仍几块砖给我。<br>&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD XHTML 1.0 Transitional//EN&quot; &quot;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&quot;&gt;
&lt;html xmlns=&quot;http://www.w3.org/1999/xhtml&quot;&gt;
&lt;head&gt;
    &lt;title&gt;无标题页&lt;/title&gt;
    &lt;style&gt;
       body, html
        {
            margin: 0;
            height: 100%;
            background-color: Aqua;
            overflow: hidden;
        }
        #top
        {
            height: 80px;
            width: auto;
        }
        #middle
        {
            border-style: solid;
            border-width: 1px 0px;
            border-color: #000000;
            width: 100%;
            height: auto;
            top: 80px;
            bottom: 20px;
            position: absolute;
            background-color: DarkGray;
        }
        #bottom
        {
            height: 20px;
            width: auto;
            bottom: 0;
            position: absolute;
        }
        #left
        {
            float: left;
            width: 200px;
            border-right: solid 1px White;
            overflow: auto;
            height: 100%; /* 兼容代码 */
            top: 0px;
            bottom: 0px;
            position: absolute;
            left: 0px; /* 兼容代码 */
        }
        #right
        {
            background-color: #efefef;
            overflow: auto;
            height: 100%; /* 兼容代码 */
            left: 201px;
            top: 0px;
            bottom: 0px;
            position: absolute;
            right: 0px; /* 兼容代码 */
        }
        #main
        {
            top: 0px;
            bottom: 0px;
            position: absolute;
            width: 100%;
            height: 100%;
        }
    
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;form id=&quot;form1&quot;&gt;
    &lt;div id=&quot;divAll&quot;&gt;
        &lt;div id=&quot;top&quot;&gt;
          &lt;h3&gt;Logo&lt;/h3&gt;
        &lt;/div&gt;
        &lt;div id=&quot;middle&quot;&gt;
            &lt;div id=&quot;left&quot;&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;导航
                left&lt;br /&gt;导航
                left&lt;br /&gt;导航导航
                left&lt;br /&gt;导航
                left&lt;br /&gt;导航导航
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
                left&lt;br /&gt;
            &lt;/div&gt;
            &lt;div id=&quot;right&quot;&gt;
                &lt;iframe src=&quot;http://www.cnblogs.com&quot; frameborder=&quot;0&quot; 
                 marginheight=&quot;0&quot; id=&quot;main&quot; name=&quot;main&quot;&gt;&lt;/iframe&gt;
            &lt;/div&gt;
        &lt;/div&gt;
        &lt;div id=&quot;bottom&quot;&gt;
           版权信息
        &lt;/div&gt;
    &lt;/div&gt;
    &lt;/form&gt;
&lt;/body&gt;</p>
<p></p>
<p> </p>
<p>   补：看到很多朋友提出疑问，说即用了绝对定位，有用了浮动。那是经过调了几次发现只有这样才能显示正常。如果你使用IE8浏览器，则注释和注释之间的部分都是可以省去的。对于CSS我其实也是门外汉，以上代码都是经过尝试做出的。能用就是王道。有些朋友说IE6下iframe会出现问题。这真是一个值得在修改尝试的问题，因为企业里用IE6的也不在少数。所以了解原理在加上实践才是最可行的。</p>
<p>     在实际的应用中如果要做到精致，很难不用js再对样式做出调整，总之解决问题才是最重要的王道，其次是代码简洁。而技巧，我们也只能当它是技巧。</p>
<p> </p>
<p> </p>
<p>    补：另一件郁闷的事，博客园的这个“HTML 原始程序代码编辑器”在我把代码贴进去后自动生成了两套样式，不知是什么原因。已改正。</p><img src="http://www.cnblogs.com/lauyee/aggbug/1794474.html?type=1" width="1" height="1" alt=""><p>评论: 68　<a href="http://www.cnblogs.com/lauyee/archive/2010/08/06/1794474.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/lauyee/archive/2010/08/06/1794474.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/70268/">微软将改进Windows 8语音控制功能</a><span style="color:gray">(2010-08-08 00:08)</span><br>· <a href="http://news.cnblogs.com/n/70267/">Symbian手机每天销量达30万部 远超iPhone</a><span style="color:gray">(2010-08-08 00:04)</span><br>· <a href="http://news.cnblogs.com/n/70266/">传苹果1.5亿美元收购中国网页游戏开发商</a><span style="color:gray">(2010-08-08 00:01)</span><br>· <a href="http://news.cnblogs.com/n/70265/">研究人员曝Windows内核漏洞 影响所有版本</a><span style="color:gray">(2010-08-07 23:59)</span><br>· <a href="http://news.cnblogs.com/n/70264/">谷歌中国高调招聘或和中国大型国企合作</a><span style="color:gray">(2010-08-07 23:55)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/70241/">浅谈大型网站动态应用系统架构</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>