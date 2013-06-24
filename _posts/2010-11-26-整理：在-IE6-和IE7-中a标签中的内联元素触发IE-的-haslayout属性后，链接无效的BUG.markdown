---
layout: post
title:  "整理：在 IE6 和IE7 中a标签中的内联元素触发IE 的 haslayout属性后，链接无效的BUG"
date:   2010-11-26 10:14:22
author: karry
categories: program
---

## 整理：在 IE6 和IE7 中a标签中的内联元素触发IE 的 haslayout属性后，链接无效的BUG
### by karry
### at 2010-11-26 10:14:22
### original <http://item.feedsky.com/~feedsky/PlayGoogleKarry/~8010025/463440062/5589304/1/item.html>

<p>昨天在工作中遇到一个诡异的现象，老版本IE( IE6 和IE7) 导航栏a标签无法正常使用，但onclick事件能正常执行。</p>
<p>经反复测试发现：</p>
<p>a标签内嵌套一层或多层内联元素（如span label em 等）, 最终的内联元素包含一个img元素, 这时, 如果触发了a元素和img元素之间的任何一个元素的hasLayout, 那么超链接将失效.</p>
<div>
&lt;a href=”http://playgoogle.com”&gt;
<p>&lt;span&gt;&lt;img src=”test.jpg” alt=”test” title=”test” /&gt;&lt;/span&gt;</p>
<p>&lt;/a&gt;</p>
<p>span {</p>
<p><em>display: inline-block;</em></p>
<p>}
</p></div>
<p><strong>有三种解决办法：</strong></p>
<ol>
<li>不要在a标签内嵌套任何内联元素，使用其他方式实现。</li>
<li>不要触发内联元素的haslayout属性。</li>
<li>对img 设置相对定位，并把z-index设置为负值。</li>
</ol>
<p>注：内联元素触发haslayout 的条件：</p>
<p><a href="http://www.moonless.net/demo/9/">http://www.moonless.net/demo/9/</a></p><img src="http://www1.feedsky.com/t1/463440062/PlayGoogleKarry/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/PlayGoogleKarry/~8010025/463440062/5589304/1/item.html" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/PlayGoogleKarry/463440062/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/PlayGoogleKarry/463440062/art01.gif"></a></p>