---
layout: post
title:  "JavaScript中的关键字＂VAR＂使用祥"
date:   2010-12-21 10:08:35
author: 
categories: program
---

## JavaScript中的关键字＂VAR＂使用祥
### by 
### at 2010-12-21 10:08:35
### original <http://my.oschina.net/s01u/blog/11375>

都不记得是什么时候看的JScript的语法教程了，里面说在声明变量时忽略var关键字是完全合法的。当时也因为觉得JavaScript是loosely-typed的语言，所以var可能真的就是个摆设。但是事实常常又证明想当然的结果是不可靠的。    看看下面这几个例子的结果就知道问题了：    No.1 var var00 = 0;
document.write(var0...