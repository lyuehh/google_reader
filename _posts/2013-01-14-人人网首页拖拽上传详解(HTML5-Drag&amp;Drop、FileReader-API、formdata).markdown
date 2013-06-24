---
layout: post
title:  "人人网首页拖拽上传详解(HTML5 Drag&amp;Drop、FileReader API、formdata)"
date:   2013-01-14 09:30:46
author: 
categories: program
---

## 人人网首页拖拽上传详解(HTML5 Drag&amp;Drop、FileReader API、formdata)
### by 
### at 2013-01-14 09:30:46
### original <http://item.feedsky.com/~feedsky/bingo929/~7170939/664471536/5279850/1/item.html>

<p><a href="http://blog.bingo929.com/renren-drag-drop-photo-filereader-formdata.html"><img title="人人网拖拽上传实例详解" src="http://blog.bingo929.com/wp-content/uploads/2011/12/blog-title.jpg" alt="" width="500" height="150"></a></p>
<p>　　早在公元2011年6月3日傍晚，人人网推出了一个很装B且完全无视IE浏览器的功能——拖拽上床。哦，Sorry, 是<strong>拖拽上传</strong>。到现在，这个功能已经上线了整整半年，这篇文章也足足拖延了半年才分享给大家，实在是对不住了，呵呵，今后彬Go一定要勤奋发Blog！</p>
<p><span></span></p>
<p><strong><span style="color:#ff6600">您还可以参考以下HTML5相关文章:</span></strong><br>
《<a href="http://blog.bingo929.com/google-enjoy-html5-drag-drop-filereaderenren.html">HTML5 Drag&amp;Drop 拖拽、FileReader实例教程</a>》<br>
《<a href="http://blog.bingo929.com/html5-websockets.html">HTML5 WebSockets 基础使用教程</a>》<br>
《<a href="http://blog.bingo929.com/html-5-canvas-the-basics-html5.html">关于HTML 5 canvas 的基础教程</a>》<br>
《<a href="http://blog.bingo929.com/html5-ie-enabling-script.html">让所有IE支持HTML5的解决方案</a>》<br>
《<a href="http://blog.bingo929.com/power-of-html5-css3-div-css.html">一起感受HTML5和CSS3的能量</a>》</p>
<p>　　关于这个拖拽上传，其实国外有很多网站已经有这样的应用，最早推出拖拽上传应用的是Gmail，它支持标准浏览器下拖拽本地文件到浏览器中作为邮件的附件发送。人人网的这个拖拽上传也是同理，可以让使用标准浏览器的用户通过简单的拖拽行为，将本地文件夹中的照片直接上传到人人网，用户体验能得到提升的同时，也希望借此机会推广一下标准浏览器，淘汰IE。人人网当时也向广大用户推出升级浏览器活动，并喊出口号：”工欲善其计算机，必先利其浏览器”。本次拖拽上传的宣传口号：你敢”脱”，我就敢上传…</p>
<p><a href="http://blog.bingo929.com/wp-content/uploads/2011/12/b_large_pbOj_3004000003b45c41.jpg"><img title="人人网 - 拖拽上传" src="http://blog.bingo929.com/wp-content/uploads/2011/12/b_large_pbOj_3004000003b45c41-500x259.jpg" alt="人人网 - 拖拽上传" width="500" height="259"></a></p>
<p>　　言归正题，首先看看效果，大家如果有<a href="http://www.renren.com">人人网</a>帐号的话可以在首页试一试拖拽上传功能，下面是演示视频：</p>
<p><iframe src="http://reader.googleusercontent.com/reader/embediframe?src=http://player.youku.com/player.php/sid/XMjcyNzU0ODY4/v.swf&amp;width=500&amp;height=430" width="500" height="430"></iframe></p>
<p><strong>拖拽上传应用主要使用了以下<a href="http://blog.bingo929.com/tag/html5">HTML5</a>技术：</strong></p>
<ul>
<li><a href="http://dev.w3.org/html5/spec/Overview.html#dnd"><strong>Drag&amp;Drop</strong></a> : <a href="http://blog.bingo929.com/tag/html5">HTML5</a>基于拖拽的事件机制.</li>
<li><a href="http://www.w3.org/TR/FileAPI/"><strong>File API</strong></a> :  可以很方便的让Web应用访问文件对象，File API包括<a style="font-weight:normal" href="http://dev.w3.org/2006/webapi/FileAPI/#dfn-filelist">FileList</a>、<a style="font-weight:normal" href="http://dev.w3.org/2006/webapi/FileAPI/#dfn-Blob">Blob</a>、<a style="font-weight:normal" href="http://dev.w3.org/2006/webapi/FileAPI/#dfn-file">File</a>、<a style="font-weight:normal" href="http://dev.w3.org/2006/webapi/FileAPI/#dfn-filereader">FileReader</a>、<a style="font-weight:normal" href="http://dev.w3.org/2006/webapi/FileAPI/#url">URI scheme</a>，本文主要讲解拖拽上传中用到的FileList和FileReader接口。</li>
<li><a href="http://www.w3.org/TR/XMLHttpRequest2/#the-formdata-interface"><strong>FormData</strong></a> : FormData是基于<a style="font-weight:normal" href="http://www.w3.org/TR/XMLHttpRequest2/">XMLHttpRequest Level 2</a>的新接口，可以方便web应用模拟Form表单数据，最重要的是它支持文件的二进制流数据，这样我们就能够通过它来实现AJAX向后端发送文件数据了。</li>
</ul>
<h3><strong>HTML5 Drag&amp;Drop 拖拽事件</strong></h3>
<p>　　关于Drag&amp;Drop拖拽事件，之前我写过一篇专门介绍的文章<a style="font-weight:bold" href="http://blog.bingo929.com/google-enjoy-html5-drag-drop-filereaderenren.html">《给力的 Google HTML5 训练营(HTML5 Drag&amp;Drop 拖拽、FileReader实例教程)》</a>，那篇文章详细讲解了Drag &amp; Drap事件的原理和代码实例，这里的拖拽上传实现原理基本上是一样的，大家有兴趣或不太了解的话可以先看看那篇文章，我在这里就不再过多啰嗦了~下面直接出拖拽上传简要代码实例：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> oDragWrap <span style="color:#339933">=</span> document.<span style="color:#660066">body</span><span style="color:#339933">;</span><br>
<br>
<span style="color:#006600;font-style:italic">//拖进</span><br>
oDragWrap.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'dragenter'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　e.<span style="color:#660066">preventDefault</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
<span style="color:#006600;font-style:italic">//拖离</span><br>
oDragWrap.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'dragleave'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　dragleaveHandler<span style="color:#009900">(</span>e<span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
<span style="color:#006600;font-style:italic">//拖来拖去 , 一定要注意dragover事件一定要清除默认事件</span><br>
<span style="color:#006600;font-style:italic">//不然会无法触发后面的drop事件</span><br>
oDragWrap.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'dragover'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　e.<span style="color:#660066">preventDefault</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
<span style="color:#006600;font-style:italic">//扔</span><br>
oDragWrap.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'drop'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　dropHandler<span style="color:#009900">(</span>e<span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
<span style="color:#003366;font-weight:bold">var</span> dropHandler <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
<span style="color:#006600;font-style:italic">//将本地图片拖拽到页面中后要进行的处理都在这</span><br>
<span style="color:#009900">}</span></div></div>
<h3>获取文件数据 HTML5 File API</h3>
<p>　　在之前<a href="http://blog.bingo929.com/google-enjoy-html5-drag-drop-filereaderenren.html">那篇文章</a>中我也有介绍过关于File API中的FileReader接口，作为 <a href="http://www.w3.org/TR/FileAPI/">File API</a> 的一部分，<a href="http://www.w3.org/TR/FileAPI/#dfn-filereader">FileReader</a> 专门用于读取文件，根据 W3C 的定义，FileReader 接口 “提供一些读取文件的方法与一个包含读取结果的事件模型”。关于FileReader的详细介绍和代码实例大家可以先去看看那篇<a href="http://blog.bingo929.com/google-enjoy-html5-drag-drop-filereaderenren.html">文章</a>。</p>
<p>　　今天我着重介绍一下File API中的FileList接口,它主要通过两个途径获取本地文件列表，一是<strong>&lt;input type=”file”&gt;</strong>的表单形式，另一种则是<strong>e.dataTransfer.files</strong>拖拽事件传递的文件信息。很显然，我们这里会用到后者。</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> fileList <span style="color:#339933">=</span> e.<span style="color:#660066">dataTransfer</span>.<span style="color:#660066">files</span><span style="color:#339933">;</span></div></div>
<p>　　使用<strong>files方法</strong>将会获取到拖拽文件的数组形势的数据，每个文件占用一个数组的索引，如果该索引不存在文件数据，将返回null值。可以通过<strong>length</strong>属性获取文件数量.</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> fileNum <span style="color:#339933">=</span> fileList.<span style="color:#660066">length</span><span style="color:#339933">;</span></div></div>
<p>　　拖拽上传需要注意的是需要判断两个条件，1:拖拽的是文件不是页面中的元素; 2:拖拽的是图片而不是其它文件,可以通过file.type属性获取文件的类型</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#006600;font-style:italic">//检测是否是拖拽文件到页面的操作</span><br>
<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>fileList.<span style="color:#660066">length</span> <span style="color:#339933">===</span> <span style="color:#cc0000">0</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066;font-weight:bold">return</span><span style="color:#339933">;</span><span style="color:#009900">}</span><span style="color:#339933">;</span><br>
<span style="color:#006600;font-style:italic">//检测文件是不是图片</span><br>
<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span>.<span style="color:#660066">type</span>.<span style="color:#660066">indexOf</span><span style="color:#009900">(</span><span style="color:#3366cc">'image'</span><span style="color:#009900">)</span> <span style="color:#339933">===</span> <span style="color:#339933">-</span><span style="color:#cc0000">1</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066;font-weight:bold">return</span><span style="color:#339933">;</span><span style="color:#009900">}</span></div></div>
<p>　　下面让我们来看看如何结合之前的拖拽事件来实现拖拽图片并在页面中进行预览：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> dropHandler <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　e.<span style="color:#660066">preventDefault</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
　<span style="color:#006600;font-style:italic">//获取文件列表</span><br>
　<span style="color:#003366;font-weight:bold">var</span> fileList <span style="color:#339933">=</span> e.<span style="color:#660066">dataTransfer</span>.<span style="color:#660066">files</span><span style="color:#339933">;</span><br>
<br>
　<span style="color:#006600;font-style:italic">//检测是否是拖拽文件到页面的操作</span><br>
　<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>fileList.<span style="color:#660066">length</span> <span style="color:#339933">==</span> <span style="color:#cc0000">0</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066;font-weight:bold">return</span><span style="color:#339933">;</span><span style="color:#009900">}</span><span style="color:#339933">;</span><br>
<br>
　<span style="color:#006600;font-style:italic">//检测文件是不是图片</span><br>
　<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span>.<span style="color:#660066">type</span>.<span style="color:#660066">indexOf</span><span style="color:#009900">(</span><span style="color:#3366cc">'image'</span><span style="color:#009900">)</span> <span style="color:#339933">===</span> <span style="color:#339933">-</span><span style="color:#cc0000">1</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066;font-weight:bold">return</span><span style="color:#339933">;</span><span style="color:#009900">}</span><br>
<br>
　<span style="color:#006600;font-style:italic">//实例化file reader对象</span><br>
　<span style="color:#003366;font-weight:bold">var</span> reader <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">new</span> FileReader<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
　<span style="color:#003366;font-weight:bold">var</span> img <span style="color:#339933">=</span> document.<span style="color:#660066">createElement</span><span style="color:#009900">(</span><span style="color:#3366cc">'img'</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
　reader.<span style="color:#000066">onload</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　img.<span style="color:#660066">src</span> <span style="color:#339933">=</span> <span style="color:#000066;font-weight:bold">this</span>.<span style="color:#660066">result</span><span style="color:#339933">;</span><br>
　　oDragWrap.<span style="color:#660066">appendChild</span><span style="color:#009900">(</span>img<span style="color:#009900">)</span><span style="color:#339933">;</span><br>
　<span style="color:#009900">}</span><br>
　reader.<span style="color:#660066">readAsDataURL</span><span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
<span style="color:#009900">}</span></div></div>
<p>　　这里有一个简单的<a href="http://www.bingo929.com/mytest/dragtest.html">拖拽图片预览的Demo</a></p>
<p>　　这时你如果用FireBug等类似调试工具查看DOM的话，会看到&lt;img&gt;标签的src属性是一个超长的文件二进制数据，所以如果DOM有很多这类图片，那就要当心浏览器性能了，因为这些数据极大地扩充的页面的代码量，而每次页面的reflow都会对浏览器形成很大的负担，So,如果这些图片还在DOM中，那就尽量不要做动画或任何重绘操作，如果真的要做就尽量让图片脱离文档流，让其绝对定位比较靠谱。</p>
<p style="background:#fff7d1;padding:10px;color:#d6006d">
补充：可以使用<strong><a href="https://developer.mozilla.org/en/DOM/window.URL.createObjectURL">window.URL.createObjectURL(file)</a></strong>来获取文件的URL(Chrome下用<strong>window.webkitURL.createObjectURL(file)</strong>)，这种方式获取的URL要比上面说的<strong>readAsDataURL</strong>简短很多。而且可以省去使用FileReader。下面是使用readAsDataURL与createObjectURL生成的&lt;img&gt;代码对比：
</p>
<p><a href="http://blog.bingo929.com/wp-content/uploads/2011/12/readasdataurl-vs-createobjecturl.png"><img src="http://blog.bingo929.com/wp-content/uploads/2011/12/readasdataurl-vs-createobjecturl-500x241.png" alt="readasdataurl-vs-createobjecturl" title="readAsDataURL与createObjectUrl对比" width="500" height="241"></a></p>
<p>优化后的代码：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> dropHandler <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　e.<span style="color:#660066">preventDefault</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
　<span style="color:#003366;font-weight:bold">var</span> fileList <span style="color:#339933">=</span> e.<span style="color:#660066">dataTransfer</span>.<span style="color:#660066">files</span><span style="color:#339933">;</span>　　<span style="color:#006600;font-style:italic">//获取文件列表</span><br>
　<span style="color:#003366;font-weight:bold">var</span> img <span style="color:#339933">=</span> document.<span style="color:#660066">createElement</span><span style="color:#009900">(</span><span style="color:#3366cc">'img'</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
　<span style="color:#006600;font-style:italic">//检测是否是拖拽文件到页面的操作</span><br>
　<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>fileList.<span style="color:#660066">length</span> <span style="color:#339933">==</span> <span style="color:#cc0000">0</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066;font-weight:bold">return</span><span style="color:#339933">;</span><span style="color:#009900">}</span><span style="color:#339933">;</span><br>
<br>
　<span style="color:#006600;font-style:italic">//检测文件是不是图片</span><br>
　<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span>.<span style="color:#660066">type</span>.<span style="color:#660066">indexOf</span><span style="color:#009900">(</span><span style="color:#3366cc">'image'</span><span style="color:#009900">)</span> <span style="color:#339933">===</span> <span style="color:#339933">-</span><span style="color:#cc0000">1</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066;font-weight:bold">return</span><span style="color:#339933">;</span><span style="color:#009900">}</span><br>
　<br>
　<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>window.<span style="color:#660066">URL</span>.<span style="color:#660066">createObjectURL</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　<span style="color:#006600;font-style:italic">//FF4+</span><br>
　　img.<span style="color:#660066">src</span> <span style="color:#339933">=</span> window.<span style="color:#660066">URL</span>.<span style="color:#660066">createObjectURL</span><span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
　<span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>window.<span style="color:#660066">webkitURL</span>.<span style="color:#660066">createObjectURL</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　<span style="color:#006600;font-style:italic">//Chrome8+</span><br>
　　img.<span style="color:#660066">src</span> <span style="color:#339933">=</span> window.<span style="color:#660066">webkitURL</span>.<span style="color:#660066">createObjectURL</span><span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
　<span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#009900">{</span><br>
　　<span style="color:#006600;font-style:italic">//实例化file reader对象</span><br>
　　<span style="color:#003366;font-weight:bold">var</span> reader <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">new</span> FileReader<span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
　　reader.<span style="color:#000066">onload</span> <span style="color:#339933">=</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　　img.<span style="color:#660066">src</span> <span style="color:#339933">=</span> <span style="color:#000066;font-weight:bold">this</span>.<span style="color:#660066">result</span><span style="color:#339933">;</span><br>
　　　oDragWrap.<span style="color:#660066">appendChild</span><span style="color:#009900">(</span>img<span style="color:#009900">)</span><span style="color:#339933">;</span><br>
　　<span style="color:#009900">}</span><br>
　　reader.<span style="color:#660066">readAsDataURL</span><span style="color:#009900">(</span>fileList<span style="color:#009900">[</span><span style="color:#cc0000">0</span><span style="color:#009900">]</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
　<span style="color:#009900">}</span><br>
<span style="color:#009900">}</span></div></div>
<p>　　需要注意的是，<strong>window.URL.createObjectURL</strong>是有生命周期的，也就意味着你每用此方法获取URL，其生命周期都会和DOM一样，它会单独占用内存，所以当删除图片或不再需要它是，记得用<strong>window.URL.revokeObjectURL(file)</strong>来释放其内存。当然，如果你没有释放，刷新页面也是可以释放的。</p>
<h3>AJAX上传图片(file.getAsBinary &amp; FormData)</h3>
<p>　　既然已经获取到了拖拽到web页面中图片的数据，下一步就是将其发送到服务器端了。</p>
<p>　　话说HTML5时代之前，AJAX传输文件二进制流数据是不可能完成的事情，而现在我们完全可以通过<strong>file.getAsBinary</strong>获取文件的二进制数据流，进而将其当做XHR的data数据传送到后端，8过由于Chrome不支持file的getAsBinary方法，FF3.6+支持此方法。所以Chrome就要另寻它法了，这时我们发现<a href="http://www.w3.org/TR/XMLHttpRequest2/">XMLHttpRequest Level 2</a>中的FormData接口完美解决了这个问题，它可以很快捷的模拟Form表单数据并通过AJAX发送至后端，FormData的支持情况是FF5及以上支持，Chrome12及以上支持。</p>
<p>　　<strong> file.getAsBinary</strong>获取文件流很简单，但是要想上传</p>