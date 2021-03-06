---
layout: post
title:  "自适应网页设计（Responsive Web Design）"
date:   2012-05-01 14:57:01
author: 阮一峰
categories: program
---

## 自适应网页设计（Responsive Web Design）
### by 阮一峰
### at 2012-05-01 14:57:01
### original <http://www.udpwork.com/item/7198.html>

<p>随着3G的普及，越来越多的人使用手机上网。</p>
<p>移动设备正超过桌面设备，成为访问互联网的最常见终端。于是，网页设计师不得不面对一个难题：如何才能在不同大小的设备上呈现同样的网页？</p>
<p><img src="http://image.beekka.com/blog/201205/bg2012050101.jpg"></p>
<p>手机的屏幕比较小，宽度通常在600像素以下；PC的屏幕宽度，一般都在1000像素以上（目前主流宽度是1366×768），有的还达到了2000像素。同样的内容，要在大小迥异的屏幕上，都呈现出满意的效果，并不是一件容易的事。</p>
<p>很多网站的解决方法，是为不同的设备提供不同的网页，比如专门提供一个mobile版本，或者iPhone / iPad版本。这样做固然保证了效果，但是比较麻烦，同时要维护好几个版本，而且如果一个网站有多个portal（入口），会大大增加架构设计的复杂度。</p>
<p>于是，很早就有人设想，能不能&quot;一次设计，普遍适用&quot;，让同一张网页自动适应不同大小的屏幕，根据屏幕宽度，自动调整布局（layout）？</p>
<p><img src="http://image.beekka.com/blog/201205/bg2012050107.jpg"></p>
<p><strong>一、&quot;自适应网页设计&quot;的概念</strong>
</p>
<p>2010年，Ethan Marcotte提出了<a href="http://www.alistapart.com/articles/responsive-web-design/">&quot;自适应网页设计&quot;</a>（Responsive Web Design）这个名词，指可以自动识别屏幕宽度、并做出相应调整的网页设计。</p>
<p>他制作了一个<a href="http://www.alistapart.com/d/responsive-web-design/ex/ex-site-flexible.html">范例</a>，里面是《福尔摩斯历险记》六个主人公的头像。如果屏幕宽度大于1300像素，则6张图片并排在一行。</p>
<p><img src="http://image.beekka.com/blog/201205/bg2012050103.jpg"></p>
<p>如果屏幕宽度在600像素到1300像素之间，则6张图片分成两行。</p>
<p><img src="http://image.beekka.com/blog/201205/bg2012050104.jpg"></p>
<p>如果屏幕宽度在400像素到600像素之间，则导航栏移到网页头部。</p>
<p><img src="http://image.beekka.com/blog/201205/bg2012050105.jpg"></p>
<p>如果屏幕宽度在400像素以下，则6张图片分成三行。</p>
<p><img src="http://image.beekka.com/blog/201205/bg2012050106.jpg"></p>
<p><a href="http://mediaqueri.es/">mediaqueri.es</a>上面有更多这样的例子。</p>
<p>这里还有一个<a href="http://www.benjaminkeen.com/misc/bricss/">测试小工具</a>，可以在一张网页上，同时显示不同分辨率屏幕的测试效果，我推荐安装。</p>
<p><strong>二、允许网页宽度自动调整</strong>
</p>
<p>&quot;自适应网页设计&quot;到底是怎么做到的？其实并不难。</p>
<p>首先，在网页代码的头部，加入一行<a href="https://developer.mozilla.org/en/mobile/viewport_meta_tag">viewport元标签</a>。</p>
<p>　　&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot; /&gt;</p>
<p><a href="https://developer.apple.com/library/ios/#DOCUMENTATION/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html">viewport</a>是网页默认的宽度和高度，上面这行代码的意思是，网页宽度默认等于屏幕宽度（width=device-width），原始缩放比例（initial-scale=1）为1.0，即网页初始大小占屏幕面积的100%。</p>
<p>所有主流浏览器都支持这个设置，包括IE9。对于那些老式浏览器（主要是IE6、7、8），需要使用<a href="http://code.google.com/p/css3-mediaqueries-js/">css3-mediaqueries.js</a>。</p>
<p>　　&lt;!--[if lt IE 9]&gt;
<br>
　　　　&lt;script src=&quot;http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js&quot;&gt;&lt;/script&gt;
<br>
　　&lt;![endif]--&gt;</p>
<p><strong>三、不使用绝对宽度</strong>
</p>
<p>由于网页会根据屏幕宽度调整布局，所以不能使用绝对宽度的布局，也不能使用具有绝对宽度的元素。这一条非常重要。</p>
<p>具体说，CSS代码不能指定像素宽度：</p>
<p>　　width:xxx px;</p>
<p>只能指定百分比宽度：</p>
<p>　　width: xx%;</p>
<p>或者</p>
<p>　　width:auto;</p>
<p><strong>四、相对大小的字体</strong>
</p>
<p>字体也不能使用绝对大小（px），而只能使用相对大小（em）。</p>
<p>　　body {
<br>
　　　　font: normal 100% Helvetica, Arial, sans-serif;
<br>
　　}</p>
<p>上面的代码指定，字体大小是页面默认大小的100%，即16像素。</p>
<p>　　h1 {
<br>
　　　　font-size: 1.5em;
<br>
　　}</p>
<p>然后，h1的大小是默认大小的1.5倍，即24像素（24/16=1.5）。</p>
<p>　　small {
<br>
　　　　font-size: 0.875em;
<br>
　　}</p>
<p>small元素的大小是默认大小的0.875倍，即14像素（14/16=0.875）。</p>
<p><strong>五、流动布局（fluid grid）</strong>
</p>
<p><a href="http://www.alistapart.com/articles/fluidgrids/">&quot;流动布局&quot;</a>的含义是，各个区块的位置都是浮动的，不是固定不变的。</p>
<p>　　.main {
<br>
　　　　float: right;
<br>
　　　　width: 70%;
<br>
　　}</p>
<p>　　.leftBar {
<br>
　　　　float: left;
<br>
　　　　width: 25%;
<br>
　　}</p>
<p><a href="http://designshack.net/articles/css/everything-you-never-knew-about-css-floats/">float</a>的好处是，如果宽度太小，放不下两个元素，后面的元素会自动滚动到前面元素的下方，不会在水平方向overflow（溢出），避免了水平滚动条的出现。</p>
<p>另外，绝对定位（position: absolute）的使用，也要非常小心。</p>
<p><strong>六、选择加载CSS</strong>
</p>
<p>&quot;自适应网页设计&quot;的核心，就是CSS3引入的<a href="http://www.w3.org/TR/CSS21/media.html">Media Query</a>模块。</p>
<p>它的意思就是，自动探测屏幕宽度，然后加载相应的CSS文件。</p>
<p>　　&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot;
<br>
　　　　media=&quot;screen and (max-device-width: 400px)&quot;
<br>
　　　　href=&quot;tinyScreen.css&quot; /&gt;</p>
<p>上面的代码意思是，如果屏幕宽度小于400像素（max-device-width: 400px），就加载tinyScreen.css文件。</p>
<p>　　&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot;
<br>
　　　　media=&quot;screen and (min-width: 400px) and (max-device-width: 600px)&quot;
<br>
　　　　href=&quot;smallScreen.css&quot; /&gt;</p>
<p>如果屏幕宽度在400像素到600像素之间，则加载smallScreen.css文件。</p>
<p>除了用html标签加载CSS文件，还可以在现有CSS文件中加载。</p>
<p>　　@import url(&quot;tinyScreen.css&quot;) screen and (max-device-width: 400px);</p>
<p><strong>七、CSS的@media规则</strong>
</p>
<p>同一个CSS文件中，也可以根据不同的屏幕分辨率，选择应用不同的CSS规则。</p>
<p>　　@media screen and (max-device-width: 400px) {</p>
<p>　　　　.column {
<br>
　　　　　　float: none;
<br>
　　　　　　width:auto;
<br>
　　　　}</p>
<p>　　　　#sidebar {
<br>
　　　　　　display:none;
<br>
　　　　}</p>
<p>　　}</p>
<p>上面的代码意思是，如果屏幕宽度小于400像素，则column块取消浮动（float:none）、宽度自动调节（width:auto），sidebar块不显示（display:none）。</p>
<p><strong>八、图片的自适应（fluid image）</strong>
</p>
<p>除了布局和文本，&quot;自适应网页设计&quot;还必须实现图片的<a href="http://unstoppablerobotninja.com/entry/fluid-images">自动缩放</a>。</p>
<p>这只要一行CSS代码：</p>
<p>　　img { max-width: 100%;}</p>
<p>这行代码对于大多数嵌入网页的视频也有效，所以可以写成：</p>
<p>　　img, object { max-width: 100%;}</p>
<p>老版本的IE不支持max-width，所以只好写成：</p>
<p>　　img {  width: 100%; }</p>
<p>此外，windows平台缩放图片时，可能出现图像失真现象。这时，可以尝试使用IE的<a href="http://css-tricks.com/ie-fix-bicubic-scaling-for-images/">专有命令</a>：</p>
<p>　　img { -ms-interpolation-mode: bicubic; }</p>
<p>或者，Ethan Marcotte的<a href="http://unstoppablerobotninja.com/demos/resize/imgSizer.js">imgSizer.js</a>。</p>
<p>　　addLoadEvent(function() {</p>
<p>　　　　var imgs = document.getElementById(&quot;content&quot;).getElementsByTagName(&quot;img&quot;);</p>
<p>　　　　imgSizer.collate(imgs);</p>
<p>　　});</p>
<p>不过，有条件的话，最好还是根据不同大小的屏幕，加载不同分辨率的图片。有<a href="http://blog.cloudfour.com/responsive-imgs-part-2/">很多方法</a>可以做到这一条，服务器端和客户端都可以实现。</p>
<p>（完）</p>
<div><h3>文档信息</h3>
<ul><li>版权声明：自由转载-非商用-非衍生-保持署名 |<a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2012/05/responsive_web_design.html">http://www.ruanyifeng.com/blog/2012/05/responsive_web_design.html</a></li>
<li>最后修改时间：2012年5月 7日 10:53</li>
<li>付费支持：<a href="https://mai.alipay.com/p.htm?id=2012033102826278"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" alt="支付宝担保交易"></a>|<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=2.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="paypal"></a></li>
</ul>
</div>
<div><p><a href="http://yipinzao.com/"><img src="http://www.ruanyifeng.com/blog/images/ad_yipinzao.jpg"></a></p>
</div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7198.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7198.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7198.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>