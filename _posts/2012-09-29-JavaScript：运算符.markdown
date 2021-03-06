---
layout: post
title:  "JavaScript：运算符"
date:   2012-09-29 08:50:40
author: 
categories: program
---

## JavaScript：运算符
### by 
### at 2012-09-29 08:50:40
### original <http://blog.jobbole.com/28884/?utm_source=rss&utm_medium=rss&utm_campaign=javascript%25ef%25bc%259a%25e8%25bf%2590%25e7%25ae%2597%25e7%25ac%25a6>

<p>来源：<a href="http://www.cnblogs.com/ziyunfei/archive/2012/09/24/2699386.html">紫云飞的博客</a></p>
<p>在<a href="http://blog.jobbole.com/26859/">上一篇文章中</a>,我们讲到了JavaScript中的数据类型和类型转换.今天,我们接着讲一下JavaScript中的每个运算符到底是如何进行类型转换的.下面会依次讲解六个最常用的运算符的工作机制:</p>
<p><strong><a href="http://ecma-international.org/ecma-262/5.1/#sec-11.4.3"><code>typeof</code></a></strong></p>
<p><code>typeof运算符会返回操作数类型的字符串表示</code>.主要有两个需要注意的地方:<span></span></p>
<p>●未定义或未声明的变量将会返回<code>"undefined"</code>, 比如.如果a没有被声明,那么<code>typeof a将会返回</code><code>"undefined"</code>.</p>
<p><code>●typeof在操作数是</code><code>n</code><code>ull或函数的</code><code>种情况下会"撒谎"</code><code></code>.</p>
<p>除了这些, 操作数和对应的类型字符串可以从下表查出:</p>
<p style="text-align:center"><span style="text-align:center">Undefined                            ”undefined”</span></p>
<p style="text-align:center"><span style="text-align:center">Null          ”object”</span></p>
<p style="text-align:center"><span style="text-align:center">Boolean       “boolean”</span></p>
<p style="text-align:center"><span style="text-align:center">Number       “number”</span></p>
<p style="text-align:center"><span style="text-align:center">String        ”string”</span></p>
<p style="text-align:center"><span style="text-align:center">Object,不可以被调用     “object”</span></p>
<p style="text-align:center"><span style="text-align:center">Object,可以被调用      ”function”</span></p>
<p>我标记“⚠”的两处地方就是上面提到的typeof误导我们的地方:type of <code>null应该返回"</code>Null”,而任意函数的类型应该是”object”.</p>
<p><strong><a href="http://ecma-international.org/ecma-262/5.1/#sec-11.6.2">减法(-)</a></strong></p>
<p>将两边的操作数都转换为数字. <code>"8" - true</code> 会被转换为 <code>8 - 1</code>.的确非常的简单,不过下面的加法可就不是了<img src="http://blogs.adobe.com/webplatform/wp-includes/images/smilies/icon_wink.gif" alt=";)">.</p>
<p><strong><a href="http://ecma-international.org/ecma-262/5.1/#sec-11.6.1">加法(+)</a></strong></p>
<p>加法是JavaScript中最麻烦的运算符了.让我们看看在执行<code>a + b的时到底发生了什么</code>:</p>
<p>1. 两边的操作数首先被转换成原始值.这里我们称之为<code>A</code> 和 <code>B</code>.</p>
<p>2.如果有任意一个原始值是字符串,则把另一个也转换成字符串,执行<code>A和</code><code>B的连接操作并返回连接后的字符串</code>.</p>
<p>3.否则把<code>A和</code><code>B都转换为数字,返回两个数字的和</code>.</p>
<p>例如:</p>
<p>8 + “5″ ➙ “8″ + “5″ ➙ “85″; //”5″是字符串,所以把8也转换成字符串,连接后值为”85″</p>
<p>8 + true ➙ 8 + 1 ➙ 9; //没有字符串,两边都转换成数字,true转换成数字为1,返回相加的和9</p>
<p>“8″ + true ➙ “8″ + “true” ➙ “8true”; //”8″是字符串,把true也转换成字符串”true”,连接后值为”8true”</p>
<p><strong><a href="http://ecma-international.org/ecma-262/5.1/#sec-11.8.1">小于(&lt;)</a></strong></p>
<p>和加法运算符不同,小于运算符只在两个操作数都为字符串的时候才将他们作为字符串来比较.下面是正式的操作步骤:</p>
<p>1.两边的操作数都转换成原始值.这里我们称之为<code>A</code> <code>和 B</code>.</p>
<p>2.如果这个两个原始值都为字符串,则把<code>A和</code><code>B按照字符串来比较</code>.</p>
<p>3.否则将他们都转换位数字,按照数字大小比较.</p>
<p>例如:</p>
<p>8 &gt; “5″ ➙ 8 &gt; 5 ➙ true; //两边不都是字符串,”5″转换为数字5</p>
<p>8 &gt; true ➙ 8 &gt; 1 ➙ true; //两边不都是字符串,true转换成数字1</p>
<p>“8″ &gt; “18″ ➙ true; //两边都是字符串,8的ascii码大于1的ascii码</p>
<p><strong><a href="http://ecma-international.org/ecma-262/5.1/#sec-11.9.4">严格相等(===)</a></strong></p>
<p>许多运算符中最让人省心的一个,也被称为三等号(<code>===</code>),它的操作很简单:检查两个操作数的类型是否相等,如果相等的话,检查他们的值是否相等.他的兄弟运算符(==)就比较复杂了.</p>
<p><strong><a href="http://ecma-international.org/ecma-262/5.1/#sec-11.9.1">相等(==)</a></strong></p>
<p>JavaScript中最让人讨厌的运算符.它的工作机制是这样的:</p>
<p>1.首先检查两个操作数的类型,如果他们是相同的类型,那么继续执行严格相等比较.</p>
<p>2.如果两个操作数都是<code>null或者是</code><code>undefined</code>,返回<code>true</code>.</p>
<p>3.如果其中一个操作数是字符串另外一个是数字,则将他们都转换数字,再执行严格相等比较.</p>
<p>4.如果其中一个操作数是布尔值,把它转换成数字,然后回到步骤1继续执行.</p>
<p>5.如果其中一个操作数是字符串或者数字,另外一个是对象值,把这个对象值转换成原始值,然后回到步骤1继续执行.</p>
<p>6.返回<code>false</code>.</p>
<p>这基本上意味着,如果两个操作数的类型不同,则判断的工作机制类似于小于&lt;比较,如果类型相同,则类似于严格相等比较.总结一下就是:当类型不同时,将两个操作数都转换为原始值,除非两个原始值都是字符串,否则再次将两个原始值转换成数字再比较,还有就是<code>null == undefined是</code><code>true</code>.</p>
<p>8 == “5″ ➙ 8 == 5 ➙ false; //”5″转换成数字5</p>
<p>1 == true ➙ 1 == 1 ➙ true; //true转换成数字1</p>
<p>0 == “” ➙ 0 == 0 ➙ true; //”&quot;转换成数字0</p>
<p>0 == “0″ ➙ 0 == 0 ➙ true; //”0″转换成数字0</p>
<p>“” == “0″ ➙ false; //字符串直接判断是否相等</p>
<p>“1000″ == “1e3″ ➙ false;　　　　　　　　//字符串直接判断是否相等</p>
<p>1000 == “1e3″ ➙ true; //”1e3″转换成1000,科学计数法</p>
<p>5 == {valueOf: function () { return 5; }} ➙ 5 == 5 ➙ true; //对象值转换成原始值5</p>
<p>这六个运算符并不是全部的运算符,但肯定是最麻烦的几个了.</p>
<p> </p>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/29061/" title="JavaScript：打破所有规则">JavaScript：打破所有规则</a></li><li><a href="http://blog.jobbole.com/28840/" title="基于 HTML5 构建 Web 操作系统">基于 HTML5 构建 Web 操作系统</a></li><li><a href="http://blog.jobbole.com/26554/" title="CoffeeScript：以优美方式编写JavaScript代码">CoffeeScript：以优美方式编写JavaScript代码</a></li><li><a href="http://blog.jobbole.com/26859/" title="JavaScript: 数据类型">JavaScript: 数据类型</a></li><li><a href="http://blog.jobbole.com/26101/" title="田永强：优秀的JavaScript模块是怎样炼成的">田永强：优秀的JavaScript模块是怎样炼成的</a></li></ul>