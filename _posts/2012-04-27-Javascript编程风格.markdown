---
layout: post
title:  "Javascript编程风格"
date:   2012-04-27 18:30:18
author: 阮一峰
categories: program
---

## Javascript编程风格
### by 阮一峰
### at 2012-04-27 18:30:18
### original <http://www.udpwork.com/item/7189.html>

<p><a href="http://www.crockford.com/">Douglas Crockford</a>是Javascript权威，<a href="http://www.ruanyifeng.com/blog/2009/05/data_types_and_json.html">Json格式</a>就是他的发明。</p>
<p>去年11月他有一个演讲（<a href="http://www.youtube.com/watch?v=taaEzHI9xyY">Youtube</a>），谈到了好的Javascript编程风格是什么。</p>
<p><a href="http://www.youtube.com/watch?v=taaEzHI9xyY"><img src="http://image.beekka.com/blog/201204/bg2012042701.jpg"></a></p>
<p>我非常推荐这个演讲，它不仅有助于学习Javascript，而且能让你心情舒畅，因为Crockford讲得很幽默，时不时让听众会心一笑。</p>
<p>下面，我根据这个演讲和Crockford编写的<a href="http://javascript.crockford.com/code.html">代码规范</a>，总结一下&quot;Javascript编程风格&quot;。</p>
<p><img src="http://image.beekka.com/blog/201204/bg2012042702.jpg"></p>
<p><strong>所谓&quot;编程风格&quot;（programming style），指的是编写代码的样式规则。</strong>
不同的程序员，往往有不同的编程风格。</p>
<p>有人说，编译器的规范叫做&quot;语法规则&quot;（grammar），这是程序员必须遵守的；而编译器忽略的部分，就叫&quot;编程风格&quot;（programming style），这是程序员可以自由选择的。这种说法不完全正确，<strong>程序员固然可以自由选择编程风格，但是好的编程风格有助于写出质量更高、错误更少、更易于维护的程序。</strong>
</p>
<p>所以，有一点应该明确，<strong>&quot;编程风格&quot;的选择不应该基于个人爱好、熟悉程度、打字工作量等因素，而要考虑如何尽量使代码清晰易读、减少出错。你选择的，不是你喜欢的风格，而是一种能够清晰表达你的意图的风格。</strong>
这一点，对于Javascript这种语法自由度很高、设计<a href="http://www.ruanyifeng.com/blog/2010/01/12_javascript_syntax_structures_you_should_not_use.html">不完全成熟</a>的语言尤其重要。</p>
<p><img src="http://image.beekka.com/blog/201204/bg2012042704.jpg"></p>
<p><strong>一、大括号的位置</strong>
</p>
<p>绝大多数的编程语言，都用大括号（{}）表示区块（block）。起首的大括号的位置，有许多不同的<a href="http://en.wikipedia.org/wiki/Indent_style">写法</a>。</p>
<p>最流行的有两种。一种是起首的大括号另起一行：</p>
<p>　　block</p>
<p>　　{</p>
<p>　　　　...</p>
<p>　　}</p>
<p>另一种是起首的大括号跟在关键字的后面：</p>
<p>　　block {</p>
<p>　　　　...</p>
<p>　　}</p>
<p>一般来说，这两种写法都可以接受。但是，Javascript要使用后一种，因为Javascript会自动添加句末的分号，导致一些难以察觉的错误。</p>
<p>　　return</p>
<p>　　{</p>
<p>　　　　key:value;</p>
<p>　　};</p>
<p>上面的代码的原意，是要返回一个对象，但实际上返回的是undefined，因为Javascript自动在return语句后面添加了分号。为了避免这一类错误，需要写成下面这样：</p>
<p>　　return {</p>
<p>　　　　key : value;</p>
<p>　　};</p>
<p>因此，</p>
<p>　　<strong>规则1：表示区块起首的大括号，不要另起一行。</strong>
</p>
<p><strong>二、 圆括号</strong>
</p>
<p>圆括号（parentheses）在Javascript中有两种作用，一种表示调用函数，另一种表示不同的值的组合（grouping）。我们可以用空格，区分这两种不同的括号。</p>
<p>　　<strong>规则2：调用函数的时候，函数名与左括号之间没有空格。</strong>
</p>
<p>　　<strong>规则3：函数名与参数序列之间，没有空格。</strong>
</p>
<p>　　<strong>规则4：所有其他语法元素与左括号之间，都有一个空格。</strong>
</p>
<p>按照上面的规则，下面的写法都是不规范的：</p>
<p>　　foo (bar)</p>
<p>　　return(a+b);</p>
<p>　　if(a === 0) {...}</p>
<p>　　function foo (b) {...}</p>
<p>　　function(x) {...}</p>
<p><img src="http://image.beekka.com/blog/201204/bg2012042705.jpg"></p>
<p><strong>三、分号</strong>
</p>
<p>分号表示语句的结束。大多数情况下，如果你省略了句尾的分号，Javascript会自动添加。</p>
<p>　　var a = 1</p>
<p>等同于</p>
<p>　　var a = 1;</p>
<p>因此，有人<a href="http://mislav.uniqpath.com/2010/05/semicolons/">提倡</a>省略句尾的分号。但麻烦的是，如果下一行的第一个字元（token）是下面这五个字符之一，Javascript将不对上一行句尾<a href="http://inimino.org/~inimino/blog/javascript_semicolons">添加分号</a>：&quot;(&quot;、&quot;[&quot;、&quot;/&quot;、&quot;+&quot;和&quot;-&quot;。</p>
<p>　　x = y</p>
<p>　　(function (){</p>
<p>　　　　...</p>
<p>　　})();</p>
<p>上面的代码等同于</p>
<p>　　x = y(function (){...})();</p>
<p>因此，</p>
<p>　　<strong>规则5：不要省略句末的分号。</strong>
</p>
<p><strong>四、with语句</strong>
</p>
<p>with可以减少代码的书写，但是会造成混淆。</p>
<p>　　with (o) {</p>
<p>　　　　foo = bar;</p>
<p>　　}</p>
<p>上面的代码，可以有四种运行结果：</p>
<p>　　o.foo = bar;</p>
<p>　　o.foo = o.bar;</p>
<p>　　foo = bar;</p>
<p>　　foo = o.bar;</p>
<p>这四种结果都可能发生，取决于不同的变量是否有定义。因此，</p>
<p>　　<strong>规则6：不要使用with语句。</strong>
</p>
<p><img src="http://image.beekka.com/blog/201204/bg2012042706.jpg"></p>
<p><strong>五、相等和严格相等</strong>
</p>
<p>Javascript有两个表示&quot;相等&quot;的运算符：&quot;相等&quot;（==）和&quot;严格相等&quot;（===）。</p>
<p>因为&quot;相等&quot;运算符会自动转换变量类型，造成很多意想不到的<a href="http://www.2ality.com/2011/06/javascript-equality.html">情况</a>：</p>
<p>　　0 == ''// true</p>
<p>　　1 == true // true</p>
<p>　　2 == true // false</p>
<p>　　0 == '0' // true</p>
<p>　　false == 'false' // false</p>
<p>　　false == '0' // true</p>
<p>　　&quot; \t\r\n &quot; == 0 // true</p>
<p>因此，</p>
<p>　　<strong>规则7：不要使用&quot;相等&quot;（==）运算符，只使用&quot;严格相等&quot;（===）运算符。</strong>
</p>
<p><strong>六、语句的合并</strong>
</p>
<p>有些程序员追求简洁，喜欢合并不同目的的语句。比如，原来的语句是</p>
<p>　　a = b;</p>
<p>　　if (a) {...}</p>
<p>他喜欢写成下面这样:</p>
<p>　　if (a = b) {...}</p>
<p>虽然语句少了一行，但是可读性大打折扣，而且会造成误读，让别人误以为这行代码的意思是：</p>
<p>　　if （a === b）{...}</p>
<p>另外一种情况是，有些程序员喜欢在同一行中赋值多个变量：</p>
<p>　　var a = b = 0;</p>
<p>他以为，这行代码等同于</p>
<p>　　var a = 0, b = 0;</p>
<p>实际上不是，它的真正效果是下面这样：</p>
<p>　　b = 0;
<br>

<br>
　　var a = b;</p>
<p>因此，</p>
<p>　　<strong>规则8：不要将不同目的的语句，合并成一行。</strong>
</p>
<p><img src="http://image.beekka.com/blog/201204/bg2012042707.jpg"></p>
<p><strong>七、变量声明</strong>
</p>
<p>Javascript会自动将变量声明&quot;提升&quot;（hoist）到代码块（block）的头部。</p>
<p>　　if (!o) {</p>
<p>　　　　var o = {};</p>
<p>　　}</p>
<p>等同于</p>
<p>　　var o;</p>
<p>　　if (!o) {</p>
<p>　　　　o = {};</p>
<p>　　}</p>
<p>为了避免可能出现的问题，不如把变量声明都放在代码块的头部。</p>
<p>　　for (var i ...) {...}</p>
<p>最好写成：</p>
<p>　　var i;
<br>

<br>
　　for (i ...) {...,}</p>
<p>因此，</p>
<p>　　<strong>规则9：所有变量声明都放在函数的头部。</strong>
</p>
<p>　　<strong>规则10：所有函数都在使用之前定义。</strong>
</p>
<p><strong>八、全局变量</strong>
</p>
<p>Javascript最大的语法缺点，可能就是全局变量对于任何一个代码块，都是可读可写。这对代码的模块化和重复使用，非常不利。</p>
<p>　　<strong>规则11：避免使用全局变量；如果不得不使用，用大写字母表示变量名，比如UPPER_CASE。</strong>
</p>
<p><img src="http://image.beekka.com/blog/201204/bg2012042708.jpg"></p>
<p><strong>九、new命令</strong>
</p>
<p>Javascript使用new命令，从建构函数生成一个新对象。</p>
<p>　　var o = new myObject();</p>
<p>这种做法的问题是，一旦你忘了加上new，myObject()内部的this关键字就会指向全局对象，导致所有绑定在this上面的变量，都变成全部变量。</p>
<p>　　<strong>规则12：不要使用new命令，改用<a href="https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Object/create">Object.create()</a>命令。</strong>
</p>
<p>如果不得不使用new，为了防止出错，最好在视觉上把建构函数与其他函数区分开来。</p>
<p>　　<strong>规则13：建构函数的函数名，采用首字母大写（InitialCap）；其他函数名，一律首字母小写。</strong>
</p>
<p><strong>十、自增和自减运算符</strong>
</p>
<p>自增（++）和自减（--）运算符，放在变量的前面或后面，返回的值不一样，很容易发生错误。</p>
<p>事实上，所有的++运算符都可以用&quot;+= 1&quot;代替。</p>
<p>　　++x</p>
<p>等同于</p>
<p>　　x += 1;</p>
<p>代码变得更清晰了。有一个很可笑的例子，某个Javascript函数库的源代码中出现了下面的片段：</p>
<p>　　++x;</p>
<p>　　++x;</p>
<p>这个程序员忘了，还有更简单、更合理的写法：</p>
<p>　　x += 2;</p>
<p>因此，</p>
<p>　　<strong>规则14：不要使用自增（++）和自减（--）运算符，用+=和-=代替。</strong>
</p>
<p><strong>十一、区块</strong>
</p>
<p>如果循环和判断的代码体只有一行，Javascript允许该区块（block）省略大括号。</p>
<p>下面的代码</p>
<p>　　if (a) b(); c();</p>
<p>原意可能是</p>
<p>　　if (a) { b(); c();}</p>
<p>但是，实际效果是</p>
<p>　　if (a) { b();} c();</p>
<p>因此，</p>
<p>　　<strong>规则15：总是使用大括号表示区块。</strong>
</p>
<p>（完）</p>
<div><h3>文档信息</h3>
<ul><li>版权声明：自由转载-非商用-非衍生-保持署名 |<a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2012/04/javascript_programming_style.html">http://www.ruanyifeng.com/blog/2012/04/javascript_programming_style.html</a></li>
<li>最后修改时间：2012年5月 2日 18:00</li>
<li>付费支持：<a href="https://mai.alipay.com/p.htm?id=2012033102826278"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" alt="支付宝担保交易"></a>|<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=2.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="paypal"></a></li>
</ul>
</div>
<div><p><a href="http://yipinzao.com/"><img src="http://www.ruanyifeng.com/blog/images/ad_yipinzao.jpg"></a></p>
</div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7189.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7189.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7189.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>