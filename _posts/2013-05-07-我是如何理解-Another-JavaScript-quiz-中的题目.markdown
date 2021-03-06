---
layout: post
title:  "我是如何理解”Another JavaScript quiz”中的题目"
date:   2013-05-07 18:02:14
author: 张 鑫旭
categories: program
---

## 我是如何理解”Another JavaScript quiz”中的题目
### by 张 鑫旭
### at 2013-05-07 18:02:14
### original <http://www.zhangxinxu.com/wordpress/2013/05/%e7%90%86%e8%a7%a3another-javascript-quiz-%e9%a2%98%e7%9b%ae/>

<p>by <a href="http://www.zhangxinxu.com/">zhangxinxu</a> from <a href="http://www.zhangxinxu.com/">http://www.zhangxinxu.com</a><br>
本文地址：<a href="http://www.zhangxinxu.com/wordpress/?p=3223">http://www.zhangxinxu.com/wordpress/?p=3223</a></p>

<h3>一、你这个变态，给我滚开</h3>
<p>时光冉冉，或多或少见过一些JavaScript相关的题目，其中很多属于变态级别的！各种奇怪符号写法拼在一起、尼玛还有兼容性问题，估计道格拉斯都不知道答案。</p>
<p>对于这种整得亲妈都不认识的变态问题，实际上是没有什么参考价值的。好比要考察外星人对人类的了解，结果你那下面这货来做测试，看到亲戚的外星人一定会云里雾里的，但有意义吗？<br>
<img src="http://image.zhangxinxu.com/image/blog/201305/biantai.jpg" width="180" height="194" alt="变态人类"></p>
<p>好的JavaScript测试题目应该是：门外汉见了皱眉，行家见了疑惑题目是不是简单了点，同时考察点覆盖全面。<span>//zxx: 我目前还没有这个能耐设计出如此优秀的题目。</span></p>
<p>这里要介绍的”<a href="http://james.padolsey.com/javascript/another-javascript-quiz/">Another JavaScript quiz</a>“(by james)中的题目不是属于变态题目，而是确实属于变态题目，不过是表面上的，很多内容确实可能会遇到的。综合评价下就是：面试价值不及格，学习价值是很赞的，因此，探讨分享很有意义。</p>
<p>“<a href="http://james.padolsey.com/javascript/another-javascript-quiz/">Another JavaScript quiz</a>“中有25个很简洁的JavaScript测试题，全部都是考察返回值的，例如<code>1 &amp;&amp; 3</code>的返回值是？</p>
<p>25道题目全部完成后，可以点击下图所示的按钮，检查你的正确率以及那些题目出错了。<br>
<img src="http://image.zhangxinxu.com/image/blog/201305/2013-05-02_174700.png" width="485" height="200" alt="检查答案按钮示意"></p>
<p>我自己快速测试了下，如果每题算4分的话，我的<a href="http://weibo.com/1263362863/ztn6GCI0Z" title="微博查看">成绩是56分</a>，不及格，可见自己对JS的学习以及理解还有一段路要走。</p>
<p>您也可以做一下这些题目，完成之后，若有疑惑，可以参考我下面纯个人的理解 – 部分理解可能不准确，欢迎指正。同时，网上应该有其他一些前辈的解释，您也可以参考参考。</p>
<h3>二、公子，不急，慢慢来嘛</h3>
<ol>
<li>
<div>
<pre>1 &amp;&amp; 3	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>3</code>. </p>
<p><code>&amp;&amp;</code>是几乎无人不知，表示“与”。男方要娶水灵灵的妹子，七大姑八大婆都要同意，否则继续锻炼右手吧。</p>
<p>上面<code>1 &amp;&amp; 3</code>本质上等同于<code>大姑妈 &amp;&amp; 三姑妈</code>。</p>
<p>从左往右，如果“大姑妈”通过，继续“三姑妈”；否则直接返回“大姑妈”。因为“三姑妈”后面没有其他亲戚了，因此直接返回“三姑妈”。</p>
<p>在JavaScript中， <code>1 == true // true</code>，因此，<code>1 &amp;&amp; 3</code>等同于“大姑妈”通过了，返回最后一个检测的亲戚“三姑妈”，也就是这里的结果<code>3</code>.</p>
<p>因此，实际上，我们平时的 <code>if (1 &amp;&amp; 3) {}</code> 等同于 <code>if (3) {}</code>.</p>
<p>如果这里的题目换成<code>0 &amp;&amp; 3</code>返回的是？那结果是如何呢？</p>
<p>因为<code>0 == false // true</code>, 因此走不到“三姑妈”这一关，直接返回了<code>0</code>. 也就是<code>if (0 &amp;&amp; 3) {}</code> 等同于 <code>if (0) { <span style="color:#aaa">/* 不会执行 */</span> }</code>.</p>
<p><strong>实用性</strong><br>
可以避免<code>if</code>嵌套。例如，要问页面上某个dom绑定点击事件，我们需要先判断这个dom元素存不存在，我们可能会这样做：</p>
<div>
<pre>var dom = document.querySelector("#dom");
if (dom) { dom.addEventListener("click", function() {});  }</pre>
</div>
<p>实际上，我们可以使用<code>&amp;&amp;</code>做一些简化：</p>
<div>
<pre>var dom = document.querySelector(&quot;#dom&quot;);
dom &amp;&amp; dom.addEventListener(&quot;click&quot;, function() {});</pre>
</div>
</li>
<li>
<div>
<pre>1 &amp;&amp; &quot;foo&quot; || 0	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>很简单的一道题，结果是<code>"foo"</code>. </p>
<p>这里出现了一个新的关系符<code>||</code>, 表示“或”的意思。男方要娶干巴巴的妹子，七大姑八大婆只要一个说好就可以了。比方说<code>1 || 3</code>表示：如果大姑妈说可以，则成了，返回大姑妈；如果大姑妈不允许，再看看三姑妈的意思。</p>
<p>因此，<code>if (1 || 3) {}</code>实际等同于<code>if (1) {}</code>; <code>if (0 || 3) {}</code> 等同于<code>if (3) {}</code>.</p>
<p><strong>实用性</strong><br>
<code>||</code>可以让我们使用一种更快捷简易的方式为参数添加默认值。例如，写jQuery插件的时候，可选参数是可以缺省的，此时实际上值为<code>undefined</code>，会让后面的参数<code>extend</code>产生困扰。因此，我们会经常见到类似这样的代码：</p>
<div>
<pre>$.fn.plugin = function(options) {
    options = options || {};
    <span style="color:graytext">// ...</span>
};</pre>
</div>
<p>显然，根据上面的理解，<code>1 &amp;&amp; &quot;foo&quot;</code>返回的是<code>"foo"</code>, 而<code>"foo" || 0</code>, 因<code>"foo" == true</code>为<code>true</code>, 自然返回是“大姑妈” – <code>"foo"</code>. 这就是最终结果<code>"foo"</code>的由来。</p></li>
<li>
<div>
<pre>1 || &quot;foo&quot; &amp;&amp; 0	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>1</code>.</p>
<p>据说逻辑与运算符(<code>&amp;&amp;</code>)优先级要大于逻辑或(<code>||</code>)。如果是这样，那这里的结果返回应该是这样子的：<code>1 || &quot;foo&quot; &amp;&amp; 0</code> → <code>1 || 0</code> → <code>1</code>.</p></li>
<li>
<div>
<pre>(1, 2, 3)	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>3</code>.</p>
<p>这里考察的是逗号运算符，也称多重求值。逗号运算符据说是优先级最低的运算符。<br>
一般表现形式是：大姑妈，二姑妈，三姑妈，……N姑妈。<br>
运算规则如下：折腾大姑妈，折腾二姑妈，折腾三姑妈，……折腾N姑妈，最后返回的N姑妈的返回值。</p>
<p>因此，这里<code>1, 2, 3</code>就是折腾了<code>1</code>，折腾了<code>2</code>，折腾了<code>3</code>并且返回了<code>3</code>的返回值。因此，结果是<code>3</code>.</p>
<p>好，现在提问：<code>alert(1, 2, 3);</code>的弹出值是？</p>
<p>结果是<code>3</code>……………………是不可能的！正确结果是<strong>1</strong>.</p>
<p>不要困扰。原因很简单，<code>alert</code>身后的<code>()</code>实际上也是一种运算符，这里指函数调用，优先级相当的高。这里，<code>1, 2, 3</code>已经被奴役成<code>alert</code>方法（弹出第一个参数值）的参数了，因此，弹出的是<code>1</code>. </p>
<p>如果是<code>alert((1, 2, 3));</code>则弹出的就是<code>3</code>了。</p>
<p><strong>实用性</strong><br>
举个很简单的例子，<code>i</code>, <code>j</code>两个变量同时递增，可能我们会这样写：</p>
<div>
<pre>
var i=1;
var j=1;
i+=1;
j+=2;
</pre>
</div>
<p>我们可以借助逗号运算符获得更好地阅读体验，至于性能是否有提高，我个人并不清楚，就算有，差异也是可以忽略的。</p>
<div>
<pre>
var i=1, j=1;
i+=1, j+=2;
</pre>
</div>
</li>
<li>
<div>
<pre>x = { shift: [].shift };
x.shift();
x.length;	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是IE6/IE7 <code>undefined</code>, 其他浏览器<code>0</code>. 不过，个人观点，IE6/IE7其实并不属于这个题目的考察浏览器。因此，我们着重讨论为何IE8+等浏览器为何结果是<code>0</code>.</p>
<p>首先，我们了解下数组的<code>shift</code>方法。 我个人一般把<code>pop/push</code>归为一对基友，移除尾部元素和尾部添加元素，都是做扫尾工作的。<code>shift/unshift</code>当作另外一对基友，移除头部元素和头部添加元素，都是在头部打点的。记住，<em>单词字符个数少的(<code>pop</code>, <code>shift</code>)都是做删除的</em>，这样记忆就不会混淆了。</p>
<p>从外表上看，<code>shift</code>方法移除数组中的第一个元素并返回这个元素。如果这个数组是个空数组，则返回<code>undefined</code>. 一般而言，<code>shift</code>操作会改变后面所有数组项在内存中的地址，因此，相比<code>pop</code>方法要慢得多。</p>
<p>从内在来看，<code>shift</code>还能给对象自动添加<code>length</code>属性。<del>Mozilla开发者中心(MDC)</del>Mozilla开发者网络(MDN – Mozilla Developer Network)的解释<a href="https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Array/shift">是这样子的</a>：</p>
<blockquote><p><code>shift</code> is intentionally generic; this method can be <code>called</code> or <code>applied</code> to objects resembling arrays. Objects which do not contain a length property reflecting the last in a series of consecutive, zero-based numerical properties may not behave in any meaningful manner.</p></blockquote>
<p>简体中文表示就是（这里的释义自己只有80%确认，若有不准确，请极力指证<img src="http://mat1.gtimg.com/www/mb/images/face/30.gif">）：<br>
<img src="http://image.zhangxinxu.com/image/blog/201305/dahuangya.jpg" width="119" height="120" alt="大黄鸭"><br>
<blockquote><code>shift</code>可以有意泛化（变身成鸭子）；该方法可以被类数组对象<code>call</code>或者<code>apply</code>. 对象如果没有<code>length</code>属性，可能会以无意义的方式在最后反射一系列连续，基于<code>0</code>的数值属性。</blockquote></p>
<p>什么意思呢？我们看下面这个更容易理解的问题：</p>
<div>
<pre>x = {};
[].shift.call(x);
x.length;	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>0</code>. <span>//zxx: IE6/IE7 <code>undefined</code></span>.</p>
<p><code>x</code>对象原本木有<code>length</code>属性，在被数组<code>shift</code>方法<code>call</code>后，添加了一个值为<code>0</code>的<code>length</code>属性。此为数组<code>shift</code>方法的泛化性，专业术语为<em>泛型(generic)</em>。其基友方法，例如<code>pop</code>, <code>push</code>等都是如此。</p>
<p>OK, 回到原题，实际上，<code>x.shift()</code>的调用等同于<code>[].shift.call(x)</code>, 不明白？看下面的一步一步分析。</p>
<div>
<pre>var x = {
    shift: function() {
        console.log(this === x); <span style="color:green">// true</span>
    }
};
x.shift();</pre>
</div>
<p>因此，在
<div>
<pre>x = { shift: [].shift }</pre>
</div>
<p>条件下，<br>
<code>x.shift</code>执行的就是<code>[].shift</code>的执行，只不过，<code>[].shift()</code>函数中的<code>this</code>上下文就是<code>x</code>(因为<code>this===x</code>)，就等同于直接的<code>[].shift.call(x)</code>调用。</p>
<p>这条题目中<code>x</code>对象的<code>shift</code>属性名实际上是用来干扰，提高解答难度的刻意命名。我们使用其他命名，结果也是一样的。</p>
<div>
<pre>x = { shit: [].shift };
x.shit();
x.length;	<span style="color:green">// 0</span></pre>
</div>
</p></li>
<li>
<div>
<pre>{foo:1}[0]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>[0]</code>, 或者这种表现形式<code>0 { 0 : 0 }</code> – 来自IE控制台.</p>
<p>不要试图使用<code>alert</code>或者控制台<code>console.log</code>输出，这只会返回不一样的结果<code>undefined</code>，哦？为何会有这等差异？</p>
<p>出题者james在”<a href="http://james.padolsey.com/javascript/labelled-blocks-useful/">Labelled blocks, useful?</a>“中有这样的解释：</p>
<blockquote><p>Since JavaScript doesn’t have block scope, using a block anywhere other than in the conventional places (if/while etc.) is almost totally pointless. However, as I mentioned, we could use them to annotate and contain logically related pieces of code…</p></blockquote>
<p>意思是说：</p>
<blockquote><p>因为JavaScript没有块作用域，所以，如果语句块不是常规使用，如<code>if/while</code>等，其几乎就是打酱油的。甚至，我们可以利用这个特性注释或者包含相关的逻辑片段代码…</p></blockquote>
<p>我们有必要好好理解这里“打酱油的”意思，这里的“打酱油”并不是指<code>{}</code>块中语句是打酱油，而是其本身就是个酱油。嘛意思，实例说明一切：</p>
<div>
<pre>2 <span style="color:graytext">// 返回值为<span style="color:green">2</span></span></pre>
</div>
<div>
<pre>{2} <span style="color:graytext">// 返回值为<span style="color:green">2</span></span></pre>
</div>
<div>
<pre>str = "string" <span style="color:graytext">// 返回值为<span style="color:green">string</span></span></pre>
</div>
<div>
<pre>{ str = "string" } <span style="color:graytext">// 返回值为<span style="color:green">string</span></span></pre>
</div>
<div>
<pre>foo: 1 <span style="color:graytext">// 返回值为<span style="color:green">1</span></span></pre>
</div>
<div>
<pre>{ foo: 1 } <span style="color:graytext">// 返回值为<span style="color:green">1</span></span></pre>
</div>
<p>也就是说花括号几乎就是皇帝的新衣。因此，这里的答案就不难理解了，<code>{foo:1}[0]</code>实际上就是<code>foo:1; [0]</code>. 返回的就是<code>[0]</code>本身。</p>
<p>注意这里反复出现的措辞“几乎”。“几乎酱油”的潜台词是有时候还能顶个臭皮匠。james举了个在块中使用<code>break</code>语句的例子，如下：</p>
<div>
<pre>var x = 1; 
foo: {
    x = 2;
    break foo;
    x = 3;
}
x === 2; <span style="color:green">// true</span></pre>
</div>
<p>非这种情况的<code>break/continue</code>只能在<code>switch</code>语句以及循环中使用。很有意思吧，我反正是学习了<img src="http://mat1.gtimg.com/www/mb/images/face/30.gif">！
</p></li>
<li>
<div>
<pre>[true, false][+true, +false]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>true</code>.</p>
<p>首先，我们了解下<code>+true</code>和<code>+false</code>是个什么东西。</p>
<p>想必都清楚<code>+1</code>与<code>-1</code>是什么东东。指的是<code>正数1</code>与<code>负数1</code>. 同时，稍微对JS有了解的人也清楚，<code>true == 1</code>, <code>false == 0</code>. 因此，实际上，<code>[+true, +false]</code>就是<code>[+1, +0]</code>.</p>
<p><code>[true, false]</code>为数组，后面的<code>[+true, +false]</code>实际为索引，然而索引只需要一个值，因此，<code>[+true, +false]</code>返回的实际是我们上面提到的逗号运算——返回最后一个值，也就是<code>+0</code>, 也就是<code>0</code>. </p>
<p>因此，本题的问题其实是：</p>
<div>
<pre>[true, false][0]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>旺财估计都已经明白了，就不多说了。
</p></li>
<li>
<div>
<pre>++'52'.split('')[0]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是<code>6</code>.</p>
<p>此题难点在于运算符的优先级，看来有必要把运算符优先级都展示下。参见下表（数据取自<a href="http://blog.meituo.net/2010/08/29/%E4%B8%8D%E5%8F%AF%E4%B8%8D%E7%9F%A5%E7%9A%84javascript%E8%BF%90%E7%AE%97%E7%AC%A6%E4%BC%98%E5%85%88%E7%BA%A7/">美拓blog</a>）：</p>
<table width="100%" border="0" cellspacing="1">
<tr>
<th> </th>
<th>运算符</th>
<th>描述</th>
</tr>
<tr>
<th>1</th>
<td>. [] ()</td>
<td>字段访问、数组下标、函数调用以及表达式分组</td>
</tr>
<tr>
<th>2</th>
<td>++ — – ~ ! delete new typeof void</td>
<td>一元运算符、返回数据类型、对象创建、未定义值</td>
</tr>
<tr>
<th>3</th>
<td>* / %</td>
<td>乘法、除法、取模</td>
</tr>
<tr>
<th>4</th>
<td>+ – +</td>
<td>加法、减法、字符串连接</td>
</tr>
<tr>
<th>5</th>
<td>&lt; &lt; &gt;&gt; &gt;&gt;&gt;</td>
<td>移位</td>
</tr>
<tr>
<th>6</th>
<td>&lt; &lt;= &gt; &gt;= instanceof</td>
<td>小于、小于等于、大于、大于等于、instanceof</td>
</tr>
<tr>
<th>7</th>
<td>== != === !==</td>
<td>等于、不等于、严格相等、非严格相等</td>
</tr>
<tr>
<th>8</th>
<td>&amp;</td>
<td>按位与</td>
</tr>
<tr>
<th>9</th>
<td>^</td>
<td>按位异或</td>
</tr>
<tr>
<th>10</th>
<td>|</td>
<td>按位或</td>
</tr>
<tr>
<th>11</th>
<td>&amp;&amp;</td>
<td>逻辑与</td>
</tr>
<tr>
<th>12</th>
<td>||</td>
<td>逻辑或</td>
</tr>
<tr>
<th>13</th>
<td>?:</td>
<td>条件</td>
</tr>
<tr>
<th>14</th>
<td>= += -=</td>
<td>赋值、运算赋值</td>
</tr>
<tr>
<th>15</th>
<td>,</td>
<td>多重求值</td>
</tr>
</table>
<p>从上表可以看出点<code>.</code>以及<code>[]</code>要比<code>++</code>的优先级高。因此，这里的问题等同于：</p>
<div>
<pre>
var result;
result = "52".split("");	<span style="color:green">// ["5", "2"]</span>
result = result[0];	<span style="color:green">// "5"</span>
result = ++result; 	<span style="color:green">// 6</span>
</pre>
</div>
<p><strong>注意：</strong><br>
虽然说<code>++["5", "2"][0]</code>的返回值是<code>6</code>, 但是直接<code>++"5"</code>或者<code>++5</code>却是会报错的（FireBug控制台显示<cite>“invalid increment operand”</cite>错误），据说是因为只有变量才能<code>++</code> 而<code>'5'</code>只是一个字符串。
</p></li>
<li>
<div>
<pre>a: b: c: d: e: f: g: 1, 2, 3, 4, 5;	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>5</code>.</p>
<p>这里需要讲下标记语句(Labelled Statements). 下文为我理解的<a href="http://ecmascript.cn/#232">ECMAScript文档中的解释</a>：</p>
<blockquote><p>
语句可以有个标签前缀。标记语句只和<code>break</code>或<code>continue</code>标记的语句结合使用（想想第7题最后那个<code>break</code>的例子）。ECMAScript中并无goto语句。</p>
<p>ECMAScript程序中，如果标记语句的标签标识符有重复，则会出错。比方说<code>a: {a: 1};</code>就会嗝屁(FireBug显示SyntaxError: duplicate label错误)，而<code>a: {b: 1};</code>继续逍遥快活。但是，这并不适用于标记语句中或嵌套或直接或间接FunctionDeclaration体中出现的标签。如<code>a: (function() {a: 1});</code>就是OK的。</p>
<p>生产标识符(Identifier)：语句的执行是通过给标签集增加标识符，然后再执行语句。如果标签语句本身就有一个非空的标签集（冒号后面的语句还有类似结构，例如<code>a: b: 1</code>，<code>b:1</code>为<code>a</code>标签集的标签语句，但这个语句本身就含有一个标签集），则在语句执行之前这些标签们添加到标签集之中。如果语句执行的结果是<code>(break, V, L)</code>，其中L等于标识符，则产生的结果是<code>(normal, V, empty)</code>. <span>//zxx: 标识符实际就是指变量名、函数名、数组名等. 此处我是看不懂的，因此无法具体解释</span></p>
<p>执行标记语句(LabelledStatement)之前，其包含的语句被认为具有一个空的标签集，除非他是一个迭代语句或switch语句。在这种情况下,它被视为拥有一个单一元素组成的空标签集。
</p></blockquote>
<p>上面这点东西折腾了个把小时都没搞清楚要说的是什么，貌似也不能很好解释这里的结果，投入与产出比太低，郁闷！只怪功力不够，或许过两年再过来看，就很轻松了。</p>
<p>后又搜索了下，发现<a href="https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/label"><del>MDC</del>MDN的解释</a>要通俗的多：<br>
所谓标记语句，语法如下：</p>
<div>
<pre>label :
   statement</pre>
</div>
<p>就是：</p>
<div>
<pre>标签 :
   语句</pre>
</div>
<p>其中<code>label</code>可以是JavaScript中任意非保留关键字的标识符；<code>statement</code>中<code>break</code>可以用在任何标记语句中，<code>continue</code>用在循环标记语句中。</p>
<p><strong><del>MDC</del>MDN建议不使用labels</strong><br>
<code>labels</code>在JavaScript中并不常用，因为其降低了可读性以及易理解性，尽可能避免使用<code>labels</code>标记，根据不同情况，选择函数调用或者抛出错误。</p>
<p>下面为一个典型的标记语句的例子：</p>
<div>
<pre>var i, j;
 
loop1:
for (i = 0; i &lt; 3; i++) {      <span style="color:green">// 首页语句标记是 "loop1"</span>
   loop2:
   for (j = 0; j &lt; 3; j++) {   <span style="color:green">//第二个语句标记为 "loop2"</span>
      if (i == 1 &amp;&amp; j == 1) {
         continue loop1;
      } else {
         console.log(&quot;i = &quot; + i + &quot;, j = &quot; + j);
      }
   }
}
 
<span style="color:green">// 输出是:
//   "i = 0, j = 0"
//   "i = 0, j = 1"
//   "i = 0, j = 2"
//   "i = 1, j = 0"
//   "i = 2, j = 0"
//   "i = 2, j = 1"
//   "i = 2, j = 2"
// 注意是如何不输出 "i = 1, j = 1"和"i = 1, j = 2"的</span></pre>
</div>
<p>扯了这么多，回到问题本身。</p>
<p>根据规范，<code>a: b: c: ... g:</code>在语句执行之前会归到一个标签集中，为一个集合。因此，走个极端的话，我们可以这么理解：<br>
<code>abcdefg: 1, 2, 3, 4, 5</code>. <code>1, 2, 3, 4, 5</code>一开始有说明的逗号多重运算啦——返回最后一个值，因此，本题就类似于提问：</p>
<div>
<pre>abcdefg: 5	// 返回的是？</pre>
</div>
</li>
<li>
<div>
<pre>{a: 1, b: 2}[["b"]]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>FireBug控制台显示：SyntaxError: invalid label.</p>
<p>得知是这个结果的时候，我的懵懂硕大的眼睛立马无语成了一条缝，￣﹏￣</p>
<p>第6题中，我们已经讲过，JavaScript没有块作用域，块本身几乎是个酱油，因此这里的测试题实际等同于：</p>
<div>
<pre>a: 1, b: 2; [["b"]]</pre>
</div>
<p>结果显示：SyntaxError: invalid label. </p>
<p><img src="http://image.zhangxinxu.com/image/blog/201305/2013-05-07_091416.png" width="397" height="197" alt="无效标签的错误"></p>
<p>现在的疑问是，为何<code>a: 1, b: 2</code>会报错？微博提问……</p>
<p>30分钟后，<a href="http://weibo.com/1708684567">@紫云妃</a>给出了这样的解释：</p>
<blockquote><p>逗号运算符右侧必须是个表达式,不能是非表达式的语句,这个例子中<code>a: 1, b: 2</code>的右侧的<code>b: 2</code>是一个LabelledStatement, 不是表达式。</p></blockquote>
<p>我觉得是相当靠谱的回答。
</p></li>
<li>
<div>
<pre>"b" + 45	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>尼玛是不是感动地眼泪哗啦的。这种感觉就像是被后妈饿了个把月，在今天这个阳光明媚的日子赏了一块红烧肉，一下子感动地眼泪喷泉般涌出来。</p>
<p>结果是：<code>"b45"</code>. </p>
<p>字符串+数值=字符串。</p></li>
<li>
<div>
<pre>{a:{b:2}}	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>2</code>.</p>
<p>自从翻越了珠穆朗玛，才知道原来佘山就是个小土包。这个问题已经是小菜了，JavaScript没有块作用域，因此，<code>{a:{b:2}}</code>近乎于<code>a:b:2</code>, 想起<code>a,b,c,...g</code>的例子没有，显然，这里返回值是<code>2</code>.
</p></li>
<li>
<div>
<pre>(function(){}())	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>undefined</code>.</p>
<p>函数如果是空，或者没有指定返回值，则返回的是<code>undefined</code>.</p>
<p>关于此题，我在眼睛微博上抛出了这么个问题：<cite>(function(){}())和(function(){})()这两种写法有什么区别呢？</cite></p>
<p>1个小时过去了，以下几个评论比较见血：</p>
<blockquote><p><a href="http://weibo.com/cohlint1q81">@川川哥哥勤劳致富</a> 效果是一样的,周爱民那本书上提到了语法解读上的区别,不过感觉讲得不是很清晰 </p>
<p><a href="http://weibo.com/itapir">@貘吃馍香</a> 括号 匿名函数表达式执行 括号 跟 括号 函数表达式定义 括号 =&gt; 返回匿名函数句柄执行 的区别…… </p>
<p><a href="http://weibo.com/1900199050">@_Franky</a> 抽象语法树 和 运行时 无差异. 除非这个解释引擎脑残了. 因为分组运算符 “()” 在生成语法树的过程中被消除了. <a title="http://www.cnblogs.com/_franky/archive/2012/08/16/2641100.html" href="http://t.cn/zWTxgKa">http://t.cn/zWTxgKa</a> 这篇我有顺道提到过这个问题
</p></blockquote>
</li>
<li>
<div>
<pre>[1,2,3,4,5][0..toString.length]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果还是：<code>2</code>.</p>
<p>本题涉及的知识点挺多的。</p>
<p><strong>① </strong>首先是诡异的连续两个点<code>..</code>. 我们在写CSS的时候，常常会有这样的写法：</p>
<div>
<pre>font-size: <span style="color:#cd0000">.9em</span>;
color: rgba(0, 0, 0, <span style="color:#cd0000">.35</span>);</pre>
</div>
<p>点前面如果是<code>0</code>则可以自动缺省。这条规则似乎在JavaScript中也是适用的，比方说<code>.9</code>，返回的就是<code>0.9</code>.<br>
<img src="http://image.zhangxinxu.com/image/blog/201305/2013-05-07_111058.png" width="141" height="76" alt="JavaScript中点前面自动补0截图"></p>
<p>因此，这里<code>0..toString</code>实际上等同于<code>0.0.toString</code>. 下面有个疑问了，为什么不直接<code>0.toString</code>而是<code>0.0.toString</code>要多搞出一个<code>0</code>呢？据说是这样子的，数值后面的点<code>·</code>有两种作用，一种就是当作小数点适用，另外就是用做字段访问。显然，<code>0.toString</code>这里的点会被当作小数点，于是，直接报错了！写成<code>(0).toString</code>可以避免此问题。再来看看<code>0.0.toString</code>, 显然，再做解析的时候，<code>0.0</code>中的点当作了小数点，显然，<code>0.0</code>后面的那个点没有任何理由再被当作小数点了（小数不可能有两个小数点的），而是表示字段访问。于是，万事大吉，阳光明媚（下图版权所有）！</p>
<p><img src="http://image.zhangxinxu.com/image/blog/201305/0-0-tostring.png" width="300" height="150" alt="小数点和字段访问点的示意 张鑫旭-鑫空间-鑫生活"></p>
<p><strong>② </strong>下面看看<code>toString</code>, <code>toString</code>为JS内置方法，ECMAScript规范上转换规则如下：</p>
<table width="100%" cellpadding="0" cellspacing="1">
<tbody>
<tr>
<th>输入类型</th>
<th>结果</th>
</tr>
<tr>
<td>Undefined</td>
<td>“undefined”</td>
</tr>
<tr>
<td>Null</td>
<td>“null”</td>
</tr>
<tr>
<td>Boolean</td>
<td>如果参数是 true，那么结果为 “true”。<br>如果参数是 false，那么结果为 “false”。</td>
</tr>
<tr>
<td>Number</td>
<td>结果等于<strong>输入的参数</strong>（不转换）。</td>
</tr>
<tr>
<td>String</td>
<td>参见下文的文法和注释。</td>
</tr>
<tr>
<td>Object</td>
<td>应用下列步骤：
<ol>
<li>调用 <code>ToPrimitive</code>(<var> 输入参数 </var>, 暗示 <var>字符串类型</var>)。 </li>
<li>调用 ToString(Result(1))。 </li>
<li>返回 Result(2)。</li>
</ol>
</td>
</tr>
</tbody>
</table>
<p>从上表可以看出，数值使用toString方法是有参数的。</p>
<p>在JavaScript中，这个参数仅<code>1</code>个，称之为“基”，决定了数值转换的进制大小。默认情况下是10进制。例如：</p>
<div>
<pre>(10).toString(); <span style="color:green">// "10"</span></pre>
</div>
<p>我们还可以设置“基”为2进制，8进制或16进制，则结果大变样：</p>
<div>
<pre>(10).toString(2); <span style="color:green">// "1010"</span>
(10).toString(8); <span style="color:green">// "12"</span>
(10).toString(16); <span style="color:green">// "a"</span></pre>
</div>
<p>在<a href="http://www.zhangxinxu.com/wordpress/?p=646">实现颜色值16进制与rgb转化</a>的时候，这些基数参数就很管用。</p>
<p><strong>③ </strong>最后了解下函数的length与参数个数，看这个：<code>(function(){}).length</code>的结果是<code>0</code>. 为何？<br>
 每个<code>function</code>函数似乎都有一个<strong>不可写</strong>的<code>length</code>属性，对应这个函数的参数个数。</p>
<div>
<pre>var fun = function() {};
fun.length = 3;

fun.length; <span style="color:green">// 0</span></pre>
</div>
<p><img src="http://image.zhangxinxu.com/image/blog/201305/2013-05-07_143803.png" width="265" height="181" alt="函数的length属性不可写"></p>
<p><strong>④ </strong>现在，回到我们的问题，事情就豁然开朗了：<code>[0..toString.length]</code>实际上就是数值(<code>0.0</code>)应用toString方法的参数个数是多少？根据上面描述，数值使用<code>toString</code>转换参数个数为<code>1</code>。于是，本题结果就是<code>[1,2,3,4,5][1]</code> =&gt; <code>2</code>.
</p></li>
<li>
<div>
<pre>({} + &#39;b&#39; &gt; {} + &#39;a&#39;)	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>true</code>.</p>
<p>需要注意最外面的括号。如果没有最外面的括号，则<code>{}</code>则几乎无意义，但是这里，作为常规用法，<code>{}</code>表示原生对象。因此，这里的比较实际上就是比较<code>(&quot;[object Object]b&quot; &gt; &quot;[object Object]a&quot;)</code>, 因此返回的是<code>true</code>.</p>
<p>说点题外的，如果最外部没有括号，<code>{} + 'b'</code>返回的是<code>NaN</code>. 于是<code>{} + &#39;b&#39; &gt; {} + &#39;a&#39;</code>变成了比较<code>NaN &gt; NaN</code>, 结果为<code>false</code>.
</p></li>
<li>
<div>
<pre>Number.prototype.x = function(){ return this === 123; };
(123).x();	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>false</code>.</p>
<p>我们这里使用了严格相等。实际上<code>this</code>和<code>123</code>属于不同的类型。</p>
<div>
<pre>typeof this === “object”
typeof 123 === “number” </pre>
</div>
<p>因此，结果为<code>false</code>. 如果我们把题目修改成弱等于，则返回结果就是<code>true</code>了，见下截图：</p>
<div>
<pre>Number.prototype.x = function(){ return this == 123; };
(123).x();	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p><img src="http://image.zhangxinxu.com/image/blog/201305/2013-05-07_150408.png" width="464" height="81" alt="弱等于结果为true">
</p></li>
<li>
<div>
<pre>Array(2).join()	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>","</code></p>
<p><code>Array(2)</code>的返回值是<code>[undefined, undefined]</code>，因此，其使用join连接之后，就是个逗号<code>","</code>（数组join为指定连接符时候使用默认的逗号<code>","</code>）。</p>
<p>这里我要抛出一个微博上没有满意解答的问题：<code>Array(2)</code>和<code>new Array(2)</code>的区别在什么地方呢？ 求指点迷津！
</p></li>
<li>
<div>
<pre>vars: var vars = vars;	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>undefined</code>.</p>
<p>现在看此题就简单多了，标记语句，返回值就是<code>var vars = vars</code>的返回值<code>undefined</code>.</p>
<p><code>var vars = vars</code>并不会报<code>vars</code>为定义的错误是在于JS的置顶解析，其实<code>var vars = vars</code>的运作是这样子的：</p>
<div>
<pre>var vars;
vars = vars;</pre>
</div>
</li>
<li>
<div>
<pre>{ foo = 123 }	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>123</code>.</p>
<p>花括号不在括号中、if语句或者循环中，属于非常规酱油用法，形同虚设，问题等同于<code>foo = 123</code>的返回值是？
</p></li>
<li>
<div>
<pre>x = 1; (function(){return x; var x = 2;}())	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>undefined</code>. </p>
<p>此处考察的是JavaScript的“预解析(hoisting)”，也称“置顶解析”，我之前有<a href="http://www.zhangxinxu.com/wordpress/?p=1162">翻译过相关文章</a>。</p>
<p>所谓“预解析”指的是在当前的作用域内，无论在哪里变量声明，在幕后，其都在顶部被“预解析”了。因此，本题的实际“解析”是：</p>
<div>
<pre>x = 1; (function(){var x; <span style="color:green">// 此时x为undefined</span>
return x;  x= 2;}())</pre>
</div>
<p>因此结果是<code>undefined</code>.
</p></li>
<li>
<div>
<pre>delete [].length;	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果为<code>false</code>.</p>
<p><code>delete</code>用来删除对象属性，成功删除返回<code>true</code>, 如果对方防御很强删不动则返回<code>false</code>.</p>
<p>数组中的<code>length</code>属性是不可删除的，因此这里返回的是<code>false</code>.</p>
<p><code>delete</code>相关的知识点是很多的，比说法<code>window.x = 1</code>这里的<code>x</code>可以被<code>delete</code>. <code>var x =1</code>这里的<code>x</code>就不能被<code>delete</code>. 图灵社区有篇不错的译文：“<a href="http://www.ituring.com.cn/article/details/7620">理解delete</a>”，推荐阅读。</p></li>
<li>
<div>
<pre>RegExp.prototype.toString = function() {return this.source};
/3/-/2/;	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>1</code>. </p>
<p>正则表达式有如下一些属性：<code>source</code>, <code>global</code>, <code>ignoreCase</code>, <code>multiline</code>, <code>lastIndex</code>. 其中：<code>source</code>属性为构成正则表达式Pattern的字符串；<code>global</code>属性是一Boolean值，表示正则表达式flags是否有<code>"g"</code>；<code>ignoreCase</code>属性是一Boolean值，表示正则表达式flags是否有<code>"i"</code>；<code>multiline</code>属性是一Boolean值，表示正则表达式flags是否有<code>"m"</code>；<code>lastIndex</code>属性指定从何处开始下次匹配的一个字符串类型的位置索引，当需要时该值会转换为一个整型数。</p>
<p><code>RegExp.prototype.toString</code>扩展改变了默认的<code>toString</code>方法，当正则表达式需要应用<code>toString</code>方法进行字符串转换的时候，返回的就是正则表达式的<code>source</code>属性值。</p>
<p>例如：<code>/^\.dd\d+$/ + ""</code>的结果就是<code>"^\.dd\d+$"</code>.<br>
<img src="http://image.zhangxinxu.com/image/blog/201305/2013-05-07_163854.png" width="516" height="104" alt="正则表达式的source值示意"></p>
<p>于是，/3/-/2/实际上等同于<code>"3"-"2"</code>，因此结果就是<code>1</code>.</p>
<p>如果没有<code>toString</code>方法的重置，/3/-/2/实际上等同于<code>"/3/"-"/2/"</code>，因此结果就是<code>NaN</code>.
</p></li>
<li>
<div>
<pre>{break;4;}	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是报如下错误：SyntaxError: unlabeled <code>break</code> must be inside loop or <code>switch</code>.</p>
<p>意思是——解析错误：未标记的<code>break</code>必须在循环或<code>switch</code>中。</p>
<p>对照错误，我们加个标记，使之成为标记语句，就不会出错了。类似下面：</p>
<div>
<pre>foo: { break foo; 4;} </pre>
</div>
</li>
<li>
<div>
<pre>'foo' == new function(){ return String('foo'); };	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果为：<code>false</code>.</p>
<p>由于这里是<code>==</code>, <code>'foo'</code>又是正宗的字符串，因此，后面的<code>new...</code>需要转换成字符串。</p>
<div>
<pre>new function(){ return String('foo'); } + ""; <span style="color:green">// "[object Object]"</span></pre>
</div>
<p>显然，<code>'foo' == "[object Object]"</code>为<code>false</code>.</p>
<p>本题如果稍作一点修改，则结果完全不一样：</p>
<div>
<pre>'foo' == new function(){ return <span style="color:#cd0000">new</span> String('foo'); };	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果为：<code>true</code>.</p>
<p>为何？</p>
<p>在怿飞（圆心）“<a href="http://www.planabc.net/2008/02/20/javascript_new_function/">详解new function(){}和function(){}()</a>”一文中如下的解释(<span>//zxx: 08年初的文章啊，那个时候我处在被女神嫌弃，发奋图强的阶段。</span>)：</p>
<blockquote><p>只要<code>new</code>表达式之后的<code>constructor</code>返回（<code>return</code>）一个引用对象（数组，对象，函数等），都将覆盖new创建的匿名对象，如果返回（<code>return</code>）一个原始类型（无<code>return</code>时其实为<code>return</code>原始类型<code>undefined</code>），那么就返回<code>new</code>创建的匿名对象。</p></blockquote>
<p>什么意思呢？众所周知，JavaScript中有<code>5</code>种基本类型（Undefined类型、Null类型、Boolean类型、Number类型、String类型），如果<code>new</code>后面的<code>function</code> <code>return</code>的是这5中基本类型之一，<code>new</code>会认为你是纯屌丝，不理你，还是返回自己创建的匿名对象；当然，如果返回数组啊、函数、对象这类高富帅，<code>new</code>立马变龟孙子了，返回的就是这些高富帅。</p>
<p>由于<code>String("foo")</code>是字符串，而<code>new String("foo")</code>是对象。因此，前者返回的是匿名函数对象——显然不等于<code>"foo"</code>；后者就是<code>new String("foo")</code>对象，加上<code>"foo" == new String("foo")</code>，于是，结果为<code>true</code>.
</p></li>
<li>
<div>
<pre>'foo'.split('') + []	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>结果是：<code>"f,o,o"</code>.</p>
<p>记住，<code>数组加数组，字符成老母</code>。<code>'foo'.split('')</code>结果为数组<code>["f", "o", "o"]</code>，其变身字符串就是<code>"f,o,o"</code>跟后面的<code>[]</code>也就是<code>""</code>相加，就是最终的<code>"f,o,o"</code>了。</p>
<p>下面考考你，
<div>
<pre>[1, 2] + [3, 4]	<span style="color:graytext">//返回的是？</span></pre>
</div>
<p>是不是<code>"1,2,3,4"</code>？恭喜你，<img src="http://mat1.gtimg.com/www/mb/images/face/99.gif">，回答…………错误！<img src="http://mat1.gtimg.com/www/mb/images/face/64.gif"></p>
<p>这又是整哪样啊！哥，你只是稍微粗心了点。<code>[1, 2]</code>变成字符串是<code>"1,2"</code>, <code>[3, 4]</code>变成字符串是<code>"3,4"</code>，因此两者相加是<code>"1,23,4"</code>而不是<code>"1,2,3,4"</code>.</p>
<p>空数组实际上是个很有意思的东西。</p>
<div>
<pre>[] == 0 <span style="color:green">//true</span>
!0 <span style="color:green">// true</span>
![] <span style="color:green">// false</span></pre>
</div>
<p>纯属题外东西，就不扩展了。
</p></p></li>
</ol>
<h3>三、公子来时雪花飘，公子离去知了叫</h3>
<p>不容易啊，终于看到结尾了，从五一前写到五一后。</p>
<p>正如开始提到了，纯属个人理解，虽观点基本都多方考证，难免还有不准确的地方，欢迎有相关研究的同行指正。</p>
<p>油枯灯尽，想不出什么油麦的话语了，就这样吧。我个人是学到很多东西，希望对您的学习也能有所帮助。</p>
<p>文章的一些提问欢迎回答，您的回答会对其他过来学习的人提供很多的帮助，人的价值不正是在于留下什么吗？<img src="http://mat1.gtimg.com/www/mb/images/face/14.gif"></p>
<p>末了，附上ECMAScript5.1中文版链接：<a href="http://ecmascript.cn/">http://ecmascript.cn/</a></p>
<p>原创文章，转载请注明来自<a href="http://www.zhangxinxu.com/">张鑫旭-鑫空间-鑫生活</a>[<a href="http://www.zhangxinxu.com/">http://www.zhangxinxu.com</a>]<br>
本文地址：<a href="http://www.zhangxinxu.com/wordpress/?p=3223">http://www.zhangxinxu.com/wordpress/?p=3223</a></p>
<p>（本篇完）</p>
<div>
<p>觉得这里的文章不错，希望他一直走下去？您可以：<a href="https://me.alipay.com/zhangxinxu" title="小小赞助大大帮助"><img src="http://www.zhangxinxu.com/wordpress/wp-content/themes/default/images/pay_encourage.png" width="159" height="37" alt="支付鼓励"></a></p>
</div>
<div>有话要说？点击<a href="http://www.zhangxinxu.com/wordpress/2013/05/%e7%90%86%e8%a7%a3another-javascript-quiz-%e9%a2%98%e7%9b%ae/#respond">这里</a>发表评论。</div>