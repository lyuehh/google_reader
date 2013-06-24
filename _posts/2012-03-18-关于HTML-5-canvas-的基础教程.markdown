---
layout: post
title:  "关于HTML 5 canvas 的基础教程"
date:   2012-03-18 15:13:28
author: 
categories: program
---

## 关于HTML 5 canvas 的基础教程
### by 
### at 2012-03-18 15:13:28
### original <http://item.feedsky.com/~feedsky/bingo929/~7170939/597761176/5279850/1/item.html>

<p><a href="http://blog.bingo929.com/html-5-canvas-the-basics-html5.html"><img src="http://blog.bingo929.com/wp-content/uploads/2010/06/1.jpg" alt="" title="HTML5 Canvas 基础教程" width="500" height="150"></a></p>
<p><a href="http://www.whatwg.org/specs/web-apps/current-work/multipage/">HTML 5 规范</a>引进了很多新特性，其中最令人期待的之一就是 <a href="http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html">canvas 元素</a>。<a href="http://blog.bingo929.com/category/technology/html5">HTML 5</a> canvas 提供了通过 <a href="http://blog.bingo929.com/category/technology/javascript">JavaScript</a> 绘制图形的方法，此方法使用简单但功能强大。每一个canvas 元素都有一个”上下文( context )” (想象成绘图板上的一页)，在其中可以绘制任意图形。浏览器支持多个 canvas 上下文，并通过不同的<abbr title="Application Programmable         Interfaces"> API</abbr> 提供图形绘制功能。</p>
<p><span></span></p>
<p>大部分的浏览器都支持 2D canvas 上下文——包括 <a href="http://opera.com/">Opera</a>, <a href="http://mozilla.com/">Firefox</a>, <a href="http://konqueror.org/">Konqueror</a> 和 <a href="http://apple.com/safari">Safari</a>。而且某些版本的 Opera 还支持 3D canvas ，Firefox 也可以通过插件形式支持 3D canvas ：</p>
<ul>
<li><a href="http://labs.opera.com/news/2008/11/25/">下载支持 3D canvas, HTML video 和 File I/O 的 Opera</a></li>
<li><a href="http://my.opera.com/timjoh/blog/2007/11/13/taking-the-canvas-to-another-dimension">关于 Opera 3D canvas 上下文的文章</a></li>
<li><a href="http://blog.vlad1.com/2007/11/26/canvas-3d-gl-power-web-style/">关于 Firefox 3D canvas<br>
        上下文的文章</a></li>
</ul>
<p>本文介绍 2D canvas<br>
    基础以及如何使用基本 canvas 函数，如线条、形状、图像和文字等。为了理解此文章，你最好了解 JavaScript 基础知识。</p>
<p><a href="http://dev.opera.com/articles/view/html-5-canvas-the-basics/canvas-primer.zip">可以点击此处批量下载本文实例代码</a></p>
<h3>目录</h3>
<ul>
<li><a href="http://blog.bingo929.com/#introduction">简述</a></li>
<li><a href="http://blog.bingo929.com/#thebasicsofusingcanvas">canvas 基础</a></li>
<li><a href="http://blog.bingo929.com/#the2dcontextapi"> 2D context API</a>
<ul>
<li><a href="http://blog.bingo929.com/#basiclinesandstrokes">基本线条</a></li>
<li><a href="http://blog.bingo929.com/#paths">路径</a></li>
<li><a href="http://blog.bingo929.com/#insertingimages">插入图像</a></li>
<li><a href="http://blog.bingo929.com/#pixelbasedmanipulation">像素级操作</a></li>
<li><a href="http://blog.bingo929.com/#text">文字</a></li>
<li><a href="http://blog.bingo929.com/#shadows">阴影</a></li>
<li><a href="http://blog.bingo929.com/#gradients">颜色渐变</a></li>
</ul>
</li>
<li><a href="http://blog.bingo929.com/#summary">小节</a></li>
</ul>
<h3>canvas 基础</h3>
<p>创建 canvas  的方法很简单，只需要在 HTML 页面中添加 &lt;canvas&gt; 元素：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#009900">&lt;canvas <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;myCanvas&quot;</span> <span style="color:#000066">width</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;300&quot;</span> <span style="color:#000066">height</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;150&quot;</span>&gt;</span><br>
Fallback content, in case the browser does not support Canvas.<br>
<span style="color:#009900">&lt;<span style="color:#66cc66">/</span>canvas&gt;</span></div></div>
<p>为了能在 JavaScript 中引用元素，最好给元素设置 ID ；也需要给 canvas 设定高度和宽度。</p>
<p>创建好了画布后，让我们来准备画笔。要在画布中绘制图形需要使用 JavaScript 。首先通过 getElementById 函数找到 canvas<br>
    元素，然后初始化上下文。之后可以使用上下文 API 绘制各种图形。下面的脚本在 canvas 中绘制一个矩形 (<a href="http://www.robodesign.ro/coding/canvas-primer/20081208/example-using-canvas.html">点击此处查看效果</a>)：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#006600;font-style:italic">// Get a reference to the element.</span><br>
<span style="color:#003366;font-weight:bold">var</span> elem <span style="color:#339933">=</span> document.<span style="color:#660066">getElementById</span><span style="color:#009900">(</span><span style="color:#3366cc">'myCanvas'</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
 <br>
<span style="color:#006600;font-style:italic">// Always check for properties 和 methods, to make sure your code doesn't break </span><br>
<span style="color:#006600;font-style:italic">// in other browsers.</span><br>
<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>elem <span style="color:#339933">&amp;</span>amp<span style="color:#339933">;&amp;</span>amp<span style="color:#339933">;</span> elem.<span style="color:#660066">getContext</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
  <span style="color:#006600;font-style:italic">// Get the 2d context.</span><br>
  <span style="color:#006600;font-style:italic">// Remember: you can only initialize one context per element.</span><br>
  <span style="color:#003366;font-weight:bold">var</span> context <span style="color:#339933">=</span> elem.<span style="color:#660066">getContext</span><span style="color:#009900">(</span><span style="color:#3366cc">'2d'</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
  <span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>context<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
    <span style="color:#006600;font-style:italic">// You are done! Now you can draw your first rectangle.</span><br>
    <span style="color:#006600;font-style:italic">// You only need to provide the (x,y) coordinates, followed by the width and </span><br>
    <span style="color:#006600;font-style:italic">// height dimensions.</span><br>
    context.<span style="color:#660066">fillRect</span><span style="color:#009900">(</span><span style="color:#cc0000">0</span><span style="color:#339933">,</span> <span style="color:#cc0000">0</span><span style="color:#339933">,</span> <span style="color:#cc0000">150</span><span style="color:#339933">,</span> <span style="color:#cc0000">100</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
  <span style="color:#009900">}</span><br>
<span style="color:#009900">}</span></div></div>
<p>可以把上面代码放置在文档 head 部分中，或者放在外é</p>