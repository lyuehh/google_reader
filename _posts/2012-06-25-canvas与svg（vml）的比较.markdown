---
layout: post
title:  "canvas与svg（vml）的比较"
date:   2012-06-25 13:37:33
author: island205
categories: program
---

## canvas与svg（vml）的比较
### by island205
### at 2012-06-25 13:37:33
### original <http://island205.com/2012/06/25/canvas-vs-svgvml/>

<p>在图形可视化开发方面有很多技术可以使用（例如Flash、canvas、svg和vml等等，甚至可以是使用PHP等一些后台直接生成静态图片），本文仅对canvas和svg（包括vml，技术与svg有相似之处，下面省略）进行比较。两者都有各自的优缺点，适用与不同的场景和需求，大家可按需选用。</p>
<p><strong>像素图与矢量图</strong></p>
<p>从图形学方面来看，这是两者最大的不同，canvas是像素点阵式的，形象点说基本上就是一张可操作像素的图片。而svg是矢量（方向+长度）的。</p>
<p>在svg中可以加载点阵图，在canvas中可以画line和图形，但是最终的表现，svg是矢量的，而canvas中都是一个个点。</p>
<p><strong>输出源</strong></p>
<p>这里的输出源指的就是产生方式。svg是基于xml的，可以像写HTML那样编写svg，当然也可以由后端直接输出。svg有DOM接口，同时也可以使用JavaScript来生成和修改（可参考<a href="http://www.i-programmer.info/programming/graphics-and-imaging/3254-svg-javascript-and-the-dom.html">svg、javascript和dom</a>）。</p>
<p>而canvas是基于JavaScript的，通过canvas的DOM接口来绘制图形，这意味着使用后端来生成canvas绘图基本不可能（当然后端编译生成JavaScript输出到前端绘制canvas还是可以的）。在禁用了JavaScript之后，canvas也绘图无能了。</p>
<p><strong>可操作的DOM和冷冰冰的像素</strong></p>
<p>svg基于xml有DOM接口，每个图元都会对应DOM对象（请联想HTML元素的DOM对象），这些对象都是可以单独操作的，而且还能绑定<strong>onclick</strong>这类事件。甚至可以使用Firebug、Chrome Inspector等查看DOM结构。不过这在性能方面就会大打折扣。</p>
<p>而canvas就显得比较苍白。注意区别下面的canvas和context。</p>
<pre><code> var canvas,context;
 canvas=document.createElementByTagName("canvas");
 context=canvas.getContext("2d"); </code></pre>
<p>也不能说canvas不能绑定事件，但是要把canvas这个HTML的DOM对象和getContext返回的这个2D context区分开来。canvas是本身是一个DOM对象，确实可以绑定onclick等等事件，做通常DOM可以做的一些操作。而context只是一个像素画布的接口（<a href="http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html#2dcontext">接口文档</a>），fillRect的返回值并不指向某个可绑定事件的DOM实体。要实现画布中物体的事件，只能通过canvas的DOM接口，用坐标来判断（之前在某个分享会上，有人实现过）。</p>
<p>是福是祸，canvas因此而具有更高的绘图性能。关于性能可参考<a href="http://smus.com/canvas-vs-svg-performance/">这篇文章</a>。</p>
<p><strong>可访问性和替代文本</strong></p>
<p>其实这个问题没这么严肃，在不支持svg的浏览器中，svg图形中的文本会以普通文本的形式显示出来，而canvas在不支持JavaScript的页面上就是一片空白。</p>
<p><strong>浏览器的支持</strong></p>
<p>先看<a href="http://caniuse.com/#search=canvas">canvas</a>，恩，IE！</p>
<p><img title="canvas" src="http://pic.yupoo.com/island205/C4bZwd7K/CDXkl.png" alt="" width="640" height="auto"></p>
<p><a href="http://caniuse.com/#search=svg">svg</a>，恩，还是IE（vml在IE5.5+上有支持，Google Map以此实现其在IE5.5上的矢量绘图）！</p>
<p><img title="svg" src="http://pic.yupoo.com/island205/C4bZvZjF/juSf5.png" alt="" width="640" height="auto"></p>
<p>IE出尽了风头，在IE上就只能使用vml或Flash（或silverlight！？）等各中插件吧！提供<a href="http://www.w3.org/Graphics/SVG/IG/wiki/SVG_Plugin_for_IE">svg的插件</a>。</p>
<p><strong>选谁？？？</strong></p>
<p>选谁，百家争鸣。</p>
<p>svg适合灵活的<strong>交互性强</strong>的图形，而canvas适合简单的图形或者有超多图形需要绘制（性能，比方说游戏）的程序。两者的取舍在于交互性的复杂度和性能上，总之简单开发就好。至于在浏览器方面，IE对svg和canvas的支持都很贫乏，可以使用vml、flash等等来适配，但适配是必不可少的。</p>
<p><strong>参考资料&amp;扩展阅读</strong></p>
<ol>
<li><a href="http://www.sitepoint.com/canvas-vs-svg-how-to-choose/">http://www.sitepoint.com/canvas-vs-svg-how-to-choose/</a></li>
<li><a href="http://www.w3schools.com/html5/html5_canvas_vs_svg.asp">http://www.w3schools.com/html5/html5_canvas_vs_svg.asp</a></li>
<li><a href="http://smus.com/canvas-vs-svg-performance/">http://smus.com/canvas-vs-svg-performance/</a></li>
<li><a href="http://msdn.microsoft.com/en-us/library/ie/gg193983(v=vs.85).aspx">http://msdn.microsoft.com/en-us/library/ie/gg193983(v=vs.85).aspx</a></li>
<li><a href="http://dev.opera.com/articles/view/svg-or-canvas-choosing-between-the-two/">http://dev.opera.com/articles/view/svg-or-canvas-choosing-between-the-two/</a></li>
</ol>
<p> </p>