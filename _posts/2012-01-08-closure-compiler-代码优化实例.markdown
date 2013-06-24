---
layout: post
title:  "closure compiler 代码优化实例"
date:   2012-01-08 03:23:29
author: 
categories: program
---

## closure compiler 代码优化实例
### by 
### at 2012-01-08 03:23:29
### original <http://yiminghe.iteye.com/blog/1337452>

<p><a href="http://code.google.com/p/closure-compiler/">closure compiler</a>
 可以进行不少有意思的<a href="http://yiminghe.iteye.com/blog/800881">优化</a>
，一般只有在编译器优化中才会出现的，比如：</p>
<p> </p>
<h1>编译时计算（computation during compilation）</h1>
<p> </p>
<h3>优化前：</h3>
<p> </p>
<pre name="code">var x=5*11;

alert(x*Math.random());</pre>
<p> </p>
<h3>优化后：</h3>
<p> </p>
<p>优化时进行直接量计算，得到：</p>
<p> </p>
<pre name="code">var x=55;
alert(x*Math.random());</pre>
<p> </p>
<p>高级模式下，甚至直接消除没有显式导出的变量 x：</p>
<p> </p>
<pre name="code">alert(55*Math.random());</pre>
<p> </p>
<p> </p>
<h1>复写传播( copy propagation)</h1>
<p> </p>
<h3>优化前</h3>
<p> </p>
<pre name="code">var x=Math.random()+1;
var y=x;
alert(y*Math.random());
alert(y);</pre>
<p> </p>
<h3> 优化后</h3>
<p> </p>
<p>高级模式下，去除无意义的直接变量 copy 赋值以及未显式导出的 y，并替换所有使用 y 的地方为 x：</p>
<p> </p>
<pre name="code">var a=Math.random()+1;alert(a*Math.random());alert(a);</pre>
<p> </p>
<p> </p>
<h1>无用代码消除 (useless code elimination)</h1>
<p> </p>
<p>上面的无用变量消除也算这一种，更广泛的应用是分支代码消除：</p>
<p> </p>
<h3>优化前：</h3>
<p> </p>
<pre name="code">if(1&gt;2){ // if(debug)
alert(1);
}</pre>
<h3> 优化后为：</h3>
<pre name="code">   </pre>
<h1> 但还有些没做的：</h1>
<p> </p>
<h2>循环展开(loop unrolling)</h2>
<p> </p>
<h3>优化前：</h3>
<p> </p>
<pre name="code">for(var i=0;i&lt;3;i++){
alert(i);
}</pre>
<p> </p>
<h3>优化后：</h3>
<p> </p>
<pre name="code">alert(1);
alert(2);
alert(3);</pre>
 
<p>更好些，不过考虑到大多数循环代码比较长，以及 closure compiler 的重点在于减少代码体积，这点没加也是应该。</p>
<p> </p>
<p> </p>
<h2>循环不变量迁移（motion of loop invariant）以及代码提升（code hoisting）</h2>
<p> </p>
<h3>优化前：</h3>
<p> </p>
<pre name="code">var c=Math.random(),d=Math.random(),x=Math.floor(Math.random()*2);
 switch(x){
  case 0: 
	alert(c*d);
  break;
  case 1: 
	alert(c*d);
  break;
 }</pre>
 
<pre name="code">var i,c=Math.random(),d=Math.random(),j;
 for(var i=0;i&lt;10;i++){
   j=c*d+10;
 }</pre>
<h3> 期待优化后：</h3>
<p> </p>
<p> </p>
<pre name="code">var c=Math.random(),d=Math.random(),x=Math.floor(2*Math.random()),t=c*d;switch(x){case 0:alert(t);break;case 1:alert(t)};</pre>
 
<p> </p>
<pre name="code">var i,c=Math.random(),d=Math.random(),j,t=c*d;for(i=0;10&gt;i;i++)j=t+10;</pre>
<h3> 实际优化：</h3>
<p> </p>
<pre name="code">var c=Math.random(),d=Math.random(),x=Math.floor(2*Math.random());switch(x){case 0:alert(c*d);break;case 1:alert(c*d)};</pre>
 
<pre name="code">var i,c=Math.random(),d=Math.random(),j;for(i=0;10&gt;i;i++)j=c*d+10;</pre>
 
<p>即将重复的 c*d 不变量提取出来。</p>
<p> </p>
<p>虽然写出上述代码是程序员的责任，但工具如果能帮忙优化下显然会更好，希望 closure compiler <a href="http://code.google.com/p/closure-compiler/issues/detail?id=637">后续会考虑加入</a>
。</p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
              
              <br><br>
              <span style="color:red">
                <a href="http://yiminghe.iteye.com/blog/1337452#comments" style="color:red">已有 <strong>3</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://yiminghe.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>