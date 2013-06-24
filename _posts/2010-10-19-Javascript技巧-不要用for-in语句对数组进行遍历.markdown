---
layout: post
title:  "Javascript技巧-不要用for in语句对数组进行遍历"
date:   2010-10-19 13:51:00
author: Mamboer
categories: program
---

## Javascript技巧-不要用for in语句对数组进行遍历
### by Mamboer
### at 2010-10-19 13:51:00
### original <http://www.cnblogs.com/mamboer/archive/2010/10/19/no-for-in-on-array.html>

<p><h3>一，为什么不要用for in语句</h3>
<p><img src="http://pic002.cnblogs.com/images/2010/15178/2010101916303147.jpg" alt=""></p>
<p>jqModal这个jquery插件估计很多人都使用过，在jqModal源码内部，有一个函数为hs，其中有个嵌套循环如下，</p>
<p> </p>
<div>
<pre><div><span style="color:#0000ff">for</span><span style="color:#000000">(</span><span style="color:#0000ff">var</span><span style="color:#000000"> i </span><span style="color:#0000ff">in</span><span style="color:#000000"> {jqmShow:</span><span style="color:#000000">1</span><span style="color:#000000">,jqmHide:</span><span style="color:#000000">1</span><span style="color:#000000">})<br>    </span><span style="background-color:#ffff99"><span style="color:#0000ff">for</span><span style="color:#000000">(</span><span style="color:#0000ff">var</span><span style="color:#000000"> s </span><span style="color:#0000ff">in</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span></span><span style="color:#000000"><span style="background-color:#ffff99">[i])</span><br>        </span><span style="color:#0000ff">if</span><span style="color:#000000">(H[</span><span style="color:#0000ff">this</span><span style="color:#000000">[i][s]])<br>            H[</span><span style="color:#0000ff">this</span><span style="color:#000000">[i][s]].w[i](</span><span style="color:#0000ff">this</span><span style="color:#000000">);<br>        </span><span style="color:#0000ff">return</span><span style="color:#000000"> F;<br>}</span></div></pre>
</div>
<p> </p>
<p>第一个for in遍历的目标是个匿名对象，没有问题。</p>
<p>第二个for in遍历，根据上下文确认this[i]是一个数组对象(Array)。</p>
<p>很多JS先驱者都告诫过我们<span style="background-color:#ffff00">不要对数组对象使用for in语句进行遍历，原因除了性能外，还有可能产生意料之内的bug</span>。不听先人言，吃亏在眼前呵呵。</p>
<p>今天<a href="http://vivasky.com">偶</a>拿jqModal为例，说明下这种bug到底什么时候会出现，当引以为戒。</p>
<h3>二，问题重现</h3>
<p>关键词：原生Array类、扩展Array类</p>
<p>for in 语句对数组对象进行遍历潜在的bug在于：如果原生Array类被其他的js脚本库进行了原型扩展（比如多加一个toJSON方法即Array.prototype.toJSON=xxxx)，那么用for in遍历扩展后的Array对象的逻辑将与遍历原生Array对象的逻辑发生差异。</p>
<p>举个简单的例子，</p>
<p> </p>
<div>
<pre><div><span style="color:#0000ff">var</span><span style="color:#000000"> x</span><span style="color:#000000">=</span><span style="color:#000000">[</span><span style="color:#000000">1</span><span style="color:#000000">];<br><br></span><span style="color:#0000ff">for</span><span style="color:#000000">(</span><span style="color:#0000ff">var</span><span style="color:#000000"> s </span><span style="color:#0000ff">in</span><span style="color:#000000"> x){<br><br>　　alert(s);<br><br>};<br></span></div></pre>
</div>
<p>
按常理，如果Array是原生js类，上面语句应该只执行一次alert方法，且s为数组的索引0。但是，如果Array类被扩展了，多了一个toJSON方法，那么上面的语句将执行两次alert，第一次s为索引0，第二次s为方法名'toJSON'。</p>
<p> </p>
<p>如果你设计的代码的逻辑以原生Array类为基准，在某一天你的同事在页面里面引用了一个第三方的JS库，这个库又恰好扩展了Array类，结果将难以想象，很有可能原来的代码逻辑将不再成立。</p>
<p> </p>
<p>关于这种扩展原生JS类的库，很有名的一个就是prototype.js，它给Array类扩展了很多方法诸如toJSON,each等等。我现在明白为啥jquery的创始人曾经对prototype火大了(不少人因为特殊原因在一个页面里用jquery同时又用prototype，会有很多意料之外的冲突问题，仅仅一个noConflict是无法解决的)。另外，jqModal的作者如果看得懂我这篇文章估计也会对埋怨prototype，说：“我用for in对数组遍历是不明智的，但是更该死的还是prototype。。。”</p>
<p> </p>
<p>如上所述，如果你在用jqModal，同时因为别的原因在用prototype，恭喜你中招了。冲突将导致jqModal的弹框在ie6、ie7下面将无法利用closeClass设置的按钮进行自动关闭。跟踪调试代码你将发现，异常的地方就在本文开头提到的hs方法的for in 循环中。。。</p>
<h3>三，解决问题</h3>
<p>遍历数组的地方，用for var 语句代替for in。</p><img src="http://www.cnblogs.com/mamboer/aggbug/1855500.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/mamboer/">Mamboer</a> 发表于 2010-10-19 13:51 <a href="http://www.cnblogs.com/mamboer/archive/2010/10/19/no-for-in-on-array.html">原文链接</a></p><p>评论: 6　<a href="http://www.cnblogs.com/mamboer/archive/2010/10/19/no-for-in-on-array.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/mamboer/archive/2010/10/19/no-for-in-on-array.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/77877/">从Office 2010的华丽转型看微软的战略野心</a><span style="color:gray">(2010-10-19 22:54)</span><br>· <a href="http://news.cnblogs.com/n/77876/">微软全球高级副总裁：基础研究保障微软生存</a><span style="color:gray">(2010-10-19 22:50)</span><br>· <a href="http://news.cnblogs.com/n/77875/">iPad将成为与iPhone比肩的苹果盈利支柱</a><span style="color:gray">(2010-10-19 21:56)</span><br>· <a href="http://news.cnblogs.com/n/77871/">一些重要的算法</a><span style="color:gray">(2010-10-19 20:20)</span><br>· <a href="http://news.cnblogs.com/n/77869/">乔布斯暗示苹果近期将实施重大收购</a><span style="color:gray">(2010-10-19 20:07)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/77829/">被绞杀的网景：互联网门口第一滴血</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>