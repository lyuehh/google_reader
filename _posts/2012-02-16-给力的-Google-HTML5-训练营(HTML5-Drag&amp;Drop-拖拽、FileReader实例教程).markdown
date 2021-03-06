---
layout: post
title:  "给力的 Google HTML5 训练营(HTML5 Drag&amp;Drop 拖拽、FileReader实例教程)"
date:   2012-02-16 12:31:56
author: 
categories: program
---

## 给力的 Google HTML5 训练营(HTML5 Drag&amp;Drop 拖拽、FileReader实例教程)
### by 
### at 2012-02-16 12:31:56
### original <http://item.feedsky.com/~feedsky/bingo929/~7170939/597761171/5279850/1/item.html>

<p><a href="http://blog.bingo929.com/google-enjoy-html5-drag-drop-filereaderenren.html"><img src="http://blog.bingo929.com/wp-content/uploads/2011/01/HTML5-drag-drop-filereader.jpg" alt="HTML5 Drag&amp;Drop 、FileReader实例教程" title="HTML5-drag-drop-filereader" width="500" height="150"></a></p>
<p>　　上周六有幸同我们人人网的几个同事一起去Google总部参加了<strong>《给力HTML5 —— 2011 Google HTML5 训练营》</strong>，从下午1点多到晚上9点多8个小时左右的时间，收获还是很大的，8个小时的时间把以前一直想学或想做的事情都给搞了一些。好久没有写博客了，今天也该分享些给力的东西了，正好借此次机会跟大家分享些HTML5相关的给力的技术（sessionStorage、localStorage、Web SQL DateBase、Indexed DB、Canvas绘图、FileReader、Drag&amp;Drop…），何止是给力，简直就是给力！<span></span></p>
<p><strong><span style="color:#ff6600">您还可以参考以下HTML5相关文章:</span></strong><br>
《<a href="http://blog.bingo929.com/html5-websockets.html">HTML5 WebSockets 基础使用教程</a>》<br>
《<a href="http://blog.bingo929.com/html-5-canvas-the-basics-html5.html">关于HTML 5 canvas 的基础教程</a>》<br>
《<a href="http://blog.bingo929.com/html5-ie-enabling-script.html">让所有IE支持HTML5的解决方案</a>》<br>
《<a href="http://blog.bingo929.com/power-of-html5-css3-div-css.html">一起感受HTML5和CSS3的能量</a>》</p>
<h3>Google HTML5 训练营</h3>
<p style="color:#666;font-size:12px">先简单介绍一下这个训练营的活动流程：<br>
　　　1:00 – 1:30 注册<br>
　　　1:30 – 2:30 HTML5 技术剖析<br>
　　　技术讲座1 —— Google 工程师<br>
　　　技术讲座2 —— 特邀嘉宾（李靖威、李继成，就职于人人网）<br>
　　　2:30 – 3:00 宣布竞赛规则，开始组队<br>
　　　3:00 – 6:30 作品创作 / 提交<br>
　　　6:30 – 7:00 作品展示<br>
　　　7:00 – 7:30 评奖、颁奖</p>
<p>　　讲座部分，先是Google工程师寒蕊MM跟我们分享了些HTML5的新技术概况，其中大致包括：<a href="http://dev.w3.org/html5/webstorage/#dom-sessionstorage">sessionStorage</a>、<a href="http://dev.w3.org/html5/webstorage/#dom-localstorage">localStorage</a>、<a href="http://dev.w3.org/html5/webdatabase/#web-sql">Web SQL DateBase</a> 、<a href="http://www.chromium.org/developers/design-documents/indexeddb">Indexed DB</a>，寒蕊MM还专门写了篇关于IndexedDB的文章<a href="http://enjoyhtml5.com/blogs/storage.html">《客户端数据存储》</a>，文章用实例的方式讲了Web Storage(localStorage)、Web SQL Database 和 Indexed Database的用法。</p>
<p>　　其中Web SQL Database虽是HTML5的技术，但由于某些原因，W3C组织(Web Applications Working Group)已不再维护这项技术，也就是说虽然目前Chrome、FireFox新版本浏览器支持这项技术，但不一定未来的版本还会支持。以下为w3c官网的声明：
<p style="background:black;color:white;font:900 2em serif;padding:0.5em 1em;border:dotted yellow 0.5em;text-align:center">Beware. This specification is no longer in active maintenance and the Web<br>
Applications Working Group does not intend to maintain it further.</p>
<p>　　Indexed DataBase最初是由Oracle提出的数据库API，没记错的话起初叫做Simple DataBase，之后演变成Indexed DataBase，而且已被Chrome和FireFox(版本4)所支持。</p>
<p>　　这次活动规模比较小，由于人数限制在了30人，所以组队Coding阶段，每队5人，分成了6组~接下来就是短短3个小时左右的Coding了，我们人人网的四个人和另一位有Canvas经验的邱亮同学一起开发一个纯HTML5的可涂鸦日记本，其中有个插曲:一开始项目名字没多想就直接叫做《我的日记本》了，到最后提交项目的时候，Google MM说要起一个简洁且酷一些的名字，好吧，那我们就起一个简洁酷一些的名字，最后就叫《我日》！哇咔咔~让大家”贱笑”了~</p>
<p><a href="http://blog.bingo929.com/wp-content/uploads/2011/01/html5-daily-book.jpg"><img src="http://blog.bingo929.com/wp-content/uploads/2011/01/html5-daily-book-500x406.jpg" alt="html5-daily-book" title="html5-daily-book" width="500" height="406"></a></p>
<p>　　言归正传，《我日》项目之所以叫纯HTML5，其实只是使用了些HTML5技术，在短短的3个小时时间，从功能到分工开发，时间很紧~所以只是做了个小小的Demo版，基本功能实现的差不多了，但愿以后还能有时间把它完善一下~其中使用了这些HTML5技术：</p>
<p>　　1.Web SQL DataBase ：存取日记数据<br>
　　2.HTML5 Drag&amp;Drop 事件 ：监听网页内图片拖拽及本地图片直接拖拽到日记中的事件<br>
　　3.FileReader ：获取本地图片拖拽到日记本里的图片数据<br>
　　4.<a href="http://blog.bingo929.com/html-5-canvas-the-basics-html5.html">Canvas</a> ：实现日记涂鸦功能(画笔、调色、文字、图片绘制到画布等…)<br>
　　5.HTML5新标签 ：如:&lt;input type=”range”… /&gt; 、 &lt;section&gt; 、&lt;article&gt;等…<br>
　　6.<a href="http://blog.bingo929.com/tag/css3">CSS3</a></p>
<p>　　今天我只详细讲讲我的分工部分：HTML5 Drag &amp; Drop 和 FireReader这两个东西，以后有机会在和大家分享其他技术。</p>
<h3><a href="http://dev.w3.org/html5/spec/dnd.html">HTML5 Drag &amp; Drop 事件</a></h3>
<p>　　过去我们想实现网页中的拖拽效果，基本上都是使用DOM事件模型中的mousedown、mousemove、mouseup的事件监听来模拟拖拽效果，为了实现实时的拖拽移动效果，还要不停地获取鼠标的坐标，还要不停的修改元素的位置，代码要堆很多，而且性能上也很不好(不停地修改元素位置会导致页面reflow，除非绝对定位)，现在有了html5原生的Drag &amp; Drop 拖拽事件，真的是方便了许多，用”事半功倍”来形容绝不为过。</p>
<p><strong>Drag &amp; Drop 包括以下事件</strong>：
<ul>
<li><strong>dragstart</strong>：要被拖拽的元素开始拖拽时触发，这个事件对象是被拖拽元素</li>
<li><strong>dragenter</strong>：拖拽元素进入目标元素时触发，这个事件对象是目标元素</li>
<li><strong>dragover</strong>：拖拽某元素在目标元素上移动时触发，这个事件对象是目标元素</li>
<li><strong>dragleave</strong>：拖拽某元素离开目标元素时触发，这个事件对象是目标元素</li>
<li><strong>dragend</strong>：在drop之后触发，就是拖拽完毕时触发，这个事件对象是被拖拽元素</li>
<li><strong>drop</strong>：将被拖拽元素放在目标元素内时触发，这个事件对象是目标元素</li>
</ul>
<p>完成一次成功页面内元素拖拽的行为事件过程应该是： dragstart –&gt; dragenter –&gt; dragover –&gt; drop –&gt; dragend</p>
<h4 style="font-size:16px">Drag &amp; Drop 网页内的元素拖拽：</h4>
<p>　　HTML5为元素新增了用于拖拽的属性<strong>draggable</strong>，这个属性决定了元素是否能被拖拽，如果draggable=”true”，则元素可被拖拽，否则只能选择元素的文本。</p>
<p>　　值得一提的是HTML5支持拖拽数据存储，使用<strong>dataTransfer</strong>接口，作用于元素的拖拽基础上，dataTransfer对象包含以下属性和方法：
<ul>
<li><strong>dataTransfer.dropEffect [ = value ]</strong>：返回已选择的拖放效果，如果该操作效果与起初设置的effectAllowed效果不符，则拖拽操作失败。可以设置修改，包含这几个值：<i>“none”, “copy”, “link” 和 “move”</i>。</li>
<li><strong>dataTransfer.effectAllowed [ = value ]</strong>：返回允许执行的拖拽操作效果，可以设置修改，包含这些值：<i>“none”, “copy”, “copyLink”, “copyMove”, “link”, “linkMove”, “move”, “all” 和 “uninitialized”</i></li>
<li><strong>dataTransfer.types</strong>：返回在dragstart事件出发时为元素存储数据的格式，如果是外部文件的拖拽，则返回”files”</li>
<li><strong>dataTransfer.clearData ( [ format ] )</strong>：删除指定格式的数据，如果未指定格式，则删除当前元素的所有携带数据</li>
<li><strong>dataTransfer.setData(format, data)</strong>：为元素添加指定数据</li>
<li><strong>dataTransfer.getData(format)</strong>：返回指定数据，如果数据不存在，则返回空字符串</li>
<li><strong>dataTransfer.files</strong>：如果是拖拽文件，则返回正在拖拽的文件列表FileList</li>
<li><strong>dataTransfer.setDragImage(element, x, y)</strong>：制定拖拽元素时跟随鼠标移动的图片，x、y分别是相对于鼠标的坐标(据测试，Chrome暂不支持)</li>
<li><strong>dataTransfer.addElement(element)</strong>：添加一起跟随拖拽的元素，如果你想让某个元素跟随被拖拽元素一同被拖拽，则使用此方法(据测试，Chrome暂不支持)</li>
</ul>
<p>在dragstart事件触发时可以为被拖拽元素存储数据，就是使用上面说到的<strong>dataTransfer.setData</strong>，setData的数据格式一般有两种：”text/plain”(用于文本数据)和”text/uri-list”(用于url)，你可以先为某个可拖拽元素设置微数据，然后为它设置draggable属性为true，之后在其dragstart事件触发时存储数据：<br>
html部分：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;dragMe&quot;</span> builddate<span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;2011-1-10&quot;</span> draggable<span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;true&quot;</span>&gt;</span>拖拽我！<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span><br>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;dropHere&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span></div></div>
<p>javascript部分：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> oDragMe <span style="color:#339933">=</span> document.<span style="color:#660066">getElementById</span><span style="color:#009900">(</span><span style="color:#3366cc">'dragMe'</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<br>
oDragMe.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'dragstart'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　e.<span style="color:#660066">dataTransfer</span>.<span style="color:#660066">setData</span><span style="color:#009900">(</span><span style="color:#3366cc">'text/plain'</span><span style="color:#339933">,</span> e.<span style="color:#660066">target</span>.<span style="color:#660066">getAttribute</span><span style="color:#009900">(</span><span style="color:#3366cc">'builddate'</span><span style="color:#009900">)</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<span style="color:#009900">}</span><span style="color:#339933">,</span><span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span></div></div>
<p>在拖拽结束的时候便可以获取相应元素的数据:</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap"><span style="color:#003366;font-weight:bold">var</span> oDropBox <span style="color:#339933">=</span> document.<span style="color:#660066">getElementById</span><span style="color:#009900">(</span><span style="color:#3366cc">'dropHere'</span><span style="color:#009900">)</span><span style="color:#339933">,</span><br>
　　tmpData<span style="color:#339933">;</span><br>
<br>
oDropBox .<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'drop'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　tmpData <span style="color:#339933">=</span> e.<span style="color:#660066">dataTransfer</span>.<span style="color:#660066">getData</span><span style="color:#009900">(</span><span style="color:#3366cc">'text/plain'</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
<span style="color:#009900">}</span><span style="color:#339933">,</span><span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span></div></div>
<p>创建拖拽事件监听的时候记得要把默认的行为事件去掉，毕竟浏览器是有默认拖拽行为的,尤其是dragover事件一定要使用e.preventDefault()，不然drop事件可能不会被触发：</p>
<div style="overflow:auto;white-space:nowrap;border:1px solid #9f9f9f;width:435px"><div style="padding:5px;font:normal 12px/1.4em Monaco,Lucida Console,monospace;white-space:nowrap">oDropBox .<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'dragover'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span>e<span style="color:#009900">)</span> <span style="color:#009900">{</span><br>
　　e.<span style="color:#660066">stopPropagation</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#339933">;</span><br>
     e.<span style="color:#660066">preventDefault</span><span style="color:#009900">(</span>&lt;span style=</div></div></p></p></p>