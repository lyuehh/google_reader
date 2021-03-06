---
layout: post
title:  "Medial Queries的另一用法——服务于IE"
date:   2012-11-16 17:01:06
author: Airen
categories: program
---

## Medial Queries的另一用法——服务于IE
### by Airen
### at 2012-11-16 17:01:06
### original <http://www.w3cplus.com/css3/moving-ie-specific-css-into-media-blocks>

<div><div><div><p>随着<a href="http://www.w3cplus.com/search/node/responsive">Responsive</a>设计的流行，<a href="http://www.w3cplus.com/content/css3-media-queries">Medial Queries</a>可算是越来越让人观注了。他可以让Web前端工程实现不同设备下的样式选择，让站点在不同的设备中实现不同的效果。这个早前在w3cplus已经介绍过，如果你还没有接触，不仿点击<a href="http://www.w3cplus.com/content/css3-media-queries">这里</a>详细阅读。今天在看blog时发现一个其他的使用方法，就是利用@media来做一些IE下的特殊效果。</p>
<p>众所周知，有些时候为了实现IE下的某些效果与现代浏览器一致，我们不得不使用一些<a href="http://www.w3cplus.com/css/create-css-browers-hacks">hack</a>手段来实现目的。比如说使用“\0”，“\”和“\9”来仅让IE某些版本识别，而对于现代浏览器来说，他会直接无视这些代码。今天我想为大家介绍的是使用@media实现IE hack的方法：</p>
<h4>
	仅IE6和IE7识别</h4>
<pre>
@media screen\9 {
  .selector {  property: value; }
} 
</pre><h4>
	仅IE6和IE7、IE8识别</h4>
<pre>
@media \0screen\,screen\9 {
  .selector {  property: value; }
}
</pre><h4>
	仅IE8识别</h4>
<pre>
@media \0screen {
  .selector {  property: value; }
}
</pre><h4>
	仅IE8-10识别</h4>
<pre>
@media screen\0 {
  .selector {  property: value; }
} 
</pre><h4>
	仅IE9和IE10识别</h4>
<pre>
@media screen and (min-width:0\0) {
  .selector {  property: value; }
} 
</pre><h4>
	仅IE10识别</h4>
<pre>
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {
   /* IE10-specific styles go here */
}
</pre><p>上面几个@media配合“\”、“\0”和“\9”就能让不同版本的IE识别，那么具体项目中我们要怎么使用呢？</p>
<p>打个比方，如果你的body中设置了一个红色的背景，而想让不同版本IE变成别的颜色，那么我们就可以这么操作</p>
<pre>
body {
  background: red;
}

/* IE6、IE7变成绿色 */
@media all\9 {
  body {
    background: green;
  }
}

/* IE8变成蓝色 */
@media \0screen {
  body {
    background: blue;
  }
}
/*IE9和IE10变成黄色*/
@media screen and (min-width:0\0) {
  body { 
    background: yellow; 
  }
} 
 
</pre><p>IE的Hack方法我向来不提倡使用，但这些方法很少有人知道，贴上来与大家分享，希望在不得已的情况之下，这些hack方法能帮你解决问题，特别是国内的苦逼前端人员。</p>
<p>如需转载，烦请注明出处：<a href="http://www.w3cplus.com/css3/moving-ie-specific-css-into-media-blocks">http://www.w3cplus.com/css3/moving-ie-specific-css-into-media-blocks</a></p>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blog/tags/11.html">css3</a></li></ul></div><div><ul><li><a href="http://www.w3cplus.com/blog/tags/49.html">media queries</a></li><li><a href="http://www.w3cplus.com/blog/tags/42.html">ie hack</a></li></ul></div><div><div><div><div>
      <div>3</div>
                  <a href="http://www.w3cplus.com/vote/node/526/1/vote/alternate/sZeYu9sCzpet5ALc2oozSAIUwDlzBICZwQcAxNAwcBo/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><img src="http://www1.feedsky.com/t1/693832911/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/css3/moving-ie-specific-css-into-media-blocks" border="0" height="0" width="0">