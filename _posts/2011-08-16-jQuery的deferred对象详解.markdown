---
layout: post
title:  "jQuery的deferred对象详解"
date:   2011-08-16 23:26:32
author: 
categories: program
---

## jQuery的deferred对象详解
### by 
### at 2011-08-16 23:26:32
### original <http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html>

<p><a href="http://jquery.com/">jQuery</a>的开发速度很快，几乎每半年一个大版本，每两个月一个小版本。</p><p>每个版本都会引入一些新功能。今天我想介绍的，就是从jQuery 1.5.0版本开始引入的一个新功能----<a href="http://api.jquery.com/category/deferred-object/">deferred对象</a>。</p>

<p>这个功能很重要，未来将成为jQuery的核心方法，它彻底改变了如何在jQuery中使用ajax。为了实现它，jQuery的全部ajax代码都被改写了。</p>

<p>但是，它比较抽象，初学者很难掌握，网上的教程也不多。所以，我把自己的学习笔记整理出来了，希望对大家有用。</p>

<p>本文不是初级教程，针对的读者是那些已经具备jQuery使用经验的开发者。如果你想了解jQuery的基本用法，请阅读我编写的<a href="http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html">《jQuery设计思想》</a>和<a href="http://www.ruanyifeng.com/blog/2011/08/jquery_best_practices.html">《jQuery最佳实践》</a>。</p>

<p>======================================</p>

<p><strong>jQuery的deferred对象详解</strong></p>

<p>作者：阮一峰</p>

<p><img src="http://image.beekka.com/blog/201108/bg2011081601.jpg"></p>

<p><strong>一、什么是deferred对象？</strong></p>

<p>开发网站的过程中，我们经常遇到某些耗时很长的javascript操作。其中，既有异步的操作（比如ajax读取服务器数据），也有同步的操作（比如遍历一个大型数组），它们都不是立即能得到结果的。</p>

<p>通常的解决方法是，为它们指定回调函数（callback）。即事先规定，一旦它们运行结束，应该调用哪些函数。</p>

<p>但是，在回调函数方面，jQuery的功能非常弱。为了改变这一点，jQuery开发团队就设计了<a href="http://api.jquery.com/category/deferred-object/">deferred对象</a>。</p>

<p><strong>简单说，deferred对象就是jQuery的回调函数解决方案。</strong>在英语中，defer的意思是"延迟"，所以deferred对象的含义就是"延迟"到未来某个点再执行。</p>

<p>它解决了如何处理耗时操作的问题，对那些操作提供了更好的控制，以及统一的编程接口。它的主要功能，可以归结为四点。下面我们通过示例代码，一步步来学习。</p>

<p><strong>二、ajax操作的链式写法</strong></p>

<p>jQuery的ajax操作，传统写法是这样的：</p>

<blockquote>

<p>　　$.ajax({</p>

<p>　　　　url: "test.html",</p>

<p>　　　　success: function(){<br>
　　　　　　alert("哈哈，成功了！");<br>
　　　　},</p>

<p>　　　　error:function(){<br>
　　　　　　alert("出错啦！");<br>
　　　　}</p>

<p>　　});</p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/pdQYH/">代码示例1</a>）</p>

</blockquote>

<p>在上面的代码中，$.ajax()接受一个对象参数，这个对象包含两个方法：success方法指定操作成功后的回调函数，error方法指定操作失败后的回调函数。</p>

<p>$.ajax()操作完成后，如果使用的是低于1.5.0版本的jQuery，返回的是XHR对象，你没法进行链式操作；如果高于1.5.0版本，返回的是deferred对象，可以进行链式操作。</p>

<p>现在，新的写法是这样的：</p>

<blockquote>

<p>　　$.ajax("test.html")</p>

<p>　　<strong>.done(function(){ alert("哈哈，成功了！"); })</strong></p>

<p>　　<strong>.fail(function(){ alert("出错啦！"); });</strong></p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/dYKLJ/">代码示例2</a>）</p>

</blockquote>

<p>可以看到，<a href="http://api.jquery.com/deferred.done/">done()</a>相当于success方法，<a href="http://api.jquery.com/deferred.fail/">fail()</a>相当于error方法。采用链式写法以后，代码的可读性大大提高。</p>

<p><strong>三、指定同一操作的多个回调函数</strong></p>

<p>deferred对象的一大好处，就是它允许你自由添加多个回调函数。</p>

<p>还是以上面的代码为例，如果ajax操作成功后，除了原来的回调函数，我还想再运行一个回调函数，怎么办？</p>

<p>很简单，直接把它加在后面就行了。</p>

<blockquote>

<p>　　$.ajax("test.html")</p>

<p>　　.done(function(){ alert("哈哈，成功了！");} )</p>

<p>　　.fail(function(){ alert("出错啦！"); } )</p>

<p>　　<strong>.done(function(){ alert("第二个回调函数！");} );</strong></p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/sQYjs/">代码示例3</a>）</p>

</blockquote>

<p>回调函数可以添加任意多个，它们按照添加顺序执行。</p>

<p><strong>四、为多个操作指定回调函数</strong></p>

<p>deferred对象的另一大好处，就是它允许你为多个事件指定一个回调函数，这是传统写法做不到的。</p>

<p>请看下面的代码，它用到了一个新的方法<a href="http://api.jquery.com/jQuery.when/">$.when()</a>：</p>

<blockquote>

<p>　　<strong>$.when($.ajax("test1.html"), $.ajax("test2.html"))</strong></p>

<p>　　.done(function(){ alert("哈哈，成功了！"); })</p>

<p>　　.fail(function(){ alert("出错啦！"); });</p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/CdKjn/">代码示例4</a>）</p>

</blockquote>

<p>这段代码的意思是，先执行两个操作$.ajax("test1.html")和$.ajax("test2.html")，如果成功了，就运行done()指定的回调函数；如果有一个失败或都失败了，就执行fail()指定的回调函数。</p>

<p><strong>五、普通操作的回调函数接口（上）</strong></p>

<p>deferred对象的最大优点，就是它把这一套回调函数接口，从ajax操作扩展到了所有操作。也就是说，任何一个操作----不管是ajax操作还是本地操作，也不管是异步操作还是同步操作----都可以使用deferred对象的各种方法，指定回调函数。</p>

<p>我们来看一个具体的例子。假定有一个很耗时的操作wait：</p>

<blockquote>

<p>　　var wait = function(){</p>

<p>　　　　var tasks = function(){</p>

<p>　　　　　　alert("执行完毕！");</p>

<p>　　　　};</p>

<p>　　　　setTimeout(tasks,5000);</p>

<p>　　};</p>

</blockquote>

<p>我们为它指定回调函数，应该怎么做呢？</p>

<p>很自然的，你会想到，可以使用$.when()：</p>

<blockquote>

<p>　　$.when(wait())</p>

<p>　　.done(function(){ alert("哈哈，成功了！"); })</p>

<p>　　.fail(function(){ alert("出错啦！"); });</p>

</blockquote>

<p>但是，有一个问题。$.when()的参数只能是deferred对象，所以必须对wait进行改写：</p>

<blockquote>

<p>　　var dtd = $.Deferred(); // 新建一个deferred对象</p>

<p>　　var wait = function(dtd){</p>

<p>　　　　var tasks = function(){</p>

<p>　　　　　　alert("执行完毕！");</p>

<p>　　　　　　<strong>dtd.resolve();</strong> // 改变deferred对象的执行状态</p>

<p>　　　　};</p>

<p>　　　　setTimeout(tasks,5000);</p>

<p>　　　　<strong>return dtd.promise();</strong></p>

<p>　　};</p>

</blockquote>

<p>这里有两个地方需要注意。</p>

<p>首先，最后一行不能直接返回dtd，必须返回<a href="http://api.jquery.com/deferred.promise/">dtd.promise()</a>。原因是jQuery规定，任意一个deferred对象有三种执行状态----未完成，已完成和已失败。如果直接返回dtd，$.when()的默认执行状态为"已完成"，立即触发后面的done()方法，这就失去回调函数的作用了。dtd.promise()的目的，就是保证目前的执行状态----也就是"未完成"----不变，从而确保只有操作完成后，才会触发回调函数。</p>

<p>其次，当操作完成后，必须手动改变Deferred对象的执行状态，否则回调函数无法触发。<a href="http://api.jquery.com/deferred.resolve/">dtd.resolve()</a>的作用，就是将dtd的执行状态从"未完成"变成"已完成"，从而触发done()方法。</p>

<p>最后别忘了，修改完wait之后，调用的时候就必须直接传入dtd参数。</p>

<blockquote>

<p>　　$.when(wait(dtd))</p>

<p>　　.done(function(){ alert("哈哈，成功了！"); })</p>

<p>　　.fail(function(){ alert("出错啦！"); });</p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/zSddB/1/">代码示例5</a>）</p>

</blockquote>

<p><strong>六、普通操作的回调函数接口（中）</strong></p>

<p>除了使用$.when()为普通操作添加回调函数，还可以使用deferred对象的建构函数$.Deferred()。</p>

<p>这时，wait函数还是保持不变，我们直接把它传入$.Deferred()：</p>

<blockquote>

<p>　　<strong>$.Deferred(wait)</strong></p>

<p>　　.done(function(){ alert("哈哈，成功了！"); })</p>

<p>　　.fail(function(){ alert("出错啦！"); });</p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/CucGp/">代码示例6</a>）</p>

</blockquote>

<p>jQuery规定，$.Deferred()可以接受一个函数作为参数，该函数将在$.Deferred()返回结果之前执行。并且，$.Deferred()所生成的Deferred对象将作为这个函数的默认参数。</p>

<p><strong>七、普通操作的回调函数接口（下）</strong></p>

<p>除了上面两种方法以外，我们还可以直接在wait对象上部署deferred接口。</p>

<blockquote>

<p>　　var dtd = $.Deferred(); // 生成Deferred对象</p>

<p>　　var wait = function(dtd){</p>

<p>　　　　var tasks = function(){</p>

<p>　　　　　　alert("执行完毕！");</p>

<p>　　　　　　dtd.resolve(); // 改变Deferred对象的执行状态</p>

<p>　　　　};</p>

<p>　　　　setTimeout(tasks,5000);</p>

<p>　　};</p>

<p>　　<strong>dtd.promise(wait);</strong></p>

<p>　　wait.done(function(){ alert("哈哈，成功了！"); })</p>

<p>　　.fail(function(){ alert("出错啦！"); });</p>

<p>　　wait(dtd);</p>

<p>（运行<a href="http://jsfiddle.net/ruanyf/PF7Xf/">代码示例7</a>）</p>

</blockquote>

<p>这里的关键是dtd.promise(wait)这一行，它的作用就是在wait对象上部署Deferred接口。正是因为有了这一行，后面才能直接在wait上面调用done()和fail()。</p>

<p><strong>八、小结：deferred对象的方法</strong></p>

<p>前面已经讲到了deferred对象的多种方法，下面做一个总结：</p>

<p>　　（1） <a href="http://api.jquery.com/category/deferred-object/">$.Deferred()</a> 生成一个deferred对象。</p>

<p>　　（2） <a href="http://api.jquery.com/deferred.done/">deferred.done()</a> 指定操作成功时的回调函数</p>

<p>　　（3） <a href="http://api.jquery.com/deferred.fail/">deferred.fail()</a> 指定操作失败时的回调函数</p>

<p>　　（4） <a href="http://api.jquery.com/deferred.promise/">deferred.promise()</a> 没有参数时，作用为保持deferred对象的运行状态不变；接受参数时，作用为在参数对象上部署deferred接口。</p>

<p>　　（5） <a href="http://api.jquery.com/deferred.resolve/">deferred.resolve()</a> 手动改变deferred对象的运行状态为"已完成"，从而立即触发done()方法。</p>

<p>　　（6） <a href="http://api.jquery.com/jQuery.when/">$.when()</a> 为多个操作指定回调函数。</p>

<p>除了这些方法以外，deferred对象还有三个重要方法，上面的教程中没有涉及到。</p>

<p>　　（7）<a href="http://api.jquery.com/deferred.then/">deferred.then()</a></p>

<p>有时为了省事，可以把done()和fail()合在一起写，这就是then()方法。</p>

<blockquote>

<p>　　$.when($.ajax( "/main.php" ))</p>

<p>　　<strong>.then(successFunc, failureFunc );</strong></p>

</blockquote>

<p>如果then()有两个参数，那么第一个参数是done()方法的回调函数，第二个参数是fail()方法的回调方法。如果then()只有一个参数，那么等同于done()。</p>

<p>　　（8）<a href="http://api.jquery.com/deferred.reject/">deferred.reject()</a></p>

<p>这个方法与deferred.resolve()正好相反，调用后将deferred对象的运行状态变为"已失败"，从而立即触发fail()方法。</p>

<p>　　（9）<a href="http://api.jquery.com/deferred.always/">deferred.always()</a></p>

<p>这个方法也是用来指定回调函数的，它的作用是，不管调用的是deferred.resolve()还是deferred.reject()，最后总是执行。</p>

<blockquote>

<p>　　$.ajax( "test.html" )</p>

<p>　　.always( function() { alert("已执行！");} );</p>

</blockquote>

<p>（完）<br>
</p><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;border:1px solid #d3d3d3;margin:1em;background-color:#aad2f0;border-radius:10px"><h3>文档信息</h3>
<ul>
<li>版权声明：自由转载-非商用-非衍生-保持署名 | <a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html">http://www.ruanyifeng.com/blog/2011/08/a_detailed_explanation_of_jquery_deferred_object.html</a></li>
<li>最后修改时间：2011年8月31日 21:25</li>
<li>付费支持（<a href="http://www.ruanyifeng.com/blog/2011/05/my_google_adsense_is_disabled.html" style="text-decoration:underline">说明</a>）：<a href="https://lab.alipay.com/p.htm?id=2011081500435188"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" alt="支付宝担保交易" style="border:none;vertical-align:middle"></a> | <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=0.3&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="paypal" style="border:none;vertical-align:middle"></a> </li>
</ul></div><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;margin:1em;border-radius:10px"><p><a href="http://www.nanrenwa.com/click/go/ruanyifeng.banner.feed.yfblogfeed/gift"><img src="http://nanrenwa.com/_img/a/2011/yfblog_feed.png"></a></p>
</div>