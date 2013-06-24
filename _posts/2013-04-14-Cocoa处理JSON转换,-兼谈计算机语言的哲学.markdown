---
layout: post
title:  "Cocoa处理JSON转换, 兼谈计算机语言的哲学"
date:   2013-04-14 15:51:29
author: ideawu
categories: program
---

## Cocoa处理JSON转换, 兼谈计算机语言的哲学
### by ideawu
### at 2013-04-14 15:51:29
### original <http://www.udpwork.com/item/9671.html>

<p>用了下 Objective-C Cocoa 里的 JSON 相关类 NSJSONSerialization, 发现简单的事情突然变得太复杂了. 想想用 php 语言的时候, 如果想把 php 语言对象转成字符串, 直接 json_encode(); 如果想把网络或者文件中的一段二进制数据(JSON 字符串)转成 php 对象, 直接 json_decode().</p>
<p>但是, 在 Cocoa 里就不那么直接了. 首先, 你会遇到字符编码的问题, 所以你得到的和操作的不能是字符串, 而是 NSData. 其次, NSJSONSerialization 无谓地增加了限制, 顶层 Objective-C 对象只能是数组和字典.</p>
<p>我不想探讨这里面的原因, 我当然知道这里面的原因, 我只是说, 这些原因可以避免.</p>
<p>第一, 为解决后一个限制, 我可以做一些封装, 支持语言的整数对象和字符串对象(非常重要)直接转成 JSON 字符串, 而忽略所谓的 NSData(后面讲为什么 NSData 不是一个好东西).</p>
<p>第二, 函数名应该简洁. 我是 Cocoa 的我不喜欢的”哲学”之一, 所有的东西都一致的. 但这种一致性不是朝简单方向一致, 而是像某某主义把所有人都均贫一样, 这种一致的结果是所有函数命名都非常繁琐.</p>
<p>第三, 计算机语言的对象模型. 我们知道 Java 语言有几千上万个类和对象, Objective-C Cocoa 也同样有上万个类和对象. 当然, php 语言也有许多类和对象. 但为什么我一直推崇php并认为php是非常简单的呢? 因为 php的核心对象就是这几种: 数字, 字符串, 数组和表(操作顺序排序的关联数组). 这几种核心数据结构可以和 JSON 的对象模型一一对应.</p>
<h2>理想的计算机语言的数据模型</h2>
<h3>二进制数据(字节数组)即是字符串</h3>
<p>给定一个字符串(一段数据), 如果你认为它是 ASCII 字符串, 那么你就用 ASCII 字符串的函数操作它. 如果你认为它是宽字符集(如UTF-8)的字符串, 那么你就用相应的函数来操作它. 如果你认为它表示GBK文本, 那么你就按GBK文本的方法来处理它. 如果你认为它表示一张图片, 那么就用图片处理的方法来操作它… 一份数据, 不同表述. 数据是死的, 方法才是活的. 同样的一段数据, 用不同的方法来操作, 它就有了不同的生命.</p>
<h3>映射表(map, 字典, 关联数组, 哈希表)应该是空间序的</h3>
<p>“空间序”比较直接的印象就是数组, 下标0号元素在下标1号元素的前面. 而字典没有下标, 但它有操作序列, 往一个表中插入数据, 如果先插入1再插入2, 那么1应该排在2的前面. 如果先插入2再插入1, 那么2应该排在1的前面. 而许多语言的核心字典类, 因为性能和实现的原因, 顺序是不可预期的(如哈希表), 或者是其它顺序的如字典序, 其实. “空间有序”才是最有用的. php 中的关联数组是”空间有序”的, 所以可以按不同的顺序模型来排序, 但 Cocoa, Java 等核心的字典不是. 所以, 对于存储了学生和考试分数的一个字典表, php可以在这个字典表上按分数进行排序, 而后者要引入辅助的数据结构.<strong>显然, php的API对用户更友好.</strong>
</p>
<h3>简单哲学: 常用的东西无成本, 复杂的东西有成本</h3>
<p>比如在 JavaScript 和 PHP 中, 操作数组是非常频繁的操作, 所以这方面的语法非常简洁有效:</p>
<pre>
// JavaScript:
arr = [1, 2];
arr.push(3);
// PHP
$arr = array(1, 2);
$arr[] = 3;
// Objective-C
arr = [NSArray arrayWithObjects: [NSNumber numberWithInt:1], [NSNumber numberWithInt:2], nil];
[arr addObject: [NSNumber numberWithInt:3]];
</pre><p>如果是建造原子弹, 你可以设计得非常复杂. 但如果只是平常呼吸一口空气, 这就过度了.</p>
<h3>借鉴优秀的现有例子</h3>
<p>如果要设计一门语言, 我的建议是:</p>
<ul><li>使用类C语法</li>
<li>使用JavaScript数据模型, 但字典(关联数组)的模型应该是空间有序的</li>
<li>使用PHP/C语言的字节数组(字符串)模型</li>
<li>如果是面向函数的语言, 核心函数命名参考PHP的命名</li>
<li>如果是”面向类”的语言, 核心类库命名参考JavaScript</li>
</ul>
<p>最后, 如果有使用 Objective-C Cocoa 的朋友, 希望大家将常用的函数封装成简单命名的类和方法, 贡献到这个项目中:<a href="https://github.com/ideawu/soc">https://github.com/ideawu/soc</a>.</p>
<h3>Related posts:</h3>
<ol><li><a href="http://www.ideawu.net/blog/archives/534.html" title="Permanent Link: C语言解析JSON">C语言解析JSON</a></li>
<li><a href="http://www.ideawu.net/blog/archives/338.html" title="Permanent Link: Prado 中解决 Ajax 中文乱码问题">Prado 中解决 Ajax 中文乱码问题</a></li>
<li><a href="http://www.ideawu.net/blog/archives/701.html" title="Permanent Link: Cocoa 应用检测 ESC 按键">Cocoa 应用检测 ESC 按键</a></li>
<li><a href="http://www.ideawu.net/blog/archives/661.html" title="Permanent Link: 几种极其隐蔽的XSS注入的防护">几种极其隐蔽的XSS注入的防护</a></li>
<li><a href="http://www.ideawu.net/blog/archives/647.html" title="Permanent Link: 用C语法来写Python代码">用C语法来写Python代码</a></li>
</ol>
<div><p><img src="http://www.ideawu.net/favicon.ico">你现在看的文章是:<a href="http://www.ideawu.net/blog/archives/704.html" title="Cocoa处理JSON转换, 兼谈计算机语言的哲学">Cocoa处理JSON转换, 兼谈计算机语言的哲学</a></p>
<div><a href="http://www.benegg.com/linode-ad.php">Linode VPS - 美国虚拟主机</a>|<a href="http://www.udpwork.com/">IT牛人博客聚合网站</a></div>
</div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/9671.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">1</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/9671.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">udpwork.com</a> 聚合
					|
					<a href="http://www.udpwork.com/item/9671.html#reviews">评论: 1</a>
					|
					<a href="http://www.jikenow.com/">要! 要! 即刻! Now!</a>
				</p>
			</div>