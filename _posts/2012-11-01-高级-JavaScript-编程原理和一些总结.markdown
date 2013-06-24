---
layout: post
title:  "高级 JavaScript 编程原理和一些总结"
date:   2012-11-01 13:10:58
author: Bruce Dou
categories: program
---

## 高级 JavaScript 编程原理和一些总结
### by Bruce Dou
### at 2012-11-01 13:10:58
### original <http://blog.eood.cn/advanced_javascript>

<h1>JavaScript 的特殊之处</h1>
<p>JavaScript 用 prototype 和 constructor 实现了继承等 OOP 特性，但是他们的理解非常”绕”。</p>
<p>每个函数对象都有一个 prototype 对象<br>
 每个函数对象的 prototype 都有个 constructor 属性<br>
 函数构建的时候 prototype 指向父对象的 prototype，或者理解为复制一份<br>
 函数构建的时候 prototype 的 constructor 指向自己<br>
 调用函数的时候会执行 constructor 方法<br>
 函数可以调用自己 prototype 对象的属性和方法<br>
 函数可以覆盖自己 prototype 对象的属性和方法</p>
<p>你可能会在很多代码里看到类似 Array.prototype.slice 这样的调用方式。</p>
<h1>JavaScript 函数的隐含特性</h1>
<h2>函数参数获取的第二种方式</h2>
<p>不论你如何定义函数的参数，在调用函数的时候，所有参数都会以数组元素的形式赋值给函数的 arguments 这个变量<br>
 调用函数的对象被赋值为 caller ，被调用函数被赋值为 callee</p>
<h2>作用域链和函数提升</h2>
<p>普通 function 的定义可以做到前置调用，而变量式声明的函数则不能前置调用。</p>
<p>浏览器的顶级作用域是 window 。</p>
<h2>其他</h2>
<p><strong>一切皆为对象和匿名函数的支持，让你可以这样定义和执行函数:</strong></p>
<p>(function(){})()</p>
<p><strong>JavaScript 的 call apply caller callee</strong></p>
<p>call 和 apply 是将函数绑定到其他对象上执行。caller callee 的解释见前文。</p>
<h1>ECMA 5 和 AMD， CommonJS</h1>
<p>AMD (Asynchronous Module Definition) 提供了在浏览器端异步加载所需 js 文件的方式。</p>
<p>CommonJS 定义了服务器端模块化的支持方式，其中:</p>
<p>require() 来引用和调用文件中的对象</p>
<p>module.exports 和 exports 来为文件中的对象提供调用接口</p>
<h2>引用:</h2>
<p>http://ecma-international.org/ecma-262/5.1/</p>
<p>http://www.commonjs.org/</p>
<p>http://addyosmani.com/writing-modular-js/</p>
<p>http://requirejs.org/docs/whyamd.html</p>