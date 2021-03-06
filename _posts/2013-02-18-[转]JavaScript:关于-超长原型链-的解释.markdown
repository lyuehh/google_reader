---
layout: post
title:  "[转]JavaScript:关于”超长原型链”的解释"
date:   2013-02-18 17:26:37
author: mrhack
categories: program
---

## [转]JavaScript:关于”超长原型链”的解释
### by mrhack
### at 2013-02-18 17:26:37
### original <http://myjser.com/?p=482&utm_source=rss&utm_medium=rss&utm_campaign=javascript%25e5%2585%25b3%25e4%25ba%258e%25e8%25b6%2585%25e9%2595%25bf%25e5%258e%259f%25e5%259e%258b%25e9%2593%25be%25e7%259a%2584%25e8%25a7%25a3%25e9%2587%258a>

<p>前两天我在一篇文章中讲到了”超长原型链”这个东西,有些读者不是很明白.我觉的有必要再详细的解释一下,首先出道题:</p>
<p>function Foo() {}                      //构造函数</p>
<p>Foo.prototype.say = function () {<br>
    return “I’m a Foo!”;<br>
};                                     //原型对象上添加一个say方法<br>
var foo1 = new Foo;                    //生成一个Foo实例<br>
foo1.say()                             //继承了原型对象上的say方法,输出”I’m a Foo!”<br>
Foo.prototype = {}                     //原型对象指向一个空对象<br>
foo1.say()                             //输出什么呢?<br>
你也许会这么认为:</p>
<p>foo1本身并没有say方法,所以在最后执行foo1.say()时,foo1会从Foo.prototype上找say方法,但这是Foo.prototype已经是个空对象,say属性肯定是undefined,所以上面题目的答案是抛出异常”TypeError: foo1.say is not a function”</p>
<p>好像的确是这么回事,但是答案错了.正确的答案是仍然输出”I’m a Foo!”.为什么呢?貌似上面的解释没问题啊?</p>
<p>的确.上面这样的解释大部分都对,错就错在了唯一的细节上:</p>
<p>foo1会从Foo.prototype上找say方法</p>
<p>更严格的说法应该是:foo1会从foo1的内部属性[[prototype]]指向的对象上找say方法,也就是foo1.__proto__.say.虽然Foo.prototype指向了一个空对象,但foo1.__proto__仍然指向旧的Foo.prototype指向的那个对象.因此,可以继承到say方法.</p>
<p>另一个需要注意的现象是,一旦你修改了Foo.prototype的指向.instanceof操作符不再认为foo1是Foo类型的实例了.</p>
<p>foo1 instanceof Foo     //false<br>
因为instanceof运算符的工作原理就是检查当前的Foo.prototype等于还是不等于foo1.__proto__或者foo1.__proto__.__proto__等等,一直到原型链的尽头null.当前的Foo.prototype指向一个空对象,foo1.__proto__指向原先Foo.prototype指向的那个对象,两者并不相等.所以instanceof会返回false.接下来,让我们再看一个例子:</p>
<p>function Foo() {}                      //构造函数</p>
<p>console.log(Foo.prototype.__proto__ === Object.prototype)   //原型对象会自动建立,且类型是Object的实例.<br>
Foo.prototype.say = function () {<br>
    return “I’m a Foo!”;<br>
};                                     //原型对象上添加一个say方法<br>
var foo1 = new Foo;                    //生成一个Foo实例<br>
foo1.say()                             //继承了原型对象上的say方法,输出”I’m a Foo!”<br>
Foo.prototype = new Foo;               //Foo.prototype指向一个Foo实例,就像String.prototype是个String实例一样<br>
Foo.prototype.say = function () {<br>
    return “I’m a new Foo!”;<br>
};                                     //原型对象上添加一个新的say方法<br>
console.log(foo1.say())                //仍然输出”I’m a Foo!”<br>
var foo2 = new Foo;                    //再次新建一个Foo实例<br>
foo2.say()                             //输出”I’m a new Foo!”<br>
这时foo1和foo2的原型链是这样的.</p>
<p>foo1-&gt;原来的Foo.prototype-&gt;Object.prototype-&gt;null</p>
<p>foo2-&gt;新的Foo.prototype-&gt;原来的Foo.prototype-&gt;Object.prototype-&gt;null</p>
<p>可以用我们上篇文章中写的getPrototypeChain函数验证一下:</p>
<p>&gt;getPrototypeChain(foo2).forEach(function (obj) {<br>
    console.log(obj.say.toString())<br>
})<br>
function () {                                  //输出了新的Foo.say方法<br>
    return “I’m a new Foo!”;<br>
}<br>
function () {                                  //输出了旧的Foo.say方法<br>
    return “I’m a Foo!”;<br>
}<br>
TypeError: Cannot call method ‘toString’ of undefined  //Object.prototype没有say方法,抛出异常<br>
现在应该能理解上篇文章中给出的代码了:</p>
<p>function Foo() {}</p>
<p>for (var i = 0; i &lt; 100; i++) {<br>
    Foo.prototype["prop" + i] = i;<br>
    Foo.prototype = new Foo;<br>
}<br>
var foo1 = new Foo;<br>
console.log(getPrototypeChain(foo1).length);          //包含null,一共有103个上层原型<br>
console.log(Object.getOwnPropertyNames(foo1));        //空数组,foo1没有任何自身属性<br>
for(var prop in foo1){console.log(prop)}              //foo1从100个不同的原型上继承了100个不同的属性</p>
<p>原文：<a href="http://www.cnblogs.com/ziyunfei/archive/2012/10/19/2731280.html">http://www.cnblogs.com/ziyunfei/archive/2012/10/19/2731280.html</a></p>
<p><a href="http://www.addtoany.com/share_save#url=http%3A%2F%2Fmyjser.com%2F%3Fp%3D482&amp;title=%5B%E8%BD%AC%5DJavaScript%3A%E5%85%B3%E4%BA%8E%E2%80%9D%E8%B6%85%E9%95%BF%E5%8E%9F%E5%9E%8B%E9%93%BE%E2%80%9D%E7%9A%84%E8%A7%A3%E9%87%8A"><img src="http://myjser.com/wp-content/plugins/add-to-any/share_save_171_16.png" width="171" height="16" alt="Share"></a></p>