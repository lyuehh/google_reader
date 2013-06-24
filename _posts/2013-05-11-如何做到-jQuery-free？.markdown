---
layout: post
title:  "如何做到 jQuery-free？"
date:   2013-05-11 19:59:41
author: 阮一峰
categories: program
---

## 如何做到 jQuery-free？
### by 阮一峰
### at 2013-05-11 19:59:41
### original <http://www.udpwork.com/item/9821.html>

<p><a href="http://jquery.com/">jQuery</a>是现在最流行的JavaScript工具库。</p>
<p>据<a href="http://w3techs.com/technologies/details/js-jquery/all/all">统计</a>，目前全世界57.3%的网站使用它。也就是说，10个网站里面，有6个使用jQuery。如果只考察使用工具库的网站，这个比例就会上升到惊人的91.7%。</p>
<p><img src="http://image.beekka.com/blog/201305/bg2013051101.jpg"></p>
<p>虽然jQuery如此受欢迎，但是它臃肿的<a href="http://mathiasbynens.be/demo/jquery-size">体积</a>也让人头痛不已。jQuery 2.0的原始大小为235KB，优化后为81KB；如果是支持IE6、7、8的jQuery 1.8.3，原始大小为261KB，优化后为91KB。</p>
<p>这样的体积，即使是宽带环境，完全加载也需要1秒或更长，更不要说移动设备了。这意味着，如果你使用了jQuery，用户至少延迟1秒，才能看到网页效果。考虑到本质上，jQuery只是一个操作DOM的工具，我们不仅要问：如果只是为了几个网页特效，是否有必要动用这么大的库？</p>
<p><img src="http://image.beekka.com/blog/201305/bg2013051102.jpg"></p>
<p>2006年，jQuery诞生的时候，主要用于消除不同浏览器的差异（主要是IE6），为开发者提供一个简洁的统一接口。相比当时，<a href="http://en.wikipedia.org/wiki/Usage_share_of_web_browsers">如今的情况</a>已经发生了很大的变化。IE的市场份额不断下降，以ECMAScript为基础的JavaScript标准语法，正得到越来越广泛的支持。开发者直接使用JavScript标准语法，就能同时在各大浏览器运行，不再需要通过jQuery获取兼容性。</p>
<p>下面就探讨如何用JavaScript标准语法，取代jQuery的一些主要功能，做到jQuery-free。</p>
<p><img src="http://image.beekka.com/blog/201305/bg2013051103.jpg"></p>
<p><strong>一、选取DOM元素</strong>
</p>
<p>jQuery的核心是通过各种选择器，选中DOM元素，可以用querySelectorAll方法模拟这个功能。</p>
<blockquote><p>　　var $ = document.querySelectorAll.bind(document);</p>
</blockquote>
<p>这里需要注意的是，querySelectorAll方法返回的是NodeList对象，它很像数组（有数字索引和length属性），但不是数组，不能使用pop、push等数组特有方法。如果有需要，可以考虑将Nodelist对象转为数组。</p>
<blockquote><p>　　myList = Array.prototype.slice.call(myNodeList);</p>
</blockquote>
<p><strong>二、DOM操作</strong>
</p>
<p>DOM本身就具有很丰富的操作方法，可以取代jQuery提供的操作方法。</p>
<p>尾部追加DOM元素。</p>
<blockquote><p>　　// jQuery写法
<br>
　　$(parent).append($(child));</p>
<p>　　// DOM写法
<br>
　　parent.appendChild(child)</p>
</blockquote>
<p>头部插入DOM元素。</p>
<blockquote><p>　　// jQuery写法
<br>
　　$(parent).prepend($(child));</p>
<p>　　// DOM写法
<br>
　　parent.insertBefore(child, parent.childNodes[0])</p>
</blockquote>
<p>删除DOM元素。</p>
<blockquote><p>　　// jQuery写法
<br>
　　$(child).remove()</p>
<p>　　// DOM写法
<br>
　　child.parentNode.removeChild(child)</p>
</blockquote>
<p><strong>三、事件的监听</strong>
</p>
<p>jQuery的on方法，完全可以用addEventListener模拟。</p>
<blockquote><p>　　Element.prototype.on = Element.prototype.addEventListener;</p>
</blockquote>
<p>为了使用方便，可以在NodeList对象上也部署这个方法。</p>
<blockquote><p>　　NodeList.prototype.on = function (event, fn) {
<br>
　　　　[]['forEach'].call(this, function (el) {
<br>
　　　　　　el.on(event, fn);
<br>
　　　　});
<br>
　　　　return this;
<br>
　　};</p>
</blockquote>
<p><strong>四、事件的触发</strong>
</p>
<p>jQuery的trigger方法则需要单独部署，相对复杂一些。</p>
<blockquote><p>　　Element.prototype.trigger = function (type, data) {
<br>
　　　　var event = document.createEvent('HTMLEvents');
<br>
　　　　event.initEvent(type, true, true);
<br>
　　　　event.data = data || {};
<br>
　　　　event.eventName = type;
<br>
　　　　event.target = this;
<br>
　　　　this.dispatchEvent(event);
<br>
　　　　return this;
<br>
　　};</p>
</blockquote>
<p>在NodeList对象上也部署这个方法。</p>
<blockquote><p>　　NodeList.prototype.trigger = function (event) {
<br>
　　　　[]['forEach'].call(this, function (el) {
<br>
　　　　　　el['trigger'](event);
<br>
　　　　});
<br>
　　　　return this;
<br>
　　};</p>
</blockquote>
<p><strong>五、document.ready</strong>
</p>
<p>目前的最佳实践，是将JavaScript脚本文件都放在页面底部加载。这样的话，其实document.ready方法（jQuery简写为$(function)）已经不必要了，因为等到运行的时候，DOM对象已经生成了。</p>
<p><strong>六、attr方法</strong>
</p>
<p>jQuery使用attr方法，读写网页元素的属性。</p>
<blockquote><p>　　$(&quot;#picture&quot;).attr(&quot;src&quot;, &quot;http://url/to/image&quot;);</p>
</blockquote>
<p>DOM元素允许直接读取属性值，写法要简洁许多。</p>
<blockquote><p>　　$(&quot;#picture&quot;).src = &quot;http://url/to/image&quot;;</p>
</blockquote>
<p>需要注意，input元素的this.value返回的是输入框中的值，链接元素的this.href返回的是绝对URL。如果需要用到这两个网页元素的属性准确值，可以用this.getAttribute('value')和this.getAttibute('href')。</p>
<p><strong>七、addClass方法</strong>
</p>
<p>jQuery的addClass方法，用于为DOM元素添加一个class。</p>
<blockquote><p>　　$('body').addClass('hasJS');</p>
</blockquote>
<p>DOM元素本身有一个可读写的className属性，可以用来操作class。</p>
<blockquote><p>　　document.body.className = 'hasJS';</p>
<p>　　// or</p>
<p>　　document.body.className += ' hasJS';</p>
</blockquote>
<p>HTML 5还提供一个classList对象，功能更强大（IE 9不支持）。</p>
<blockquote><p>　　document.body.classList.add('hasJS');</p>
<p>　　document.body.classList.remove('hasJS');</p>
<p>　　document.body.classList.toggle('hasJS');</p>
<p>　　document.body.classList.contains('hasJS');</p>
</blockquote>
<p><strong>八、CSS</strong>
</p>
<p>jQuery的css方法，用来设置网页元素的样式。</p>
<blockquote><p>　　$(node).css( &quot;color&quot;, &quot;red&quot; );</p>
</blockquote>
<p>DOM元素有一个style属性，可以直接操作。</p>
<blockquote><p>　　element.style.color = &quot;red&quot;;;</p>
<p>　　// or</p>
<p>　　element.style.cssText += 'color:red';</p>
</blockquote>
<p><strong>九、数据储存</strong>
</p>
<p>jQuery对象可以储存数据。</p>
<blockquote><p>　　$(&quot;body&quot;).data(&quot;foo&quot;, 52);</p>
</blockquote>
<p>HTML 5有一个dataset对象，也有类似的功能（IE 10不支持），不过只能保存字符串。</p>
<blockquote><p>　　element.dataset.user = JSON.stringify(user);</p>
<p>　　element.dataset.score = score;</p>
</blockquote>
<p><strong>十、Ajax</strong>
</p>
<p>jQuery的Ajax方法，用于异步操作。</p>
<blockquote><p>　　$.ajax({
<br>
　　　　type: &quot;POST&quot;,
<br>
　　　　url: &quot;some.php&quot;,
<br>
　　　　data: { name: &quot;John&quot;, location: &quot;Boston&quot; }
<br>
　　}).done(function( msg ) {
<br>
　　　　alert( &quot;Data Saved: &quot; + msg );
<br>
　　});</p>
</blockquote>
<p>我们可以定义一个request函数，模拟Ajax方法。</p>
<blockquote><p>　　function request(type, url, opts, callback) {</p>
<p>　　　　var xhr = new XMLHttpRequest();</p>
<p>　　　　if (typeof opts === 'function') {
<br>
　　　　　　callback = opts;
<br>
　　　　　　opts = null;
<br>
　　　　}</p>
<p>　　　　xhr.open(type, url);</p>
<p>　　　　var fd = new FormData();</p>
<p>　　　　if (type === &#39;POST&#39; &amp;&amp; opts) {
<br>
　　　　　　for (var key in opts) {
<br>
　　　　　　　　fd.append(key, JSON.stringify(opts[key]));
<br>
　　　　　　}
<br>
　　　　}</p>
<p>　　　　xhr.onload = function () {
<br>
　　　　　　callback(JSON.parse(xhr.response));
<br>
　　　　};</p>
<p>　　　　xhr.send(opts ? fd : null);</p>
<p>　　}</p>
</blockquote>
<p>然后，基于request函数，模拟jQuery的get和post方法。</p>
<blockquote><p>　　var get = request.bind(this, 'GET');</p>
<p>　　var post = request.bind(this, 'POST');</p>
</blockquote>
<p><strong>十一、动画</strong>
</p>
<p>jQuery的animate方法，用于生成动画效果。</p>
<blockquote><p>　　$foo.animate('slow', { x: '+=10px' })；</p>
</blockquote>
<p>jQuery的动画效果，很大部分基于DOM。但是目前，CSS 3的动画远比DOM强大，所以可以把动画效果写进CSS，然后通过操作DOM元素的class，来展示动画。</p>
<blockquote><p>　　foo.classList.add('animate')；</p>
</blockquote>
<p>如果需要对动画使用回调函数，CSS 3也定义了相应的事件。</p>
<blockquote><p>　　el.addEventListener(&quot;webkitTransitionEnd&quot;, transitionEnded);</p>
<p>　　el.addEventListener(&quot;transitionend&quot;, transitionEnded);</p>
</blockquote>
<p><strong>十二、替代方案</strong>
</p>
<p>由于jQuery体积过大，替代方案层出不穷。</p>
<p>其中，最有名的是<a href="http://zeptojs.com/">zepto.js</a>。它的设计目标是以最小的体积，做到最大兼容jQuery的API。zepto.js 1.0版的原始大小是55KB，优化后是29KB，gzip压缩后为10KB。</p>
<p>如果不求最大兼容，只希望模拟jQuery的基本功能，那么，<a href="https://github.com/remy/min.js">min.js</a>优化后只有200字节，而<a href="https://github.com/lelandrichardson/dolla">dolla</a>优化后是1.7KB。</p>
<p>此外，jQuery本身采用模块设计，可以只选择使用自己需要的模块。具体做法参见它的<a href="https://github.com/jquery/jquery">github网站</a>，或者使用专用的<a href="http://projects.jga.me/jquery-builder/">Web界面</a>。</p>
<p><strong>十三、参考链接</strong>
</p>
<p>　　- Remy Sharp，<a href="http://remysharp.com/2013/04/19/i-know-jquery-now-what/">I know jQuery. Now what?</a>
<br>
　　- Hemanth.HM，<a href="http://h3manth.com/new/blog/2013/power-of-vanilla-js/">Power of Vanilla JS</a>
<br>
　　- Burke Holland，<a href="http://flippinawesome.org/2013/05/06/5-things-you-should-stop-doing-with-jquery/">5 Things You Should Stop Doing With jQuery</a></p>
<p>（完）
<br>
</p>
<div><h3>文档信息</h3>
<ul><li>版权声明：自由转载-非商用-非衍生-保持署名 |<a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2013/05/jquery-free.html">http://www.ruanyifeng.com/blog/2013/05/jquery-free.html</a></li>
<li>最后修改时间：2013年5月14日 13:24</li>
<li>付费支持：<a href="https://me.alipay.com/ruanyf"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" alt="人民币 - 支付宝"></a>|<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=0.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="美元 - paypal"></a></li>
</ul>
</div>
<div><p><strong>[广告]</strong>
　<a href="http://www.hi-vps.com/?utm_source=ruanyifeng.com">www.hi-vps.com创建于2010年，致力于提供更适合国人使用，性价比更高的VPS。我们的支付方式为支付宝，并且提供7天无理由退款服务。各种其他使用指南，请参考我们的WIKI。</a></p>
</div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/9821.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/9821.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">udpwork.com</a> 聚合
					|
					<a href="http://www.udpwork.com/item/9821.html#reviews">评论: 0</a>
					|
					<a href="http://www.jikenow.com/">要! 要! 即刻! Now!</a>
				</p>
			</div>