---
layout: post
title:  "[原]CSS3制作加载中loading动画效果"
date:   2012-03-05 07:36:23
author: soasme
categories: program
---

## [原]CSS3制作加载中loading动画效果
### by soasme
### at 2012-03-05 07:36:23
### original <http://blog.csdn.net/soasme/article/details/7318039>

<div>
<p><span style="background-color:rgb(255,255,255)"> 常见的loading加载中通常是使用一个gif来实现动画效果，实际上我们可以使用css3的特性来制作，本文介绍了一种线性加载效果的css3制作方法。</span></p>
<p>在web开发中经常遇到ajax的加载，那个加载中的图标大家是不是都是用gif制作的呢？<br>
这里顺便一提，有一个定制loader gif的小图标的工具网站，<a title="PreLoader [Loading GIF &amp; APNG (AJAX loaders) generator]" href="http://preloaders.net/">点击这里查看</a></p>
<p>此文中介绍一种线性样式的加载样式，<a title="css3制作加载中小图标loading" href="http://soasmeblog.sinaapp.com/wp-content/uploads/20120301/demo-linear-loader.html">点击这里查看demo</a></p>
<p>我们都知道，动画的本质是一帧一振画面按顺序播放，因此我们就有如下的设计思路：</p>
<p>画一排五个小圆圈，五个小圆圈总是花1秒时间完成透明度从1到0.1的动画，但是每个动画播放时间不同，依次延后0.2秒。</p>
<p>见代码实现：</p>
<p>首先画五个小圆圈：</p>
<p>HTML代码</p>
<p></p>
<pre name="code">&lt;div class=&quot;loader&quot;&gt;
	&lt;span&gt;&lt;/span&gt;
	&lt;span&gt;&lt;/span&gt;
	&lt;span&gt;&lt;/span&gt;
	&lt;span&gt;&lt;/span&gt;
	&lt;span&gt;&lt;/span&gt;
&lt;/div&gt;</pre><br>
<br>
<p></p>
<p>CSS代码</p>
<p></p>
<pre name="code">.loader span{
	display: inline-block;
	vertical-align: middle;
	width: 10px;
	height: 10px;
	background: black;
	border-radius: 50px;
}</pre><br>
<br>
<p></p>
<p>其次，我们来定制动画</p>
<p>CSS代码</p>
<pre><pre name="code">.loader span{
	display: inline-block;
	vertical-align: middle;
	width: 10px;
	height: 10px;
	background: black;
	border-radius: 50px;
	-webkit-animation: loader 1s infinite alternate;
	-moz-animation: loader 1s infinite alternate;
}
@-webkit-keyframes loader {
	0% {
		opacity: 1;
	}
	100% {
		opacity: 0.1;
	}
}
@-moz-keyframes loader {
	0% {
		opacity: 1;
	}
	100% {
		opacity: 0.1;
	}
}</pre><br><br></pre>
<p>最后，我们添加延时动画的命令：<br>
CSS代码</p>
<pre><pre name="code">.loader span{
	display: inline-block;
	vertical-align: middle;
	width: 10px;
	height: 10px;
	background: black;
	border-radius: 50px;
	-webkit-animation: loader 1s infinite alternate;
	-moz-animation: loader 1s infinite alternate;
}
.loader span:nth-of-type(2) {
	-webkit-animation-delay: 0.2s;
	-moz-animation-delay: 0.2s;
}
.loader span:nth-of-type(3) {
	-webkit-animation-delay: 0.4s;
	-moz-animation-delay: 0.4s;
}
.loader span:nth-of-type(4) {
	-webkit-animation-delay: 0.6s;
	-moz-animation-delay: 0.6s;
}
.loader span:nth-of-type(5) {
	-webkit-animation-delay: 0.8s;
	-moz-animation-delay: 0.8s;
}
@-webkit-keyframes loader {
	0% {
		opacity: 1;
	}
	100% {
		opacity: 0.1;
	}
}
@-moz-keyframes loader {
	0% {
		opacity: 1;
	}
	100% {
		opacity: 0.1;
	}
}</pre><br><br></pre>
<p>最后完成制作。</p>
<p>如需调整个数、时间也可自行根据原理定制，不过最好保证渐变的时间间隔，如果太慢会让人感觉加载有气无力，而太快了又会让人感觉不适。</p>
<p> </p>
<p> </p>
<ul>
<li>版权声明：自由转载-非商用-非衍生-保持署名 | <a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li><li>原文网址：<a href="http://soasmeblog.sinaapp.com/posts/46.html">http://soasmeblog.sinaapp.com/posts/46.html</a></li></ul>
<p> </p>
<br>
<br>
</div>

            <div>
                作者：soasme 发表于2012-3-4 15:36:23 <a href="http://blog.csdn.net/soasme/article/details/7318039">原文链接</a>
            </div>
            <div>
            阅读：1469 评论：0 <a href="http://blog.csdn.net/soasme/article/details/7318039#comments">查看评论</a>
            </div>