---
layout: post
title:  "【外刊IT评论网】HTML5 Canvas(画布)教程 – 图像处理"
date:   2011-11-25 00:26:23
author: Aqee
categories: program
---

## 【外刊IT评论网】HTML5 Canvas(画布)教程 – 图像处理
### by Aqee
### at 2011-11-25 00:26:23
### original <http://www.aqee.net/html-5-canvas-tutorial-displaying-images/>

本文是从 <a href="http://www.switchonthecode.com/tutorials/html-5-canvas-tutorial-displaying-images">HTML 5 Canvas Tutorial - Displaying Images</a> 这篇文章翻译而来。 <hr><p>Canvas标记很多年前就被当作一个新的HTML标记成员加入到了HTML5标准中。在此之前，人们要想实现动态的网页应用，只能借助于第三方的插件，比如Flash或Java，而引入了Canvas标记后，人们直接打通了通往神奇的动态应用网页的大门。本教程内容只覆盖了一小部分、但却是非常重要的canvas标记的应用功能——图像显示和处理。</p>
<p><span></span></p>
<h3>图像来源</h3>
<p>最常见的在canvas上画图的方法是使用Javascript Image对象。所支持的来源图片格式依赖于浏览器的支持，然而，一些典型的图片格式(png，jpg，gif等)基本上都没有问题。</p>
<p>图片可以从DOM中已经加载的元素中抓取，也可以按需即时创建。</p>

<div>
<div style="font-family:monospace"><span style="color:#008000;font-style:italic">// 抓取页面上已有的图片。</span><br>
<span style="color:#0600ff">var</span> myImage <span style="color:#000000">=</span> document.<span style="color:#000000">getElementById</span><span style="color:#000000">(</span><span style="color:#ff0000">‘myimageid’</span><span style="color:#000000">)</span><span style="color:#000000">;</span><span style="color:#008000;font-style:italic">// 或创建一个新图片。</span><br>
myImage <span style="color:#000000">=</span> <span style="color:#0600ff">new</span> Image<span style="color:#000000">(</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
myImage.<span style="color:#000000">src</span> <span style="color:#000000">=</span> <span style="color:#ff0000">‘image.png’</span><span style="color:#000000">;</span></div>
</div>
<p>大多数支持canvas标记的浏览器的当前版本中，当图片还没有加载完成时，如果你要去画它，结果是什么事情都不会发生。也就是说，如果你想画一个图片，你需要等它完全加载。你可以使用图片对象的onload函数来进行判断。</p>
<div>
<div style="font-family:monospace"><span style="color:#008000;font-style:italic">// Create an image.</span><br>
myImage <span style="color:#000000">=</span> <span style="color:#0600ff">new</span> Image<span style="color:#000000">(</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
myImage.<span style="color:#0600ff">onload</span> <span style="color:#000000">=</span> <span style="color:#0600ff">function</span><span style="color:#000000">(</span><span style="color:#000000">)</span> <span style="color:#000000">{</span><br>
<span style="color:#008000;font-style:italic">// Draw image.</span><br>
<span style="color:#000000">}</span><br>
myImage.<span style="color:#000000">src</span> <span style="color:#000000">=</span> <span style="color:#ff0000">‘image.png’</span><span style="color:#000000">;</span></div>
</div>
<p>在下面的所有例子中，我们的图片源将会使用这个256×256尺寸的大猩猩。</p>
<div>
<div><img src="http://www.aqee.net/wordpress/wp-content/uploads/2011/11/image.png" alt="大猩猩图片源"></div>
</div>
<h3>基本绘画</h3>
<p>在最基本的画图操作中，你需要的只是希望图像出现处的位置(x和y坐标)。图像的位置是相对于其左上角来判断的。使用这种方法，图像可以简单的以其原尺寸被画在画布上。</p>
<div>
<div style="font-family:monospace">drawImage<span style="color:#000000">(</span>image<span style="color:#000000">,</span> x<span style="color:#000000">,</span> y<span style="color:#000000">)</span></div>
</div>
<div>
<div style="font-family:monospace"><span style="color:#0600ff">var</span> canvas <span style="color:#000000">=</span> document.<span style="color:#000000">getElementById</span><span style="color:#000000">(</span><span style="color:#ff0000">‘myCanvas’</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
<span style="color:#0600ff">var</span> ctx <span style="color:#000000">=</span> canvas.<span style="color:#000000">getContext</span><span style="color:#000000">(</span><span style="color:#ff0000">’2d’</span><span style="color:#000000">)</span><span style="color:#000000">;</span>ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">210</span><span style="color:#000000">,</span> <span style="color:#000000">210</span><span style="color:#000000">)</span><span style="color:#000000">;</span></div>
</div>
<div>
<div><img src="http://www.aqee.net/wordpress/wp-content/uploads/2011/11/basic.png" alt="基本画布绘图输出"></div>
</div>
<h3>缩放及调整尺寸</h3>
<p>改变图像的尺寸，你需要使用重载的drawImage函数，提供给它希望的宽度和高度参数。</p>
<div>
<div style="font-family:monospace">drawImage<span style="color:#000000">(</span>image<span style="color:#000000">,</span> x<span style="color:#000000">,</span> y<span style="color:#000000">,</span> width<span style="color:#000000">,</span> height<span style="color:#000000">)</span></div>
</div>
<div>
<div style="font-family:monospace"><span style="color:#0600ff">var</span> canvas <span style="color:#000000">=</span> document.<span style="color:#000000">getElementById</span><span style="color:#000000">(</span><span style="color:#ff0000">‘myCanvas’</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
<span style="color:#0600ff">var</span> ctx <span style="color:#000000">=</span> canvas.<span style="color:#000000">getContext</span><span style="color:#000000">(</span><span style="color:#ff0000">’2d’</span><span style="color:#000000">)</span><span style="color:#000000">;</span>ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">200</span><span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">210</span><span style="color:#000000">,</span> <span style="color:#000000">210</span><span style="color:#000000">,</span> <span style="color:#000000">500</span><span style="color:#000000">,</span> <span style="color:#000000">500</span><span style="color:#000000">)</span><span style="color:#000000">;</span></div>
</div>
<p>这个例子演示了如何画一个比原图小的图像，一个不同长宽比的图像和一个比原图大的图像的方法。</p>
<div>
<div><img src="http://www.aqee.net/wordpress/wp-content/uploads/2011/11/scale.png" alt="画布裁剪图像"></div>
</div>
<h3>图像裁剪</h3>
<p>最后一个drawImage方法的功用是对图像进行裁剪。</p>
<div>
<div style="font-family:monospace">drawImage<span style="color:#000000">(</span>image<span style="color:#000000">,</span><br>
sourceX<span style="color:#000000">,</span><br>
sourceY<span style="color:#000000">,</span><br>
sourceWidth<span style="color:#000000">,</span><br>
sourceHeight<span style="color:#000000">,</span><br>
destX<span style="color:#000000">,</span><br>
destY<span style="color:#000000">,</span><br>
destWidth<span style="color:#000000">,</span><br>
destHeight<span style="color:#000000">)</span></div>
</div>
<p>参数很多，但基本上你可以把它想成从原图中取出一个矩形区域，然后把它画到画布上目标区域里。</p>
<div>
<div><img src="http://www.aqee.net/wordpress/wp-content/uploads/2011/11/slice.png" alt="画布裁剪例子"></div>
</div>
<div>
<div style="font-family:monospace"><span style="color:#0600ff">var</span> canvas <span style="color:#000000">=</span> document.<span style="color:#000000">getElementById</span><span style="color:#000000">(</span><span style="color:#ff0000">‘myCanvas’</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
<span style="color:#0600ff">var</span> ctx <span style="color:#000000">=</span> canvas.<span style="color:#000000">getContext</span><span style="color:#000000">(</span><span style="color:#ff0000">’2d’</span><span style="color:#000000">)</span><span style="color:#000000">;</span>ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">0</span><span style="color:#000000">,</span> <span style="color:#000000">0</span><span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">,</span> <span style="color:#000000">50</span><span style="color:#000000">,</span> <span style="color:#000000">25</span><span style="color:#000000">,</span> <span style="color:#000000">25</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">125</span><span style="color:#000000">,</span> <span style="color:#000000">150</span><span style="color:#000000">,</span> <span style="color:#000000">150</span><span style="color:#000000">)</span><span style="color:#000000">;</span><br>
ctx.<span style="color:#000000">drawImage</span><span style="color:#000000">(</span>myImage<span style="color:#000000">,</span> <span style="color:#000000">80</span><span style="color:#000000">,</span> <span style="color:#000000">80</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">,</span> <span style="color:#000000">100</span><span style="color:#000000">,</span> <span style="color:#000000">250</span><span style="color:#000000">,</span> <span style="color:#000000">250</span><span style="color:#000000">,</span> <span style="color:#000000">220</span><span style="color:#000000">,</span> <span style="color:#000000">220</span><span style="color:#000000">)</span><span style="color:#000000">;</span></div>
</div>
<div>
<div><img src="http://www.aqee.net/wordpress/wp-content/uploads/2011/11/slices.png" alt="画布裁剪"></div>
</div>
<p>这些就是HTML5中的canvas(画布)标记里进行绘图和处理图像的基本操作。绘图只是canvas能提供的功能之一，将来我们会发布更多的关于这方面的教程，会介绍关于这个标记的更多的特征和功能。如果你有任何的问题和想法，请在下面评论的写出来。</p>
<hr>本文来自<a href="http://www.aqee.net">外刊IT评论网</a>(<a href="http://www.aqee.net">www.aqee.net</a>)，原始地址：<a href="http://www.aqee.net/html-5-canvas-tutorial-displaying-images/" rel="bookmark">HTML5 Canvas(画布)教程 – 图像处理</a><br><img src="http://www1.feedsky.com/t1/584476474/aqee-net/feedsky/s.gif?r=http://www.aqee.net/html-5-canvas-tutorial-displaying-images/" border="0" height="0" width="0">