---
layout: post
title:  "1 &lt; 2 &lt; 3为true， 3 &gt; 2 &gt; 1为false"
date:   2011-11-23 10:29:00
author: 司徒正美
categories: program
---

## 1 &lt; 2 &lt; 3为true， 3 &gt; 2 &gt; 1为false
### by 司徒正美
### at 2011-11-23 10:29:00
### original <http://www.cnblogs.com/rubylouvre/archive/2011/11/23/2259678.html>

<p><p>如题目，javascript的运算符有时很有趣，把它们粘到firefox上看看吧。</p>
<p>为什么会这样呢？</p>
<p>其实这是由从左到右比较，通过内部的ToNumber方法比较计算出来的</p>
<pre>
3 &gt; 2 &gt; 1 → (3 &gt; 2) &gt; 1 → true &gt; 1 → 1 &gt; 1 → false
</pre>
<p>反过来</p>
<pre>
1 &lt; 2 &lt; 3 → (1 &lt; 2) &lt; 3 → true &lt; 3 → 1 &lt; 3 → true
</pre>
<p>如果把大于号小于号改成==等，这也很有趣。1 == 1 == 1 为true, 2 == 2 == 2为false.</p>
<img src="http://www.cnblogs.com/rubylouvre/aggbug/2259678.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/rubylouvre/">司徒正美</a> 发表于 2011-11-23 10:29 <a href="http://www.cnblogs.com/rubylouvre/archive/2011/11/23/2259678.html">原文链接</a></p><p>评论: 1　<a href="http://www.cnblogs.com/rubylouvre/archive/2011/11/23/2259678.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/rubylouvre/archive/2011/11/23/2259678.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/125094/">苹果第三季度游说支出46万美元 同比增长35%</a><span style="color:gray">(2011-12-13 15:12)</span><br>· <a href="http://news.cnblogs.com/n/125093/">解析Twitter人才流失困境：骄傲自满 庸才泛滥</a><span style="color:gray">(2011-12-13 15:06)</span><br>· <a href="http://news.cnblogs.com/n/125092/">百度收购日文手机输入法Simeji</a><span style="color:gray">(2011-12-13 14:58)</span><br>· <a href="http://news.cnblogs.com/n/125090/">苹果的重新崛起</a><span style="color:gray">(2011-12-13 14:57)</span><br>· <a href="http://news.cnblogs.com/n/125089/">新版 Twitter 值得欣赏</a><span style="color:gray">(2011-12-13 14:42)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/leslies2/archive/2011/12/12/2272722.html">结合领域驱动设计的SOA分布式软件架构</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://q.cnblogs.com/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>