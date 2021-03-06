---
layout: post
title:  "如何创建一个JavaScript裸对象"
date:   2012-08-27 02:11:06
author: 
categories: program
---

## 如何创建一个JavaScript裸对象
### by 
### at 2012-08-27 02:11:06
### original <http://hax.iteye.com/blog/1663476>

所谓裸对象，即 naked object ，是指没有原型（spec中以[[proto]]内建属性表示）的对象。
<br>
<br>JavaScript是少见的采用原型继承的语言。访问一个对象的属性时，会首先看它自己的属性，所谓 own property 是也，如果找不到，则在其原型中查找，再找不到就继续找这个原型的原型，这就构成所谓的原型链。
<br>
<br>原型继承提供了一种很独特的共享信息的方式，不过也带来一些有趣的问题。比如with构造。
<br>
<br>我在2011年w3ctech广州的演讲中提到过with构造的问题，所以在strict模式中with就被禁用了。
<br>
<br>其中一个问题是，with(obj)时，obj如果是你<strong>不可掌控</strong>的对象，会引入无法控制的风险。所谓不可掌控，例如浏览器对象（像在演讲中举的event对象），或者第三方库的对象或可能被第三方库修改的对象（例如DOM对象就是如此，许多库会在上面加各种东西）。
<br>
<br>这和原型有什么关系？很有关系！因为with不是仅仅查找own property，而是也会上溯原型链。
<br>
<br>例子：
<br>
<br><pre name="code">var name = 'hax'
with (console) {
  log('Hello ' + name)
}
</pre>
<br>
<br>注意，许多实现里 console.log 的 log 方法并非直接在 console 对象上，而是在 console 的原型上。
<br>
<br>如此看上溯原型链似乎是件好事，但是考虑这个代码：
<br>
<br><pre name="code">var name = &#39;hax&#39;, count = 3
with (console) {
  for (var i = 0; i &lt; count; i++)
    log(&#39;Hello &#39; + name)
}
</pre>
<br>这代码在有些环境可以跑，如NodeJS；但在Chrome里就不行了，会死循环（我没高兴实验，欢迎小白鼠尝试）。虽然大家用的都是V8引擎，但是Chrome里的console上有一个count()方法。
<br>
<br>更讨厌的是，几乎所有对象都有原型，一直到Object.prototype。考虑如下代码：
<br>
<br><pre name="code">function constructor() {
  ...
}
with ({}) {
  var x = constructor() 
}
</pre>
<br>
<br>实际结果等价于 x = Object() ，因为 Object.prototype.constructor == Object 。
<br>
<br>这也导致修改Object.prototype就可能干扰所有的 with 块。而Object.prototype是所有对象的原型，换句话说，几乎所有情况下都是<strong>不可掌控</strong>的。
<br>
<br>【注意，确实是存在不以 Object.prototype 为原型的对象，比如某些环境里的host对象，但是显然它们也不属于你可掌控的对象。】
<br>
<br>可见with的最初设计就有失误，如果是仅仅查找 own property 会好很多。不过现在已经不可能做这样的修改，所以只能想其他方式，比如如果我们真正的有 naked object，那么就可以做到对对象的完全掌控。
<br>
<br>
<br>听上去不太可能，除了null和undefined，所有对象（包括number、string和boolean通常会自动装箱成包装对象）都是Object的实例，也就是有Object.prototype作为原型链的最末端。
<br>
<br>不过ES5所增加的Object.create()方法就提供了这样一个能力！只要调用Object.create(null)就可以得到一个裸对象！这这不错。所以我们得到了一个相对安全的with方式如下：
<br>
<br><pre name="code">with (Context({a: 1, b: 2})) {
  console.log(a, b)
}

function Context(obj) {
  var pds = {}
  var names = Object.getOwnPropertyNames(obj)
  for (var i = 0; i &lt; names.length; i++) {
    pds[names[i]] = Object.getOwnPropertyDescriptor(obj, names[i])
  }
  return Object.create(null, pds)
}</pre>
<br>
<br>不过，对于老的，没有Object.create方法的浏览器怎么办？
<br>
<br>许多浏览器有 __proto__ 属性可以直接访问对象的 __proto__ 。所以NCZ写过一篇关于裸对象的文章：<a href="http://www.nczonline.net/blog/2008/07/10/naked-javascript-objects/">Naked JavaScript objects</a>就提到这个方式。看上去其实比Object.create还要简单：
<br>
<br><pre name="code">with({a: 1, b: 2, __proto__: null}) { ... }</pre>
<br>
<br>不过还有一些老浏览器不支持 __proto__ ，例如IE。怎么办呢？
<br>
<br>
<br>其实有办法。
<br>
<br>可以发现，在整个JavaScript对象世界里，似乎所有的对象都有原型，但是有一个例外。
<br>
<br>没错，那就是所有对象之源：Object.prototype 。它自己是没有原型的！
<br><pre name="code">var root = Object.prototype
Object.getPrototypeOf(root) === null // true
</pre>
<br>
<br>如果我们把Object.prototype上的所有属性都delete掉，我们就得到了一个裸对象！
<br>
<br>不过这样把Object.prototype给破坏，显然会影响我们的程序。好在咱有 iframe ！每个iframe的执行环境是独立的！于是可以每次创建一个iframe，把它里面的Object.prototype给拿来用！（听上去挺浪费的是吧……）
<br>
<br>代码如下：
<br><pre name="code">function nakedObject() {
	var iframe = document.createElement(&#39;iframe&#39;)
	iframe.width = iframe.height = 0
	iframe.style.display = &#39;none&#39;
	document.appendChild(iframe)
	iframe.src = &#39;javascript:&#39;
	var proto = iframe.contentWindow.Object.prototype
	iframe.parentNode.removeChild(iframe)
	iframe = null
	
	var props = [
		&#39;constructor&#39;, &#39;hasOwnProperty&#39;, &#39;propertyIsEnumerable&#39;,
		&#39;isPrototypeOf&#39;, &#39;toLocaleString&#39;, &#39;toString&#39;, &#39;valueOf&#39; 
	]
	for (var i = 0; i &lt; props.length; i++) {
		delete proto[props[i]]
	}
	return proto
}</pre>
<br>
<br>不过这个技巧确实相当浪费，仅仅为了创建一个对象就建立了一整个iframe及其环境！那可相当于一个完整的网页！在IE中创建几千个这样的对象，就需要好几秒时间和几十兆内存！所以这个技巧只能用于真正需要的地方，比如with。虽然同样可以用于模拟Object.create(null)的行为，但是是否真的有这样的需求？或许更简单的方式是把一个普通对象上从Object.prototype继承来的属性遮蔽掉（即设为undefined），尽管这对于with来说不适合（原因请读者自己想），但是对于其他大多数情况可能就够用了。因此，我也不打算把这个trick提交给es5-shim项目，而只是仅用到我自己的一个项目中：<a href="https://github.com/hax/my.js">my.js</a>。
<br>
<br>以上。
<br>
<br>
<br>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hax.iteye.com/blog/1663476#comments" style="color:red">已有 <strong>5</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hax.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>