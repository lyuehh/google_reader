---
layout: post
title:  "可爱的 Python : Python中函数式编程，第二部分"
date:   2013-03-07 16:23:40
author: 童海波
categories: program
---

## 可爱的 Python : Python中函数式编程，第二部分
### by 童海波
### at 2013-03-07 16:23:40
### original <http://blog.jobbole.com/35042/?utm_source=rss&utm_medium=rss&utm_campaign=%25e5%258f%25af%25e7%2588%25b1%25e7%259a%2584-python-python%25e4%25b8%25ad%25e5%2587%25bd%25e6%2595%25b0%25e5%25bc%258f%25e7%25bc%2596%25e7%25a8%258b%25ef%25bc%258c%25e7%25ac%25ac%25e4%25ba%258c%25e9%2583%25a8%25e5%2588%2586>

<p>英文原文：<a style="font-style:normal;font-weight:bold" href="http://www.ibm.com/developerworks/linux/library/l-prog2/index.html">Charming Python: Functional programming in Python, Part 2</a>，翻译：<a href="http://www.oschina.net/translate/python-functional-programming-part2">开源中国</a></p>
<p><strong>摘要：</strong>  本专栏继续让David对Python中的函数式编程(FP)进行介绍。读完本文，可以享受到使用不同的编程范型（paradigm）解决问题所带来的乐趣。David在本文中对FP中的多个中级和高级概念进行了详细的讲解。</p>
<p>一个对象就是附有若干过程（procedure）的一段数据。。。一个闭包（closure）就是附有一段数据的一个过程（procedure）。</p>
<p>在我讲解函数式编程的上一篇文章，<a href="http://blog.jobbole.com/35028/" rel="nofollow">第一部分</a>，中，我介绍了FP中的一些基本概念。 本文将更加深入的对这个内容十分丰富的概念领域进行探讨。在我们探讨的大部分内容中，Bryn Keller的”Xoltar Toolkit”为我们提供一些非常有价值的帮助作用。Keller将FP中的许多强项集中到了一个很棒且很小的模块中，他在这个模块中用纯Python代码实现了这些强项。除了<em>functional</em>模块外，Xoltar Toolkit还包含了一个<em>延迟（</em><em>lazy</em><em>）</em>模块，对“仅在需要时”才进行求值提供了支持。许多传统的函数式语言中也都具有延迟求值的手段，这样，使用Xoltar Toolkit中的这些组件，你就可以做到使用象Haskell这样的函数式语言能够做到的大部分事情了。</p>
<p><strong>绑定（Binding）</strong></p>
<p>有心的读者会记得，我在第一部分中所述的函数式技术中指出过Python的一个局限。具体讲，就是Python中没有任何手段禁止对用来指代函数式表达式的名字进行重新绑定。 在FP中，名字一般是理解为对比较长的表达式的简称，但这里面隐含了一个诺言，就是“同一个表达式总是具有同一个值”。如果对用来指代的名字重新进行绑定，就会违背这个诺言。例如， 假如我们如以下所示，定义了一些要用在函数式程序中的简记表达式:<br>
Python中由于重新绑定而引起问题的FP编程片段</p>
<pre>&gt;&gt;&gt; car = lambda lst: lst[0]
&gt;&gt;&gt; cdr = lambda lst: lst[1:]
&gt;&gt;&gt; sum2 = lambda lst: car(lst)+car(cdr(lst))
&gt;&gt;&gt; sum2(range(10))
1
&gt;&gt;&gt; car = lambda lst: lst[2]
&gt;&gt;&gt; sum2(range(10))
5</pre>
<p>非常不幸，程序中完全相同的表达式sum2(range(10))在两个不同的点求得的值却不相同, 尽管在该表达式的参数中根本没有使用任何可变的（mutable）变量。</p>
<p>幸运的是， functional模块提供了一个叫做Bindings(由鄙人向Keller进行的提议，proposed to Keller by yours truly)的类，可以用来避免这种重新绑定（至少可以避免意外的重新绑定，Python并不阻止任何拿定主意就是要打破规则的<span><a href="http://blog.jobbole.com/821/" title="程序员的本质">程序员</a></span>）。尽管要用Bindings类就需要使用一些额外的语法，但这么做就能让这种事故不太容易发生。 Keller在functional模块里给出的例子中，有个Bindings的实例名字叫做let（我推测这么叫是为了仿照ML族语言中的let关键字）。例如，我们可以这么做：<br>
Python中对重新绑定进行监视后的FP编程片段</p>
<pre>&gt;&gt;&gt; from functional import *
&gt;&gt;&gt; let = Bindings()
&gt;&gt;&gt; let.car = lambda lst: lst[0]
&gt;&gt;&gt; let.car = lambda lst: lst[2]
Traceback (innermost last):
  File &quot;&lt;stdin&gt;&quot;, line 1, in ?
  File &quot;d:\tools\functional.py&quot;, line 976, in __setattr__
    raise BindingError, &quot;Binding &#39;%s&#39; cannot be modified.&quot; % name
functional.BindingError:  Binding &#39;car&#39; cannot be modified.
&gt;&gt;&gt; car(range(10))
0</pre>
<p>显而易见，在真正的程序中应该去做一些事情，捕获这种”BindingError”异常，但发出这些异常这件事，就能够避免产生这一大类的问题。</p>
<p>functional模块随同Bindings一起还提供了一个叫做namespace的函数，这个函数从Bindings实例中弄出了一个命名空间 (实际就是个字典) 。如果你想计算一个表达式，而该表达式是在定义于一个Bindings中的一个（不可变）命名空间中时，这个函数就可以很方便地拿来使用。Python的eval()函数允许在命名空间中进行求值。举个例子就能说明这一切：<br>
Python中使用不可变命名空间的FP编程片段</p>
<pre>&gt;&gt;&gt; let = Bindings()      # &quot;Real world&quot; function names
&gt;&gt;&gt; let.r10 = range(10)
&gt;&gt;&gt; let.car = lambda lst: lst[0]
&gt;&gt;&gt; let.cdr = lambda lst: lst[1:]
&gt;&gt;&gt; eval(&#39;car(r10)+car(cdr(r10))&#39;, namespace(let))
&gt;&gt;&gt; inv = Bindings()      # &quot;Inverted list&quot; function names
&gt;&gt;&gt; inv.r10 = let.r10
&gt;&gt;&gt; inv.car = lambda lst: lst[-1]
&gt;&gt;&gt; inv.cdr = lambda lst: lst[:-1]
&gt;&gt;&gt; eval(&#39;car(r10)+car(cdr(r10))&#39;, namespace(inv))
17</pre>
<p>FP中有一个特别有引人关注的概念叫做闭包。实际上，闭包充分引起了很多程序员的关注，即使通常意义上的非函数式<span><a href="http://blog.jobbole.com/tag/%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80/" title="如何选择语言和编程语言排名相关文章">编程语言</a></span>，比如Perl和Ruby，都包含了闭包这一特性。此外，Python 2.1 目前一定会添加上词法域（lexical scoping）， 这样一来就提供的闭包的绝大多数功能。<br>
那么，闭包到底是什么？Steve Majewski最近在Python新闻组中对这个概念的特性提出了一个准确的描述：<br>
就是说，闭包就象是FP的Jekyll，OOP（面向对象编程）的 Hyde （或者可能是将这两个角色互换）（译者注：Jekyll和Hyde是一部小说中的两个人物）. 和象对象实例类似，闭包是一种把一堆数据和一些功能打包一起进行传递的手段。</p>
<p>先让我们后退一小步，看看对象和闭包都能解决一些什么样的问题，然后再看看在两样都不用的情况下这些问题是如何得到解决的。函数返回的值通常是由它在计算过程中使用的上下文决定的。最常见可能也是最显然的指定该上下文的方式就是给函数传递一些参数，让该函数对这些参数进行一些运算。但有时候在参数的“背景”（background）和“前景”（foreground）两者之间也有一种自然的区分，也就是说，函数在某特定时刻正在做什么和函数“被配置”为处于多种可能的调用情况之下这两者之间有不同之处。<br>
在集中处理前景的同时，有多种方式进行背景处理。一种就是“忍辱负重”，每次调用时都将函数需要的每个参数传递给函数。这通常就相对于在函数调用链中不断的将很多值（或者是一个具有很多字段的数据结构）传上传下，就是因为在链中的某个地方可能会用到这些值。下面举个简单的例子：<br>
用了货船变量的Python代码片段</p>
<pre>&gt;&gt;&gt; def a(n):
...     add7 = b(n)
...     return add7
...
&gt;&gt;&gt; def b(n):
...     i = 7
...     j = c(i,n)
...     return j
...
&gt;&gt;&gt; def c(i,n):
...     return i+n
...
&gt;&gt;&gt; a(10)     # Pass cargo value for use downstream
17</pre>
<p>在上述的货船变量例子中，函数b()中的变量n毫无意义，就只是为了传递给函数c()。另一种办法是使用全局变量:<br>
使用全局变量的Python代码片段</p>
<pre>&gt;&gt;&gt; N = 10
&gt;&gt;&gt; def  addN(i):
...     global N
...     return i+N
...
&gt;&gt;&gt; addN(7)   # Add global N to argument
17
&gt;&gt;&gt; N = 20
&gt;&gt;&gt; addN(6)   # Add global N to argument
26</pre>
<p>全局变量N只要你想调用ddN()就可以直接使用，就不需要显式地传递这个全局背景“上下文”了。有个稍微更加Python化的技巧，可以用来在定义函数时，通过使用缺省参数将一个变量“冻结”到该函数中：<br>
使用冻结变量的Python代码片段</p>
<pre>&gt;&gt;&gt; N = 10
&gt;&gt;&gt; def addN(i, n=N):
...     return i+n
...
&gt;&gt;&gt; addN(5)   # Add 10
15
&gt;&gt;&gt; N = 20
&gt;&gt;&gt; addN(6)   # Add 10 (current N doesn&#39;t matter)
16</pre>
<p>我们冻结的变量实质上就是个闭包。我们将一些数据“附加”到了addN()函数之上。对于一个完整的闭包而言，在函数addN()定义时所出现的数据，应该在该函数被调用时也可以拿到。然而，本例中（以及更多更健壮的例子中），使用缺省参数让足够的数据可用非常简单。函数addN()不再使用的变量因而对计算结构捕获产生丝毫影响。</p>
<p>现在让我们再看一个用OOP的方式解决一个稍微更加现实的问题。今年到了这个时候，让我想起了颇具“面试”风格的计税程序，先收集一些数据，数据不一定有什么特别的顺序，最后使用所有这些数据进行一个计算。让我们为这种情况些个简化版本的程序：<br>
Python风格的计税类/实例</p>
<pre>class TaxCalc:
    def taxdue(self):return (self.income-self.deduct)*self.rate
taxclass = TaxCalc()
taxclass.income = 50000
taxclass.rate = 0.30
taxclass.deduct = 10000
print&quot;Pythonic OOP taxes due =&quot;, taxclass.taxdue()</pre>
<p>在我们的TaxCalc类 (或者更准确的讲，在它的实例中)，我们先收集了一些数据，数据的顺序随心所欲，然后所有需要的数据收集完成后，我们可以调用这个对象的一个方法，对这堆数据进行计算。所有的一切都呆在一个实例中，而且，不同的实例可以拥有一堆不同的数据。能够创建多个实例，而多个实例仅仅是数据不同，这通过“全局变量”和“冻结变量”这两种方法是无法办到的。”货船”方法能够做到这一点，但从那个展开的例子中我们能够看出，它可能不得不在开始时就传递多个数值。讨论到这里，注意到OOP风格的消息传递方式可能会如何来解决这一问题会非常有趣(Smalltalk或者Self与此类似，我所用过的好几种xBase的变种OOP语言也是类似的）：</p>
<p>Smalltalk风格的(Python) 计税程序</p>
<pre>class TaxCalc:
    def taxdue(self):return (self.income-self.deduct)*self.rate
    def setIncome(self,income):
        self.income = income
        return self
    def setDeduct(self,deduct):
        self.deduct = deduct
        return self
    def setRate(self,rate):
        self.rate = rate
        return self
print&quot;Smalltalk-style taxes due =&quot;, \
      TaxCalc().setIncome(50000).setRate(0.30).setDeduct(10000).taxdue()</pre>
<p>每个”setter”方法都返回self可以让我们将每个方法调用的结果当作“当前”对象进行处理。这和FP中的闭包方式有些相似。</p>
<p>通过使用Xoltar toolkit，我们可以生成完整的闭包，能够将数据和函数结合起来，获得我们所需的特性；另外还可以让多个闭包（以前成为对象）包含不同的数据：<br>
Python的函数式风格的计税程序</p>
<pre>from functional import *

taxdue        = lambda: (income-deduct)*rate
incomeClosure = lambda income,taxdue: closure(taxdue)
deductClosure = lambda deduct,taxdue: closure(taxdue)
rateClosure   = lambda rate,taxdue: closure(taxdue)

taxFP = taxdue
taxFP = incomeClosure(50000,taxFP)
taxFP = rateClosure(0.30,taxFP)
taxFP = deductClosure(10000,taxFP)
print&quot;Functional taxes due =&quot;,taxFP()

print&quot;Lisp-style taxes due =&quot;, \
      incomeClosure(50000,
          rateClosure(0.30,
              deductClosure(10000, taxdue)))()</pre>
<p>我们所定义的每个闭包函数可以获取函数定义范围内的任意值，然后将这些值绑定到改函数对象的全局范围之中。然而，一个函数的全局范围并不一定就是真正的模块全局范围，也和不同的闭包的“全局”范围不相同。闭包就是“将数据带”在了身边。<br>
在我们的例子中，我们利用了一些特殊的函数把特定的绑定限定到了一个闭包作用范围之中(income, deduct, rate)。要想修改设计，将任意的绑定限定在闭包之中，也非常简单。只是为了好玩，在本例子中我们也使用了两种稍微不同的函数式风格。第一种风格连续将多个值绑定到了闭包的作用范围；通过允许taxFP成为可变的变量，这些“添加绑定”的代码行可以任意顺序出现。然而，如果我们想要使用tax_with_Income这样的不可变名字，我们就需要以特定的顺序来安排这几行进行绑定的代码，将靠前的绑定结果传递给下一个绑定。无论在哪种情况下，在全部所需数据都绑定进闭包范围之后，我们就可以调用“种子”（seeded）方法了。</p>
<p>第二种风格在我看来，更象是Lisp（那些括号最象了）。除去美学问题，这第二种风格有两点值得注意。第一点就是完全避免了名字绑定，变成了一个单个的表达式，连语句都没有使用（关于为什么不使用语句很重要，请参见 P第一部分）。<br>
第二点是闭包的“Lips”风格的用法和前文给出的“Smalltalk”风格的信息传递何其相似。实际上两者都在调用taxdue()函数/方法的过程中积累了所有值(如果以这种原始的方式拿不到正确的数据，两种方式都会出错）。“Smalltalk”风格的方法中每一步传递的是一个对象，而“Lisp”风格的方法中传递是持续进行的。 但实际上，函数式编程和面向对象式编程两者旗鼓相当。</p>
<p>在本文中，我们干掉了函数式编程领域中更多的内容。剩下的要比以前（本小节的题目是个小玩笑；很不幸，这里还没有解释过尾递归的概念）少多了（或者可以证明也简单多了？）。阅读functional模块中的源代码是继续探索FP中大量概念的一种非常好的方法。该模块中的注释很完备，在注释里为模块中的大多数方法/类提供了相关的例子。其中有很多简化性的元函数（meta-function）本专栏里并没有讨论到的，使用这些元函数可以大大简化对其它函数的结合（combination）和交互（interaction ）的处理。对于想继续探索函数式范型的Python程序员而言，这些绝对值得好好看看。</p>
<h4>相关文章</h4>
<ul>
<li><a href="http://blog.jobbole.com/35045/"><img src="http://www.python.org/community/logos/python-logo-master-v3-TM.png" alt="可爱的 Python : Python中的函数式编程，第三部分"></a><a href="http://blog.jobbole.com/35045/">可爱的 Python : Python中的函数式编程，第三部分</a></li>
<li><a href="http://blog.jobbole.com/35028/"><img src="http://www.python.org/community/logos/python-logo-master-v3-TM.png" alt="可爱的 Python : Python中函数式编程，第一部分"></a><a href="http://blog.jobbole.com/35028/">可爱的 Python : Python中函数式编程，第一部分</a></li>
<li><a href="http://blog.jobbole.com/28506/"><img src="http://blog.jobbole.com/wp-content/uploads/2012/02/python-logo.png" alt="Python关键字yield详解"></a><a href="http://blog.jobbole.com/28506/">Python关键字yield详解</a></li>
<li><a href="http://blog.jobbole.com/25253/"><img width="150" height="125" src="http://blog.jobbole.com/wp-content/uploads/2012/08/scheme-150x125.jpg" alt="scheme"></a><a href="http://blog.jobbole.com/25253/">王垠：怎样写一个解释器</a></li>
<li><a href="http://blog.jobbole.com/19835/"><img width="150" height="150" src="http://blog.jobbole.com/wp-content/uploads/2012/05/import_python-150x150.jpg" alt="import_python"></a><a href="http://blog.jobbole.com/19835/">Python编程中需要注意的一些事</a></li>
<li><a href="http://blog.jobbole.com/20874/"><img src="http://blog.jobbole.com/wp-content/uploads/2013/02/Summer_Glau_as_River_Tam_in_Serenity_Wallpaper__yvt2-300x2253-150x150.jpg" alt="我的大脑不能再处理面向对象编程了 "></a><a href="http://blog.jobbole.com/20874/">我的大脑不能再处理面向对象编程了 </a></li>
<li><a href="http://blog.jobbole.com/15005/"><img src="http://blog.jobbole.com/wp-content/uploads/2012/02/python-logo.png" alt="趣文：Python程序员的进化史"></a><a href="http://blog.jobbole.com/15005/">趣文：Python程序员的进化史</a></li>
<li><a href="http://blog.jobbole.com/21351/"><img width="150" height="150" src="http://blog.jobbole.com/wp-content/uploads/2012/06/python-icon-150x150.jpg" alt="python-icon"></a><a href="http://blog.jobbole.com/21351/">深刻理解Python中的元类(metaclass)</a></li>
<li><a href="http://blog.jobbole.com/32876/"><img src="http://blog.jobbole.com/wp-content/uploads/2012/02/python-logo.png" alt="Python yield 使用浅析"></a><a href="http://blog.jobbole.com/32876/">Python yield 使用浅析</a></li>
<li><a href="http://blog.jobbole.com/34289/"><img src="http://bind10.isc.org/raw-attachment/wiki/Mascot/BIND10_Bundy.jpg" alt="为什么 BIND 10 要用 C++ 和 Python 来写"></a><a href="http://blog.jobbole.com/34289/">为什么 BIND 10 要用 C++ 和 Python 来写</a></li>
</ul>
<p><a href="http://blog.jobbole.com/35042/">可爱的 Python : Python中函数式编程，第二部分</a>，首发于<a href="http://blog.jobbole.com">博客 - 伯乐在线</a>。</p>