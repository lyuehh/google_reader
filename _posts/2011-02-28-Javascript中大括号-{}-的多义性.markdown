---
layout: post
title:  "Javascript中大括号“{}”的多义性"
date:   2011-02-28 13:40:00
author: snandy
categories: program
---

## Javascript中大括号“{}”的多义性
### by snandy
### at 2011-02-28 13:40:00
### original <http://www.cnblogs.com/snandy/archive/2011/02/28/1966894.html>

<p><p>JS中大括号有四种语义作用<br><br><strong>语义1，组织复合语句,这是最常见的</strong>
</p>
<div>
<pre>if( condition ) {
  //...
}else {
  //...
}
for() {
  //...
}
</pre>
</div>
<br>
<p><strong>语义2，对象直接量声明</strong>
</p>
<div>
<pre>var obj = {
	name : 'jack',
	age : 23
};
</pre>
</div>
<p>整个是个赋值语句，其中的{name:'jack',age:23}是个表达式。</p>
<br>
<p><strong>语义3，声明函数或函数直接量</strong>
</p>
<div>
<pre>function f1(){
	//...
}

var f2 = function(){
	//...
}
</pre>
</div>
<p>f1与非f2的区别是前者在语法解释期，后者在运行期。区别在于：如果调用该函数的代码在函数定义之后，则没有区别；如果调用该函数的代码在函数定义之前，则f1仍然可以调用，f2则会报错，提示f2未定义。</p>
<br>
<p><strong>语义4，结构化异常处理的语法符号</strong>
</p>
<div>
<pre>try {
	//...
}catch( ex ){
	//...
}finally{
	//...
}
</pre>
</div>
<p>这里的大括号与符合语句(<strong>语义1</strong> )是有区别的，大括号中如果只有一条语句，在if/else/for等中大括号是可以省略的，但try/catch/finally则不能省略。</p>
<br>
<p>以下代码纠结了偶N久
<div>
<pre>function(){}() //匿名函数立即执行， 语法分析期报
{}.constructor //获取对象直接量的构造器，语法分析期报错
</pre>
</div>
</p>
令人不解的是为何[].constructor这么写却不报错呢，一个是想获取对象直接量的构造器，一个是获取数组直接量的构造器而已。<br><br>当然添加个变量接收也不会报错 <br><br>var c = {}.constructor;<br><br>同样的情况如 <br><br>var fn = function(){}()，也不会报错。<br><br>实际上是js的“语句优先”在作怪，即{}被理解成复合语句块(<strong>语义1</strong> )而不是对象直接量(<strong>语义2</strong> )或声明函数(<strong>语义3</strong> )的语义。<br><br>function(){}()，大括号被理解成复合语句，自然前面的function()声明函数的语法不完整导致语法分析期出错。<br>{}.constructor，大括号被理解成复合语句，大括号后面是点运算符，点运算符前没有合理的对象自然也报错。<br><br>修复方式众所周知：加个强制运算符()<br>(function(){})()，(function(){});//强制其理解为函数(<strong>语义3</strong> )，“函数()”表示执行该函数，即声明后立即执行了。<br>({}).constructor //({})强制把大括号理解成对象直接量(<strong>语义2 </strong>)，“对象.xx”表示获取对象的成员，自然后面的点运算符可以正常执行了。
<img src="http://www.cnblogs.com/snandy/aggbug/1966894.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/snandy/">snandy</a> 发表于 2011-02-28 13:40 <a href="http://www.cnblogs.com/snandy/archive/2011/02/28/1966894.html">原文链接</a></p><p>评论: 4　<a href="http://www.cnblogs.com/snandy/archive/2011/02/28/1966894.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/snandy/archive/2011/02/28/1966894.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/92438/">在 Chrome 上使用 ActiveX 控件</a><span style="color:gray">(2011-02-28 21:52)</span><br>· <a href="http://news.cnblogs.com/n/92437/">中国企业家：乔布斯中国CEO的三堂必修课</a><span style="color:gray">(2011-02-28 21:43)</span><br>· <a href="http://news.cnblogs.com/n/92435/">风投连夜约谈投资 开价1.4亿收购谷姐</a><span style="color:gray">(2011-02-28 21:41)</span><br>· <a href="http://news.cnblogs.com/n/92434/">摩托罗拉Xoom平板电脑的八大缺点</a><span style="color:gray">(2011-02-28 21:39)</span><br>· <a href="http://news.cnblogs.com/n/92433/">当当网将于3月9日发布第四季度财报</a><span style="color:gray">(2011-02-28 21:38)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/92343/">（麻省理工免费课程）计算机科学和编程</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>