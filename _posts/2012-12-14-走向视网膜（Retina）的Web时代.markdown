---
layout: post
title:  "走向视网膜（Retina）的Web时代"
date:   2012-12-14 00:29:17
author: Airen
categories: program
---

## 走向视网膜（Retina）的Web时代
### by Airen
### at 2012-12-14 00:29:17
### original <http://www.w3cplus.com/css/towards-retina-web.html>

<div><div><div><p><a href="http://zh.wikipedia.org/wiki/Retina%E6%98%BE%E7%A4%BA%E5%B1%8F">维基百科</a>将Retina译为“视网膜”。"Retina"一词，原意是“视网膜”的意思，指显示屏的分辨率极高，使得肉眼无法分辨单个像素。</p>
<p>苹果的“iPhone4”和"new iPad"以及“Macbook Pro”中已经使用了Retina(视网膜)技术。这是一种新的屏幕的显示技术。苹果表示，Retina屏幕是一种具备超高像素密度的液晶屏，它可以将960x640的分辨率压缩到一个3.5英寸的显示屏内。也就是说，该屏幕的像素密度达到326像素/英寸（ppi）。</p>
<p>苹果采用Retina屏幕技术得到了更多人的亲眯，因为Retina给画面带来了前所未有的清晰平滑效果。相比之下，互联网非常的庞大，从当初的普通屏幕过渡到Retina是件缓慢而又痛苦的事情。在缺少行业标准来简化这个过程，每个Web开发人员和设计者为了确保他们的用户得到最好的视觉体验，Retina还是给他们带来了些小麻烦。</p>
<p>为了解决这些麻烦，更好的从事Web开发和设计，做为一名紧跟时代前沿技术的Web前端人员是很有必要了解学习Retina。</p>
<p>在深入了解和学习Retina之前，我们需要了先学习一些基本概念，这些基本概念将更好的带领我们深入下去。</p>
<h4>设备像素</h4>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-1.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>设备像素也被称为物理像素,他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。正是这些设备像素的微小距离欺骗了我们肉眼看到的图像效果。</p>
<h4>屏幕密度</h4>
<p>屏幕密度是指一个设备表面上存在的像素数量，它通常以每英寸有多少像素来计算（PPI）。苹果将这个营销出一个专业的术语“Retina”,将其称为双密度显示，声称人类的肉眼将无法区分单个像素。当一个显示屏像素密度超过300ppi时，人眼就无法区分出单独的像素。这也是讲：显示设备清晰度已达到人视网膜可分辨像素的极限。因此行动电话显示器的像素密度达到或高于300ppi就不会再出现颗粒感，而手持平板类电器显示器的像素密度达到或高于260ppi就不会再出现颗粒感，苹果行动电脑的Retina显示器像素密度只要超过200ppi就无法区分出单独的像素。</p>
<h4>CSS像素</h4>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-2.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>CSS像素是一个抽象的单位，主要使用在浏览器上，用来精确的度量（确定）Web页面上的内容。一般情况下，CSS像素被称为与设备无关的像素（device-independent像素），简称为“DIPs”。在一个标准的显示密度下，一个CSS像素对应着一个设备像素。</p>
<pre>
&lt;div height=&quot;200&quot; width=&quot;300&quot;&gt;&lt;/div&gt;
</pre><p>上面的代码，将会在显示屏设备上绘制一个200x300像素的盒子。但是在Retina屏幕下，相同的div却使用了400x600设备像素，保持相同的物理尺寸显示，导致每个像素点实际上有4倍的普通像素点，如图所示：</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-3.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>反过来说，一个CSS像素点实际分成了四个，这样就造成了颜色只能近似选取，于是，我们看上去就变得模糊了。那么在Web开发和设计中，我们可以通过"device-pixel-ratio"来解决。</p>
<pre>
device-pixel-ratio,
-o-device-pixel-ratio,
-moz-device-pixel-ratio,
-Webkit-device-pixel-ratio {
  …
}
</pre><p>或者你可以使用将来的写法：</p>
<pre>
device-pixel-ratio,
-o-min-device-pixel-ratio,
min--moz-device-pixel-ratio,
-Webkit-min-device-pixel-ratio {
  …
}
</pre><p>在javascript中，“window.devicePixelRatio”可以用来获得相同的比率，虽然目前浏览器支持还有限。但这种技术将会慢慢被支持。</p>
<p>"window.devicePixelRatio"其实指的就是“devicePixelRatio”,我们来看看“window.devicePixelRatio”是什么？简单点说“window.devicePixelRatio”是设备上物理像素和设备像素（device-independent pixels (dips)）的比例。他有一个计算公式：window.devicePixelRatio = 物理像素/dips。</p>
<p>dip或dp,（device independent pixels，设备独立像素）与屏幕密度有关。dip可以用来辅助区分视网膜设备还是非视网膜设备。</p>
<p>所有非视网膜屏幕的iPhone在垂直屏幕显示的时候，他的宽度为320物理像素，当你通过“meta”改变“viewport”时：</p>
<pre>
&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width&quot;&gt;
</pre><p>这个时候，视窗布局宽度变成320px（不同于视觉区域宽度，不放大显示情况下，两者大小一致），这样视觉上，整个页面很自然的覆盖在屏幕上，如下图所示：</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-4.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc"></p>
<p>这样，非视网膜屏幕的iphone上，屏幕物理像素320像素，独立像素也是320像素，因此window.devicePixelRatio等于1.</p>
<p>而对于视网膜屏幕的iphone，如iphone4s, 纵向显示的时候，屏幕物理像素640像素。同样，当用户设置</p>
<pre>
&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width&quot;&gt;
</pre><p>这个时候，其视区宽度并不是640像素，而是320像素，这是为了有更好的阅读体验 – 更合适的文字大小。</p>
<p>这样，在视网膜屏幕的iphone上，屏幕物理像素640像素，独立像素还是320像素，因此，window.devicePixelRatio等于2.</p>
<h4>位图像素</h4>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-5.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>位图是由像素（Pixel）组成的，像素是位图最小的信息单元，存储在图像栅格中(PNG, JPG, GIF等等)。每个像素都具有特定的位置和颜色值。按从左到右、从上到下的顺序来记录图像中每一个像素的信息，如：像素在屏幕上的位置、像素的颜色等。位图图像质量是由单位长度内像素的多少来决定的。单位长度内像素越多，分辨率越高，图像的效果越好。</p>
<p>位图也称为“位图图像”“点阵图像”“数据图像”“数码图像”。</p>
<p>基于图像的栅格分辨率，图像在Web中是由CSS像素定义了一个抽象的大小。浏览器挤压或者拉伸图像都是基于其CSS高度或者宽度来进行呈现的一个过程。</p>
<p>当一个光栅图像在标准设备下全屏显示时,一位图像素对应的就是一设备像素,导致一个完全保真的显示。因为一个位置像素不能进一步分裂，当在Retina屏幕下时，他要放大四倍来保持相同的物理像素的大小，这样就会丢失很多细节，造成失真的情形。换句话说，每一位图像素被乘以四填补相同的物理表面在视网膜屏幕下显示。</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-6.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<h4>高分辨率屏幕与高像素密度屏幕</h4>
<p>在 Retina 视网膜屏幕面世之前人们很少关注像素密度，尤其在 Windows 系统下，提高屏幕分辨率一般都是通过提高屏幕尺寸。而随着屏幕分辨率的提升，图像和文字显示目标会相应缩小，造成观看极其不便。因为系统并不会自动根据屏幕尺寸和分辨率关系相应的调整文字和图标的大小。（即使手动调整也会因为微软一直采用的点阵字体和大多数位图在提高分辨率后，因为多于出的像素点没有填充渲染会出现拉伸，进而会产生锯齿，这也是系统不会自动适配的原因之一）这也就给我们造成一种假象：显示器尺寸越大，分辨率就会越大。</p>
<p>所以当最近苹果的 Retina 视网膜屏幕令很多人困惑不已，为什么那么小的屏幕会有那么大的分辨率。为什么那么大的分辨率，非但没有使得文字和图像变小，反而更加清晰了呢？</p>
<h4>高像素密度屏幕（高 ppi）</h4>
<p>严格来说，高像素密度屏幕也是属于高分辨率屏幕，不同的是高像素密度屏幕在提升了分辨率的同时也提高了其像素密度，即相同的尺寸有着更大的分辨率。以评估的 Retina 视网膜屏幕为例，它并不是像普通显示器那样通过增大尺寸来增加分辨率，而是靠提升单位面积屏幕的像素数量，即像素密度来提升分辨率，这样就有了高像素密度屏幕。</p>
<p>同时操作系统也会自动采取相应的模式（如 Mac 下的 HiDPI）进行适配，将缩小后的字体（苹果一直采用矢量字体）和图标重新放大，这样苹果用了更多的像素数来显示同样的内容，显示尺寸仍然不变，但是多了更多细节，因此会有非常明显的视觉效果提升。</p>
<p>这样一来我们可以看一个简单的换算</p>
<p><strong>iPhone/iPod Touch</strong></p>
<pre>
普通屏幕分辨率 ====&gt;  320px X 480px
Retion分辨率   ====&gt;  640px X 960px
</pre><p><strong>iPad,iPad2/New iPad</strong></p>
<pre>
普通屏幕分辨率 ====&gt;  768px X 1024px
Retion分辨率   ====&gt;  1536px X 2048px
</pre><p><strong>换算关系</strong></p>
<pre>
普通屏幕分辨率 ====&gt;  1点=1像素
Retion分辨率   ====&gt;  1点=2像素
</pre><p>上面两段文字内容摘自于<a href="http://www.nangongruiyang.com">南宮瑞揚</a>的《<a href="http://www.nangongruiyang.com/index.php/webdesign/retinahtml.html">retina视网膜时代的页面</a>》一文。</p>
<h4>Retina设备</h4>
<p>Retina是由苹果提出来的，根据苹果发布产品的定义：PPI高于210（笔记本电脑）、260（平板电脑）、300（手机）的屏幕都称为Retina屏幕</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-7.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>通过上面的内容介绍，我们大致了解了Retina周边相关的术语，以及他们之间的关系和区别，如果你需要深入的了解Retina，我们就继续往下阅读。</p>
<p>尽管我们仍处理普通屏幕向Retina屏幕的转变中，但如何使用Retina来优化Web图形的方法还是如雨后春笋涌现，而且还有更多的人在关注和分享。</p>
<h4>THML和CSS的标准</h4>
<p>最直截了当的方法就是通过手动制图或以编程的方式制作两种不同的图形，一张是普通屏幕的图片，另外一种是Retina屏幕的图形，而且Retina屏幕下的图片是普通屏幕的两倍像素。看个实例，有一张200x300像素的图片（CSS像素），然后将一个位图的像素400x600上传到你的服务器上，通过CSS样式或HTML属性将其压缩50%，这个时候，在一个标签分辨率的显示器上，其呈现的图像是位图的四分之一像素。简单点理解，普通屏幕下的图像被压缩，减少像素取样（只是位图含像素的四分之一），这样就造成了资源浪费。同时把这个过程称为＂Downsampled＂。</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-8.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>但在Retina屏幕下，相同的图像会使用四倍的物理像素显示，这样一来，每个物理像素最终完全匹配一位图像素，使用图像得到完全的呈现。</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-9.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc;width:560px"></p>
<p>有几种方法可以实现这样的效果。</p>
<p><strong>1、使用HTML</strong></p>
<p>最简单的方法就是通过“img”标签，设置“width”和“height”属性：</p>
<pre>
&lt;img src=&quot;example@2x.png&quot; width=&quot;200&quot; height=&quot;300&quot; /&gt;
</pre><p>这里注意了，即使指定的图片高度是可选的，但是在加载图片前，浏览器已经预留了所需的空间。这样就可以防址页面布局更改图片时，在加载一次。</p>
<p><strong>２、使用JavaScript</strong></p>
<p>同样的效果，我们可以通过JavaScript脚本对图像（为Retina屏幕准备的图像）进行减半。</p>
<pre>
$(window).load(function() {
  var images = $('img');
  images.each(function(i) {
    $(this).width($(this).width() / 2);
  });
});
</pre><p><strong>３、使用CSS</strong></p>
<p>另外一种方法就是通过CSS来实现。那么常见的方法就是给元素设置一个背景图像，比如说给“div”元素设置一个背景图像，最关键的是给这个背景图像设置＂background-size＂，来指定背景图像的大小，你也可以给元素设置一个大小，然后给＂background-size＂设置一个“contain”属性值。不过可惜的是ＩＥ７８不支持这个属性的运用。</p>
<pre>
.image {
  background-image: url(example@2x.png);
  background-size: 200px 300px;
  /*或者设置background-size: contain; */
  height: 300px;
  width: 200px;
}
</pre><p>你还可以使用元素的伪元素来代替</p>
<pre>
.image-container:before {
  background-image: url(example@2x.png);
  background-size: 200px 300px;
  content:'';
  display: block;
  height: 300px;
  width: 200px;
}
</pre><p>上面通过三种方法来实现，那么来看看他们的对比：</p>
<p><strong>HTML和CSS方法的优点</strong></p>
<ol>
<li>很容易实现</li>
<li>能跨浏览器兼容</li>
</ol>
<p><strong>HTML和CSS方法的缺点</strong></p>
<ol>
<li>非Retina屏幕下必须下载更大的图片资源</li>
<li>Downsampled图像在不同的分辨下可能会失去一定的清晰度</li>
<li>background-size在IE９以下浏览器不能得到友好支持</li>
</ol>
<p>来看一个<a href="http://www.zhangxinxu.com">张旭鑫</a>大师的经验之谈《<a href="http://www.zhangxinxu.com/wordpress/2012/10/new-pad-retina-devicepixelratio-css-page">视网膜New iPad与普通分辨率iPad页面的兼容处理</a>》。</p>
<p>有一张200x200像素的图片（CSS像素，也就是普通像素点或者说是标准像素点），我们给图片设置一个CSS样式：</p>
<pre>
img {
  width:200px;
  height:200px;
}
</pre><p>在iPad2或Mini iPad中就是很正常显示的图片；但是，在New iPad中，1个CSS像素点实际上有4个位图像素点，1个分成4个，显然不够分啊，只能颜色近似选取，于是，图片感觉就是模糊的。</p>
<p>因此，要想让视网膜屏幕下的图片高清晰显示，我们需要的图片的原始大小不能是200×200像素，而需要2倍高宽，即400×400像素，CSS像素限制依然是:</p>
<pre>
img {
  width:200px;
  height:200px;
}
</pre><p>此时，视网膜屏幕下图片就显示OK了（非视网膜屏幕图片被压缩-减少像素取样——资源浪费！）。</p>
<h4>查询像素密度</h4>
<p>为Retina屏幕下查询Web图像的像素密度，然后调用对应的图像。目前较为流行的方式是通过CSS或者JavaScript来实现。</p>
<p><strong>1 、使用CSS Media Queries</strong></p>
<p>可以通过“device-pixel-ratio”属性或者其扩展属性“min-device-pixel-ratio”和“max-device-pixel-ratio”。这几个Media Queries可以使用background-image为Retina准备高精密度像素的图片。</p>
<pre>
.icon {
  background-image: url(example.png);
  background-size: 200px 300px;
  height: 300px;
  width: 200px;
}

@media only screen and (-Webkit-min-device-pixel-ratio: 1.5),
only screen and (-moz-min-device-pixel-ratio: 1.5),
only screen and (-o-min-device-pixel-ratio: 3/2),
only screen and (min-device-pixel-ratio: 1.5) {
  .icon {
    background-image: url(example@2x.png);
  }
}
</pre><p>通过“window.devicePixelRatio”的比例“1.5”或"2"为不同的苹果设备做相对应的查询。</p>
<p><strong>iPhone4</strong></p>
<pre>
@media only screen and (-webkit-min-device-pixel-ratio : 1.5),only screen and (min-device-pixel-ratio : 1.5) {
	/* Styles */
}
</pre><p><strong>Retian屏幕和普通屏幕</strong></p>
<pre>
@media only screen and (min-width: 320px) {

  /* Small screen, non-retina */

}

@media
only screen and (-webkit-min-device-pixel-ratio: 2)      and (min-width: 320px),
only screen and (   min--moz-device-pixel-ratio: 2)      and (min-width: 320px),
only screen and (     -o-min-device-pixel-ratio: 2/1)    and (min-width: 320px),
only screen and (        min-device-pixel-ratio: 2)      and (min-width: 320px),
only screen and (                min-resolution: 192dpi) and (min-width: 320px),
only screen and (                min-resolution: 2dppx)  and (min-width: 320px) { 

  /* Small screen, retina, stuff to override above media query */

}

@media only screen and (min-width: 700px) {

  /* Medium screen, non-retina */

}

@media
only screen and (-webkit-min-device-pixel-ratio: 2)      and (min-width: 700px),
only screen and (   min--moz-device-pixel-ratio: 2)      and (min-width: 700px),
only screen and (     -o-min-device-pixel-ratio: 2/1)    and (min-width: 700px),
only screen and (        min-device-pixel-ratio: 2)      and (min-width: 700px),
only screen and (                min-resolution: 192dpi) and (min-width: 700px),
only screen and (                min-resolution: 2dppx)  and (min-width: 700px) { 

  /* Medium screen, retina, stuff to override above media query */

}

@media only screen and (min-width: 1300px) {

  /* Large screen, non-retina */

}

@media
only screen and (-webkit-min-device-pixel-ratio: 2)      and (min-width: 1300px),
only screen and (   min--moz-device-pixel-ratio: 2)      and (min-width: 1300px),
only screen and (     -o-min-device-pixel-ratio: 2/1)    and (min-width: 1300px),
only screen and (        min-device-pixel-ratio: 2)      and (min-width: 1300px),
only screen and (                min-resolution: 192dpi) and (min-width: 1300px),
only screen and (                min-resolution: 2dppx)  and (min-width: 1300px) { 

  /* Large screen, retina, stuff to override above media query */

}
</pre><p>当实没怎么搞懂上面的代码，今天可算是有明白了。更多的media queries可以点击<a href="http://www.w3cplus.com/css3/css3-media-queries-for-different-devices">CSS3 Media Queries模板</a></p>
<p><strong>CSS Media Queries的优点</strong></p>
<ol>
<li>只有对应的目标元素才会下载图片资源</li>
<li>跨浏览器兼容</li>
<li>像素可以精确控制</li>
</ol>
<p><strong>CSS Media Queries的缺点</strong></p>
<ol>
<li>单调无味的实现过程，特别是大型项目中</li>
<li>只能通过HTML元素的背景图片来实现，无任何语义化可言</li>
</ol>
<p><strong>2、使用JavaScript</strong></p>
<p>使用js对“window.devicePixelRatio”进行判断，然后根据对应的值给Retina屏幕选择图像。</p>
<pre>
$(document).ready(function(){
  if (window.devicePixelRatio &gt; 1) {
    var lowresImages = $(&#39;img&#39;);

    images.each(function(i) {
      var lowres = $(this).attr(&#39;src&#39;);
      var highres = lowres.replace(&quot;.&quot;, &quot;@2x.&quot;);
      $(this).attr(&#39;src&#39;, highres);
    });
  }
});
</pre><p>其中<a href="http://retinajs.com">Retina.js</a>是为Retina而生的，基本上实现了上面的所有功能。</p>
<p>前面也说过了，devicePixelRatio目前支持的<a href="http://www.quirksmode.org/blog/archives/2012/06/devicepixelrati.html#link2">浏览器并不多</a>，但将来会有更多的浏览器支持这一技术。</p>
<p><strong>Javascript查询的优点</strong></p>
<ol>
<li>易于实施</li>
<li>非Retina屏幕不用下载过大的资源</li>
<li>像素精确控制</li>
</ol>
<p><strong>Javascript查询的缺点</strong></p>
<ol>
<li>Retina屏幕下必须下载标准备和高精密度的两个资源</li>
<li>Retina屏幕下图像交互可见</li>
<li>浏览器兼容性不强</li>
</ol>
<h4>可缩放矢量图形</h4>
<p>不管使用什么方法，光栅衅像仍然有自己固定的位图分辨率，也就是其缩放始终受限于其像素，也绝对无法无限制的伸缩。但是矢量图就不一样，他可以随意的进行伸缩，而无任何影响。这就是在Retina屏幕下的Web图形，矢量图形具有无法可比的优势。</p>
<p>到目前为止，基于XML的svg格式制作的矢量图得到了70&amp;以上的浏览器支持，可以使用svg绘制各种图形，并且可以在Retina下任意的伸缩。</p>
<p>做为web设计人员，使用SVG最简单的方法是通过HTML的img标签、CSS的background属性或者伪元素的content中的url()。</p>
<p><strong>HTML的img标签调用svg</strong></p>
<pre>
&lt;img src=&quot;example.svg&quot; width=&quot;200&quot; height=&quot;300&quot; /&gt;
</pre><p>在这里一个svg图像可以做为普通显屏和Retina显屏的通用图像，可以做任何的伸缩，而不会像光栅位图一样失真，而且资源统一，节省带宽，方便维护。</p>
<p><strong>CSS中调用svg图像</strong></p>
<p>svg图像可以像普通图像一样，当作元素的背景图片来应用</p>
<pre>
.image {
  background-image: url(example.svg);
  background-size: 200px 300px;
  height: 200px;
  width: 300px;
}
</pre><p>除了当成元素的背景图片使用之外，还可以通过伪元素的“content”来调用</p>
<pre>
.image-container:before {
  content: url(example.svg);
}
</pre><p>如果你想在IE7-8和Android2.x上运用，那么使用使用png图片来代替svg图像</p>
<pre>
.image {
  background-image: url(example.png);
  background-size: 200px 300px;
}

.svg {
  .image {
    background-image: url(example.svg);
  }
}
</pre><p>在HTML标签中，给img标签自定义一个属性，给这个自定义属性设置一个png图片，以做备用，不过这种方法需要一定的脚本配合使用。</p>
<pre>
&lt;img src=&quot;example.svg&quot; data-png-fallback=&quot;example.png&quot; /&gt;
</pre><p>需要的脚本</p>
<pre>
$(document).ready(function(){
  if(!Modernizr.svg) {
    var images = $('img[data-png-fallback]');
    images.each(function(i) {
      $(this).attr('src', $(this).data('png-fallback'));
    });
  }
});
</pre><p><strong>SVG矢量图的优点</strong></p>
<ol>
<li>一个资源适合所有设备</li>
<li>易于维护</li>
<li>面向未来的:可伸缩向量图形</li>
</ol>
<p><strong>SVG矢量图的缺点</strong></p>
<ol>
<li>没有像素那样有精度</li>
<li>由于文件大小，不适合复杂的图形</li>
<li>不支持IE7-8和早期的安卓版本</li>
</ol>
<p>零零散散的了解了Retina是个什么东东？那么有些同学肯会问，做为前端人员，如何做，网站才能适应Retina时代的产品呢？</p>
<p><a rel="external nofollow" href="http://mir.aculo.us/2012/06/26/flowchart-how-to-retinafy-your-website/">mir.aculo.us</a>的信息图会告诉你如何让网站适应Retina分辨率</p>
<p><img src="http://www.w3cplus.com/sites/default/files/styles/print_image/public/blogs/201212/retina-web-10.jpg" alt="Retina web" style="padding:2px;border:1px solid #ccc"></p>
<p>此图由<a href="http://www.nangongruiyang.com">南宮瑞揚</a>根据<a rel="external nofollow" href="http://mir.aculo.us/2012/06/26/flowchart-how-to-retinafy-your-website/">mir.aculo.us</a>的信息图所译。有关于更详细的介绍请点击<a href="http://www.nangongruiyang.com/index.php/webdesign/retinahtml.html">这里</a></p>
<h4>总结</h4>
<p>Retina对于我来说也是一个新课题，边看边做笔记，但还是没有完全理解清楚。还需努力。花了几个晚上根据<a rel="author" href="http://coding.smashingmagazine.com/author/reda-lemeden/?rel=author" title="Posts by Reda Lemeden">Reda Lemeden</a>的《<a href="http://coding.smashingmagazine.com/2012/08/20/towards-retina-web/">Towards A Retina Web</a>》、<a href="http://www.zhangxinxu.com">张旭鑫</a>的《<a href="http://www.zhangxinxu.com/wordpress/2012/10/new-pad-retina-devicepixelratio-css-page/">视网膜New iPad与普通分辨率iPad页面的兼容处理</a>》以及<a href="http://www.nangongruiyang.com">南宮瑞揚</a>的《<a href="http://www.nangongruiyang.com/index.php/webdesign/retinahtml.html">retina视网膜时代的页面</a>》做了这篇笔记。希望对初次接触的朋友有所帮助。如果有说错的地方，还请同行朋友指正。谢谢。</p>
<p>如需转载，烦请注明出处：<a href="http://www.w3cplus.com/css/towards-retina-web.html">http://www.w3cplus.com/css/towards-retina-web.html</a></p>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blog/tags/8.html">css</a></li></ul></div><div><ul><li><a href="http://www.w3cplus.com/blog/tags/286.html">Retina</a></li></ul></div><div><div><div><div>
      <div>11</div>
                  <a href="http://www.w3cplus.com/vote/node/560/1/vote/alternate/i041hdkC-_OzAQkGTOBdLm9MP0w0OGfydaUsKvHBC5A/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><img src="http://www1.feedsky.com/t1/700892607/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/css/towards-retina-web.html" border="0" height="0" width="0">