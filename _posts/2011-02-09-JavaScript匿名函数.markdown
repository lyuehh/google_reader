---
layout: post
title:  "JavaScript匿名函数"
date:   2011-02-09 15:01:28
author: 
categories: program
---

## JavaScript匿名函数
### by 
### at 2011-02-09 15:01:28
### original <http://www.javaeye.com/topic/901971>

<p>在JavaScript中，匿名函数是一种非常强大的工具，其特点与用途之多，难于在一篇文章中概括其全部，不过掌握其基本运用并不算太难，本文将尝试从如下方面进行归纳：</p>
<ul>
<li>JavaScript匿名函数基本特点</li>
<li>
<a title="JavaScript变量及其作用域" href="http://www.iteye.com/blog/901893">JavaScript变量及其作用域</a>（另外开贴，草稿中）</li>
<li>匿名函数基本用法</li>
<li>
<a title="JavaScript闭包" href="http://www.iteye.com/blog/901891">JavaScript闭包</a>（另外开贴，草稿中）</li>
</ul>
<p>其中有2个知识点是笔者认为需要涉及的，不过好在只有你在担忧代码的性能和优雅问题之时，你才需要额外关注，不妨继续往下概览全文@_^</p>
<p> </p>
<h3>匿名函数基本特点</h3>
<p>这个特点应该是自解释的，即它是一个没有名字的函数，或说，没有指针指向该函数</p>
<p> </p>
<pre name="code">function() {
	alert("hello world");
}</pre>
<p> </p>
<p>这段代码是完全合法的，但问题是定义之后无法调用，不过本文后面有一个小案例来教你如何使用。 </p>
<p> </p>
<h3>匿名函数基本用法</h3>
<p>从其特点决定，匿名函数基本会有如下4大类用途 ：</p>
<p> </p>
<p>1、用来声明一个函数表达式，赋值给某一个变量保存起来，留待调用</p>
<p>
</p>
<pre name="code">var demo = function() {
	alert("hello world");
};
//some script
demo();

//一段设置XMLHttpRequest回调属性的代码
xhr.onreadystatechange = function() {
	//some script
};</pre>
 
<p>2、将匿名函数当作参数传递</p>
<p>
</p>
<pre name="code">//一段调用dwr发布的远程对象逻辑的代码
dwrobj.findById(uid, function(data) {
	//some script
});</pre>

<p>3、当作返回值动态获取函数实现</p>
<p> 
</p>
<pre name="code">function createFn(flag) {
	if (flag)
		return function() {
			alert("dude");
		};
	else
		return function() {
			alert("buddy");
		};
}
var demo = createFn(1);
demo();
demo = createFn(0);
demo();
demo = createFn("");
demo();
demo = createFn(NaN);
demo();
demo = createFn(undefined);
demo();
demo = createFn(null);
demo();</pre>

<p> 4、利用其变量作用域的特点创建“块级作用域”</p>
<p>之所以打上引号是因为，在ECMA-262中，没block-level scope这个概念，但是通过匿名函数，我们可以模拟出来</p>
<p>首先来看这段代码：</p>
<p>
</p>
<pre name="code">function demo() {
	for ( var i = 0; i &lt; 3; i++) {
		var j = &quot;inner variable&quot;;
	}
	alert(i);
	alert(j);
}
demo();
//alert(j);//勿取消，留待在JavaScript变量及其作用域中进行分析</pre>

<p>结论为：先后弹出3，“inner variable”</p>
<p>看来在for语句块中定义的i和j在语句块结束时并没销毁，但在函数运行结束后销毁了，虽然有些出乎意料之外，但是请先记住这个效果，留待以后分析。</p>
<p> </p>
<p>为了满足某种需求，并且利用上刚才的效果，你可能需要在某一个块中定义某些变量，然后做到用后即弃的效果，但是需要对上述代码进行修改：</p>
<p>
</p>
<pre name="code">function() {
	var j = "inner variable";
	alert("do sth. with variable j");
}();
try {
	alert(j);
} catch (e) {
	alert(e);
}</pre>

<p> 首先定义一个匿名函数，然后加了一对小括号让它立马运行，如果能弹出一个警示框，而不是j在块中定义的值（“inner variable”），应该就是ok的，但是最后在浏览器中会报错：</p>
<p> </p>
<p>因为JavaScript将function关键字视作一个函数声明的开始，其后不能跟随小括号，要这样做的话，得将它外面再套一层小括号，从而转换成一个函数表达式。</p>
<p>
</p>
<pre name="code">(function() {
	var j = "inner variable";
	alert("do sth. with variable j");
})();
try {
	alert(j);
} catch (e) {
	alert(e);
}</pre>

<p>这样做的目的无非就是避免向全局作用域中添加过多的咚咚，因为他们所占用的资源要等到浏览器关闭的时候才会释放，同时也是为了避免命名冲突。</p>
<p> </p>
<p>没错，这其实也就是jQuery中一段比较经典的语法。你再想想看为什么上面的try语句块不能替换成if？</p>
<p>
</p>
<pre name="code">if (f)
	alert(j);
else
	alert("variable j has been released");</pre>

<p> </p>
<p>在JavaScript中，匿名函数的用法非常之灵活，非master不能阐述详尽且得当，因此本文纯属抛砖引玉，职业习惯告诉我，一个好的问题胜似一个好的答案。</p>
          
          <br><br>
          作者: <a href="http://cuesky.javaeye.com">cues</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/901971" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>