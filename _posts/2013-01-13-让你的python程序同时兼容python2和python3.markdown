---
layout: post
title:  "让你的python程序同时兼容python2和python3"
date:   2013-01-13 13:50:28
author: 
categories: program
---

## 让你的python程序同时兼容python2和python3
### by 
### at 2013-01-13 13:50:28
### original <http://simple-is-better.com/news/957>

<p>
	python邮件列表里有人发表言论说“python3在10内都无法普及”。在我看来这样的观点有些过于悲观，python3和python2虽然不兼容，但他们之间差别并没很多人想像的那么大。你只需要对自己的代码稍微做些修改就可以很好的同时支持python2和python3的。下面我将简要的介绍一下如何让自己的python代码如何同时支持python2和python3。</p>
<ul>
	<li>
		<strong>放弃python 2.6之前的python版本</strong><br>
		python 2.6之前的python版本缺少一些新特性，会给你的迁移工作带来不少麻烦。如果不是迫不得已还是放弃对之前版本的支持吧。</li>
	<li>
		<strong>使用 2to3 工具对代码检查</strong><br>
		2to3是python自带的一个代码转换工具，可以将python2的代码自动转换为python3的代码。当然，不幸的是转换出的代码并没有对python2的兼容做任何的处理。所以我们并不真正使用2to3转换出的代码。执行<em>2to3 t.py</em> 查看输出信息，并修正相关问题。</li>
	<li>
		<strong>使用python -3执行python程序</strong><br>
		2to3 可以检查出很多python2＆3的兼容性问题，但也有很多问题是2to3发现不了的。在加上 <em>-3</em> 参数后，程序在运行时会在控制台上将python2和python3不一致，同时2to3无法处理的问题提示出来。比如python3和python2中对除法的处理规则做过改变。使用-3参数执行4/2将提示 DeprecationWarning: classic int division 。</li>
	<li>
		<strong><code>from __future__ import</code></strong><br>
		“<code>from __future__ import</code>”后即可使使用python的未来特性了。python的完整future特性可见 <a href="http://docs.python.org/2/library/__future__.html"><code>__future__</code></a> 。python3中所有字符都变成了unicode。在python2中unicode字符在定义时需要在字符前面加 <em>u</em>，但在3中则不需要家u，而且在加u后程序会无法编译通过。为了解决该问题可以 “from <strong>future</strong> import unicode_literals” ，这样python2中字符的行为将和python3中保持一致，python2中定义普通字符将自动识别为unicode。</li>
	<li>
		<strong>import问题</strong><br>
		python3中“少”了很多python2的包，在大多情况下这些包之是改了个名字而已。我们可以在import的时候对这些问题进行处理。</li>
</ul>
<pre>
try:#python2
    from UserDict import UserDict
    #建议按照python3的名字进行import
    from UserDict import DictMixin as MutableMapping
except ImportError:#python3
    from collections import UserDict
    from collections import MutableMapping</pre>
<ul>
	<li>
		<strong>使用python3的方式写程序</strong><br>
		python2中print是关键字，到了python3中print变成了函数。事实上在python2.6中已经带了print函数，所以对print你直接按照2to3中给出的提示改为新写法即可。在python3中对异常的处理做了些变化，这个和print类似，直接按照2to3中的提示修改即可。</li>
	<li>
		<strong>检查当前运行的python版本</strong><br>
		有时候你或许必须为python2和python3写不同的代码，你可以用下面的代码检查当前系统的python版本。</li>
</ul>
<pre>
import sys
if sys.version &gt; &#39;3&#39;:
    PY3 = True
else:
    PY3 = False</pre>
<ul>
	<li>
		<strong>six</strong><br>
		<a href="http://packages.python.org/six/">six</a> 提供了一些简单的工具用来封装 Python 2 和 Python 3 之间的差异性。我并不太推荐使用six。如果不需要支持python2.6之前的python版本，即使不用six也是比较容易处理兼容性问题的。使用six会让你的代码更像python2而不是python3。</li>
</ul>
<p>
	python3的普及需要每位pythoner的推动，或许你还无法立即升级到python3，但请现在就开始写兼容python3的代码，并在条件成熟时升级到python3。</p>
<p>
	<strong>注：</strong></p>
<ul>
	<li>
		<a href="http://python3porting.com/differences.html">python2同python3的差异</a></li>
	<li>
		如果你更全面的了解从python2迁移到python3的相关问题，推荐阅读 <a href="http://python3porting.com/">Porting to Python 3</a> 这是一本免费的python读物。</li>
</ul>

                    <p># 来源：<a href="http://item.feedsky.com/~feedsky/vicalloy/~8740469/707169926/5348363/1/item.html" title="让你的python程序同时兼容python2和python3">天地一沙鸥</a></p>
                    <hr>
                    <p>
                        在微博上关注：
                        <a href="http://t.sina.com.cn/pythoncn">新浪</a>,
                        <a href="http://t.qq.com/simple-is-better">腾讯</a>
                         
                        <a href="http://simple-is-better.com/tougao">投稿</a>
                    </p>
                    <p><strong>最新招聘</strong></p>
                    <ul>
                        <li><a href="http://simple-is-better.com/jobs/detail-313">[上海] Python 程序开发(Django, Scrapy, 2年+)</a> - 若邻网</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-443">[深圳] Python Web后台开发</a> - Tencent</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-438">[北京] 初/中/高级软件工程师</a> - App Annie</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-219">[深圳] Python 程序开发(Django, 网络爬虫开发, 数据分析)</a> - 北京创新在线网络技术有限公司</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-402">[北京] Python/Java研发工程师</a> - 北京豆豆网络科技有限责任公司</li>
                    </ul>
                    <p><a href="http://simple-is-better.com/jobs/">更多&gt;&gt;</a></p><img src="http://www1.feedsky.com/t1/723779444/simple-is-better/feedsky/s.gif?r=http://simple-is-better.com/news/957" border="0" height="0" width="0">