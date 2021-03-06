---
layout: post
title:  "JavaScript: 数据类型"
date:   2012-09-12 22:13:56
author: 
categories: program
---

## JavaScript: 数据类型
### by 
### at 2012-09-12 22:13:56
### original <http://blog.jobbole.com/26859/?utm_source=rss&utm_medium=rss&utm_campaign=javascript-%25e6%2595%25b0%25e6%258d%25ae%25e7%25b1%25bb%25e5%259e%258b>

<p>英文原文：<a href="http://blogs.adobe.com/webplatform/2012/08/27/javascript-types/" rel="nofollow">JavaScript: Types</a>   编译：<a href="http://www.cnblogs.com/ziyunfei/archive/2012/09/12/2681657.html" rel="nofollow">紫云妃</a></p>
<p>我喜欢JavaScript.它是一门集强大与灵活于一身的语言,当然前提是你得知道如何去正确的使用它.一旦你真正掌握了JavaScript,你几乎可以用它来做任何事情,而且能做的既快又好.</p>
<p>如果你认为JavaScript太简单或者太低级, 那么你已经掉入了一个陷阱. <span></span>并且你会发现有很多人已经掉入了这样的陷阱中了.这些所谓的JavaScript开发者也许会告诉你,一些其他的语言 “X” 更好.</p>
<p>他们甚至会说,如果有一个将能将X语言转换为JavaScript的系统,那就太好了.想要逃出这个陷阱一直到真正的掌握JavaScript需要付出很多的努力和贡献.相信我,因为从1997年开始,我就已经在学习JavaScript了.</p>
<p>我是通过学习<a title="Standard ECMA-262" href="http://www.ecma-international.org/publications/standards/Ecma-262.htm">官方标准文档</a>掌握了JavaScript的所有高级知识,所以你也可以通过这种方法来掌握完整的语言知识.如果你的职称中包含了“JavaScript开发者”,那么你应该这样做.</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" rel="lightbox[26859]" title="JavaScript: 数据类型"><img title="JavaScript: 数据类型" src="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" alt="JavaScript: 数据类型" width="200" height="200"></a></p>
<p>在本篇博客中,我将会给出一个短小的JavaScript代码片段,然后让你给出这段代码的正确输出.如果你是一个JavaScript开发者,你会发现这样的题目真是太简单了.如果你仍然处在学习这门语言的过程中,你可能会遇到一些困难,不过你可以好好读一下代码下面的解释部分.</p>
<p>下面的JavaScript代码显示了一个弹出框.弹出框中显示什么?</p>
<pre>var five = 5;
five.three = 3;
alert(five + five.three);</pre>
<p>跳到本篇文章的最后查看正确的答案.接下来是为什么会有这样的结果的解释.</p>
<p>在JavaScript中,一共有六种<a title="http://ecma-international.org/ecma-262/5.1/#sec-8" href="http://ecma-international.org/ecma-262/5.1/#sec-8">数据类型</a>: <a href="http://ecma-international.org/ecma-262/5.1/#sec-8.6">Object</a>, <a href="http://ecma-international.org/ecma-262/5.1/#sec-8.5">Number</a>, <a href="http://ecma-international.org/ecma-262/5.1/#sec-8.4">String</a>, <a href="http://ecma-international.org/ecma-262/5.1/#sec-8.3">Boolean</a>, <a href="http://ecma-international.org/ecma-262/5.1/#sec-8.2">Null</a>, 以及 <a href="http://ecma-international.org/ecma-262/5.1/#sec-8.1">Undefined</a>.</p>
<p>对象(Objects)类型包含了数组(arrays), 函数(functions),以及其他的一般对象.数字(Numbers)类型可以是整型(integers)或者浮点数(floating point)类型以及特殊值NaN和Infinity. 字符串(Strings)类型包含了空字符串, ”&quot;. 布尔值(Booleans)类型只有两个值:true和false.最后两个基本数据类型有点特殊:Null类型只有一个值:null, Undefined类型只有一个值:undefined.所有Object除外的类型都称之为“原始值(primitive)”.JavaScript变量的类型不是在定义的时候明确指定的,还是在脚本运行的时候自动推断出来的.在上面的代码中,名为five的变量是Number类型的,因为它被赋值了一个Number字面量5.</p>
<p>和其他的计算机<span><a href="http://blog.jobbole.com/tag/%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80/" title="如何选择语言和编程语言排名相关文章">编程语言</a></span>类似,JavaScript也会隐式的将某个值的类型转换成适合对该值进行操作的运算符的类型.和其他语言不同的是, JavaScript会非常<a href="http://ecma-international.org/ecma-262/5.1/#sec-9">积极地做这些转换</a>.比如表达式”5″ – “3″的运算结果是数字2,因为减号运算符会把两边的操作数全部转换为数字再进行运算.如果一个操作数不能被转换为数字,则会使用NaN (“<a href="http://ecma-international.org/ecma-262/5.1/#sec-4.3.23">Not a Number</a>”)来替代.例如表达式”5″ – “Fred”会被隐式的转换为5 – NaN,则运算结果也会是NaN.</p>
<p>一套完整的隐式转换规则并不是十分复杂,你只需要知道每个操作符需要什么类型的操作数.</p>
<p>任意值转换为原始值遵循这样的规则“<a href="http://ecma-international.org/ecma-262/5.1/#sec-9.1">任意值转换为原始值</a>”. 其中对象到原始值的转换比较复杂:如果操作数的类型必须是Number类型,这就意味着JavaScript引擎会调用对象的valueOf()方法,如果返回的结果仍然不是一个原始值,则会调用对象的toString()方法转换成一个String类型的值.如果操作数的类型必须是String类型,则会首先调用对象的toString()方法,如果返回的结果不是一个原始值,再调用对象的valueOf()方法.不管那种情况,如果最终的转换结果仍然不是一个原始值,则抛出异常.</p>
<p> </p>
<p>如果<a href="http://ecma-international.org/ecma-262/5.1/#sec-9.3">操作数的类型必须是一个数字</a>,但该操作数的实际类型是:</p>
<p><strong>Object</strong>:</p>
<p>该值首先被转换成原始值,如果转换的结果不是一个数字,则会进入下面的转换分支.</p>
<p> </p>
<p><strong>String</strong>:</p>
<p>字符串类型的值会按照通常的JavaScript规则转换成数字类型</p>
<p> </p>
<p><strong>Boolean</strong>:</p>
<p>如果值为true,转换为1,否则转换为0</p>
<p> </p>
<p><strong>Null</strong>:</p>
<p>0</p>
<p> </p>
<p><strong>Undefined</strong>:</p>
<p>NaN</p>
<p> </p>
<p>如果<a href="http://ecma-international.org/ecma-262/5.1/#sec-9.8">操作数的类型必须是一个字符串</a>,但该操作数的实际类型是:</p>
<p> </p>
<p><strong>Object</strong>:</p>
<p>该值首先被转换成原始值,如果转换的结果不是一个字符串,则会进入下面的转换分支.</p>
<p> </p>
<p><strong>Number</strong>:</p>
<p>数字直接转为字符串, 比如 ”123″ or ”12.34″</p>
<p> </p>
<p><strong>Boolean</strong>:</p>
<p>转为”true”或”false”</p>
<p> </p>
<p><strong>Null</strong>:</p>
<p>“null”</p>
<p> </p>
<p><strong>Undefined</strong>:</p>
<p>“undefined”</p>
<p> </p>
<p>如果<a href="http://ecma-international.org/ecma-262/5.1/#sec-9.2">操作数的类型必须是一个布尔值</a>,但该操作数的实际类型是:</p>
<p> </p>
<p><strong>Object</strong>:</p>
<p>true</p>
<p> </p>
<p><strong>Number</strong>:</p>
<p>如果值为0,则转为false,否则转为true  (译者注:NaN也会转为false)</p>
<p> </p>
<p><strong>String</strong>:</p>
<p>如果值为一个空字符串”&quot;,则转为false,否则转为true</p>
<p> </p>
<p><strong>Null</strong>:</p>
<p>false</p>
<p> </p>
<p><strong>Undefined</strong>:</p>
<p>false</p>
<p> </p>
<p>如果<a href="http://ecma-international.org/ecma-262/5.1/#sec-9.9">操作数的类型必须是一个对象值</a>,但该操作数的实际类型是:</p>
<p> </p>
<p><strong>Number</strong>:</p>
<p>使用包装对象类型Number来包装原始值,new Number(value)</p>
<p> </p>
<p><strong>String</strong>:</p>
<p>使用包装对象类型String来包装原始值,new String(value)</p>
<p> </p>
<p><strong>Boolean</strong>:</p>
<p>使用包装对象类型Boolean来包装原始值,new Boolean(value)</p>
<p> </p>
<p><strong>Null</strong>:</p>
<p>抛出异常</p>
<p> </p>
<p><strong>Undefined</strong>:</p>
<p>抛出异常</p>
<p> </p>
<p>现在所有的转换规则已经很清晰了,让我们回到最初的例子中.</p>
<pre>var five = 5;
five.three = 3;
alert(five + five.three);</pre>
<p>正如我们前面提到的,第一行代码创建了一个名为five,类型为Number的变量.</p>
<p>当用<a href="http://ecma-international.org/ecma-262/5.1/#sec-11.2.1">属性访问符</a>作用在变量five上时,它的值会转换为Object类型.这种操作称之为“包装”,并且依赖于Number构造函数,该构造函数会产生一个对象,而不是一个原始值.第二行代码实际上等同于下面的代码:</p>
<pre>(new Number(five)).three=3;</pre>
<p>正如你看到的,我们并没有将新生成的Number对象的引用保存到一个变量中.在该表达式运行过后,被添加three属性的这个对象会被丢弃.而five变量的值没有任何变化.</p>
<p>第三行代码中的表达式five.three会再一次创建一个Number对象.这个新的对象并没有three属性,所以返回了特殊值undefined.结果等同于:</p>
<pre>alert(5+undefined);</pre>
<p>加法运算符会把两边的操作数全部转换为数字.在本例中,undefined会被转换为NaN,也就成了:</p>
<pre>alert(5+NaN);</pre>
<p>这也就解释了为什么最后的弹出框中仅仅显示了一个NaN.</p>
<p>(译者注:还差最后一步隐式转换,在进行alert()的时候,NaN会被自动转换为”NaN”)</p>
<p> </p>
<p> </p>
<p><strong>评论:</strong></p>
<p> </p>
<blockquote><p><strong>jerone:</strong></p>
<p>你这篇文章所讲的内容只是<strong>JavaScript</strong>的一部分,确切的说只包含了<strong>ECMAScript</strong>.除了这些,JavaScript还包含了DOM,也就是说还有Node, Element, HTMLElement等更多的数据类型.</p>
<p><strong>作者回复:</strong></p>
<p>Node, Element 以及 HTMLElement 都不是数据类型,就像Array, Date 和 RegExp一样,他们都属于对象(Object)类型.</p></blockquote>
<p> </p>
<blockquote><p><strong>Marcus Pope:</strong></p>
<p>我认为应该指出正确的编写方式,下面的代码才能够正常运行</p>
<pre>var five = new Number(5);
alert(five); //5
five.three = 3;
alert(five + five.three); //8</pre>
<p>我还认为最好应该用“number”来指代数字原始值,而用“Number”来指代数字类型的包装对象,这样才能减少读者的困惑.这也是在JavaScript中使用typeof操作符来查看这些值的内部类属性时显示的字符串.(译者注:这里指的是typeof 5 === “number”)</p>
<p><strong>ACRESTAN:</strong></p>
<p>是啊,这篇文章很让人困惑,就是因为作者使用了大写字母开头的类型字符串来描述原始值…</p>
<p>比如这句 “the first line creates a variable called five whose type is Number”,这是不对的!因为你的代码就证明了这一点.</p></blockquote>
<p> </p>
<blockquote><p><strong>metadings:</strong></p>
<p>还有第七种数据类型,JavaScript中最重要的类型:函数(<strong>Function</strong>).在JavaScript中,使用函数可以创建新的对象,所以原始值类型和函数类型还是有区别的.</p>
<pre>var Project = function () { };
// Project instanceof Function === true
// Project instanceof Object === true
// (typeof Project === &#39;function&#39;) === true

var o = new Project();
// o instanceof Project === true
// o instanceof Object === true
// (typeof Project === &#39;object&#39;) === true</pre>
<p>请仔细阅读这个链接<a href="http://metadea.de/V/">http://metadea.de/V/</a>，看看我是怎么写出真正的JavaScript类函数的.(译者注:还有一句没翻译,看不明白,觉的这个人在扯蛋.)</p>
<p><strong>作者回复:</strong></p>
<p>很显然函数在JavaScript中非常重要,值得用单独的文章来讲解它,但函数不是另外一种数据类型.函数只是一种特殊的对象.虽然特殊,但仍是对象.</p>
<p>由许许多多不同的构造函数创建的对象们都继承了同一个类型:Object.这很容易检测到:如果Object(a) === a,则a是一个对象类型,而不是其他原始值类型.</p></blockquote>
<p> </p>
<blockquote><p><strong>ulu:</strong></p>
<p>这篇文章正好进一步证明了JavaScript不只是一个最强大的语言,同时也是最令人困惑的语言.应该不惜一切代价避免使用它.总有一天,会有一个更人性化的语言集成在所有的主流<span><a href="http://blog.jobbole.com/12749/" title="浏览器">浏览器</a></span>中,我希望在我有生之年能够赶上.</p>
<p><strong>作者回复:</strong></p>
<p>当你手中拿着一把锤子时,所有事物看上去都像钉子.当你认为JavaScript很让人迷惑时,任何特性都能成为你的证据.</p></blockquote>
<p> </p>
<blockquote><p><strong>Dave Chapman :</strong></p>
<p>这种类型的编码错误正是我们为什么需要一个更好的能够在我们码字的同时,在运行代码之前进行语法检查的IDE的原因.比如.在closure compiler中运行例子中的代码会产生如下的警告信息:</p>
<p>“JSC_INEXISTENT_PROPERTY: Property three never defined on Number at line 4 character 13:</p>
<p>alert(five + five.three);”</p>
<p>(译者注:closure compiler是谷歌的代码压缩器或者叫编译器.https://developers.google.com/closure/compiler/)</p></blockquote>
<p> </p>
<blockquote><p><strong>Sponge Bob:</strong></p>
<p>我推荐你看一下这个视频: <a href="https://www.destroyallsoftware.com/talks/wat/">https://www.destroyallsoftware.com/talks/wat/</a></p>
<p>Enjoy</p>
<p>(译者注:这个视频主要吐槽了ruby和JavaScript的诡异用法,这里有中文字幕版 <a href="http://v.ku6.com/show/nhlYYNrbd5d62ASt-aaDrA...html">http://v.ku6.com/show/nhlYYNrbd5d62ASt-aaDrA…html</a>)</p></blockquote>
<p> </p>
<blockquote><p><strong>simonleung :</strong></p>
<p>一个数字可以是“object”类型的,也可以是“number”类型的.</p>
<p>当用在条件语句中或者其他需要转换为布尔值的地方,这两种类型的数字会产生不同的结果,例如.</p>
<p>Number(0)会得到true,因为Number(0)是一个对象,而0显然会得到false.</p>
<p>(译者注:这里他说错了.Number作为函数只是一个类型转换函数,返回的仍然是原始值,只有用new Number(0),Number才算是一个构造函数,返回的才是对象)</p>
<p>另外一个,在null上进行typeof操作,返回”object”,但它不是真正的对象,null是假值,会被转换为false.</p></blockquote>
<p> </p>
<p> </p>
<p> </p>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/26554/" title="CoffeeScript：以优美方式编写JavaScript代码">CoffeeScript：以优美方式编写JavaScript代码</a></li><li><a href="http://blog.jobbole.com/26101/" title="田永强：优秀的JavaScript模块是怎样炼成的">田永强：优秀的JavaScript模块是怎样炼成的</a></li><li><a href="http://blog.jobbole.com/25390/" title="一种隐藏javascript代码的技术">一种隐藏javascript代码的技术</a></li><li><a href="http://blog.jobbole.com/24878/" title="JavaScript宝座：八大框架论剑">JavaScript宝座：八大框架论剑</a></li><li><a href="http://blog.jobbole.com/24602/" title="向非程序员解释 JavaScript">向非程序员解释 JavaScript</a></li><li><a href="http://blog.jobbole.com/24329/" title="了解 JavaScript 中的内置对象">了解 JavaScript 中的内置对象</a></li><li><a href="http://blog.jobbole.com/23563/" title="Javascript定义类（class）的三种方法">Javascript定义类（class）的三种方法</a></li><li><a href="http://blog.jobbole.com/23517/" title="Dojo与jQuery综合比较分析">Dojo与jQuery综合比较分析</a></li><li><a href="http://blog.jobbole.com/21962/" title="高性能JavaScript模板引擎原理解析">高性能JavaScript模板引擎原理解析</a></li><li><a href="http://blog.jobbole.com/21419/" title="pffp（一个JavaScript公共函数接口规范）的简介">pffp（一个JavaScript公共函数接口规范）的简介</a></li></ul>