---
layout: post
title:  "理解Javascript的闭包"
date:   2012-03-07 08:30:43
author: Neo
categories: program
---

## 理解Javascript的闭包
### by Neo
### at 2012-03-07 08:30:43
### original <http://coolshell.cn/articles/6731.html>

<p>【<span style="color:#cc0000">感谢 Neo 投递本文 – 微博帐号：<a title="_锟_" href="http://weibo.com/gandalfthegrey">_锟_</a> </span>】</p>
<p><strong>前言：还是一篇入门文章。</strong>Javascript中有几个非常重要的语言特性——对象、原型继承、闭包。其中闭包对于那些使用传统静态语言C/C++的程序员来说是一个新的语言特性。本文将以例子入手来介绍Javascript闭包的语言特性，并结合一点ECMAScript语言规范来使读者可以更深入的理解闭包。</p>
<p>注：<strong>本文是入门文章，例子素材整理于网络<strong>，如果你是高手，欢迎针对文章提出技术性建议和意见。本文讨论的是Javascript，不想做语言对比，如果您对Javascript天生不适，请自行绕道。</strong></strong></p>
<h4><strong><span style="color:#008000">什么是闭包</span></strong></h4>
<p>闭包是什么?闭包是Closure，这是静态语言所不具有的一个新特性。但是闭包也不是什么复杂到不可理解的东西，简而言之，闭包就是：<strong></strong></p>
<ul>
<li><strong>闭包就是函数的局部变量集合，只是这些局部变量在函数返回后会继续存在。</strong></li>
<li><strong>闭包就是就是函数的“堆栈”在函数返回后并不释放，我们也可以理解为这些函数堆栈并不在栈上分配而是在堆上分配</strong></li>
<li><strong>当在一个函数内定义另外一个函数就会产生闭包</strong></li>
</ul>
<p>上面的第二定义是第一个补充说明，抽取第一个定义的主谓宾——闭包是<strong>函数的‘局部变量’集合</strong>。只是这个局部变量是可以在函数返回后被访问。（这个不是官方定义，但是这个定义应该更有利于你理解闭包）</p>
<p>做为局部变量都可以被函数内的代码访问，这个和静态语言是没有差别。闭包的差别在于局部变变量可以在函数执行结束后仍然被函数外的代码访问。这意味着函数必须返回一个指向闭包的“引用”，或将这个”引用”赋值给某个外部变量，才能保证闭包中局部变量被外部代码访问。当然包含这个引用的实体应该是一个对象，因为在Javascript中除了基本类型剩下的就都是对象了。可惜的是，ECMAScript并没有提供相关的成员和方法来访问闭包中的局部变量。但是在ECMAScript中，函数对象中定义的<strong>内部函数(inner function)</strong>是可以直接访问外部函数的局部变量，通过这种机制，我们就可以以如下的方式完成对闭包的访问了。</p>
<p><span></span></p>
<pre>
function greeting(name) {
    var text = &#39;Hello &#39; + name; // local variable
    // 每次调用时，产生闭包，并返回内部函数对象给调用者
    return function() { alert(text); }
}
var sayHello=greeting(&quot;Closure&quot;);
sayHello()  // 通过闭包访问到了局部变量text
</pre>
<p>上述代码的执行结果是：Hello Closure，因为sayHello()函数在greeting函数执行完毕后，仍然可以访问到了定义在其之内的局部变量text。</p>
<p>好了，这个就是传说中闭包的效果，闭包在Javascript中有多种应用场景和模式，比如Singleton，Power Constructor等这些Javascript模式都离不开对闭包的使用。</p>
<h4><strong><span style="color:#008000">ECMAScript闭包模型</span></strong></h4>
<p>ECMAScript到底是如何实现闭包的呢？想深入了解的亲们可以获取<a href="http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf">ECMAScript 规范</a>进行研究，我这里也只做一个简单的讲解，内容也是来自于网络。</p>
<p>在ECMAscript的脚本的函数运行时，每个函数关联都有一个执行上下文场景(Execution Context) ，这个执行上下文场景中包含三个部分</p>
<ul>
<li>文法环境（The LexicalEnvironment）</li>
<li>变量环境（The VariableEnvironment）</li>
<li>this绑定</li>
</ul>
<p>其中第三点this绑定与闭包无关，不在本文中讨论。文法环境中用于解析函数执行过程使用到的变量标识符。我们可以将文法环境想象成一个对象，该对象包含了两个重要组件，环境记录(Enviroment Recode)，和外部引用(指针)。环境记录包含包含了函数内部声明的局部变量和参数变量，外部引用指向了外部函数对象的上下文执行场景。全局的上下文场景中此引用值为NULL。这样的数据结构就构成了一个单向的链表，每个引用都指向外层的上下文场景。</p>
<p>例如上面我们例子的闭包模型应该是这样，sayHello函数在最下层，上层是函数greeting，最外层是全局场景。如下图：<br>
<img src="http://coolshell.cn/wp-content/uploads/2012/03/closure.png" alt="" width="658" height="478"><br>
因此当sayHello被调用的时候，sayHello会通过上下文场景找到局部变量text的值，因此在屏幕的对话框中显示出”Hello Closure”<br>
变量环境(The VariableEnvironment)和文法环境的作用基本相似，具体的区别请参看ECMAScript的规范文档。</p>
<h4><strong><span style="color:#008000">闭包的样列</span></strong></h4>
<p>前面的我大致了解了Javascript闭包是什么，闭包在Javascript是怎么实现的。下面我们通过针对一些例子来帮助大家更加深入的理解闭包，下面共有5个样例，例子来自于<a href="http://blog.morrisjohns.com/javascript_closures_for_dummies.html">JavaScript Closures For Dummies(</a><a href="http://web.archive.org/web/20080209105120/http://blog.morrisjohns.com/javascript_closures_for_dummies">镜像</a><a href="http://blog.morrisjohns.com/javascript_closures_for_dummies.html">)</a>。<br>
<strong>例子1:闭包中局部变量是引用而非拷贝</strong></p>
<pre>
function say667() {
    // Local variable that ends up within closure
    var num = 666;
    var sayAlert = function() { alert(num); }
    num++;
    return sayAlert;
}

var sayAlert = say667();
sayAlert()
</pre>
<p>因此执行结果应该弹出的667而非666。</p>
<p><strong>例子2：多个函数绑定同一个闭包，因为他们定义在同一个函数内。</strong></p>
<pre>
function setupSomeGlobals() {
    // Local variable that ends up within closure
    var num = 666;
    // Store some references to functions as global variables
    gAlertNumber = function() { alert(num); }
    gIncreaseNumber = function() { num++; }
    gSetNumber = function(x) { num = x; }
}
setupSomeGlobals(); // 为三个全局变量赋值
gAlertNumber(); //666
gIncreaseNumber();
gAlertNumber(); // 667
gSetNumber(12);//
gAlertNumber();//12
</pre>
<p><strong>例子3：当在一个循环中赋值函数时，这些函数将绑定同样的闭包</strong></p>
<pre>
function buildList(list) {
    var result = [];
    for (var i = 0; i &lt; list.length; i++) {
        var item = &#39;item&#39; + list[i];
        result.push( function() {alert(item + &#39; &#39; + list[i])} );
    }
    return result;
}

function testList() {
    var fnlist = buildList([1,2,3]);
    // using j only to help prevent confusion - could use i
    for (var j = 0; j &lt; fnlist.length; j++) {
        fnlist[j]();
    }
}
</pre>
<p>testList的执行结果是弹出item3 undefined窗口三次，因为这三个函数绑定了同一个闭包，而且item的值为最后计算的结果，但是当i跳出循环时i值为4，所以list[4]的结果为undefined.</p>
<p><strong>例子4：外部函数所有局部变量都在闭包内，即使这个变量声明在内部函数定义之后。</strong></p>
<pre>
function sayAlice() {
    var sayAlert = function() { alert(alice); }
    // Local variable that ends up within closure
    var alice = 'Hello Alice';
    return sayAlert;
}
var helloAlice=sayAlice();
helloAlice();
</pre>
<p>执行结果是弹出”Hello Alice”的窗口。即使局部变量声明在函数sayAlert之后，局部变量仍然可以被访问到。</p>
<p><strong>例子5：每次函数调用的时候创建一个新的闭包</strong></p>
<pre>
function newClosure(someNum, someRef) {
    // Local variables that end up within closure
    var num = someNum;
    var anArray = [1,2,3];
    var ref = someRef;
    return function(x) {
        num += x;
        anArray.push(num);
        alert('num: ' + num +
        '\nanArray ' + anArray.toString() +
        '\nref.someVar ' + ref.someVar);
    }
}
closure1=newClosure(40,{someVar:'closure 1'});
closure2=newClosure(1000,{someVar:'closure 2'});

closure1(5); // num:45 anArray[1,2,3,45] ref:'someVar closure1'
closure2(-10);// num:990 anArray[1,2,3,990] ref:'someVar closure2'
</pre>
<h4><strong><span style="color:#008000">闭包的应用</span></strong></h4>
<p><strong>Singleton 单件：</strong></p>
<pre>
var singleton = function () {
    var privateVariable;
    function privateFunction(x) {
        ...privateVariable...
    }

    return {
        firstMethod: function (a, b) {
            ...privateVariable...
        },
        secondMethod: function (c) {
            ...privateFunction()...
        }
    };
}();
</pre>
<p>这个单件通过闭包来实现。通过闭包完成了私有的成员和方法的封装。匿名主函数返回一个对象。对象包含了两个方法，方法1可以方法私有变量，方法2访问内部私有函数。需要注意的地方是匿名主函数结束的地方的’()’，如果没有这个’()’就不能产生单件。因为匿名函数只能返回了唯一的对象，而且不能被其他地方调用。这个就是利用闭包产生单件的方法。</p>
<h2><strong><span style="color:#008000">参考：</span></strong></h2>
<p><a href="http://blog.morrisjohns.com/javascript_closures_for_dummies.html">JavaScript Closures For Dummies(</a><a href="http://web.archive.org/web/20080209105120/http://blog.morrisjohns.com/javascript_closures_for_dummies">镜像</a><a href="http://blog.morrisjohns.com/javascript_closures_for_dummies.html">)</a> 可惜都被墙了。<br>
<a href="http://yuiblog.com/blog/2006/11/27/video-crockford-advjs/">Advance Javascript</a> （Douglas Crockford 大神的视频，一定要看啊）</p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="再谈javascript面向对象编程 " style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6668.html&amp;from=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6731.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/02/27/16161591.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">再谈javascript面向对象编程 </font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Javascript 面向对象编程" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6441.html&amp;from=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6731.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Javascript 面向对象编程</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="又一个Javascript试验田" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fcoolshell.cn%2Farticles%2F2276.html&amp;from=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6731.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">又一个Javascript试验田</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Javascript的两本书" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fcoolshell.cn%2Farticles%2F1608.html&amp;from=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6731.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/11/08/10437626.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Javascript的两本书</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="一段Javascript的代码" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fcoolshell.cn%2Farticles%2F3540.html&amp;from=http%3A%2F%2Fcoolshell.cn%2Farticles%2F6731.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">一段Javascript的代码</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="5" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><h3>相关文章</h3><ul><li>2012年02月27日 -- <a href="http://coolshell.cn/articles/6668.html" title="再谈javascript面向对象编程 ">再谈javascript面向对象编程 </a></li><li>2012年01月09日 -- <a href="http://coolshell.cn/articles/6441.html" title="Javascript 面向对象编程">Javascript 面向对象编程</a></li><li>2011年08月15日 -- <a href="http://coolshell.cn/articles/5202.html" title="对象的消息模型">对象的消息模型</a></li><li>2011年12月07日 -- <a href="http://coolshell.cn/articles/6043.html" title="Web开发中需要了解的东西">Web开发中需要了解的东西</a></li><li>2011年11月24日 -- <a href="http://coolshell.cn/articles/5537.html" title="一些文章资源和趣闻">一些文章资源和趣闻</a></li><li>2011年10月31日 -- <a href="http://coolshell.cn/articles/5709.html" title="API设计：用流畅接口构造内部DSL">API设计：用流畅接口构造内部DSL</a></li><li>2011年07月21日 -- <a href="http://coolshell.cn/articles/5035.html" title="面向对象的Shell脚本">面向对象的Shell脚本</a></li></ul>