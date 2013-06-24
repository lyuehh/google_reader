---
layout: post
title:  "Cross-browser TextStorage"
date:   2011-09-16 11:34:56
author: sofish
categories: program
---

## Cross-browser TextStorage
### by sofish
### at 2011-09-16 11:34:56
### original <http://sofish.de/1872>

<p>一直用着 UserData 来实现 IE 中的伪 LocalStorage。不过，并没有装成一个模块用。9.9 号去北京的那天上午写了一个解决方案。目标是模仿 HTML5 LocalStorage 的 API，实现 IE 上使用相同 API 就可以存储文本的方案。看代码不一定能了解所有逻辑，这里简单说一下，然后有兴趣的再看代码吧，或者直接拿去用。</p>
<ul>
<li>利用 IE 的 userData Behavior 来实现 IE 的文本存储</li>
<li>将所有保存属性存于 &lt;body&gt; 中，并在 &lt;html&gt; 的 data-userData 做记号
<p>为什么要只存储在 body 标签？因为如果存于不同标签，实现 localStorage.clear() 的效率将非常低。在 html 做标记只是为了方便 localStorage.clear() 的实现。至于没有 &lt;body&gt; / &lt;html&gt; 也可以运行 html，但这个方案不能运行，我觉得不用去支持，程序应该写给会最基本标准的人用。</p>
</li>
<li>getItem / setItem / removeItem / clear 方法保持与 HTML5 LocalStorage 一样的用法</li>
</ul>
<p>好吧，其他的，就自己看代码吧。Demo 在这里：<a href="http://sofish.de/file/demo/localStorage.html"><span>Cross-browser TextStorage</span></a> 。代码下面：</p>
<p></p>
<div style="color:#888;margin-top:30px"><p><hr>© 2012 <a href="http://sofish.de" title="幸福收藏夹">幸福收藏夹</a>。 版权所有，转载务必注明。域名已经更新为：<a href="http://sofish.de" title="幸福收藏夹">sofish.de</a>。<br><strong>注意：</strong>当你觉得某篇文章有用，请直接看原文，因为通常我都会在写了文章后更新、去错别字、升级观点之类的。</p></div><img src="http://www1.feedsky.com/t1/592690557/sofish/feedsky/s.gif?r=http://sofish.de/1872" border="0" height="0" width="0">