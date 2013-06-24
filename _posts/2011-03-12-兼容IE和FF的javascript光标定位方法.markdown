---
layout: post
title:  "兼容IE和FF的javascript光标定位方法"
date:   2011-03-12 12:05:00
author: 空中飞人飞空中
categories: program
---

## 兼容IE和FF的javascript光标定位方法
### by 空中飞人飞空中
### at 2011-03-12 12:05:00
### original <http://www.cnblogs.com/jiaguang/archive/2011/03/12/1982126.html>

<p><p>今天在做一个博客转发的功能，就像新浪微薄转播的那个功能一样，有一个textArea,就是我们发微薄的那个框，别人的微薄都有一个转发的按钮，当我们点击这个转发按钮的时候，他的微薄就会进入到textArea中，看起来很简单的文本框赋值，<span style="color:#ff0000">其实难点就在于，这时候的光标是定位在最前面的。</span></p>
<p><span style="color:#ff0000"><span style="color:#000000">于是搜索了baidu,google,找到了</span></span></p>
<p><span style="color:#ff0000"><span style="color:#000000"><strong>IE下面的方法</strong><br></span></span></p>
<p><span style="color:#3366ff">var tea=document.getElementById("文本框的ID");</span><br><span style="color:#3366ff">var txt=textArea.createTextRange();</span><br><span style="color:#3366ff">txt.moveEnd("character",0-tempText.text.length);</span><br><span style="color:#3366ff">txt.select();</span></p>
<p>但是这个方法只有在IE的浏览器下面才可用，于是通过网络搜索找到了博客园的一篇博客</p>
<p><a href="http://www.cnblogs.com/zmz911/archive/2010/03/24/1694061.html">http://www.cnblogs.com/zmz911/archive/2010/03/24/1694061.html</a></p>

<p><strong>找到了FF下面的方法</strong></p>
<p><span style="color:#3366ff">var tea=document.getElementById("文本框的ID");</span><br><span style="color:#3366ff">tea</span><span style="color:#3366ff">.setSelectionRange(0, 0); </span> <span style="color:#c0c0c0">//将光标定位在textarea的开头，需要定位到其他位置的请自行修改</span><br><span style="color:#3366ff">tea</span><span style="color:#3366ff">.focus();</span></p>

<p><span style="color:#3366ff">所以兼容的方法我们可以用一个if加入判断，整合方法如下，也正如那个帖子里的一样</span></p>
<span style="color:#99cc00">html部分</span><br>
<div>
<pre>&lt;input id=&quot;tea&quot; type=&quot;text&quot; size=&quot;100&quot; value=&quot;&quot;&gt;
&lt;button onclick=&quot;xx()&quot;&gt;转发&lt;/button&gt;
</pre>
</div>
<br><br><span style="color:#99cc00">JS部分</span><br><span style="color:#993300">
<div>
<pre>&lt;script language=&quot;javascript&quot;&gt;     
var tea = document.getElementById(&quot;tea&quot;);
function locatePoint(){
  if (tea.setSelectionRange) {
    setTimeout(function() {
      tea.setSelectionRange(0, 0);  //将光标定位在textarea的开头，需要定位到其他位置的请自行修改
      tea.focus();
    }, 0);
  }else if (tea.createTextRange) {
    var txt=tea.createTextRange();
    txt.moveEnd(&quot;character&quot;,0-txt.text.length);
    txt.select();
  }
}
function xx(){
  tea.value = &#39;bbb&#39;;
  locatePoint();
}
&lt;/script&gt; 
</pre>
</div>
</span><span style="color:#993300"></span>    <br>

<p><span style="color:#3366ff"><br></span></p><img src="http://www.cnblogs.com/jiaguang/aggbug/1982126.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/jiaguang/">空中飞人飞空中</a> 发表于 2011-03-12 12:05 <a href="http://www.cnblogs.com/jiaguang/archive/2011/03/12/1982126.html">原文链接</a></p><p>评论: 2　<a href="http://www.cnblogs.com/jiaguang/archive/2011/03/12/1982126.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/jiaguang/archive/2011/03/12/1982126.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/93894/">iPad阴影下的平板电脑：遭遇黎明前的黑暗</a><span style="color:gray">(2011-03-13 10:22)</span><br>· <a href="http://news.cnblogs.com/n/93893/">Engadget主编宣布离职疑因不满AOL政策</a><span style="color:gray">(2011-03-13 10:20)</span><br>· <a href="http://news.cnblogs.com/n/93892/">Facebook计划测试团购服务挑战Groupon</a><span style="color:gray">(2011-03-13 10:19)</span><br>· <a href="http://news.cnblogs.com/n/93891/">惠普首席执行官李艾科：将收购更多软件公司</a><span style="color:gray">(2011-03-13 09:55)</span><br>· <a href="http://news.cnblogs.com/n/93890/">LinkedIn推出新闻整合服务LinkedIn Today</a><span style="color:gray">(2011-03-13 09:51)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/cmt/archive/2011/03/12/1982414.html">【公告】服务器成功迁移至新机房</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>