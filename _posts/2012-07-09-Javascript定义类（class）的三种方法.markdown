---
layout: post
title:  "Javascript定义类（class）的三种方法"
date:   2012-07-09 23:58:15
author: 阮一峰
categories: program
---

## Javascript定义类（class）的三种方法
### by 阮一峰
### at 2012-07-09 23:58:15
### original <http://www.udpwork.com/item/7674.html>

<p>将近20年前，<a href="http://www.ruanyifeng.com/blog/2011/06/birth_of_javascript.html">Javascript诞生</a>的时候，只是一种简单的网页脚本语言。如果你忘了填写用户名，它就跳出一个警告。</p>
<p><img src="http://image.beekka.com/blog/201207/bg2012070901.png"></p>
<p>如今，它变得几乎无所不能，从前端到<a href="http://nodejs.org/">后端</a>，有着各种<a href="http://www.netmagazine.com/features/10-things-you-didnt-know-javascript-could-do">匪夷所思的用途</a>。程序员用它完成越来越庞大的项目。</p>
<p>Javascript代码的复杂度也直线上升。单个网页包含10000行Javascript代码，早就司空见惯。2010年，一个工程师<a href="http://googlesystem.blogspot.tw/2010/06/gmail-to-use-more-html5-features.html">透露</a>，Gmail的代码长度是443000行！</p>
<p><img src="http://image.beekka.com/blog/201207/bg2012070902.png"></p>
<p>编写和维护如此复杂的代码，必须使用<a href="http://en.wikipedia.org/wiki/Modular_design">模块化策略</a>。目前，业界的主流做法是采用&quot;面向对象编程&quot;。因此，Javascript如何实现面向对象编程，就成了一个热门课题。</p>
<p>麻烦的是，Javascipt语法不支持&quot;类&quot;（class），导致传统的面向对象编程方法无法直接使用。程序员们做了很多探索，研究如何用Javascript模拟&quot;类&quot;。本文总结了Javascript定义&quot;类&quot;的三种方法，讨论了每种方法的特点，着重介绍了我眼中的最佳方法。</p>
<p>==============================================</p>
<p><strong>Javascript定义类（class）的三种方法</strong>
</p>
<p>作者：阮一峰</p>
<p><img src="http://image.beekka.com/blog/201207/bg2012070903.jpg"></p>
<p>在面向对象编程中，类（class）是对象（object）的模板，定义了同一组对象（又称&quot;实例&quot;）共有的属性和方法。</p>
<p>Javascript语言不支持&quot;类&quot;，但是可以用一些变通的方法，模拟出&quot;类&quot;。</p>
<p><strong>一、构造函数法</strong>
</p>
<p>这是经典方法，也是教科书必教的方法。它用构造函数模拟&quot;类&quot;，在其内部用this关键字指代实例对象。</p>
<p>　　function Cat() {</p>
<p>　　　　this.name = &quot;大毛&quot;;</p>
<p>　　}</p>
<p>生成实例的时候，使用new关键字。</p>
<p>　　var cat1 = new Cat();</p>
<p>　　alert(cat1.name); // 大毛</p>
<p>类的属性和方法，还可以定义在构造函数的prototype对象之上。</p>
<p>　　Cat.prototype.makeSound = function(){</p>
<p>　　　　alert(&quot;喵喵喵&quot;);</p>
<p>　　}</p>
<p>关于这种方法的详细介绍，请看我写的系列文章<a href="http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html">《Javascript 面向对象编程》</a>，这里就不多说了。它的主要缺点是，比较复杂，用到了this和prototype，编写和阅读都很费力。</p>
<p><strong>二、Object.create()法</strong>
</p>
<p>为了解决&quot;构造函数法&quot;的缺点，更方便地生成对象，Javascript的国际标准<a href="http://en.wikipedia.org/wiki/ECMAScript">ECMAScript</a>第五版（目前通行的是第三版），提出了一个新的方法<a href="https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Object/create/">Object.create()</a>。</p>
<p>用这个方法，&quot;类&quot;就是一个对象，不是函数。</p>
<p>　　var Cat = {</p>
<p>　　　　name: &quot;大毛&quot;,</p>
<p>　　　　makeSound: function(){ alert(&quot;喵喵喵&quot;); }</p>
<p>　　};</p>
<p>然后，直接用Object.create()生成实例，不需要用到new。</p>
<p>　　var cat1 = Object.create(Cat);</p>
<p>　　alert(cat1.name); // 大毛</p>
<p>　　cat1.makeSound(); // 喵喵喵</p>
<p>目前，各大浏览器的最新版本（包括IE9）都部署了这个方法。如果遇到老式浏览器，可以用下面的代码自行部署。</p>
<p>　　if (!Object.create) {</p>
<p>　　　　Object.create = function (o) {</p>
<p>　　　　　　 function F() {}</p>
<p>　　　　　　F.prototype = o;</p>
<p>　　　　　　return new F();</p>
<p>　　　　};</p>
<p>　　}</p>
<p>这种方法比&quot;构造函数法&quot;简单，但是不能实现私有属性和私有方法，实例对象之间也不能共享数据，对&quot;类&quot;的模拟不够全面。</p>
<p><strong>三、极简主义法</strong>
</p>
<p>荷兰程序员Gabor de Mooij提出了一种比Object.create()更好的<a href="http://www.gabordemooij.com/articles/jsoop.html">新方法</a>，他称这种方法为&quot;极简主义法&quot;（minimalist approach）。这也是我推荐的方法。</p>
<p><strong>3.1 封装</strong>
</p>
<p>这种方法不使用this和prototype，代码部署起来非常简单，这大概也是它被叫做&quot;极简主义法&quot;的原因。</p>
<p>首先，它也是用一个对象模拟&quot;类&quot;。在这个类里面，定义一个构造函数createNew()，用来生成实例。</p>
<p>　　var Cat = {</p>
<p>　　　　createNew: function(){</p>
<p>　　　　　　// some code here</p>
<p>　　　　}</p>
<p>　　};</p>
<p>然后，在createNew()里面，定义一个实例对象，把这个实例对象作为返回值。</p>
<p>　　var Cat = {</p>
<p>　　　　createNew: function(){</p>
<p>　　　　　　var cat = {};</p>
<p>　　　　　　cat.name = &quot;大毛&quot;;</p>
<p>　　　　　　cat.makeSound = function(){ alert(&quot;喵喵喵&quot;); };</p>
<p>　　　　　　return cat;</p>
<p>　　　　}</p>
<p>　　};</p>
<p>使用的时候，调用createNew()方法，就可以得到实例对象。</p>
<p>　　var cat1 = Cat.createNew();</p>
<p>　　cat1.makeSound(); // 喵喵喵</p>
<p>这种方法的好处是，容易理解，结构清晰优雅，符合传统的&quot;面向对象编程&quot;的构造，因此可以方便地部署下面的特性。</p>
<p><strong>3.2 继承</strong>
</p>
<p>让一个类继承另一个类，实现起来很方便。只要在前者的createNew()方法中，调用后者的createNew()方法即可。</p>
<p>先定义一个Animal类。</p>
<p>　　var Animal = {</p>
<p>　　　　createNew: function(){</p>
<p>　　　　　　var animal = {};</p>
<p>　　　　　　animal.sleep = function(){ alert(&quot;睡懒觉&quot;); };</p>
<p>　　　　　　return animal;</p>
<p>　　　　}</p>
<p>　　};</p>
<p>然后，在Cat的createNew()方法中，调用Animal的createNew()方法。</p>
<p>　　var Cat = {</p>
<p>　　　　createNew: function(){</p>
<p>　　　　　　<strong>var cat = Animal.createNew();</strong>
</p>
<p>　　　　　　cat.name = &quot;大毛&quot;;</p>
<p>　　　　　　cat.makeSound = function(){ alert(&quot;喵喵喵&quot;); };</p>
<p>　　　　　　return cat;</p>
<p>　　　　}</p>
<p>　　};</p>
<p>这样得到的Cat实例，就会同时继承Cat类和Animal类。</p>
<p>　　var cat1 = Cat.createNew();</p>
<p>　　cat1.sleep(); // 睡懒觉</p>
<p><strong>3.3 私有属性和私有方法</strong>
</p>
<p>在createNew()方法中，只要不是定义在cat对象上的方法和属性，都是私有的。</p>
<p>　　var Cat = {</p>
<p>　　　　createNew: function(){</p>
<p>　　　　　　var cat = {};</p>
<p>　　　　　　<strong>var sound = &quot;喵喵喵&quot;;</strong>
</p>
<p>　　　　　　<strong>cat.makeSound = function(){ alert(sound); };</strong>
</p>
<p>　　　　　　return cat;</p>
<p>　　　　}</p>
<p>　　};</p>
<p>上例的内部变量sound，外部无法读取，只有通过cat的公有方法makeSound()来读取。</p>
<p>　　var cat1 = Cat.createNew();</p>
<p>　　<strong>alert(cat1.sound);</strong>
// undefined</p>
<p><strong>3.4 数据共享</strong>
</p>
<p>有时候，我们需要所有实例对象，能够读写同一项内部数据。这个时候，只要把这个内部数据，封装在类对象的里面、createNew()方法的外面即可。</p>
<p>　　var Cat = {</p>
<p>　　　　<strong>sound :  &quot;喵喵喵&quot;,</strong>
</p>
<p>　　　　createNew: function(){</p>
<p>　　　　　　var cat =  {};</p>
<p>　　　　　　<strong>cat.makeSound = function(){ alert(Cat.sound); };</strong>
</p>
<p>　　　　　　<strong>cat.changeSound = function(x){ Cat.sound =  x;  };</strong>
</p>
<p>　　　　　　return cat;</p>
<p>　　　　}</p>
<p>　　};</p>
<p>然后，生成两个实例对象：</p>
<p>　　var cat1 = Cat.createNew();</p>
<p>　　var cat2 = Cat.createNew();</p>
<p>　　cat1.makeSound(); // 喵喵喵</p>
<p>这时，如果有一个实例对象，修改了共享的数据，另一个实例对象也会受到影响。</p>
<p>　　<strong>cat2.changeSound(&quot;啦啦啦&quot;);</strong>
</p>
<p>　　cat1.makeSound(); // 啦啦啦</p>
<p>（完）</p>
<div><h3>文档信息</h3>
<ul><li>版权声明：自由转载-非商用-非衍生-保持署名 |<a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2012/07/three_ways_to_define_a_javascript_class.html">http://www.ruanyifeng.com/blog/2012/07/three_ways_to_define_a_javascript_class.html</a></li>
<li>最后修改时间：2012年7月12日 15:47</li>
<li>付费支持：<a href="https://me.alipay.com/ruanyf"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" alt="人民币 - 支付宝"></a>|<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=0.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="美元 - paypal"></a></li>
</ul>
</div>
<div></div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7674.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7674.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7674.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>