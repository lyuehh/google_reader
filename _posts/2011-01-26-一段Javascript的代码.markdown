---
layout: post
title:  "一段Javascript的代码"
date:   2011-01-26 08:39:39
author: 陈皓
categories: program
---

## 一段Javascript的代码
### by 陈皓
### at 2011-01-26 08:39:39
### original <http://coolshell.cn/articles/3540.html>

<p>我们先看一段Javascript的代码，如下所示：（你能看出来这是干什么的？）</p>
<pre>($=[$=[]][(__=!$+$)[_=-~-~-~$]+({}+$)[_/_]+
($$=($_=!''+$)[_/_]+$_[+$])])()[__[_/_]+__
[_+~$]+$_[_]+$$](_/_)</pre>
<p>这段代码来自<a href="http://www.blackhat.com/html/bh-dc-11/bh-dc-11-home.html">BlackHat DC 2011</a>（(黑帽安全大会，全世界最大两个黑客大会之一，另一个是Defcon）中的一个叫<a href="http://www.blackhat.com/html/bh-dc-11/bh-dc-11-speaker_bios.html#Barnett">Ryan Barnett</a>黑客做的<a href="https://docs.google.com/viewer?url=http://www.modsecurity.org/documentation/XSS_Street_Fight-Ryan_Barnett-BlackhatDC-2011.pdf&amp;embedded=true&amp;chrome=true">XSS Street-Fight</a>！的演讲(XSS是Web上比较经典的跨站式攻击，操作起来也有些复杂)，一共69页，基本上都是一些比较枯燥的Javascript，不过这段代码挺有意思的，如果上面这段代码换个样子：</p>
<pre>($=[$=[]][(__=!$+$)[_=-~-~-~$]+({}+$)[_/_]+
($$=($_=!''+$)[_/_]+$_[+$])])()[__[_/_]+__
[_+~$]+$_[_]+$$](document.cookie)</pre>
<p>你看到了document.cookie，于是你可能会想到这是偷用户帐号免登录cookie的。是的，就是这样。答案是，这代码等价于alert(document.cookie)，而最上面的那个代码等价于alert(1)——当然，还不仅仅只是alert。看到这里，你可能会想起<a title="6个变态的C语言Hello World程序 " href="http://coolshell.cn/articles/914.html">变态的C语言Hello World程序</a>，以及<a title="如何加密/混乱C源代码" href="http://coolshell.cn/articles/933.html">如何加密/混乱C源代码</a>，是的，这回的这个是Javascript版的，混乱Javascript的会比混乱C的更难懂，因为Javascript的变量类型是可以乱用的。</p>
<p>好，下面让我们来对这个代码做个解析。</p>
<p><span></span>首先，我们先明确一点，在Javascript和C中，混乱后的代码都是要使用一个或多个下划线（_）来当变量名使用的，所以，请把其中的下划线看成变量名。</p>
<p>其次，这段代码可以分成两个部分，第一个部门其实就是sort()，第二个部分才是alert()</p>
<pre>($=[$=[]][(__=!$+$)[_=-~-~-~$]+({}+$)[_/_]+
($$=($_=!''+$)[_/_]+$_[+$])])()</pre>
<pre>[__[_/_]+__[_+~$]+$_[_]+$$](_/_)</pre>
<p>我们来看看细节的解释。</p>
<ul>
<li>$=[] 是一个空数组</li>
<li>$=[$=[]] 是一个引用空数组的数组。所以 $ 的解引用就是数字 0。</li>
<li>__ =  (__ = !$ + $ )   等价于字符串”false”</li>
<li>_ = -~-~-~$    中~是位运算符“非”，~$等于-1，所以-~$ 就是+1，基本上来说，~N就是 -(N+1)，所以这个表达式的值为3。</li>
<li>因为_ = 3，所以 _/_ = 3/3 = 1</li>
</ul>
<p>于是：</p>
<ul>
<li>(__ = !$ + $ )[ _ = -~-~-~$]</li>
<li>(“false”)[_]</li>
<li>(“false”)[3]</li>
<li>“false”[3] = s</li>
</ul>
<p>而：</p>
<ul>
<li>({} + $)[_/_]</li>
<li>(” object”)[_/_]</li>
<li>(” object”)[1]</li>
<li>” object”[1] = o</li>
</ul>
<p>再来：</p>
<ul>
<li>$ = ( $_ = !” + $)[_/_]</li>
<li>$ = ( “true”)[1]</li>
<li>“true”[1] = r</li>
</ul>
<p>最后：</p>
<ul>
<li>$_[+$] = “true”[0] = t</li>
</ul>
<p>因为</p>
<p>($$ = ( $_ = !” + $)[_/_] + $_[+$] ))</p>
<p>所以我们可以经过下面的推算得出$$的值</p>
<ul>
<li>!” = “true”</li>
<li>$_ = (true)</li>
<li>$_[1] = r</li>
<li>$_[0] = t</li>
<li>$$ = rt</li>
</ul>
<p>所以第一部分就成了 sort()，也就是以下的代码</p>
<pre>($ = [ $=[]] [&quot;s&quot; + &quot;o&quot;+ &quot;r&quot;+ &quot;t&quot; ] )()</pre>
<p>Sort 接受一个作为函数的参数来运行，从而执行了第二部份。</p>
<p>[__[_/_]+__[_+~$]+$_[_]+$$](_/_)</p>
<p>我们知道：</p>
<ul>
<li>$ = 0</li>
<li>_ = 3</li>
<li>__ = “false”</li>
<li>$! = “true”</li>
<li>$$ = “rt”</li>
</ul>
<p>[__[_/_]+__[_+~$]+$_[_]+$$](_/_)</p>
<p>等价于<br>
[__[1] + __[3 + -1] + $![3] + $$)(1);</p>
<p>等价于<br>
[&quot;false&quot;[1] + “false”[3 + -1 ] + “true”[3] + “rt”] (1)</p>
<p>等价于<br>
[ a + l + e + r + t ](1)</p>
<p>等价于<br>
alert(1)</p>
<p>就是这样！于是这段代码可能绕过你的一些对Javascript的检查。</p>
<h3>相关文章</h3><ul><li>2011年02月16日 -- <a href="http://coolshell.cn/articles/3684.html" title="Web开发人员速查卡">Web开发人员速查卡</a></li><li>2011年01月20日 -- <a href="http://coolshell.cn/articles/3516.html" title="JS游戏引擎列表">JS游戏引擎列表</a></li><li>2010年12月31日 -- <a href="http://coolshell.cn/articles/3437.html" title="一些杂项资源">一些杂项资源</a></li><li>2010年12月27日 -- <a href="http://coolshell.cn/articles/3429.html" title="程序员的圣诞节">程序员的圣诞节</a></li><li>2010年11月25日 -- <a href="http://coolshell.cn/articles/3311.html" title="几篇技术文章">几篇技术文章</a></li><li>2010年11月01日 -- <a href="http://coolshell.cn/articles/3254.html" title="一个人脸识别的Javascript">一个人脸识别的Javascript</a></li><li>2010年10月25日 -- <a href="http://coolshell.cn/articles/3244.html" title="在线作图编辑服务">在线作图编辑服务</a></li></ul>