---
layout: post
title:  "Lisp中定义变量*var*和var有什么不同？"
date:   2012-08-14 15:53:00
author: Kevin Lynx
categories: program
---

## Lisp中定义变量*var*和var有什么不同？
### by Kevin Lynx
### at 2012-08-14 15:53:00
### original <http://www.udpwork.com/item/7947.html>

<p>参考<a href="http://stackoverflow.com/questions/11932876/whats-difference-between-var-and-var-when-using-defvar">What’s difference betweenvarand var when using defvar?</a></p>
<p>其实，Common Lisp中使用defvar定义变量加不加星号没有区别。这只是一种Lisp程序员的约定。Lisp中并不使用特定的符号来影响语法元素，例如Ruby中通过给变量添加@前缀来标示该变量为类成员变量。这个问题引出了lisp总dynamic scope这个话题。</p>
<p>Lisp中变量分为两种，分别为lexical和special。这两种不同的变量有不同的作用域(scope)：词法作用域(lexical scope)和动态作用域(dynamic scope)。special variables通过defvar/defparameter/declare来定义。而lexical variables通常在let中定义。</p>
<p>这两种作用域有什么不同呢？引用&lt;ANSI Common Lisp&gt;里说的：</p>
<p>Under lexical scope, a symbol refers to the variable that has that name in the context where the symbol appears (define)</p>
<p>With dynamic scope, we look for a variable in the environment where the function is called, not in the environment where it was defined.</p>
<p>所以：</p>
<div><pre>(defvar b 3)

(defun add-to-b (x)
  (+ x b))

(add-to-b 1)
  =&gt; 4

(let ((b 4))
  (list (add-to-b 1) b))
=&gt; (5 4)

(let ((a 3))
  (defun add-to-a (x)
    (+ x a)))

(add-to-a 1)
  =&gt; 4

(let ((a 4))
  (list (add-to-a 1) a))
=&gt; (4 4)
</pre></div>
<p>add-to-b这个函数中使用的变量b是special variable，所以在调用add-to-b时，取的就是调用(called)这个函数时环境中的变量，所以：</p>
<div><pre>(let ((b 4))
  (list (add-to-b 1) b))
=&gt; (5 4)
</pre></div>
<p>取的就是let中临时出现的b。而add-to-a这个函数中使用的变量a是lexical variable，所以调用这个函数时，取的就是这个函数定义(defined)时的a，所以无论在哪里调用add-to-a，都是取的：</p>
<div><pre>(let ((a 3))
  (defun add-to-a (x)
    (+ x a)))
</pre></div>
<p>这里的a，也就是一直是3。</p>
<p>原文地址：<a href="http://codemacro.com/2012/08/14/dynamic-scope-in-lisp/">http://codemacro.com/2012/08/14/dynamic-scope-in-lisp/</a>
<br>
written by<a href="http://codemacro.com">Kevin Lynx</a> posted at<a href="http://codemacro.com">http://codemacro.com</a></p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7947.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7947.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7947.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>