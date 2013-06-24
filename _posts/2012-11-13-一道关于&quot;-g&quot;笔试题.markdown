---
layout: post
title:  "一道关于&quot;/g&quot;笔试题"
date:   2012-11-13 13:44:00
author: snandy
categories: program
---

## 一道关于&quot;/g&quot;笔试题
### by snandy
### at 2012-11-13 13:44:00
### original <http://www.cnblogs.com/snandy/archive/2012/11/13/2767264.html>

<p>正则里“g”表示全局(global)的意思，比如当替换字符串时，如果正则不加g，则只替换一次。</p><div style="background-color:#f5f5f5;border:1px solid #cccccc;padding:10px">str = 'hello, jack, hello, lily';<br>reg = /hello/;<br>res = str.replace(reg, 'hi');<br>console.log(res); // 'hi, jack, hello, lily'<br></div><p>第二个hello未被替换，正则reg换成“/hello/g”后则全部替换。</p><p> </p><p>“g”还有一个作用是它会记录上次匹配时的位置(lastIndex)。这道题如下</p><div style="background-color:#f5f5f5;border:1px solid #cccccc;padding:10px">var reg = /abc/g;<br>var str = 'abcd';<br>reg.test(str);<br>reg.test(str);<br></div><p>两次test的结果分别是什么？ 相信不少人会迷惑。</p><p> </p><p>这种情况Perl里也会发生</p><div style="background-color:#f5f5f5;border:1px solid #cccccc;padding:10px">use 5.012;<br><br>my $str = 'abcd';<br>if ($str =~ /abc/g) {<br>  say 'true';<br>} else {<br>  say 'false';<br>}<br>if ($str =~ /abc/g) {<br>  say 'true';<br>} else {<br>  say 'false';<br>}<br></div><p> </p><p>对于不同的正则对象，JS中会从字符串重新开始，因此以下两次输出都是true。</p><div style="background-color:#f5f5f5;border:1px solid #cccccc;padding:10px">reg1 = /ab/g;<br>reg2 = /cd/g;<br>str = 'abcd';<br>console.log(reg2.test(str));<br>console.log(reg1.test(str));<br></div><p> </p><p>但Perl中第二次却是false，因为它记住了上次匹配的位置。从字符d后再去匹配ab就匹配不上了。</p><div style="background-color:#f5f5f5;border:1px solid #cccccc;padding:10px">use 5.012;<br><br>my $str = 'abcd';<br>if ($str =~ /cd/g) {<br>say 'true';<br>} else {<br>say 'false';<br>}<br>if ($str =~ /ab/g) {<br>say 'true';<br>} else {<br>say 'false';<br>}<br></div><p>　　</p><p> </p><img src="http://www.cnblogs.com/snandy/aggbug/2767264.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/snandy/archive/2012/11/13/2767264.html">本文链接</a></p>