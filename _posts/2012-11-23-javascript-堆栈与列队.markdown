---
layout: post
title:  "javascript 堆栈与列队"
date:   2012-11-23 21:08:00
author: 司徒正美
categories: program
---

## javascript 堆栈与列队
### by 司徒正美
### at 2012-11-23 21:08:00
### original <http://www.cnblogs.com/rubylouvre/archive/2012/11/23/2785051.html>

<p>javascript数组是一个逆天的存在，到了ecma262v5，它已经是堆栈，列队及迭代器的合体。有时候我们不需要这么强大的东西，这只要考虑到for循环太麻烦了，我们只需要非常简单的遍历，于是想用普通对象模拟一个就是。</p><p>首先是堆栈，先进后出</p><br>              function Stack(){   }<br>              Stack.prototype = {<br>                  add: function(el, pt){<br>                      this._first =  pt = {//_first是不断变的<br>                          _next:this._first,<br>                          el: el<br>                      }<br>                      if (pt._next) {<br>                          pt._next._prev = pt;<br>                      }<br>                      return this;<br>                  }<br>              }<br>              var s = new Stack;<br><br>              s.add("1").add("2").add("3")<br>             <br>              var pt = s._first;<br>              while (pt) {<br>                  alert(pt.el)<br>                  pt = pt._next;<br>              }<br><p>接着是列队,先进先出：</p><br><br>              function Queue(){   }<br>              Queue.prototype = {<br>                  add: function(el){<br>                      if( this._last ){ <br>                          this._last =  this._last._next = {//_last是不断变的<br>                              el: el,<br>                              _next: null//设置_last属性表示最后一个元素，并且让新增元素成为它的一个属性值<br>                          }<br>                      }else{<br>                          this._last = this._first = {//我们要设置一个_first属性表示第一个元素<br>                              el: el,<br>                              _next: null<br>                          }<br>                      }<br>                      return this;<br>                  }<br>              }<br>              var q = new Queue<br>              q.add("1").add("2").add("3")<br>              var pt = q._first;<br>              while (pt) {<br>                  console.log(pt.el)<br>                  pt = pt._next;<br>              }<br><br><p>由于这两种结构的每个结点都是对象，因此它可以一直循环下去，直接_next为null。这样就避免[1,0,null,2]这样的集合遇假值中断的麻烦。</p><img src="http://www.cnblogs.com/rubylouvre/aggbug/2785051.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/rubylouvre/archive/2012/11/23/2785051.html">本文链接</a></p>