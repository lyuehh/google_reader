---
layout: post
title:  "haslayout详解"
date:   2010-05-20 16:15:36
author: xiaojue
categories: program
---

## haslayout详解
### by xiaojue
### at 2010-05-20 16:15:36
### original <http://www.designsor.com/2010/05/haslayout.html>

<p>最近开始恶补基础知识，弥补自己的基础薄弱的问题。</p>
<p><strong>haslayout是什么？</strong></p>
<p>“Layout” 是 IE 的一个私有属性，并不是W3C标准。它决定了一个对象（就是一个标签div、li等）在内容中如何显示、与周围对象的位置关系、以及怎样响应程序或用户产生的事件。</p>
<p>这个属性可以被一些css强制激活。一些HTML标签默认具有haslayout。</p>
<p>下面这些标签默认拥有haslayout属性：</p>
<blockquote>
<ul style="list-style:none">
<li><code>&lt;html&gt;&lt;body&gt;</code></li>
<li><code>&lt;table&gt;&lt;tr&gt;&lt;th&gt;&lt;td&gt;</code></li>
<li><code>&lt;img&gt;</code></li>
<li><code>&lt;hr&gt;</code></li>
<li><code>&lt;input&gt;&lt;button&gt;&lt;select&gt;&lt;textarea&gt;&lt;fieldset&gt;&lt;legend&gt;</code></li>
<li><code>&lt;iframe&gt;&lt;embed&gt;&lt;object&gt;&lt;applet&gt;</code></li>
<li><code>&lt;marquee&gt;</code></li>
</ul>
</blockquote>
<p><strong>怎样激活layout？</strong></p>
<blockquote><dt>position: absolute</dt>
<dd>设置绝对定位可能会引发新的问题。</dd>
<dt>float: left|right</dt>
<dd>IE下的浮动也会产生一些莫名其妙的问题。</dd>
<dt>display: inline-block</dt>
<dd>当一个内联元素需要haslayout属性时就需要用它，但是IE本身不支持inline-block的，只是表现得像标准里说的inline-block。</dd>
<dt>width: 除’auto’外的任意值</dt>
<dd>优先考虑使用该属性。</dd>
<dt>height: 除’auto’外的任意值</dt>
<dd>对 IE6 及更早版本来说很常用，该方法被称为霍莉破解(Holly hack)，即设定这个元素的高度为 1% (height:1%;)。但是要注意，当这个元素的 overflow 属性被设置为 visible 时，这个方法就失效了。</dd>
<dt>zoom: 除’normal’外的任意值</dt>
<dd>又一个ie私有属性，不兼容标准。zoom:1可以在测试或者不追求标准的情况下使用，效果不错。</dd>
<dt>writing-mode: tb-rl</dt>
<dd>ie私有属性，不推荐用。</dd>
<p>IE7 还有一些额外的属性：</p>
<dt>min-height: (任意值)</dt>
<dt>max-height: (除 none 外任意值)</dt>
<dt>min-width: (任意值)</dt>
<dt>max-width: (除 none 外任意值)</dt>
<dt>overflow: (除 visible 外任意值)</dt>
<dt>overflow-x: (除 visible 外任意值)</dt>
<dt>overflow-y: (除 visible 外任意值)</dt>
<dt>position: fixed</dt>
</blockquote>
<p><strong>重置haslayout.</strong></p>
<p>在没有其它属性激活layout的情况下，使用下面的css可以重置haslayout属性：</p>
<blockquote>
<ul style="list-style:none">
<li>width, height (设为 “auto”)</li>
<li>max-width, max-height (设为 “none”)(在 IE 7 中)</li>
<li>position (设为 “static”)</li>
<li>float (设为 “none”)</li>
<li>overflow (设为 “visible”) (在 IE 7 中)</li>
<li>zoom (设为 “normal”)</li>
<li>writing-mode (从 “tb-rl” 设为 “lr-t”)</li>
</ul>
</blockquote>
<p>display 属性的不同：当用”inline-block”激活了haslayout 属性时，就算在一条独立的规则中覆盖这个属性为”block”或”inline”，haslayout 这个标志位也不会被重置为 false。</p>
<p>把 mid-width, mid-height 设为它们的默认值”0″仍然会赋予 hasLayout，但是 IE 7 却可以接受一个不合法的属性”auto”来重置 hasLayout。</p>
<p><strong>当一个对象的layout被激活时，它以及它的子对象的定位和尺寸计算将独立进行，不受附近对象的干扰。也就是说它拥有一个独立的布局（layout）。 因此浏览器要花费更多的代价来处理拥有haslayout的对象。为了提高性能，微软增加了layout这个IE私有的概念。</strong></p>