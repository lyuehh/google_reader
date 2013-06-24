---
layout: post
title:  "CSS3 Media Queries模板"
date:   2012-04-04 10:01:04
author: Airen
categories: program
---

## CSS3 Media Queries模板
### by Airen
### at 2012-04-04 10:01:04
### original <http://www.w3cplus.com/css3/css3-media-queries-for-different-devices>

<div><div><div><p>最早在《<a href="http://www.w3cplus.com/content/css3-media-queries">CSS3 Media Queries</a>》一文中初探了CSS3的媒体类型和媒体特性的相关应用。简单的知道了使用这个能在各种不同的设备显示不一样的样式风格。随着Responsive的响应式设计的兴起，前面跟大家一起学习了：</p>
<ol><li>《<a href="http://www.w3cplus.com/css3/responsive-design-with-css3-media-queries">Responsive设计和CSS3 Media Queries的结合</a>》</li>
<li>《<a href="http://www.w3cplus.com/css3/understanding-the-elements-of-responsive-web-design">了解Responsive网页设计的三个特性</a>》</li>
<li>《<a href="http://www.w3cplus.com/css3/responsive-design-in-3-steps">Responsive设计的关键三步</a>》</li>
</ol><p>从这几篇文章中可以知道，在Responsiv的设计中，CSS3的Media是起着非常关键性的作用，也可以说没有CSS3 Media这个属性，Responsiv设计是玩不起来，也是玩不转的。大家都知道Responsiv是为各种不同的设备进行样式设计的，但有一个问题大家或许还处在模糊状态，这个CSS3 Media要如何设置各自的临界点？</p>
<p>那么今天我们就针对上面的问题，一起来探讨一下CSS3 Media Queries在各种不同设备（桌面，手机，笔记本，ios等）下的模板制作。那么Media Queries是如何工作的？那么有关于他的工作原理大家要是感兴趣的话可以参考《<a href="http://www.w3cplus.com/content/css3-media-queries">CSS3 Media Queries</a>》一文，里面已经做过详细的介绍，这里就不在进行过多的阐述。</p>
<h4>CSS3 Media Queries模板</h4>
<p>CSS3 Media Queries一般都是使用“<strong style="color:red">max-width</strong>”和“<strong style="color:red">min-width</strong>”两个属性来检查各种设备的分辨大小与样式表所设条件是否满足，如果满足就调用相应的样式。打个比方来说，如果你的Web页面在960px的显屏下显示，那么首先会通过CSS3 Media Queries进行查询，看看有没有设置这个条件样式，如果找到相应的，就会采用对应下的样式，其他的设备是一样的道理。下面具体看看“max-width”和“min-width”模板：</p>
<p><strong>使用max-width</strong></p>
<pre>		@media screen and (max-width: 600px) {
			//你的样式放在这里....
		}
	</pre><p><strong>使用min-width</strong></p>
<pre>		@media screen and (min-width: 900px) {
			//你的样式放在这里...
		}
	</pre><p><strong>max-width和min-width的混合使用</strong></p>
<pre>		@media screen and (min-width: 600px) and (max-width: 900px) {
			//你的样式放在这里...
		}
	</pre><p>同时CSS3 Media Queries还能查询设备的宽度“<strong>device-width</strong>”来判断样式的调用，这个主要用在iPhone,iPads设备上，比如像下面的应用：</p>
<p><strong>iPhone和Smartphones上的运用</strong></p>
<pre>		/* iPhone and Smartphones (portrait and landscape) */
		@media screen and (min-device-width : 320px) and (max-device-width: 480px) {
			//你的样式放在这里...
		}
	</pre><p><strong style="color:red">max-device-width</strong>指的是设备整个渲染区宽度（设备的实际宽度）</p>
<p><strong>iPads上的运用</strong></p>
<pre>		/* iPads (landscape) */
		@media screen and (min-device-width : 768px) and (max-device-width : 1024px) and (orientation : landscape) {
			//你的样式放在这里...
		}
		/* iPads (portrait) */
		@media screen and (min-device-width : 768px) and (max-device-width : 1024px) and (orientation : portrait) {
			//你的样式放在这里...
		}
	</pre><p>针对移动设备的运用，如果你想让样式工作得比较正常，需要在“&lt;head&gt;”添加“viewport”的meta标签：</p>
<pre>		&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot; /&gt;
	</pre><p>有关于这方面的介绍大家可以看看这个<a href="http://www.wufangbo.com/mobile-3g-mobile-development/">blog</a>上进行的的移动端开发的相关总结。</p>
<h4>调用独立的样式表</h4>
<p>你可能希望在不同的设备下调用不同的样式文件，方便管理或者维护，那么你可以按下面的这种方式进行调用：</p>
<pre>		&lt;link rel=&quot;stylesheet&quot; media=&quot;screen and (min-device-width : 320px) and (max-device-width: 480px)&quot; href=&quot;iphone.css&quot; /&gt;
		&lt;link rel=&quot;stylesheet&quot; media=&quot;screen and (min-device-width : 768px) and (max-device-width : 1024px)&quot; href=&quot;ipad.css&quot; /&gt;
	</pre><h4>CSS3 Media Queries在标准设备上的运用</h4>
<p>下面我们一起来看看CSS3 Meida Queries在标准设备上的运用，大家可以把这些样式加到你的样式文件中，或者单独创建一个名为“responsive.css”文件，并在相应的条件中写上你的样式，让他适合你的设计需求：</p>
<p><strong>1、1024px显屏</strong></p>
<pre>		@media screen and (max-width : 1024px) {
			/* CSS Styles */
		}
	</pre><p><strong>2、800px显屏</strong></p>
<pre>		@media screen and (max-width : 800px) {
			/* CSS Styles */
		}
	</pre><p><strong>3、640px显屏</strong></p>
<pre>		@media screen and (max-width : 640px) {
			/* CSS Styles */
		}
	</pre><p><strong>4、iPad横板显屏</strong></p>
<pre>		@media screen and (max-device-width: 1024px) and (orientation: landscape) {
			/* CSS Styles */
		}
	</pre><p><strong>5、iPad竖板显屏</strong></p>
<pre>		@media screen and (max-device-width: 768px) and (orientation: portrait) {
			/* CSS Styles */
		}
	</pre><p><strong>iPhone 和 Smartphones</strong></p>
<pre>		@media screen and (min-device-width: 320px) and (min-device-width: 480px) {
			/* CSS Styles */
		}
	</pre><p>现在有关于这方面的运用也是相当的成熟，twitter的<a href="http://twitter.github.com/bootstrap/scaffolding.html#responsive">Bootstrap</a>第二版本中就加上了这方面的运用。大家可以对比一下：</p>
<pre>	// Landscape phones and down
	@media (max-width: 480px) { ... }
	 
	// Landscape phone to portrait tablet
	@media (max-width: 768px) { ... }
	 
	// Portrait tablet to landscape and desktop
	@media (min-width: 768px) and (max-width: 980px) { ... }
	 
	// Large desktop
	@media (min-width: 1200px) { .. }
	</pre><p>在<a href="http://twitter.github.com/bootstrap/scaffolding.html#responsive">bootstrap</a>中的responsive.css采用的是网格布局，大家可以到<a href="http://twitter.github.com/bootstrap/index.html">官网</a>查看或下载其源码进行学习。另外<a href="http://960.gs/">960gs</a>为大家提供了一个<a href="http://adapt.960.gs/">Adapt.js</a>也很方便的帮大家实现上述效果。感兴趣的同学可以去了解了解。</p>
<p>下面给大家提供几个这方面的案例，以供大家参考：</p>
<ol><li>《<a href="http://www.w3cplus.com/css3/media-queries-hicksdesign">CSS3 Media Queries案例——Hicksdesign</a>》</li>
<li>《<a href="http://www.w3cplus.com/css3/media-queries-tee-gallery">CSS3 Media Queries案例——Tee Gallery</a>》</li>
<li>《<a href="http://www.w3cplus.com/css3/media-queries-alistpart">CSS3 Media Queries案例——A List Apart</a>》</li>
</ol><h4>更新CSS3 Media Queries模板查询</h4>
<p><strong>1、Smartphones (portrait and landscape)</strong></p>
<pre>@media only screen and (min-device-width : 320px) and (max-device-width : 480px) {
	/* Styles */
}
</pre><p><strong>2、Smartphones (landscape)</strong></p>
<pre>@media only screen and (min-width : 321px) {
	/* Styles */
}
</pre><p><strong>3、Smartphones (portrait)</strong></p>
<pre>@media only screen and (max-width : 320px) {
	/* Styles */
}
</pre><p><strong>4、iPads (portrait and landscape)</strong></p>
<pre>@media only screen and (min-device-width : 768px) and (max-device-width : 1024px) {
	/* Styles */
}
</pre><p><strong>5、iPads (landscape)</strong></p>
<pre>@media only screen and (min-device-width : 768px) and (max-device-width : 1024px) and (orientation : landscape) {
	/* Styles */
}
</pre><p><strong>6、iPads (portrait)</strong></p>
<pre>@media only screen and (min-device-width : 768px) and (max-device-width : 1024px) and (orientation : portrait) {
	/* Styles */
}
</pre><p><strong>7、iPhone 4</strong></p>
<pre>@media only screen and (-webkit-min-device-pixel-ratio : 1.5),only screen and (min-device-pixel-ratio : 1.5) {
	/* Styles */
}
</pre><p><strong>8、640px显屏</strong></p>
<pre>@media screen and (max-width : 640px) {
	/* CSS Styles */
}
</pre><p><strong>9、800px显屏</strong></p>
<pre>@media screen and (max-width : 800px) {
	/* CSS Styles */
}
</pre><p><strong>10、1024显屏</strong></p>
<pre>@media screen and (max-width : 1024px) {
	/* CSS Styles */
}
</pre><p><strong>11、Desktops and laptops</strong></p>
<pre>@media only screen and (min-width : 1224px) {
	/* Styles */
}
</pre><p><strong>12、Large screens</strong></p>
<pre>@media only screen and (min-width : 1824px) {
	/* Styles */
}
</pre><p>那么有关于CSS3 Media Queries模板的相关介绍就说到这里了，最后希望大家喜欢。如果你觉得这篇文章对你有所帮助或者比较有实用价值，您可以把他推荐给您的朋友，如果你有更好的分享可以在下面的评论中直接与我们一起分享您的经验。</p>
<p>如需转载烦请注明出处：<strong><a href="http://www.w3cplus.com">W3CPLUS</a></strong></p>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blogclassify/11.html">css3</a></li></ul></div><div><div><div><div>
      <div>1</div>
                  <a href="http://www.w3cplus.com/vote/node/269/1/vote/alternate/wm169CbhjcVwZ_Z-fdOQUgy4THCXZvyMDuXC21lA3C0/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><img src="http://www1.feedsky.com/t1/654894289/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/css3/css3-media-queries-for-different-devices" border="0" height="0" width="0">