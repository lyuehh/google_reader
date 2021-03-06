---
layout: post
title:  "JavaScript语句后应该加分号么？"
date:   2012-06-19 03:10:13
author: 
categories: program
---

## JavaScript语句后应该加分号么？
### by 
### at 2012-06-19 03:10:13
### original <http://hax.iteye.com/blog/1563585>

<span style="font-size:medium">这是一个老生常谈的问题了。我之前就曾经写过<a href="http://hax.iteye.com/blog/382186">一篇blog</a>记录了我对此问题的实践与思考之旅。最近在知乎上又出现了这方面的争论，而且几乎是一面倒的支持“总是写分号”。这让我深深觉得是时候正本清源，祛除迷信了。于是我在问题<a href="http://www.zhihu.com/question/20298345">http://www.zhihu.com/question/20298345</a>下，花了整整一天时间写了以下的回答。
<br>
<br>重新发在blog上，主要是因为此文过长，作为知乎的答案或许应该精简一下，但全文内容乃心血结晶，值得留存，照录如下。
<br>
<br>
<br>首先，加还是不加，这是一个书写风格问题。而书写风格通常有一些外在的考量，比如团队所建立的规则或习惯。@玉伯  的<a href="http://www.zhihu.com/question/20298345/answer/14666757">答案</a>就是基于此。我对此基本赞同，不过这其实有点避重就轻，呵呵。另外，即使团队有这样的规则，也未必要通过强制在写代码的时候就要这样写，而可以通过工具达成。比如在源码管理工具上挂上钩子，对提交的源代码自动整理格式。
<br>
<br>其次，<a href="http://www.zhihu.com/question/20298345/answer/14665666">很多人提到代码压缩问题</a>。我觉得这是<strong>非常扯淡</strong>的理由。如果2012年的今天一个JS压缩器还不能正确处理分号，这只能说明这个JS压缩器没有达到基本的质量要求，根本不值得信任。
<br>
<br>@冯超 和 @CSS魔法 <a href="http://www.zhihu.com/question/20298345/answer/14667309">提到的jslint</a>也是一个工具的反面例子。工具是帮助人的，而不应该是<strong>强迫</strong>人的。不明白这一点，你就不会理解为什么在已经有jslint很多年的情况下，还会出现jshint。
<br>
<br>jshint对于不写分号会报warn，但可以通过asi选项关闭（在文件头加上/* jshint asi:true */即可）。
<br>
<br>在asi选项说明里，<a href="http://www.jshint.com/options/">jshint的文档</a>是这样写的：
<br><div>引用</div><div>There is a lot of FUD (fear, uncertainty and doubt) spread about semicolon spreaded by quite a few people in the community. The common myths are that semicolons are required all the time (they are not) and that they are unreliable. JavaScript has rules about semicolons which are followed by <strong>all</strong> browsers so it is up to you to decide whether you should or should not use semicolons in your code.</div>
<br>
<br>翻译如下（【】里是我添加的说明）：
<br>
<br><div>引用</div><div>关于分号有大量的FUD，且是由社区里的一小撮人【你知道是指谁】散布的。一个常见的流言是必须写分号，不写分号不可靠【流言的意思是不写分号会导致代码行为不确定】。实际上JS有明确的分号规则，并且<strong>所有</strong>浏览器【居然】都忠实遵守了规则。所以是否应该在你的代码里使用分号，完全可以由你自己决定【而不是由一小撮流言散布者或二逼工具强加于你】。</div>
<br>
<br>所以对于可不可以不加分号这个问题，社区是有结论的。
<br>
<br>然后所谓“应该不应该”，就只是利弊分析，而不是非黑即白。其中也必定有一些如“可维护性”、“可理解性”甚至“代码美感”之类的貌似“贱人贱智”的问题。不过我相信有经验的程序员还是会在大多数问题上找到共识的。
<br>
<br>
<br>这个世界上有许多语言。大量语言是不用分号作为EOS（End of Statement）的。有些偏执狂认为不用分号的语言都是垃圾，对此我没啥好说的。
<br>
<br>有些语言也是可选分号，比如python。python是可以加分号作为语句结束的。当然绝大多数python程序员是不会加分号的（除了在一行里写多个语句）。所以python和js一样是可选分号！并且python的习惯是不写分号（仅在极少数情况下写）！
<br>
<br>也有不少人会指摘python的语法太特殊，比如缩进啥的……不能算是c-style的。不过即使是C风格的语言，也有不写分号的，比如groovy。groovy和js一样是可选分号！并且groovy的习惯是不写分号（仅在极少数情况下写）！
<br>
<br>所以至少从同样两个是可选分号的语言来看，不写分号在实践上是可行的。毕竟，既然被设计为可选，那么合理的推断是：语言的设计初衷是倾向于鼓励不写分号。
<br>
<br>实际上，不少人（包括我）认为，c-style的分号<strong>本来就是多余</strong>的。为什么这么说？因为明确的EOS只是给编译器的提示而已。如果漏了分号，编译器会报错。既然它都报错了，显然它知道这里应该有EOS。既然它知道，那么干嘛还要我写？
<br>
<br>给编译器以hint，这在几十年前是一个平衡编译器和用户成本的设计。某些语言（如Fortran、Basic等）选择用换行来作为EOS，这样每行只能一个语句，并且一个语句折行必须用特殊的接续符号。某些语言（如C）则选择了通过分号来达成，这样每行可以多个语句，并且一个语句也可以分布在多行。平心而论，我更喜欢前一种策略。不过现实是c-style的语法流传更广，至少当前的工业主流语言都是c-style的。
<br>
<br>在c-style语言中，如果既要允许自由折行，又要避免额外的EOS（分号），编译器会较为复杂，光靠看token是不能确定语句是否结束的（即换行处有可能是语句结束，也有可能不是）——尽管在实践中只需要很少的规则，人就能一目了然的看清语句是否结束，但是parser要处理一切的极端情况，例如在换行前插入注释到底怎么算。而C的设计是遵循所谓worse is better的哲学，非常强调<strong>实现简单</strong>，一个明确的EOS对于编译器来说绝对是简单的。当初如果有人找K&amp;R去要求应该由编译器判断这里该不该是语句结束，我打包票肯定被K&amp;R扁死。有趣的是，lisp那一帮人更极端，如果你抱怨括号实在太密密麻麻的了，一定有人语重心长的告诉你S表达式才是王道。
<br>
<br>其实像C++编译器也已经复杂到超乎想象，按理说可选分号真是小事一桩，但它因为要保持对C的完全兼容，所以还是必须写分号。
<br>
<br>python和groovy的parser则都是有名的复杂。这并不完全由允许分号可选造成，但是可选的分号其实是整个语法设计哲学的一环。如Groovy的哲学是PHIM——Parse how I mean。
<br>
<br>话说python的语法设计真的非常有意思。它也有问题，比如tab和空格混合，计算机之子@程劭非 曾经惊叹，居然有语言能通过改变注释（注释中可定义tabsize）就改变了语义和行为，真是极品。
<br>
<br>当然后来者会吸取教训，比如coffeescript和jade之类的，也都是依赖缩进，但是都不允许tab和空格混用。
<br>
<br>所以tab/sp这是python的坑。Guido Van Rossum现在就后悔了。从某种程度上说，JavaScript的分号就有点类似python的tab/sp问题。
<br>
<br>正如混合tab/sp是出自GVR的良好初衷（让你们想用啥就用啥），可选分号也是出自BE的良好初衷（随便你写不写）。也如同tab/sp一样，良好的初衷并不代表就没有隐患。之所以python、groovy就没有可选分号的争议，而js就有争议，其实正说明js存在一些问题。
<br>
<br>其实Groovy历史上也是有关于可选分号争议的，参见：<a href="http://blog.csdn.net/hax/article/details/139490">http://blog.csdn.net/hax/article/details/139490</a>。不幸的的是，与Groovy早期经过社区激烈的讨论才得到稳定语法不同，JS是一门早熟的语言，一些早期的设计失误没有机会被修复。自动分号插入算法就是其中之一。总体上，自动分号插入算法还算正常，但是在一些小地方留下了不易发觉的坑。比如return语句。
<br></span>
<br>
<br><pre name="code">
return
{
 a:1
}
</pre>
<br><span style="font-size:medium">在return后会自动插入分号，导致完全违背期望的结果。
<br>
<br>这一古怪行为往往被解释为在JS中应采用一行内跟随大括号的书写风格（即Java的风格，或者说是K&amp;R的C的原初风格，而不是C#风格），其实追根述源，问题还是出在分号上。
<br>
<br>不要插分号的地方被插了分号，这挺坑爹了，但更更坑爹的是想要插的结果没插。这就是括号的问题。如果下一行的开始是“(”、“[”上一行的结尾不会被加上“;”。
<br>
<br>如：</span>
<br><pre name="code">a = b
(function(){
  ...
})()</pre>
<br><span style="font-size:medium">
<br>会被解释为</span><pre name="code">a = b(function(){...})()</pre>
<br>
<br>
<br><span style="font-size:medium">其实如果我们真想表达上述代码，通常会这样写：</span>
<br><pre name="code">a = b(function(){
  ...
})()
</pre>
<br><span style="font-size:medium">再如：</span>
<br><pre name="code">a = b
[1,2,3].forEach(function(e){
 console.log(e)
})</pre>
<br>
<br><span style="font-size:medium">实际效果等价于</span>
<br><pre name="code">a = b[3].forEach(function(e){
  console.log(e)
})</pre>
<br>
<br><span style="font-size:medium">坑爹的是，搞不好这代码说不定还能运行！你要事后通过调试发现这些错误是相当滴痛苦啊。
<br>
<br>当然这也不能全赖BE。在JS的早期，还没有数组迭代方法 Array.prototype.forEach/map/filter...等，也没有今天常见的 (function(){...})() 惯用法，所以这个问题其实很不明显。但是到了今天，这些坑爹的问题就都冒出来了。
<br>
<br>实际上，“+”、“-”、“/”也有问题，但是我们几乎不会在实践中遇到。因为你几乎不可能会写出行首以“+”、“-”、“/”开始的语句，除了 ++i 之类的语句（但是其实我们都会写成 i++）。
<br>
<br>不过这些问题的解决方案其实也很简单。只要在“[”、“(”、“+”、“-”、“/”等之前加分号就可以了：</span>
<br><pre name="code">a = b
;(function(){
  ...
})()

a = b
;[1,2,3].forEach(function(e){
  console.log(e)
})
</pre>
<br>
<br><span style="font-size:medium">
<br>有些同学觉得这样很丑。没问题，你可以用 void 替代“;”。
<br>
<br>也有不少人觉得<a href="http://www.zhihu.com/question/20298345/answer/14662711">这是一种“不一致”</a>，需要记住额外的法则。
<br>
<br>我承认采取这样一种方法你必须记住一些特例。但是几乎所有的语言都有一些历史原因导致的坑，并且JS也不止这一个坑。更关键的是，即使你采用了总是写“;”的方法，仍然不能避免掉进EOS的坑，因为造成问题的asi特性仍然存在。比如之前提到的return后面会自动插分号的问题。
<br>
<br>“总是写分号”，相比“不写分号但是edge case要在行首加分号”，<strong>看上去</strong>要更“简单”，但这只是描述简单，<strong>实际做起来</strong>未必更简单。
<br>
<br>比如你必须要记得，function表达式后面也要写“;”！
<br>
<br>如：</span>
<br><pre name="code">function a() {
 ...
}
[1,2,3].forEach(...)
</pre>
<br><span style="font-size:medium">
<br>这代码是没问题的，但是你改成</span>
<br><pre name="code">var a = function () {
  ...
}
[1,2,3].forEach(...)
</pre>
<br><span style="font-size:medium">
<br>就有问题了！这坑爹！
<br>
<br>对于“始终加分号派”来说，结果就会变成函数后面也一定要加分号。（你分得清函数声明和函数表达式吗？坑爹啊，不如都加！）但是为什么函数就加而 if ... {} 或 for (...) {...} 结构里的大括号后面就不加分号呢？这不是也不一致嘛。
<br>
<br>而且，同样是一条特殊规则，<strong>行首加分号的规则比函数表达式后面加分号的规则其实要简单</strong>！</span>
<br>
<br><pre name="code">var a = function () {
  ...
}
[1,2,3].forEach(...)</pre>
<br>
<br><span style="font-size:medium">还是以上面代码为例。
<br>
<br>行首是否要加分号，我只要看<strong>本行的第一个字符</strong>就可以了。因为对于object[prop]这样的意图，其实没有程序员会写出</span>
<br><pre name="code">object
[prop]</pre>
<br><span style="font-size:medium">这样的代码。如果他要折行，一定是写成</span>
<br><pre name="code">object[
 prop
]
</pre>
<br><span style="font-size:medium">所以行首第一个字符如果是括号，毋庸置疑的，这一定是一个新语句的开始。
<br>
<br>反过来，你如果要判断“}”后面是否要加“;”，你得向上回溯，看清楚整段代码是一个结构呢？还是一个函数？如果是函数的话，是函数声明呢？还是函数表达式！
<br>
<br>许多时候，你可能向上翻几页还没找到对应的“{”！或者已经忘记了是几层缩进了！
<br>
<br>由此可见，对于人来说，行首特例加分号的策略其实更简单易行。而总是加分号的策略听上去简单，执行起来却难！除非你的策略最后变成了所有“}”之后都加分号——我真见过有人这么做的。
<br>
<br>
<br>对人是这样，下面再来看看对机器（引入工具）的情形。特别的，因为有不少人表示他遵循总是写分号的方式是因为他严重依赖jslint。所以我就拿jslint开刀。
<br>
<br>对于总是加分号的策略，你希望工具能提示你哪里缺少分号。但是实际情况是，你必须尽量避免写出有歧义的跨行语句，因为工具很难判断是有意为之，还是忘记写“;”。
<br>
<br>比如：</span>
<br><pre name="code">a = b
(function(){
...
})();
</pre>
<br><span style="font-size:medium">这代码在jslint的提示是：Expected '(' at column 5, not column 1.
<br>
<br>请问你是应该真的按照它的提示把括号移动到b后面吗？？
<br>
<br>仔细考虑一下，你就知道这个问题不好回答。因为jslint给出的建议其实是基于“这是合法的代码，只是格式不妥”。虽然我们都知道这<strong>更可能</strong>是忘记写分号。
<br>
<br>再来一个更坑爹的例子：</span>
<br>
<br><pre name="code">/*jslint white: true */
var a,b,c,d,e,f,g,h,i,j,k,l,m,o,s;
a=b+c*d-e
   /f/g-h*i/j
/f/g.exec(s).map(f);</pre>
<br>
<br><span style="font-size:medium">这段代码在jslint里是<strong>不报错</strong>的！！！
<br>
<br>但是我们是可以看出来这代码很有可能是缺少分号。
<br>
<br>这里可以看出，如果排除了whitespace的格式提示（这事儿还是挺常见的，毕竟许多人不喜欢被强制加那么多空格规则），jslint其实无法在我们最需要帮助的时候帮到我们！因为它无法判断这个地方到底是有意为之（不用“;”而跨行），还是忘记写“;”。
<br>
<br>反过来说，如果采取行首特例加“;”的习惯，其实工具是很容易判断你是否忘记加了分号。如果加上一些对缩进信息的判断来排除极少数不良的折行习惯（出warning即可），工具甚至能自动把所有这类分号都加上。
<br>
<br>
<br>两种策略：
<br>
<br>1. 我总是写分号，让工具告诉我哪里我忘记写了（但是有时候可能还报不出来，或报了个其他信息）
<br>
<br>2. 我总是不写分号，让工具自动把（由于语言设计缺陷所要求的）必须的分号加上去
<br>
<br>哪种更好？
<br>
<br>
<br>总结：
<br>
<br>我所推荐的不写分号的方式，其实不仅是不写分号，而是同时采用更严格的跨行策略，即只允许在当前行处于未完成状态时跨行（就像你在jsshell中输入代码一样）。这条规则其实并不需要特别强制，因为绝大多数程序员一直就是这样在执行。诚然，存在少数人习惯写这样有歧义的折行代码：</span>
<br>
<br><pre name="code">a = b + c
     + d + e
     + f  + g</pre>
<br>
<br><span style="font-size:medium">但是这个习惯不难纠正，并且工具根据缩进等信息是完全能检测到的。
<br>
<br>
<br>说到这里，也许有些同志认为这只能说明jslint太挫，不能证明到处写“;”的风格不好。因为工具也可以同时加上其他限制嘛。不过你仔细想想，可以发现这是一个悖论。如果jslint够智能，引入了其他与分号无关的代码风格要求，比如空格和缩进，还有折行风格，确实也可以更精确的找到所有漏掉分号的地方。但是那无非再次证明了一点：<strong>编译器（代码分析器）完全可以知道哪里应该有EOS</strong>。既然所有的分号其实可以由机器自行加上（无论是加在行首还是行尾），那么我们自己还要手写所有分号的意义到底在哪里？！
<br>
<br>以上。</span>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hax.iteye.com/blog/1563585#comments" style="color:red">已有 <strong>11</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hax.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>