---
layout: post
title:  "JavaScript中的正则有几个不同于其他语言的地方"
date:   2013-02-04 10:18:20
author: 
categories: program
---

## JavaScript中的正则有几个不同于其他语言的地方
### by 
### at 2013-02-04 10:18:20
### original <http://blog.jobbole.com/33054/?utm_source=rss&utm_medium=rss&utm_campaign=javascript%25e4%25b8%25ad%25e7%259a%2584%25e6%25ad%25a3%25e5%2588%2599%25e6%259c%2589%25e5%2587%25a0%25e4%25b8%25aa%25e4%25b8%258d%25e5%2590%258c%25e4%25ba%258e%25e5%2585%25b6%25e4%25bb%2596%25e8%25af%25ad%25e8%25a8%2580%25e7%259a%2584%25e5%259c%25b0%25e6%2596%25b9>

<p>来源：<a href="http://www.cnblogs.com/ziyunfei/archive/2013/01/15/2861685.html" rel="nofollow">紫云飞的博客</a></p>
<p>我接触过不少语言，我很看重一门语言的正则表达式是否强大，还有正则与语法的结合是否紧密。在这一点上，JavaScript做的还不错，至少有正则字面量。当然，最强大的还是Perl。我最近发现，JavaScript中的正则在某些地方的表现和其他语言或工具中的正则有些不同，比较另类。虽然你几乎不可能写出也几乎用不到下面我讲的这些正则，但是了解一下毕竟是好的。<strong>本文中的代码示例都是在兼容ES5的JavaScript环境中执行的</strong>，也就是说，IE9之前版本，Fx4左右的版本等中的表现很有可能和我下面讲的不一样。</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" rel="lightbox[33054]" title="JavaScript中的正则有几个不同于其他语言的地方"><img title="JavaScript中的正则有几个不同于其他语言的地方" src="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" alt="JavaScript中的正则有几个不同于其他语言的地方" width="200" height="200"></a></p>
<p><strong>1.空字符类</strong></p>
<p>不包含任何字符的字符类[]称之为<strong>空字符类(empty char class)</strong>,我相信你没听别人这么叫过,因为在其他语言中,这种写法是非法的,所有的文档和教程都不会讲一种非法的语法.下面我演示一下其他语言或工具都是怎么报这个错的:</p>
<pre>$echo | grep &#39;[]&#39;
grep: Unmatched [ or [^

$echo | sed &#39;/[]/&#39;
sed：-e 表达式 #1，字符 4：未终止的地址正则表达式

$echo | awk &#39;/[]/&#39;
awk: cmd. line:1: /[]/
awk: cmd. line:1:  ^ unterminated regexp
awk: cmd. line:1: error: Unmatched [ or [^: /[]//

$echo | perl -ne &#39;/[]/&#39;
Unmatched [ in regex; marked by &lt;-- HERE in m/[ &lt;-- HERE ]/ at -e line 1.

$echo | ruby -ne &#39;/[]/&#39;
-e:1: empty char-class: /[]/

$python -c &#39;import re;re.match(&quot;[]&quot;,&quot;&quot;)&#39;
Traceback (most recent call last):
  File &quot;&lt;string&gt;&quot;, line 1, in &lt;module&gt;
  File &quot;E:\Python\lib\re.py&quot;, line 137, in match
    return _compile(pattern, flags).match(string)
  File &quot;E:\Python\lib\re.py&quot;, line 244, in _compile
    raise error, v # invalid expression
sre_constants.error: unexpected end of regular expression</pre>
<p>而在JavaScript中,空字符类是合法的正则组成部分,不过它的效果是”永不匹配”,也就是匹配什么都会失败.相当于一个<strong>空否定正向环视(empty negative lookahead)</strong>(?!)的效果:</p>
<pre>js&gt; &quot;whatever\n&quot;.match(/[]/g)      //空字符类,永不匹配
null
js&gt; &quot;whatever\n&quot;.match(/(?!)/g)    //空否定正向环视,永不匹配
null</pre>
<p>很显然,这种东西在JavaScript中没什么用.</p>
<p><strong>2.否定空字符类</strong></p>
<p>不包含任何字符的否定字符类[^]称之为<strong>否定空字符类(negative empty char class)</strong>或者叫<strong>空否定字符类(empty </strong><strong>negative char class)</strong>,都可以,因为这个名词是我”自创”的,和上面讲的空字符类类似,这种写法在其他语言中也是非法的:</p>
<pre>$echo | grep &#39;[^]&#39;
grep: Unmatched [ or [^

$echo | sed &#39;/[^]/&#39;
sed：-e 表达式 #1，字符 5：未终止的地址正则表达式

$echo | awk &#39;/[^]/&#39;
awk: cmd. line:1: /[^]/
awk: cmd. line:1:  ^ unterminated regexp
awk: cmd. line:1: error: Unmatched [ or [^: /[^]//

$echo | perl -ne &#39;/[^]/&#39;
Unmatched [ in regex; marked by &lt;-- HERE in m/[ &lt;-- HERE ^]/ at -e line 1.

$echo | ruby -ne &#39;/[^]/&#39;
-e:1: empty char-class: /[^]/

$python -c &#39;import re;re.match(&quot;[^]&quot;,&quot;&quot;)&#39;
Traceback (most recent call last):
  File &quot;&lt;string&gt;&quot;, line 1, in &lt;module&gt;
  File &quot;E:\Python\lib\re.py&quot;, line 137, in match
    return _compile(pattern, flags).match(string)
  File &quot;E:\Python\lib\re.py&quot;, line 244, in _compile
    raise error, v # invalid expression
sre_constants.error: unexpected end of regular expression
$</pre>
<p>而在JavaScript中,否定空字符类是合法的正则组成部分,它的效果和空字符类的效果刚刚相反,它可以匹配任意的字符,包括换行符”\n”,也就是说,等同于常见的[\s\S]和[\w\W]:</p>
<pre>js&gt; &quot;whatever\n&quot;.match(/[^]/g)                //否定空字符类,匹配任意字符
[&quot;w&quot;, &quot;h&quot;, &quot;a&quot;, &quot;t&quot;, &quot;e&quot;, &quot;v&quot;, &quot;e&quot;, &quot;r&quot;, &quot;\n&quot;]
js&gt; &quot;whatever\n&quot;.match(/[\s\S]/g)             //互补字符类,匹配任意字符
[&quot;w&quot;, &quot;h&quot;, &quot;a&quot;, &quot;t&quot;, &quot;e&quot;, &quot;v&quot;, &quot;e&quot;, &quot;r&quot;, &quot;\n&quot;]</pre>
<p>需要注意的是,它不能称之为是”永匹配正则”,因为字符类必须要有一个字符才可能匹配,如果目标字符串是空的,或者已经被左边的正则消耗完了,则匹配会失败,比如:</p>
<pre>js&gt; /abc[^]/.test(&quot;abc&quot;)     //c后面没有字符了,匹配失败.
false</pre>
<p>想要了解真正的”永匹配正则”,可以看看我以前翻译的一篇文章:<a href="http://www.cnblogs.com/ziyunfei/archive/2012/09/14/2685359.html">“空”正则</a></p>
<p><strong>3.[]]和[^]]</strong></p>
<p>这个讲起来比较简单,就是:在Perl和其他一些linux命令的正则表达式中,字符类[]中如果包含了一个紧跟着左方括号的右方括号[]],则这个右方括号会被当作一个普通字符,即只能匹配”]”,而在JavaScript中,这种正则会被识别成一个空字符类后跟一个右方括号,空字符类什么都不匹配.[^]]也类似:在JavaScript中,它匹配的是一个任意字符(否定空字符类)后跟一个右中括号,比如”a]”,”b]”,而在其他语言中,匹配的是任何非]的字符.</p>
<pre>$perl -e &#39;print &quot;]&quot; =~ /[]]/&#39;
1

$js -e &#39;print(/[]]/.test(&quot;]&quot;))&#39;
false

$perl -e &#39;print &quot;x&quot; =~ /[^]]/&#39;
1

$js -e &#39;print(/[^]]/.test(&quot;x&quot;))&#39;
false</pre>
<p><strong>4.$锚点</strong></p>
<p>有些初学者认为$匹配的是换行符”\n”,这是大错特错的,$是一个<strong>零宽断言(zero-width  assertion)</strong>,它是不可能匹配到一个真正的字符的,它只能匹配一个位置.我要的讲的区别发生在非多行模式中:你也许会认为,在非多行模式中,$匹配的不就是最后一个字符后面的位置吗?实际上没那么简单,在其他大部分语言中,如果目标字符串中的最后一个字符是换行符”\n”,则$还会匹配那个换行符之前的位置,也就是匹配了末尾的换行符左右两边的两个位置.很多语言中都有\Z和\z这两个表示法,如果你知道它们之间的区别,那你应该就明白了,在其他语言中(Perl,Python,php,Java,c#…),非多行模式下的$相当于\Z,而在JavaScript中,非多行模式下的$相当于\z(只会匹配最末尾的那个位置,不管最后一个字符是否是换行符).Ruby是个特例,因为它默认就是多行模式,多行模式下$会匹配每个换行符前面的位置,当然也会包括结尾处可能出现的那个换行符.余晟著的《正则指引》一书中也讲到了这几点.</p>
<pre>$perl -e &#39;print &quot;whatever\n&quot; =~ s/$/替换字符/rg&#39;      //全局替换
whatever替换字符                   //换行符前面的那个位置被替换
替换字符                           //换行符后面的那个位置被替换

$js -e &#39;print(&quot;whatever\n&quot;.replace(/$/g,&quot;替换字符&quot;))&#39; //全局替换
whatever
替换字符                           //换行符后面的那个位置被替换</pre>
<p><strong>5.向前引用</strong></p>
<p>我们都知道正则中有<strong>反向引用(back reference)</strong>,也就是用一个反斜杠+数字的形式引用到前面的某个捕获分组已经匹配到的字符串,目的是用来再次匹配或作为<strong>替换结果(\变成$)</strong>.但有种特殊情况是,如果那个被引用的捕获分组还没开始(左括号为界),就使用了反向引用,会怎样.比如正则/(\2(a)){2}/,(a)是第二个捕获分组,但在它的左边使用了引用它的匹配结果的\2,我们知道正则是从左向右进行匹配的,这就是本节的标题<strong>向前引用(forwards reference)</strong>的来历,它并不是一个严格的概念.那么现在你想想,下面的这句JavaScript代码将返回什么:</p>
<pre>js&gt; /(\2(a)){2}/.exec(&quot;aaa&quot;)
???</pre>
<p>在回答这个问题之前,先看看其他语言中的表现.同样,在其他语言中,这么写也基本上是无效的:</p>
<pre>$echo aaa | grep &#39;(\2(a)){2}&#39;
grep: Invalid back reference

$echo aaa | sed -r &#39;/(\2(a)){2}/&#39;
sed：-e 表达式 #1，字符 12：非法回引用

$echo aaa | awk &#39;/(\2(a)){2}/&#39;

$echo aaa | perl -ne &#39;print /(\2(a)){2}/&#39;

$echo aaa | ruby -ne &#39;print $_ = ~/(\2(a)){2}/&#39;

$python -c &#39;import re;print re.match(&quot;(\2(a)){2}&quot;,&quot;aaa&quot;)&#39;
None</pre>
<p>在awk中没有报错,是因为awk不支持反向引用,其中的\2被解释成了ASCII码为2的字符.而在Perl Ruby Python中没报错,我不知道为什么这样设计,应该都是学Perl的,但效果都一样,就是这种情况下是不可能匹配成功的.</p>
<p>而在JavaScript中,不仅不报错,还能匹配成功,看看和你刚才想的答案一样不一样:</p>
<pre>js&gt; /(\2(a)){2}/.exec(&quot;aaa&quot;)
[&quot;aa&quot;, &quot;a&quot;, &quot;a&quot;]</pre>
<p>防止你忘了exec方法返回的结果是什么,我说一下.第一个元素是完整的匹配字符串,也就是RegExp[&quot;$&amp;&quot;],后面的是每个捕获分组匹配的内容,也就是RegExp.$1和RegExp.$2.为什么能匹配成功呢,匹配过程是怎样的?我的理解是:</p>
<p>首先进入了第一个捕获分组(最左边的左括号),其中第一个有效匹配项是\2,然而这时第二个捕获分组(a)还没轮上,因此RegExp.$2的值还是undefined,所以\2匹配了目标字符串中第一个a左边的一个空字符,或者说”位置”,就像^和其他零宽断言一样.重点是匹配成功了.继续走,这时第二个捕获分组(a)匹配到了目标字符串中的第一个a,RegExp.$2的值也被赋值为”a”,然后是第一个捕获分组结束(最右边的右括号),RegExp.$1的值也是”a”.然后是量词{2},也就是说,要从目标字符串中的第一个a之后,开始进行正则(\2(a))的新的一轮匹配,<strong>很关键的一点在这里:</strong>就是RegExp.$2的值也就是\2匹配的值还是不是第一轮匹配结束时的被赋的值”a”,答案是:”<strong>不是</strong>“,RegExp.$1和RegExp.$2的值都会被清空为undefined,\1和\2又会和第一次一样,成功匹配一个空字符(相当于无任何效果,写不写都一样).成功匹配了目标字符串中的第二个a,这时RegExp.$1和RegExp.$2的值又一次成为了”a”,RegExp[&quot;$&amp;&quot;]的值成为了完整的匹配字符串,前两个a:”aa”.</p>
<p>在Firefox的早期版本(3.6)中,量词的重新一轮匹配不会清空已有的捕获分组的值,那么也就是说,在第二轮匹配的时候,\2会匹配上第二个a,从而:</p>
<pre>js&gt; /(\2(a)){2}/.exec(&quot;aaa&quot;)
[&quot;aaa&quot;, &quot;aa&quot;, &quot;a&quot;]</pre>
<p>另外,一个捕获分组的结束要看右括号是否闭合,比如/(a\1){3}/,虽然用到\1的时候,第一个捕获分组已经开始匹配了,但还没结束,这同样是向前引用,所以\1匹配的仍然是空:</p>
<pre>js&gt; /(a\1){3}/.exec(&quot;aaa&quot;)
[&quot;aaa&quot;, &quot;a&quot;]</pre>
<p>再解释一个例子:</p>
<pre>js&gt; /(?:(f)(o)(o)|(b)(a)(r))*/.exec(&quot;foobar&quot;)
[&quot;foobar&quot;, undefined, undefined, undefined, &quot;b&quot;, &quot;a&quot;, &quot;r&quot;]</pre>
<p>*号是量词,第一轮匹配过后:$1为”f”,$2为”o”,$3为”o”,$4为undefined,$5为undefined,$6为undefined.</p>
<p>第二轮匹配开始时:捕获到的值全部重置为undefined.</p>
<p>第二轮匹配过后:$1为undefined,$2为undefined,$3为undefined,$4为”b”,$5为”a”,$6为”r”.</p>
<p>$&amp;被赋值为”foobar”,匹配结束.</p>
<p>最后一个思考题:</p>
<pre>js&gt; /(?:^(a)|\1(a)|(ab)){2}/.exec(&quot;aab&quot;)
????</pre>
<br> <br>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/5172/">10个实用的PHP正则表达式</a></li><li><a href="http://blog.jobbole.com/32572/">JavaScript 项目优化总结</a></li><li><a href="http://blog.jobbole.com/32861/">用于展现图表的50种JavaScript库</a></li><li><a href="http://blog.jobbole.com/32565/">程序员，都去写一写前端代码吧</a></li><li><a href="http://blog.jobbole.com/26554/">CoffeeScript：以优美方式编写JavaScript代码</a></li><li><a href="http://blog.jobbole.com/19381/">Javascript中的循环优化 </a></li><li><a href="http://blog.jobbole.com/31166/">查询json数据结构的8种方式</a></li><li><a href="http://blog.jobbole.com/12042/">JavaScript需要Blocks</a></li><li><a href="http://blog.jobbole.com/22318/">使用Shell脚本对Linux系统和进程资源进行监控</a></li><li><a href="http://blog.jobbole.com/29973/">简洁的bash编程技巧</a></li></ul>