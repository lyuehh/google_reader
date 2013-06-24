---
layout: post
title:  "JavaScript 执行覆盖分析工具 —— Tracker"
date:   2013-01-14 01:24:06
author: dron
categories: program
---

## JavaScript 执行覆盖分析工具 —— Tracker
### by dron
### at 2013-01-14 01:24:06
### original <http://ucren.com/blog/archives/319>

<p>这个工具写了有小半个月了，本来想等做完美了再发出来，想想每天的进展这样慢就算了，先发也能更早收到使用者的反馈，初版肯定会有很多 bug，希望大家能够向我提出来。</p>
<h3>简介</h3>
<p>为了能更快速的完成开发，我们往往会在项目里引入各种各样的 JS 库，而实际上，项目本身对这些 JS 库的利用率很少，很多情况下只是用了它其中的一两个功能，这样导致项目产生大量的冗余代码，一定程度上影响了网页加载速度，如果有方便的工具，可以分析出项目在运行过程中覆盖到的 JS 代码，将大大有利于我们做后续的瘦身工作，Tracker 这个工具的想法就这样产生的。另外，和 JSCoverage 一样，Tracker 也能帮助我们方便地发现测试用例对源码的覆盖程度。</p>
<p><span></span></p>
<p>工具的界面分为两部分，刚打开时是一个 JS 列表，显示了当前页面所引入的所有 JS（包括内嵌的和外链的两种），列表显示出这些 JS 代码在当前页面下执行的执行覆盖率、执行行数、是否存在执行报错或语法错误以及状态等信息。点击其中一条记录便进入当前代码界面，执行过的 JS 代码飘绿，没有执行过的代码为灰色。</p>
<h3>使用方法</h3>
<h5>方法一</h5>
<p>打开某个网页后，在浏览器的地址栏里直接输入以下代码：</p>
<p>
<pre><code style="word-wrap:break-word;word-spacing:normal">javascript:void%20function(t,r,a,c,k){(k=t.TrackerGlobalEvent)?k.f(r):[(k=t[a]('script')).charset='utf-8',k.src='http://www.ucren.com/'+c+'/'+c+'.js?'+Math.random(),t.documentElement.appendChild(k)]}(document,'TrackerJSLoad','createElement','tracker')</code></pre>
</p>
<p>Chrome 貌似要先手工敲入“javascript:”，再将以上这段粘贴进去。</p>
<h5>方法二（推荐）</h5>
<p>将以下链接添加到（拖放到）您的书签栏里，打开指定网页后，再点击书签栏里的 Tracker 链接：</p>
<p><a style="color:#333;background-color:#a6e591;line-height:30px;padding:0 10px;border-radius:5px;display:inline-block;text-decoration:none;font-size:16px" href="javascript:void(0);">Tracker!</a>  ← 就是它了</p>
<h3>兼容性说明</h3>
<p>目前只测了 chrome 浏览器，另外，工具显式的屏蔽了 IE 系列浏鉴器。</p>
<h3>工具截图</h3>
<p><img src="http://ucren.com/blog/wp-content/uploads/2013/01/tracker-12.gif" alt="" title="tracker-1" width="650" height="687" style="background:transparent;border:none"></p>