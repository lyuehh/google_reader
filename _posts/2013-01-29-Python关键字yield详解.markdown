---
layout: post
title:  "Python关键字yield详解"
date:   2013-01-29 09:43:42
author: 
categories: program
---

## Python关键字yield详解
### by 
### at 2013-01-29 09:43:42
### original <http://blog.jobbole.com/28506/?utm_source=rss&utm_medium=rss&utm_campaign=python%25e5%2585%25b3%25e9%2594%25ae%25e5%25ad%2597yield%25e8%25af%25a6%25e8%25a7%25a3>

<p><strong>导读：</strong>此文由<a href="http://www.jobbole.com/">伯乐</a>在线 –<a href="http://blog.jobbole.com/author/%E5%88%98%E5%BF%97%E5%86%9B/">刘志军</a>编译自stackoverflow Python标签中投票率最高的一个问题《<a href="http://stackoverflow.com/questions/231767/the-python-yield-keyword-explained">The Python yield keyword explained</a>》，<a href="http://stackoverflow.com/users/9951/e-satis">e-satis</a> 详细回答了关于yield 以及 generator、iterable、iterator、iteration之间的关系。</p>
<p><strong>迭代器(Iterator)</strong></p>
<p>为了理解yield是什么，首先要明白生成器(generator)是什么，<span></span>在讲生成器之前先说说迭代器(iterator)，当创建一个列表(list)时，你可以逐个的读取每一项，这就叫做迭代（iteration）。</p>
<div>
<pre> mylist = [1, 2, 3]
 for i in mylist :
 print(i)
1
2
3</pre>
</div>
<div>Mylist就是一个迭代器，不管是使用复杂的表达式列表，还是直接创建一个列表，都是可迭代的对象。
<div>
<pre>mylist = [x*x for x in range(3)]
for i in mylist :
print(i)
0
1
4</pre>
</div>
</div>
<p>你可以使用“for··· in ···”来操作可迭代对象，如：list,string,files,这些迭代对象非常方便我们使用，因为你可以按照你的意愿进行重复的读取。但是你不得不预先存储所有的元素在内存中，那些对象里有很多元素时，并不是每一项都对你有用。</p>
<p><strong>生成器(Generators)</strong></p>
<p>生成器同样是可迭代对象，但是你只能读取一次，因为它并没有把所有值存放内存中，它动态的生成值：</p>
<div>
<pre> mygenerator = (x*x for x in range(3))
for i in mygenerator :
print(i)
0
1
4</pre>
</div>
<div>
<p>使用()和[]结果是一样的，但是，第二次执行“ for in mygenerator”不会有任何结果返回，因为它只能使用一次。首先计算0，然后计算1，之后计算4，依次类推。</p>
<p><strong>Yield</strong></p>
<p>Yield是关键字， 用起来像return，yield在告诉程序，要求函数返回一个生成器。</p>
</div>
<div>
<pre>def createGenerator() :
mylist = range(3)
for i in mylist :
yield i*i

mygenerator = createGenerator() # create a generator
print(mygenerator) # mygenerator is an object!
&lt;generator object createGenerator at 0xb7555c34&gt;
for i in mygenerator:
print(i)
0
1
4</pre>
</div>
<p>这个示例本身没什么意义，但是它很清晰地说明函数将返回一组仅能读一次的值，要想掌握yield，首先必须理解的是：当你调用生成器函数的时候，如上例中的createGenerator()，程序并不会执行函数体内的代码，它仅仅只是返回生成器对象，这种方式颇为微妙。函数体内的代码只有直到每次循环迭代(for)生成器的时候才会运行。</p>
<p>函数第一次运行时，它会从函数开始处直到碰到yield时，就返回循环的第一个值，然后，交互的运行、返回，直到没有值返回为止。如果函数在运行但是并没有遇到yield，就认为该生成器是空，原因可能是循环终止，或者没有满足任何”if/else”。</p>
<p>接下来读一小段代码来理解生成器的优点：</p>
<p><strong>控制<strong>生成器穷举</strong></strong></p>
<div>
<pre>&gt;&gt;&gt; class Bank(): # 创建银行，构造ATM机
...    crisis = False
...    def create_atm(self) :
...        while not self.crisis :
...            yield &quot;$100&quot;
&gt;&gt;&gt; hsbc = Bank() # 没有危机时，你想要多少，ATM就可以吐多少
&gt;&gt;&gt; corner_street_atm = hsbc.create_atm()
&gt;&gt;&gt; print(corner_street_atm.next())
$100
&gt;&gt;&gt; print(corner_street_atm.next())
$100
&gt;&gt;&gt; print([corner_street_atm.next() for cash in range(5)])
[&#39;$100&#39;, &#39;$100&#39;, &#39;$100&#39;, &#39;$100&#39;, &#39;$100&#39;]
&gt;&gt;&gt; hsbc.crisis = True # 危机来临，银行没钱了
&gt;&gt;&gt; print(corner_street_atm.next())
&lt;type &#39;exceptions.StopIteration&#39;&gt;
&gt;&gt;&gt; wall_street_atm = hsbc.ceate_atm() # 新建ATM，银行仍然没钱
&gt;&gt;&gt; print(wall_street_atm.next())
&lt;type &#39;exceptions.StopIteration&#39;&gt;
&gt;&gt;&gt; hsbc.crisis = False # 麻烦就是，即使危机过后银行还是空的
&gt;&gt;&gt; print(corner_street_atm.next())
&lt;type &#39;exceptions.StopIteration&#39;&gt;
&gt;&gt;&gt; brand_new_atm = hsbc.create_atm() # 构造新的ATM，恢复业务
&gt;&gt;&gt; for cash in brand_new_atm :
...    print cash
$100
$100
$100
$100
$100
$100
$100
$100
$100</pre>
</div>
<p>对于访问控制资源，生成器显得非常有用。</p>
<div></div>
<p><strong>迭代工具，你最好的朋友</strong></p>
<div></div>
<p>迭代工具模块包含了操做指定的函数用于操作迭代器。想复制一个迭代器出来？链接两个迭代器？以one liner（这里的one-liner只需一行代码能<span><a href="http://www.amazon.cn/gp/product/B007XPTAIS/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;tag=vastwork-23&amp;linkCode=as2&amp;camp=536&amp;creative=3200&amp;creativeASIN=B007XPTAIS" title="搞定(套装共3册) " rel="nofollow">搞定</a></span>的任务)用内嵌的列表组合一组值？不使用list创建Map/Zip？···，你要做的就是 import itertools，举个例子吧：</p>
<div></div>
<p>四匹马赛跑到达终点排名的所有可能性：</p>
<div>
<pre>&gt;&gt;&gt; horses = [1, 2, 3, 4]
&gt;&gt;&gt; races = itertools.permutations(horses)
&gt;&gt;&gt; print(races)
&lt;itertools.permutations object at 0xb754f1dc&gt;
&gt;&gt;&gt; print(list(itertools.permutations(horses)))
[(1, 2, 3, 4),
 (1, 2, 4, 3),
 (1, 3, 2, 4),
 (1, 3, 4, 2),
 (1, 4, 2, 3),
 (1, 4, 3, 2),
 (2, 1, 3, 4),
 (2, 1, 4, 3),
 (2, 3, 1, 4),
 (2, 3, 4, 1),
 (2, 4, 1, 3),
 (2, 4, 3, 1),
 (3, 1, 2, 4),
 (3, 1, 4, 2),
 (3, 2, 1, 4),
 (3, 2, 4, 1),
 (3, 4, 1, 2),
 (3, 4, 2, 1),
 (4, 1, 2, 3),
 (4, 1, 3, 2),
 (4, 2, 1, 3),
 (4, 2, 3, 1),
 (4, 3, 1, 2),
 (4, 3, 2, 1)]</pre>
</div>
<p><strong>理解迭代的内部机制：</strong></p>
<p>迭代(iteration）就是对可迭代对象（iterables，实现了__iter__()方法）和迭代器（iterators，实现了__next__()方法）的一个操作过程。可迭代对象是任何可返回一个迭代器的对象，迭代器是应用在迭代对象中迭代的对象，换一种方式说的话就是：iterable对象的__iter__()方法可以返回iterator对象，iterator通过调用next()方法获取其中的每一个值(译者注)，读者可以结合Java API中的 Iterable接口和Iterator接口进行类比。</p>
<p> </p>
<p>英文原文：<a href="http://stackoverflow.com/questions/231767/the-python-yield-keyword-explained">The Python yield keyword explained</a>，编译：<a href="http://www.jobbole.com/">伯乐</a>在线 –<a href="http://blog.jobbole.com/author/%E5%88%98%E5%BF%97%E5%86%9B/">刘志军</a></p>
<p>译文链接：<a href="http://blog.jobbole.com/28506/">http://blog.jobbole.com/32748/</a></p>
<p><span style="color:#ff0000">【如需转载，请在正文中标注并保留原文链接、译文链接和译者等信息，谢谢合作！】</span></p>
<p> </p>
<hr><small>Copyright © 2008<br> This feed is for personal, non-commercial use only. <br> The use of this feed on other websites breaches copyright. If this content is not in your news reader, it makes the page you are viewing an infringement of the copyright. (Digital Fingerprint:<br> )</small>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/31146/">Python之父从Google离职，加入Dropbox</a></li><li><a href="http://blog.jobbole.com/21351/">深刻理解Python中的元类(metaclass)</a></li><li><a href="http://blog.jobbole.com/32876/">Python yield 使用浅析</a></li><li><a href="http://blog.jobbole.com/23425/">Python十分钟入门</a></li><li><a href="http://blog.jobbole.com/32748/">给Python初学者的一些技巧</a></li><li><a href="http://blog.jobbole.com/19835/">Python编程中需要注意的一些事</a></li><li><a href="http://blog.jobbole.com/15005/">趣文：Python程序员的进化史</a></li><li><a href="http://blog.jobbole.com/24197/">Python 代码性能优化技巧</a></li><li><a href="http://blog.jobbole.com/12649/">创建成功的Python项目</a></li><li><a href="http://blog.jobbole.com/1414/">谁说不能用Python写出让人迷惑的代码？</a></li></ul>