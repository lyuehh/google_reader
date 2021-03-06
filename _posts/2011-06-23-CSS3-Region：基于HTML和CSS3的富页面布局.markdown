---
layout: post
title:  "CSS3 Region：基于HTML和CSS3的富页面布局"
date:   2011-06-23 20:51:17
author: 神飞
categories: program
---

## CSS3 Region：基于HTML和CSS3的富页面布局
### by 神飞
### at 2011-06-23 20:51:17
### original <http://www.qianduan.net/css-regions.html>

<div>译自：<a href="http://www.adobe.com/devnet/html5/articles/css3-regions.html">CSS3 regions: Rich page layout with HTML and CSS3</a><br>
	中文：<a href="http://www.qianduan.net/css-regions.html">CSS3 Region：基于HTML和CSS3的富页面布局</a><br>
	请尊重版权，转载请注明来源，多谢！
<hr>
</div>
<p>互联网已经成为一个提供参考、教材、新闻、文章和交互应用的大宝库了。然而，当为印刷设计内容时，一些功能显然仍然不可能或者很难使用Web标准来实现。</p>
<p><span></span></p>
<p>印刷出版物正在探索更好的方法来转换或者改变他们的内容以适应富数字格式。我们也看到这是一个让网页更富于表现和支持更复杂布局的好机会。</p>
<p>Adobe通过增强CSS进行了一些实验来展示一些传统杂志使用的复杂布局。我们也提交了一些建议到W3C CSS工作组，并构建了一个原型来通过webkit实现这些提案。你可以从<a href="http://www.adobe.com/go/cssregions/">Adobe Labs</a>下载这些原型体验下。 然后你也可以在<a href="http://dev.w3.org/csswg/css3-regions/">CSS Regions Module</a>和这个W3C网站上的<a href="http://dev.w3.org/csswg/css3-exclusions/">CSS Exclusions Module</a> 页面找到W3C编写的草案。你也可以订阅<a href="http://lists.w3.org/Archives/Public/www-style/">W3C CSS 邮件列表</a>来讨论这个问题。欢迎在邮件标题中加入”[css3-regions]“或者”[css3-exclusions]“来在这个邮件组中讨论这个问题。当然也可以通过Adobe Labs的<a href="http://adobe.com/go/cssregions">CSS Regions 论坛</a>反馈问题。</p>
<p><strong>一些说明:</strong> 这是个在进行中的工作。随着我们和广大社区的讨论，我们将会做些改变。本文中用到的语法反映出当前W3C草案的现状，但是随着W3C工作组和社区的探讨可能会有些进化。同时也请注意，按照惯例，所有的新的数学将会首先采用-webkit-前缀。不过，为了简化，我在本文中省去了这个前缀。</p>
<p>现在，让我们了解这个扩展吧，他们可以被分为4类:</p>
<ul>
<li><strong>线性内容(Threading content)</strong>: 从一个区域“流向”另一个区域的内容。</li>
<li><strong>任意形状的容器(Arbitrarily shaped containers)</strong>: 在非矩形区域内显示文本。</li>
<li><strong>任意形状环绕(Arbitrarily shaped exclusion)</strong>: 文本环绕非矩形区域。</li>
<li><strong>区域样式(Region styling):</strong>根据流向区域显示内容。</li>
</ul>
<p>下面是各种分类的例子：</p>
<h3>内容流(Content flow)</h3>
<p>在典型的HTML文档中，文字可以在多个区域显示，但是每个区域中的文字是不相关的(见图1)。如果你想要跨多个列显示文本，或者使用别的你需要的更复杂的区域来手动。在用户放大文字或者用户的字体比你设定的字体大时，这可能会无法乱掉。这个方法(css3 region)同样是的拥有在缩放窗口是自适应的流体布局成为可能，或者，当显示在平板上时，自适应竖屏(portrait)或者横屏(landscape)显示。</p>
<p><img title="文字跨3个不同宽的列显示" alt="文字跨3个不同宽的列显示" src="http://img.qianduan.net/uploads/2011/06/fig01.jpg"></p>
<p> <img title="文字跨3个不同宽的列显示" alt="文字跨3个不同宽的列显示" src="http://img.qianduan.net/uploads/2011/06/fig01b.jpg"> </p>
<p> 图1.文字跨3个不同宽的列显示</p>
<p>如果你想要单独地指定一托内容(比如文字和图片)，那些内容如何在一串区域内显示(flow)呢？这正是内容流(content flow)要做的。</p>
<p>要使用它，通过flow属性赋予内容的容器一个名字，这样做会将内容从普通的CSS布局流中去掉，然后你可以插入这个线程到1个或多个其他区域——使用<code>from()</code>作为<code>content</code>属性的值。</p>
<p>上面的三列布局的代码如下：</p>
<p><strong>CSS</strong></p>

<div><table><tr><td><pre>1
2
3
4
5
6
7
</pre></td><td><pre style="font-family:monospace"><span style="color:#cc00cc">#source</span> <span style="color:#00aa00">{</span>
	flow<span style="color:#00aa00">:</span> <span style="color:#ff0000">&quot;main-thread&quot;</span><span style="color:#00aa00">;</span>
<span style="color:#00aa00">}</span>
<span style="color:#6666ff">.region</span> <span style="color:#00aa00">{</span>
	<span style="color:#000000;font-weight:bold">content</span><span style="color:#00aa00">:</span> from<span style="color:#00aa00">(</span>main-thread<span style="color:#00aa00">)</span><span style="color:#00aa00">;</span>
	<span style="color:#000000;font-weight:bold">background</span><span style="color:#00aa00">:</span> <span style="color:#cc00cc">#C5DFF0</span><span style="color:#00aa00">;</span>
<span style="color:#00aa00">}</span></pre></td></tr></table></div>

<p><strong>HTML</strong></p>

<div><table><tr><td><pre>1
2
3
4
5
6
7
</pre></td><td><pre style="font-family:monospace"> <span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;source&quot;</span>&gt;</span>
	<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">p</span>&gt;</span>Lorem ipsum dolor [...]<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">p</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
 
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region1&quot;</span> <span style="color:#000066">class</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region2&quot;</span> <span style="color:#000066">class</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region3&quot;</span> <span style="color:#000066">class</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span></pre></td></tr></table></div>

<p>你可以在一个页面中联合多个命名的flow。你也可以使用content-order属性来控制文字流的顺序。如果没有指定，将会使用正常的文档顺序。</p>
<p>通过这个简单的构件，你可以实现更复杂的布局，包括多列文本、不同宽高的列，以及跨越多个列的区域。(见图2)</p>
<p> <img title="文字穿越堆叠的区域和列" alt="文字穿越堆叠的区域和列" src="http://img.qianduan.net/uploads/2011/06/fig02.jpg"></p>
<p> 图2. 文字穿越堆叠的区域和列</p>
<h3>形状环绕(Wrap shape)</h3>
<p>使用形状环绕，你可以控制文字经过区域的形状(见图3)。你也可以使用这个属性搭配内容流或者单独的创建更有趣的设计。</p>
<p> <img title="文字内容显示在自定义形状的内部" alt="文字内容显示在自定义形状的内部" src="http://img.qianduan.net/uploads/2011/06/fig03.jpg"></p>
<p> 图3.文字内容显示在自定义形状的内部</p>
<p>要使用这个特性，你需要使用<code>wrap-shape</code>属性来定义形状，并设定<code>wrap-shape-mode</code>属性为需要的值。通过制定content的值，文字将显示在形状内部。</p>
<p>上面的现实心形的代码如下：</p>
<p><strong>CSS</strong></p>

<div><table><tr><td><pre>1
2
3
4
5
6
7
8
9
10
11
</pre></td><td><pre style="font-family:monospace">.<span style="color:#993333">circle</span><span style="color:#00aa00">{</span>
<span style="color:#808080;font-style:italic">/* 定义元素形状为一个圆*/</span>
	wrap-shape<span style="color:#00aa00">:</span> polygon<span style="color:#00aa00">(</span><span style="color:#933">0px</span><span style="color:#00aa00">,</span> <span style="color:#933">150px</span> <span style="color:#808080;font-style:italic">/* ...更多点*/</span><span style="color:#00aa00">)</span><span style="color:#00aa00">;</span>
	wrap-shape-mode<span style="color:#00aa00">:</span> <span style="color:#000000;font-weight:bold">content</span><span style="color:#00aa00">;</span>   
<span style="color:#00aa00">}</span>
 
.heart<span style="color:#00aa00">{</span>
	<span style="color:#808080;font-style:italic">/* 定义元素形状为心形*/</span>
	wrap-shape<span style="color:#00aa00">:</span> polygon<span style="color:#00aa00">(</span><span style="color:#933">150px</span><span style="color:#00aa00">,</span> <span style="color:#933">32px</span> <span style="color:#808080;font-style:italic">/* ...更多的点 */</span><span style="color:#00aa00">)</span><span style="color:#00aa00">;</span>  
	wrap-shape-mode<span style="color:#00aa00">:</span> <span style="color:#000000;font-weight:bold">content</span><span style="color:#00aa00">;</span>   
<span style="color:#00aa00">}</span></pre></td></tr></table></div>

<p><strong>HTML</strong></p>

<div><table><tr><td><pre>1
2
</pre></td><td><pre style="font-family:monospace"><span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">class</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;circle&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">class</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;heart&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span></pre></td></tr></table></div>

<p>我们的基于WebKit原型支持使用一个简单的多边形指定一个形状，但是你可以想象其它的集合体也可以被用于设定形状，或者甚至使用一张图片的alpha值。。。</p>
<h3> 环绕(Exclusions)</h3>
<p>通过使用wrap-shap-mode属性的其它值，你可以创建不同的效果，包括指定的<code>wrap-shape</code> 属性可以理解为要完全避开的区域(看图4)。</p>
<p> <img title="图4. 文字环绕在自定义图形周围" alt="图4. 文字环绕在自定义图形周围" src="http://img.qianduan.net/uploads/2011/06/fig04.jpg"></p>
<p> 图4. 文字环绕在自定义图形周围</p>
<p><strong>CSS</strong></p>

<div><table><tr><td><pre>1
2
3
4
</pre></td><td><pre style="font-family:monospace">.exclusion<span style="color:#00aa00">{</span>
	<span style="color:#808080;font-style:italic">/* 文字环绕整个元素 */</span>
	wrap-shape-mode<span style="color:#00aa00">:</span> around<span style="color:#00aa00">;</span>
<span style="color:#00aa00">}</span></pre></td></tr></table></div>

<p><strong>HTML</strong></p>

<div><table><tr><td><pre>1
2
3
</pre></td><td><pre style="font-family:monospace"> <span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">class</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;exclusion circle&quot;</span>&gt;</span>
	<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">p</span>&gt;</span>Lorem ipsum dolor [...]<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">p</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span></pre></td></tr></table></div>

<h3>区域样式(Region Styling)</h3>
<p>在杂志中，指定内容环绕设计中的一个特定的区域是很常见的。我们称之为区域样式。例子展示了文字环绕在第一个区块(包括introduction标题的部分)被设定为深蓝色，而余下的文字则为灰色(看图 5)。</p>
<p> <img title="图5.文字样式依赖它流入的区域" alt="图5.文字样式依赖它流入的区域" src="http://img.qianduan.net/uploads/2011/06/fig05.jpg"></p>
<p> 图5.文字样式依赖它流入的区域</p>
<p><strong>CSS</strong></p>

<div><table><tr><td><pre>1
2
3
4
5
6
7
8
</pre></td><td><pre style="font-family:monospace">p <span style="color:#00aa00">{</span>
	<span style="color:#000000;font-weight:bold">color</span><span style="color:#00aa00">:</span> <span style="color:#993333">gray</span><span style="color:#00aa00">:</span>
<span style="color:#00aa00">}</span>
<span style="color:#a1a100">@region-style #region_1 {</span>
	p <span style="color:#00aa00">{</span>
		<span style="color:#000000;font-weight:bold">color</span><span style="color:#00aa00">:</span> <span style="color:#cc00cc">#0C3D5F</span><span style="color:#00aa00">;</span>
	<span style="color:#00aa00">}</span>
<span style="color:#00aa00">}</span></pre></td></tr></table></div>

<p><strong>HTML</strong></p>

<div><table><tr><td><pre>1
2
3
4
5
6
7
8
</pre></td><td><pre style="font-family:monospace"><span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;article&quot;</span>&gt;</span>
	<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">h1</span>&gt;</span>Introduction<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">h1</span>&gt;</span>
	<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">p</span>&gt;</span>This is an example [...]<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">p</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region_1&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region_2&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region_3&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span>
<span style="color:#009900">&lt;<span style="color:#000000;font-weight:bold">div</span> <span style="color:#000066">id</span><span style="color:#66cc66">=</span><span style="color:#ff0000">&quot;region_4&quot;</span>&gt;&lt;<span style="color:#66cc66">/</span><span style="color:#000000;font-weight:bold">div</span>&gt;</span></pre></td></tr></table></div>

<p>区域样式目前还没有在Adobe Labs的这个webkit原型里面实现。</p>
<h3> CSS3 区域和媒体查询</h3>
<p>那些基本的构建块可以组合起来创建更有趣更复杂的布局，类似你经常看到的印刷出版物。你也可以将它们配合其它web标准适用。比如，结合<a href="http://www.qianduan.net/media-type-and-media-query.html">CSS媒体查询</a>，可以创建适应不同设备的方向(横屏landscape、竖屏portrait等)的布局。</p>
<p>图 6展示了使用<code>wrap-shape</code>属性结合CSS3 媒体查询来实现适应不同屏幕朝向的布局。</p>
<p> <img title="图6. 使用属性结合CSS3 媒体查询来实现适应不同屏幕朝向的布局" alt="图6. 使用属性结合CSS3 媒体查询来实现适应不同屏幕朝向的布局" src="http://img.qianduan.net/uploads/2011/06/fig06a.jpg"></p>
<p> <img title="图6. 使用属性结合CSS3 媒体查询来实现适应不同屏幕朝向的布局" alt="图6. 使用属性结合CSS3 媒体查询来实现适应不同屏幕朝向的布局" src="http://img.qianduan.net/uploads/2011/06/fig06b.jpg"></p>
<p> 图6. 使用属性结合CSS3 媒体查询来实现适应不同屏幕朝向的布局</p>
<p>图7 展示如何让同样的内容适应不同的屏幕方向，以及变化的列数</p>
<p> <img title="图7 内容适应不同的屏幕方向，同时改变的列数" alt="图7 内容适应不同的屏幕方向，同时改变的列数" src="http://img.qianduan.net/uploads/2011/06/fig07a.jpg"></p>
<p> <img title="图7 内容适应不同的屏幕方向，同时改变的列数" alt="图7 内容适应不同的屏幕方向，同时改变的列数" src="http://img.qianduan.net/uploads/2011/06/fig07b.jpg"></p>
<p> 图7 内容适应不同的屏幕方向，同时改变的列数</p>
<h3> CSS3 区域和JavaScript </h3>
<p>你也可以将这些功能结合JavaScript以创建交互的内容。在下面的图8中展示的，你可以滑动双向的箭头以移动图片，这个时候，文字围绕山体和汽车重新布局。</p>
<p> <img title="图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局" alt="图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局" src="http://img.qianduan.net/uploads/2011/06/fig08a.jpg"></p>
<p> <img title="图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局" alt="图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局" src="http://img.qianduan.net/uploads/2011/06/fig08b.jpg"></p>
<p> <img title="图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局" alt="图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局" src="http://img.qianduan.net/uploads/2011/06/fig08c.jpg"></p>
<p> 图8. 滑动双向的箭头移动图片，文字围绕山体和汽车重新布局</p>
<p>这个例子包含在上面提到的WebKit原型中，你可以下载体验下。</p>
<p><strong>译注：</strong>CSS3 region这个词，着实不太好翻译，暂时按照region的意思翻译为区域，但是感觉很别扭。还有文中的一些词组，如果你觉得有更好的中文说法，请赐教——神飞。</p>
<img src="http://feeds.feedburner.com/~r/qianduannet/~4/IeVNJ6dqkuI" height="1" width="1">