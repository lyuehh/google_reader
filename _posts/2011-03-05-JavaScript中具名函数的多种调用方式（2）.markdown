---
layout: post
title:  "JavaScript中具名函数的多种调用方式（2）"
date:   2011-03-05 16:15:00
author: snandy
categories: program
---

## JavaScript中具名函数的多种调用方式（2）
### by snandy
### at 2011-03-05 16:15:00
### original <http://www.cnblogs.com/snandy/archive/2011/03/05/1971598.html>

<p><p>以函数中有无this来讨论。没有this时返回一个空的对象{},有this时返回一个非空对象。</p>
<p>下面是一个没有this的函数</p>
<div>
<pre>// 返回值是基本类型
function fun() {
	return "jack";
}

var c = new fun();
for (var atr in c) {
	alert(atr);
} 
alert(c); //[object Object]</pre>
</div>
<p>返回值c不是"jack", 从for in执行后没有输出任何属性可以看出 c 是一个空的对象{}。<br><br>再看看有this的函数，函数中有this实际上是在写一个类。但由于js的灵活性，造成了许多诡异的写法。</p>
<div>
<pre>// 返回值是基本类型
function fun() {
	this.name = "tom";
	return "jack";
}

var c = new fun();
for (var atr in c) {
	alert(atr); //name
} 
alert(c.name); //tom</pre>
</div>
<p>返回值也不是"jack"，for in输出了name属性，最后一句输出了tom，说明返回值 c 是一个非空对象。这里的return "jack"压根没起作用。所以当函数返回值是内置类型（基本类型）时，用new方式调用函数将会导致错误的结果 。<br> <br>那么当函数返回值是一个对象，数组，函数呢？</p>
<div>
<pre>// 不含this，返回值是一个对象
function fun() {
	//组装一个对象
	var obj = {};
	obj.name = 'andy';
	obj.age = 20;
	obj.msg = function(){}	
	return obj;
}

var c = new fun();
for (var atr in c) {
	alert(atr); //name,age,msg
} 
alert(c.name); //andy
</pre>
</div>
<br>
<div>
<pre>// 含this，返回值是一个对象
function fun() {
	this.sex = "man";
	//组装一个对象
	var obj = {};
	obj.name = 'andy';
	obj.age = 20;
	obj.msg = function(){}	
	return obj;
}

var c = new fun();
for (var atr in c) {
	alert(atr); //name,age,msg
} 
alert(c.name); //andy<br></pre>
</div>
<p>两段的输出结果是一样的，c都含有name,age,msg属性而不含sex属性。说明当返回值是对象类型（对象，数组，函数）时，new不会用this去构造对象，而直接返回组装的对象。 <br><br>这种方式实际上是工厂方式，在应用中更多的程序员把函数名首字母大写，让它看起来更像一个类。</p>
<div>
<pre>/**
 * 定义一个函数Car
 */
function Car(color,doors) {	
	var car = {};
	car.color = color;
	car.doors = doors;
	car.msg = function(){ 
		alert("This is a " + this.color + " car, there are " + this.doors + " doors." );
	}	
	return car;
}</pre>
</div>
<p>定义了函数Car，函数内创建了一个空对象，添加了一些属性和方法后返回。
 以下是两种创建方式</p>
<div>
<pre>// 方式1
var c1 = Car('red',2);
c1.msg();

// 方式2
var c2 = new Car('black',4);
c2.msg();
</pre>
</div>
<p>方式1是函数调用，方式2是new。方式2把Car当成了一个类，但实际上并不是（Car的this和prototype都未挂属性，方法）</p><img src="http://www.cnblogs.com/snandy/aggbug/1971598.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/snandy/">snandy</a> 发表于 2011-03-05 16:15 <a href="http://www.cnblogs.com/snandy/archive/2011/03/05/1971598.html">原文链接</a></p><p>评论: 0　<a href="http://www.cnblogs.com/snandy/archive/2011/03/05/1971598.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/snandy/archive/2011/03/05/1971598.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/93043/">Facebook进军中国“非试不可”</a><span style="color:gray">(2011-03-06 13:54)</span><br>· <a href="http://news.cnblogs.com/n/93042/">解析iPad 2低价如何炼成：买断关键零部件 买断关键零部件</a><span style="color:gray">(2011-03-06 13:36)</span><br>· <a href="http://news.cnblogs.com/n/93041/">Twitter将利用iPhone应用销售广告</a><span style="color:gray">(2011-03-06 12:39)</span><br>· <a href="http://news.cnblogs.com/n/93040/">微软称Xbox玩家每月游戏平均花费64英镑</a><span style="color:gray">(2011-03-06 12:17)</span><br>· <a href="http://news.cnblogs.com/n/93039/">法官批准索尼向谷歌等索取PS3破解者账户信息</a><span style="color:gray">(2011-03-06 12:15)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/92996/">国外女程序员的心声：当女程序员的好处</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>