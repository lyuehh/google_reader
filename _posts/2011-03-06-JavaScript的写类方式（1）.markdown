---
layout: post
title:  "JavaScript的写类方式（1）"
date:   2011-03-06 12:36:00
author: snandy
categories: program
---

## JavaScript的写类方式（1）
### by snandy
### at 2011-03-06 12:36:00
### original <http://www.cnblogs.com/snandy/archive/2011/03/06/1971764.html>

<p><p>从这篇起，会由浅到深的分析JS OO之写类方式，大概会有5-8篇。后面陆续会分析流行库（框架）的写类方式。一些写类工具函数或框架的写类方式本质上都是 <span style="color:#ff00ff">构造函数+原型</span>。只有理解这一点才能真正明白如何用JavaScript写出面向对象的代码。或者说组织代码的方式使用面向对象方式。当然用JS也可写出函数式的代码，它是多泛型的。</p>
<p>为了讨论的单一性，暂不考虑类的继承，(私有,受保护)属性或方法。EMCAScript中实际没有类(class)的概念，但可以把它理解为更广义的概念。</p>
<br>
<p><span style="font-size:16px"><strong>1、构造函数方式</strong></span></p>
<div>
<pre>/**
 * Person类：定义一个人，有个属性name，和一个getName方法
 * @param {String} name
 */
function Person(name) {
	this.name = name;
	this.getName = function() {
		return this.name;
	}
}
</pre>
</div>
<p>这种风格让写过Java的有点亲切在于构造一个对象需要配置一些参数，参数要赋值给类里面this。但与Java的区别是JS用function来代替class，参数也无需定义类型。</p>
<p>类写好了，我们造几个对象：</p>
<div>
<pre>var p1 = new Person("Jack");
var p2 = new Person("Tom");
console.log(p1 instanceof Person);//true
console.log(p2 instanceof Person);//true
</pre>
</div>
<p>控制台输出也证明了p1，p2的确是类Person的对象实例。<br><br>这种方式的优点是：可以根据参数来构造不同的对象实例 ，缺点是构造时每个实例对象都会生成getName方法版本，造成了内存的浪费 。<br> <br>经验丰富的程序员用一个外部函数来代替类方法，达到了每个对象共享同一个方法。改写后的类如下：</p>
<div>
<pre>//外部函数
function getName() {
	return this.name;
}
function Person(name) {
	this.name = name;
	this.getName = getName;//注意这里
}</pre>
</div>
 
有人可能觉得代码风格有点差强人意，怎么看也没有Java那么紧凑。但的确可以减少内存的消耗。<br><br>
<p><span style="font-size:16px"><strong>2、原型方式</strong></span></p>
<div>
<pre>/**
 * Person类：定义一个人，有个属性name，和一个getName方法
 */
function Person(){}
Person.prototype.name = "jack";
Person.prototype.getName = function() { return this.name;}
</pre>
</div>
<p>把类的属性（字段），方法都挂在prototype上。<br>造几个对象测试下</p>
<div>
<pre>var p1 = new Person();
var p2 = new Person();
console.log(p1.getName());//jack
console.log(p2.getName());//jack
</pre>
</div>
<p>可以看出输出的都是jack，原型方式的缺点就是不能通过参数来构造对象实例 (一般每个对象的属性是不相同的) ，优点是所有对象实例都共享getName方法（相对于构造函数方式），没有造成内存浪费 。</p>
<br>
<p><span style="font-size:16px"><strong>3、构造函数+原型</strong></span></p>
<p>取前面两种的优点：<br>a、用构造函数来定义类属性（字段）。<br>b、用原型方式来定义类的方法。</p>
<div>
<pre>/**
 * Person类：定义一个人，有个属性name，和一个getName方法
 * @param {String} name
 */
function Person(name) {
	this.name = name;
}
Person.prototype.getName = function() {
	return this.name;
}
</pre>
</div>
<p>这样，即可通过构造函数构造不同name的人，对象实例也都共享getName方法，不会造成内存浪费。<br>但似乎这样的代码风格似乎仍然没有Java的类那么紧凑，把属性，构造方法（函数），方法都包在大括号内。</p>
<div>
<pre>public class Person {
	//属性（字段）
	String name;	
	//构造方法（函数）
	Person(String name) {
		this.name = name;
	}	
	//方法
	String getName() {
		return this.name;
	}
}
</pre>
</div>
<p><br>为了让JS代码风格更紧凑，把挂在prototype的方法代码移到function Person的大括号内。</p>
<div>
<pre>function Person(name) {
	this.name = name;
	Person.prototype.getName = function() {
		return this.name;
	}
}
</pre>
</div>
<p>似乎很神奇，还能这么写啊！验证一下</p>
<div>
<pre>var p1 = new Person("Jack");
var p2 = new Person("Tom");
console.log(p1.getName());//Jack
console.log(p2.getName());//Tom
</pre>
</div>
<p>没有报错，控制台也正确输出了。说明可以这么写，似乎很完美。<br>a 、可以通过传参构造对象实例<br>b 、对象实例都共享同一份方法不造成内存浪费<br>c 、代码风格也比较紧凑<br> <br>但每次new一个对象的时候都会执行
</p>
<div>
<pre>Person.prototype.getName = function() {
        return this.name;
}
</pre>
</div>
<p>造成了不必要的重复的运算。因为getName方法挂在prototype上只需执行一次即可。只需稍微改造下
</p>
<div>
<pre>function Person(name) {
	this.name = name;
	if(Person._init==undefined) {
		alert("我只执行一次！");
		Person.prototype.getName = function() {
			return this.name;
		}
		Person._init = 1;	
	}	
}</pre>
</div>
<p>new两个对象，</p>
<div>
<pre>var p1 = new Person("Andy"); //第一次new会弹出'我只执行一次！'
var p2 = new Person("Lily"); //以后new的对象不会再执行了
</pre>
</div><img src="http://www.cnblogs.com/snandy/aggbug/1971764.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/snandy/">snandy</a> 发表于 2011-03-06 12:36 <a href="http://www.cnblogs.com/snandy/archive/2011/03/06/1971764.html">原文链接</a></p><p>评论: 26　<a href="http://www.cnblogs.com/snandy/archive/2011/03/06/1971764.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/snandy/archive/2011/03/06/1971764.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/93070/">视频：Larry 和 Sergey 在 1999</a><span style="color:gray">(2011-03-06 22:07)</span><br>· <a href="http://news.cnblogs.com/n/93069/">50个值得收藏的有趣网站收罗</a><span style="color:gray">(2011-03-06 21:24)</span><br>· <a href="http://news.cnblogs.com/n/93068/">谷歌更换Logo纪念动漫大师威尔埃斯纳诞辰</a><span style="color:gray">(2011-03-06 21:15)</span><br>· <a href="http://news.cnblogs.com/n/93067/">网站推荐：11个相似图片搜索网站</a><span style="color:gray">(2011-03-06 21:11)</span><br>· <a href="http://news.cnblogs.com/n/93066/">京东商城跟进补贴一代iPad用户差价</a><span style="color:gray">(2011-03-06 20:53)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/92996/">国外女程序员的心声：当女程序员的好处</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>