---
layout: post
title:  "JavaScript中有趣的反柯里化"
date:   2013-01-08 22:34:01
author: 
categories: program
---

## JavaScript中有趣的反柯里化
### by 
### at 2013-01-08 22:34:01
### original <http://blog.jobbole.com/32059/?utm_source=rss&utm_medium=rss&utm_campaign=javascript%25e4%25b8%25ad%25e6%259c%2589%25e8%25b6%25a3%25e7%259a%2584%25e5%258f%258d%25e6%259f%25af%25e9%2587%258c%25e5%258c%2596>

<p>来源：<a href="http://www.alloyteam.com/2012/12/4304/">alloyteam – svenzeng</a></p>
<p>反柯里化(UnCurrying)的话题来自javascript之父Brendan Eich去年的一段twitter. 近几天研究了一下，觉得这个东东非常有意思，分享一下。先忘记它的名字，看下它能做什么。<span></span></p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%871.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%871.png" alt="JavaScript中有趣的反柯里化" width="554" height="135"></a></p>
<p>不要小看这个功能，试想下，我们在写一个库的时候，时常会写这样的代码，拿webQQ的Jx库举例。</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%872.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%872.png" alt="JavaScript中有趣的反柯里化" width="554" height="149"></a><br>
我们想要的，其实只是借用Array原型链上的一些函数。并没有必要去显式的构造一个新的函数来改变它们的参数并且重新运算。</p>
<p>如果用uncurrying的方式显然更加优雅和美妙，就像这样：</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%873.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%873.png" alt="JavaScript中有趣的反柯里化" width="554" height="78"></a></p>
<p>还能做很多有趣和方便的事情.</p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE11.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE11.png" alt="JavaScript中有趣的反柯里化" width="536" height="172"></a></p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE21.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE21.png" alt="JavaScript中有趣的反柯里化" width="494" height="87"></a></p>
<p>甚至还能把call和apply方法都uncurrying，把函数也当作普通数据来使用. 使得javascript中的函数调用方式更像它的前生scheme, 当函数名本身是个变量的时候, 这种调用方法特别方便.</p>
<p>scheme里面调用函数是这样:<br>
<a href="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE4.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE4.png" alt="JavaScript中有趣的反柯里化" width="492" height="87"></a></p>
<p style="text-align:left">javascript里可以写的很接近.</p>
<p style="text-align:left"><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE3.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/QQ%E6%88%AA%E5%9B%BE3.png" alt="JavaScript中有趣的反柯里化" width="477" height="180"></a></p>
<p>再看看jquery库，由于jquery对象( 即通过$()创建的对象 )是一个对象冒充的伪数组，它有length属性，并且能够通过下标查找对应的元素，当需要给jquery对象添加一个成员时, 伪代码大概是：</p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%874.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%874.png" alt="JavaScript中有趣的反柯里化" width="499" height="105"></a></p>
<p>如果用uncurrying的话, 就可以</p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%875.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%875.png" alt="JavaScript中有趣的反柯里化" width="499" height="99"></a></p>
<p>借用了array对象的push函数, 让引擎去自动管理数组成员和length属性.</p>
<p>而且可以一次把需要的函数全部借过来, 一劳永逸. 一段测试代码:</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%876.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%876.png" alt="JavaScript中有趣的反柯里化" width="554" height="323"></a></p>
<p>总的来说, 使用uncurrying技术, 可以让任何对象拥有原生对象的方法. 好了，如果到这里依然没有引起你的兴趣，那么你可以去干点别的了。</p>
<p>接下来一步一步来看看原理以及实现。<br>
在了解反currying化这个奇怪的名字之前，我们得先搞清楚currying。</p>
<p>维基百科上的定义：科里化( currying ); 又称部分求值，是把接受多个参数的函数变换成接受一个单一参数的函数，并且返回接受余下的参数并且返回结果的新函数的技术。</p>
<p>通俗点讲，currying有点类似买房子时分期付款的方式，先给一部分首付( 一部分参数 )， 返回一个存折（ 返回一个函数 ），合适的时候再给余下的参数并且求值计算。</p>
<p>来看看我们都用过的currying, 我们经常在绑定context 的时候实现一个Function.prototype.bind函数.</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%877.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%877.png" alt="JavaScript中有趣的反柯里化" width="554" height="117"></a></p>
<p>高阶函数是实现currying的基础, 所谓高阶函数至少满足这2个特性：</p>
<p>1， 函数可以当作参数传递，</p>
<p>2， 函数可以作为返回值。</p>
<p>Javascript在设计之初，参考了很多scheme语言的特性。而scheme是函数式语言鼻祖lisp的2大方言之一，所以javascript也拥有一些函数式语言的特性，包括高阶函数，闭包，lambda表达式等。</p>
<p>当javascript中的函数返回另一个函数，此时会形成一个闭包，而在闭包中就可以保存第一次运算的参数，我们用这个思想，来写一个通用的currying函数。</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%878.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%878.png" alt="JavaScript中有趣的反柯里化" width="554" height="330"></a></p>
<p>我们约定, 当传入参数时候, 继续currying化, 参数为空时才开始求值.</p>
<p>假设在实现一个计算每月花费的函数, 每天结束前我们都要记录今天花了多少钱, 但我们只关心月底的花费总值, 无需每天都计算一次.</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%879.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%879.png" alt="JavaScript中有趣的反柯里化" width="554" height="337"></a></p>
<p>使用currying函数, 便可以延迟到最后一刻才一起计算, 好处不言而喻, 在很多场合可以避免无谓的计算, 节省性能, 也是实现惰性求值的一种方案.</p>
<p>好了，现在才走进正题，</p>
<p>curring是预先填入一些参数.</p>
<p>反curring就是把原来已经固定的参数或者this上下文等当作参数延迟到未来传递.</p>
<p>其实就是搞这样一个事情，将：</p>
<pre>obj.foo( arg1 ) //foo本来是只在obj上的函数. 就像push原本只在Array.prototype上</pre>
<p>转化成这样的形式</p>
<pre>foo( obj, arg1 ) // 跟我们举的第一个例子一样.将[].push转换成push( [] )</pre>
<p>就像原本是接在电视插头上的插座，把它拆下来之后，其实也能用来接冰箱。</p>
<p>Ecma上Array和String的每个原型方法后面都有这么一段话，比如push：</p>
<p>NOTE The push function is intentionally generic; it does not require that its this value be an Array object.<br>
Therefore it can be transferred to other kinds of objects for use as a method. Whether the concat function can be applied.</p>
<p>Javascript为什么要这样设计, 我们先来复习下动态语言中重要的鸭子类型思想.</p>
<p>说个故事：</p>
<p>很久以前有个皇帝喜欢听鸭子呱呱叫，于是他召集大臣组建一个一千只鸭子的合唱团。大臣把全国的鸭子都抓来了，最后始终还差一只。有天终于来了一只自告奋勇的鸡，这只鸡说它也会呱呱叫，好吧在这个故事的设定里，它确实会呱呱叫。 后来故事的发展很明显，这只鸡混到了鸭子的合唱团中。— 皇帝只是想听呱呱叫，他才不在乎你是鸭子还是鸡呢。</p>
<p>这个就是鸭子类型的概念，在javascript里面，很多函数都不做对象的类型检测，而是只关心这些对象能做什么。</p>
<p>Array构造器和String构造器的prototype上的方法就被特意设计成了鸭子类型。这些方法不对this的数据类型做任何校验。这也就是为什么arguments能冒充array调用push方法.</p>
<p>看下v8引擎里面Array.prototype.push的代码:</p>
<pre>function ArrayPush() {
 var n = TO_UINT32( this.length );
 var m = %_ArgumentsLength();
 for (var i = 0; i &lt; m; i++) {
 this[i+n] = %_Arguments(i); //属性拷贝
 this.length = n + m; //修正length
 return this.length;
 }
 }</pre>
<p>可以看到，ArrayPush方法没有对this的类型做任何显示的限制，所以理论上任何对象都可以被传入ArrayPush这个访问者。</p>
<p>我们需要解决的只剩下一个问题， 如何通过一种通用的方式来使得一个对象可以冒充array对象。</p>
<p>真正的实现代码其实很简单:</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%8710.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%8710.png" alt="JavaScript中有趣的反柯里化" width="554" height="135"></a></p>
<p>这段代码虽然很短, 初次理解的时候还是有点费力. 我们拿push的例子看看它发生了什么.</p>
<pre>var push = Array.prototype.push.uncurrying();
push( obj, ‘first’ );</pre>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%8711.png" rel="lightbox[32059]" title="JavaScript中有趣的反柯里化"><img title="JavaScript中有趣的反柯里化" src="http://blog.jobbole.com/wp-content/uploads/2013/01/%E5%9B%BE%E7%89%8711.png" alt="JavaScript中有趣的反柯里化" width="554" height="224"></a></p>
<p> </p>
<hr><small>Copyright © 2008<br> This feed is for personal, non-commercial use only. <br> The use of this feed on other websites breaches copyright. If this content is not in your news reader, it makes the page you are viewing an infringement of the copyright. (Digital Fingerprint:<br> )</small>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/30854/">JavaScript开发规范要求</a></li><li><a href="http://blog.jobbole.com/30468/">测试：你自认为理解了JavaScript？</a></li><li><a href="http://blog.jobbole.com/31178/">用Chrome开发者工具做JavaScript性能分析</a></li><li><a href="http://blog.jobbole.com/31166/">查询json数据结构的8种方式</a></li><li><a href="http://blog.jobbole.com/30134/">Testacular：Google开源的JavaScript测试执行过程管理工具</a></li><li><a href="http://blog.jobbole.com/30738/">寻找更好的Javascript单元测试工具</a></li><li><a href="http://blog.jobbole.com/31213/">headtrackr：一个头部/脸部追踪的JavaScript库</a></li><li><a href="http://blog.jobbole.com/31274/">拥抱原型面向对象编程</a></li><li><a href="http://blog.jobbole.com/31104/">简讯：Amazon拥抱Node.js</a></li><li><a href="http://blog.jobbole.com/21962/">高性能JavaScript模板引擎原理解析</a></li></ul>