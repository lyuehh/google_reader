---
layout: post
title:  "如何将let结构（block scope）转换到当前的JavaScript代码"
date:   2011-07-12 17:24:26
author: 
categories: program
---

## 如何将let结构（block scope）转换到当前的JavaScript代码
### by 
### at 2011-07-12 17:24:26
### original <http://hax.iteye.com/blog/1122690>

本文是对<a href="http://hax.iteye.com/blog/277398">如何将let结构转换到ES3代码</a>的补充。
<br>
<br>首先，原文所说的将let转换为with的方法有几个缺陷需要说明：
<br>
<br>1. with虽然可附加一个新的scope，但是由于引入的是一个JS对象，所以Object.prototype上的属性也被引入了该scope。比方说你无法在with里访问外部的toString()方法，因为你访问到的实际上变成了Object.prototype.toString。
<br>
<br>再来一个例子：
<br>
<br><pre name="code">
function login() {
	var user = getUserNameFromCookie()
	for (let i = 0; i &lt; 3; i++) {
		let pwd = prompt(&#39;Please enter your password:&#39;)
		if (securityCheck(user, pwd)) {
			return true
		} else {
			alert(&#39;Wrong password! Please try again...&#39;)
		}
	}
	return false
}

if (login()) {
	alert(&#39;Welcome!&#39;)
} else {
	alert(&#39;Failed to login!&#39;)
}

function securityCheck(user, pwd) {
	//...do some security checking
}
function getUserNameFromCookie() {
	//...read user name from cookie
}
</pre>
<br>
<br>如果这段代码里的 login 函数被转换为：
<br><pre name="code">
function login() {
	var user = getUserNameFromCookie()
	for (var i = 0; i &lt; 3; i++) {
		var temp = prompt(&#39;Please enter your password:&#39;)
		with({pwd:temp}) {
			if (securityCheck(user, pwd)) {
				return true
			} else {
				alert(&#39;Wrong password! Please try again...&#39;)
			}
		}
	}
	return false
}
</pre>
<br>
<br>就会存在一些隐患。
<br>
<br>例如：
<br>如果在login执行之前，注入了这样的代码：
<br>Object.prototype.user = 'hacker'
<br>
<br>那只要输入hacker用户对应的密码就可以通过检查。
<br>
<br>更简单的方式是：
<br>Object.prototype.securityCheck = function() { return true }
<br>
<br>【注意上述只是示例，不代表真实世界里会有这样的写法。】
<br>
<br>
<br>另一个隐藏问题是，如果with(scope)引用了一个函数，则该函数在with块内被调用是以scope作为this对象的。这造成采用with时行为不一致，并可能影响其他reference。
<br>
<br>比如：
<br><pre name="code">
function test() {
     let x = 1
     {
        let f = function(){ this.x = 2 }
        f()
        return x
     }
}
test()
</pre>
<br>这段代码执行的结果应该是返回1，并且global上的x=2。
<br>
<br>但是
<br><pre name="code">
function test() {
     with(x:1)
     {
        with({f: function(){ this.x = 2 }}) {
            f()
            return x
        }
     }
}
test()
</pre>
<br>这段代码的结果是返回2，global上并不会产生x。
<br>
<br>
<br>2. with语句在ES5的strict模式下被禁用。
<br>
<br>with实际上是在lexical scope上开了一个后门，这对依赖静态代码分析的辅助工具（如IDE、压缩器等）和性能优化（如新的支持JIT的引擎）都造成了障碍。
<br>另外从第1条可以看到with的设计天生残疾，易造成语句的二义性，并且这种二义性可以由完全无关的代码引入。
<br>
<br>因此ES5的strict模式直接禁用了该特性。
<br>
<br>
<br>上述两点注定了block scope转换到with是一个不太好的做法。
<br>
<br>那么还有什么其他方式么？
<br>
<br>幸运的是，确实存在一个我以前遗忘的特性，try catch语句中，catch语句也会增加一层新的scope！
<br>
<br>Traceur是一个Google开源项目，可以将ES5和Harmony的一些新特性编译为传统的ES3代码。让我们看看它是怎样转换上述代码的：
<br>
<br><pre name="code">
function login() { 
  var user = getUserNameFromCookie(); 
  for(var i = 0; i &lt; 3; i ++) { 
    try { 
      throw undefined; 
    } catch(pwd) { 
      pwd = prompt(&#39;Please enter your password:&#39;); 
      if(securityCheck(user, pwd)) { 
        return true; 
      } else { 
        alert(&#39;Wrong password! Please try again...&#39;); 
      } 
    } 
  } 
  return false; 
} 
</pre>
<br>
<br>不需要更多解释了。
<br>
<br>
<br>总结，用try/catch进行let转换方式如下：
<br>
<br><pre name="code">
    statement {  
        ...  
        let n1 = exp1  
        ...  
        let n2 = exp2  
        ...  
        let n3 = exp3, n4 = exp4, ...  
        ...  
    }  
</pre>
<br>转换为
<br><pre name="code">
    statement {
        try { throw undefined } catch (n1) {
        try { throw undefined } catch (n2) {
        try { throw undefined } catch (n3) {
        try { throw undefined } catch (n4) {
            ...
            n1 = exp1
            ...
            n2 = exp2
            ...
            n3 = exp3; n4 = exp4; ...
        }}}}
    }
</pre>
<br>
<br>此外，原文中的let语句形式也可以容易的转换：
<br>
<br>let语句
<br><pre name="code">
    var x = 5;    
    var y = 0;    
        
    let (x = x+10, y = 12) {    
      print(x+y + "\n");    
    }    
        
    print((x + y) + "\n");    
</pre>
<br>可转换为：
<br><pre name="code">
    var x = 5;    
    var y = 0;    
        
    try { throw x+10 } catch(x) { try { throw 12 } catch(y) {    
      print(x+y + "\n");    
    }}
        
    print((x + y) + "\n");    
</pre>
<br>
<br>不过对比之前的转换公式，上述转换有一点小小的不严谨——虽然在这个case里并不会出现问题。这就作为一个习题留给读者了。
<br>
<br>
<br>
<br>【7月13日的进一步补充】
<br>
<br>
<br>不过，使用catch也有一些实践上的问题：
<br>
<br>1. 在各浏览器脚本引擎的较早版本中，try/catch方案中也有一些类似with的副作用。比如catch块内的变量访问会优先搜索Object.prototype，或者如果catch到的是一个函数，执行该函数的this上下文会是一个隐含的scope对象。幸好这些问题现在已经都被修复了。
<br>
<br>2. 直到JScript 5.8（也就是直到IE8），对catch的处理是与其他引擎不一致的，那就是在JScript中catch不会产生新的scope。（感谢Winter指出此点）
<br>
<br>
<br>所以如果考虑转换目标必须包括IE6到IE8，则就只能用原文中的with方案。
<br>
<br>为了克服本文所指出的with的问题，可以采用防御性方法——将with块中所有用到的references都照抄一遍：
<br>
<br><pre name="code">
function login() {
	var user = getUserNameFromCookie()
	for (var i = 0; i &lt; 3; i++) {
		var temp = prompt(&#39;Please enter your password:&#39;)
		with({pwd:temp, securityCheck:securityCheck, user:user, alert:alert}) {
			if (securityCheck(user, pwd)) {
				return true
			} else {
				alert(&#39;Wrong password! Please try again...&#39;)
			}
		}
	}
	return false
}
</pre>
<br>
<br>注意 with({pwd:temp, securityCheck:securityCheck, user:user, alert:alert}) 这里。
<br>
<br>不过这个方式是有局限性的，即其只适用于读取的情形，但with块内可能出现 x = exp 或 delete x ，也就是出现对于用到的外部reference的修改或删除操作。这时就无能为力了。除非with代码中所有对外部reference的修改都是立即执行的（即不会出现在闭包中），那么可以在with结束后同步。此外，若with块内出现eval语句，也就无法静态确定会用到哪些references。不过在实践中通常较少出现上述代码形式。
<br>
<br>另一种思路是在创建scope对象时覆盖所有Object.prototype上的名称，如：
<br><pre name="code">
// create a "clean" scope object

var scope = {}
// override props from Object.prototype
scope.constructor = constructor
scope.toString = toString
scope.toLocaleString = toLocaleString
scope.valueOf = valueOf
scope.hasOwnProperty = hasOwnProperty
scope.isPrototypeOf = isPrototypeOf
scope.propertyIsEnumerable = propertyIsEnumerable
for (var k in Object.prototype) {
   scope[k] = eval(k)
}
// add bindings
scope.x = ...
scope.y = ...
...
with(scope) {
   alert(x + y)
}
</pre>
<br>
<br>不过这个方式存在漏洞。【为什么呢？请读者自行思考。】
<br>
<br>尽管如此，这个思路仍然揭示了一种可能，那就是如果能创建一个不带有prototype的“干净”的对象，则就克服了with的许多问题。
<br>
<br>恰好，IE中的DOM对象正是这样一朵奇葩。我们希望挑选的DOM对象是，可任意产生新对象（因为我们需要许多的scope对象），可添加属性（理论上若该对象可作为其他对象的prototype也可，不过JScript禁止用非native JS对象作为prototype），并且本身固有属性越少越好，最好是光秃秃的（这样就避免覆盖固有属性造成的问题）。
<br>
<br>经过我的研究，符合上述条件的最佳对象是一些collection对象（还有更好的选择么？欢迎读者提供线索）。比如document.createStyleSheet().rules 。这个对象上只有3个属性：constructor、length、item。
<br>
<br>所以只剩下两个问题，一个是如果要用到的外部变量名为constructor/length/item，另一个是用到的外部变量是一个内部访问this引用的函数。
<br>
<br>对于问题一，可以在转换时，对这三个名称做特殊处理（好在只有3个特例）。对于问题二，我们可以将with内的函数调用显式的绑定this。
<br>
<br>
<br>以上。
<br>
<br>
<br>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hax.iteye.com/blog/1122690#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hax.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>