---
layout: post
title:  "我的JavaScript之旅——从Scope Chain到Closure"
date:   2010-09-05 16:46:00
author: 菜阿彬
categories: program
---

## 我的JavaScript之旅——从Scope Chain到Closure
### by 菜阿彬
### at 2010-09-05 16:46:00
### original <http://www.cnblogs.com/CaiAbin/archive/2010/09/05/1818493.html>

<p>作者: <a href="http://www.cnblogs.com/CaiAbin/">菜阿彬</a> 发表于 2010-09-05 16:46 <a href="http://www.cnblogs.com/CaiAbin/archive/2010/09/05/1818493.html">原文链接</a> 阅读: 758 评论: 5</p><p> </p>
<div>
<pre><div><span style="color:#000000">a </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">1</span><span style="color:#000000">;<br></span><span style="color:#0000ff">function</span><span style="color:#000000"> Outer(x){<br>      </span><span style="color:#0000ff">function</span><span style="color:#000000"> Inner(y){</span><span style="color:#0000ff">return</span><span style="color:#000000"> x </span><span style="color:#000000">+</span><span style="color:#000000"> y;}<br>      </span><span style="color:#0000ff">return</span><span style="color:#000000"> Inner<br>}<br></span><span style="color:#0000ff">var</span><span style="color:#000000"> inner </span><span style="color:#000000">=</span><span style="color:#000000"> Outer(</span><span style="color:#000000">1</span><span style="color:#000000">);<br>inner(</span><span style="color:#000000">2</span><span style="color:#000000">);</span></div></pre>
</div>
<p>执行上面这段代码的过程中，有哪些事情发生？Inner函数为什么可以引用Outer函数的参数x？closure是怎么实现的？本文试图回答这些问题。</p>
<p> </p>
<p><strong>术语</strong></p>
<p>本文虽然所讲理论并不复杂，但用到不少名词，初读时相对比较晦涩，下面列出术语和简短解释，便于阅读时随时查看。</p>
<ul>
<li><em>global</em>：engine预先创建好的一个object，里面有所有built-in objects的属性。</li>
<li><em>globalContext</em>：本文术语，用作表示全局的execution context。</li>
<li><em>globalScopeChain</em>：本文术语，用作表示全局的execution context所拥有的Scope Chain，里面只有一个对象为global，用代码表示为 [global]</li>
<li><em>functionContext</em>：本文术语，用作表示执行函数代码时，进入的新的execution context。</li>
<li><em>VariableObject：ECMAScript</em>术语，在globalContext中即为global，在functionContext中是被创建的一个对象。在进入context时，被放到scope chain的最前方。</li>
<li><em>outerVariable</em>：本文术语，表示进入OuterFunctionContext时被创建的Variable Object。</li>
<li><em>innerVariable</em>：本文术语，表示进入InnerFunctionContext时被创建的Variable Object。</li>
<li><em>outerFunctionContext</em>：本文术语，用作表示执行Outer这个函数时，进入的execution context。</li>
<li><em>outerScopeChain</em>：本文术语，用作表示outerFunctionContext所拥有的Scope Chain。可用[outerVariable, global]表示。</li>
<li><em>innerFunctionContext</em>：本文术语，用作表示执行Inner这个函数时，进入的execution context。</li>
<li><em>innerScopeChain</em>：本文术语，用作表示innerFunctionContext所拥有的Scope Chain。可用[innerVariable, outerVariable, global]表示。</li>
</ul>
<p> </p>
<p><strong>JS代码种类</strong></p>
<p>JS代码分三种：</p>
<ol>
<li>Global code，全局代码</li>
<li>Functioncode，函数内的代码。</li>
<li>Eval code，为简单计，不在本文说明。</li>
</ol>
<p> </p>
<p><strong>Execution context</strong></p>
<p>任何一句JS代码，都是执行在一个特定的“execution context”下面。</p>
<p>执行Global code时，JavaScript engine将会创建一个全局的context，为表述简单，我们把它叫做<em>globalContext</em>。</p>
<p>而每次进入Functioncode时，将会创建一个新的context，在函数返回（或有未捕获的异常发生）时，退出这个新的context，本文把它叫做<em>functionContext</em>。</p>
<p> </p>
<div>
<pre><div><span style="color:#000000">a </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">1</span><span style="color:#000000">; </span><span style="color:#008000">//</span><span style="color:#008000">进入globalContext</span><span style="color:#008000"><br></span><span style="color:#000000"><br></span><span style="color:#0000ff">function</span><span style="color:#000000"> Outer(x){<br>      </span><span style="color:#0000ff">function</span><span style="color:#000000"> Inner(y){</span><span style="color:#0000ff">return</span><span style="color:#000000"> x </span><span style="color:#000000">+</span><span style="color:#000000"> y;}<br>      </span><span style="color:#0000ff">return</span><span style="color:#000000"> Inner<br>}        </span><span style="color:#008000">//</span><span style="color:#008000">在globalContext中创建Outer这个Function</span><span style="color:#008000"><br></span><span style="color:#000000"><br></span><span style="color:#0000ff">var</span><span style="color:#000000"> inner </span><span style="color:#000000">=</span><span style="color:#000000"> Outer(</span><span style="color:#000000">1</span><span style="color:#000000">); </span><span style="color:#008000">//</span><span style="color:#008000">执行Outer函数时进入新创建的outerFunctionContext上下文。</span></div><div><span style="color:#008000">　　　　　　　　　　　　   //然后退出，回到globalContext，把Outer(1)的返回值赋给inner这个变量。</span><span style="color:#008000"><br></span><span style="color:#000000">inner(</span><span style="color:#000000">2</span><span style="color:#000000">);  </span><span style="color:#008000">//</span><span style="color:#008000">进入InnerContext，执行Inner函数的return x + y，然后退出，回到globalContext</span></div></pre>
</div>
<p> </p>
<p> </p>
<p> </p>
<p><strong>Scope Chain</strong></p>
<p>每个execution context都有一个关联的Scope Chain。所谓Scope Chain，其实就是一个List，里面有若干个object。</p>
<p> </p>
<p><strong>global</strong></p>
<p><em>globalContext</em>所关联的Scope Chain，这里不妨称之为globalScopeChain，这个chain里面只有一个object，就是global，global是一个engine事先创建好的对象，所有的built-in Object（比如Function()、Object()、Math）都会作为这个global对象的属性。</p>
<p> </p>
<p><strong>Function型对象的[[Scope]]属性</strong></p>
<p>在<a href="http://www.cnblogs.com/CaiAbin/archive/2010/08/25/1808001.html">第一篇</a>创建Function型对象的步骤里，第5步说了，会为这个Function型对象创建一个[[Scope]]属性，不过当初没有提到，这个属性的值是当前context的Scope Chain。</p>
<p>Outer函数是在<em>globalContext</em>下创建起来的，因此Outer.[[Scope]] = <em>globalScopeChain</em>，也就是[global]。而Inner函数是在执行Outer函数时，也就是在<em>outerFunctionContext</em>下创建起来的，因此Inner.[[Scope]] = <em>OuterContext</em>的ScopeChain，是什么呢，往下看。</p>
<p> </p>
<p><strong>Entering execution context</strong></p>
<p>每次进入一个context（不管是<em>globaContext</em>还是<em>functionContext</em>）时，都会有一系列的事情发生。</p>
<ol>
<li>上面说到，每个context都有一个关联的Scope Chain，这个Scope Chain就是在此时会被创建起来的。</li>
<li>确定或创建一个<em>Variable Object</em>（ECMAScript术语），并把它放到Scope Chain的最前面。<br>对于<em>globalContext</em>，这个<em>Variable Object</em>就是<em>global</em>，被放到<em>globalScopeChain</em>里（也是<em>globalScopeChain</em>里唯一的一个对象）；<br>而如果进入到一个<em>functionContext</em>，则会创建一个Variable Object起来，也放到Scope Chain的最前面，并且还会额外再做一件事——就是把当前Function的[[Scope]]里所有object，放到Scope Chain里面。因此执行Outer函数时，Scope Chain是这样的：[<em>outerVariable</em>, <em>global</em>];上面知道，创建Inner函数时，这个Chain将作为Inner函数的[[Scope]]属性，因此进入Inner函数的执行时，它的Scope Chain就是[innerVariable, outerScopeChain]，也就是[<em>innerVariable</em>, <em>outerVariable</em>, <em>global</em>]。</li>
<li>实例化<em>Variable Object</em>，就是为<em>Variable Object</em>创建一些属性。<br>首先，如果是<em>functionContext</em>，则把函数的参数作为<em>Variable Object</em>的属性；<br>其次，把声明的函数作为<em>Variable Object</em>的属性；这里的属性<strong>将覆盖</strong>上面的同名属性。<br>再次，把声明的变量作为<em>Variable Object</em>的属性，属性的初始值均为undefined，只有在执行赋值语句后，才会有值。这边的属性<strong>不会覆盖</strong>上面的同名属性。</li>
<li>为当前context确定this，this在context中是不变的。<br>详细见下面的注解。</li>
</ol>
<p> </p>
<div>
<pre><div><span style="color:#008000">//在执行一切代码之前，进入globalContext，global对象也已经创建好。</span></div><div><span style="color:#008000"><br></span></div><div><span style="color:#008000">//1.然后</span><span style="color:#008000">创建Scope Chain</span><span style="color:#008000"><br></span><span style="color:#000000">globalContext.ScopeChain </span><span style="color:#000000">=</span><span style="color:#000000"> [];  <br><br></span><span style="color:#008000">//2.</span><span style="color:#008000">确定variable object为global，并加入到scope chain中</span><span style="color:#008000"><br></span><span style="color:#000000">variable </span><span style="color:#000000">=</span><span style="color:#000000"> global;<br>globalContext.ScopeChain.push(global); <br><br></span><span style="color:#008000">//3.</span><span style="color:#008000">实例化variable object，创建a、Outer和inner三个属性，初始值为null。</span><span style="color:#008000"><br></span><span style="color:#000000">variable.a </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000">;<br>variable.Outer </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000">;<br>variable.inner </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000">;<br><br></span><span style="color:#008000">//4.</span><span style="color:#008000">确定this，在globalContext中为global。</span><span style="color:#008000"><br></span><span style="color:#0000ff">this</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> global;<br><br></span><span style="color:#008000">//</span><span style="color:#008000">以上是进入globalContext时所做的事情</span></div><div><span style="color:#008000"> </span><span style="color:#008000"><br>//</span><span style="color:#008000">以下开始执行代码。</span><span style="color:#000000"><br>　　a </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">1</span><span style="color:#000000">;  </span></div><div><span style="color:#0000ff">　　function</span><span style="color:#000000"> Outer(x){<br>      　　</span><span style="color:#0000ff">function</span><span style="color:#000000"> Inner(y){</span><span style="color:#0000ff">return</span><span style="color:#000000"> x </span><span style="color:#000000">+</span><span style="color:#000000"> y;}<br>      　　</span><span style="color:#0000ff">return</span><span style="color:#000000"> Inner<br>　　}        </span></div><div><span style="color:#000000"> </span><span style="color:#008000">//</span><span style="color:#008000">对于以上这段代码，发生的事用伪代码表示如下：</span><span style="color:#008000"><br>//创建Outer函数，</span><span style="color:#008000">传入当前的scope chain，即[global]</span><span style="color:#008000"><br></span><span style="color:#000000">Outer </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">new</span><span style="color:#000000"> Function(</span><span style="color:#000000">''</span><span style="color:#000000">, </span><span style="color:#000000">''</span><span style="color:#000000"> [global])<br></span><span style="color:#008000">//</span><span style="color:#008000">为Outer.[[Scope]]赋值</span><span style="color:#008000"><br></span><span style="color:#000000">Outer.[[Scope]] </span><span style="color:#000000">=</span><span style="color:#000000"> [];<br>Outer.[[Scope]].push(global);<br></span><span style="color:#008000">//</span><span style="color:#008000">这时variable的属性Outer就指向这个函数了，不再是null。</span><span style="color:#008000"><br></span><span style="color:#000000">variable.Outer </span><span style="color:#000000">=</span><span style="color:#000000"> Outer<br><br></span><span style="color:#0000ff">　　var</span><span style="color:#000000"> inner </span><span style="color:#000000">=</span><span style="color:#000000"> Outer(</span><span style="color:#000000">1</span><span style="color:#000000">); </span><span style="color:#008000">//</span><span style="color:#008000">这段代码用伪代码表示如下：</span><span style="color:#008000"><br>//</span><span style="color:#008000">执行Outer函数，进入新创建的outerFunctionContext上下文</span><span style="color:#008000"><br>//1.</span><span style="color:#008000">创建ouerFunctionContext的Scope Chain，并放入Outer函数的[[Scope]]力所有的object</span><span style="color:#008000"><br></span><span style="color:#000000">outerFunctionContext.ScopeChain </span><span style="color:#000000">=</span><span style="color:#000000"> [];<br>outerFunctionContext.ScopeChain.push(global) </span><span style="color:#008000">//</span><span style="color:#008000">global是[[Scope]]里唯一的对象。</span><span style="color:#008000"><br>//2.</span><span style="color:#008000">创建Variable Object属性，并放到Scope Chain的最前方。</span><span style="color:#008000"><br></span><span style="color:#000000">outerVariable</span><span style="color:#000000">=</span><span style="color:#000000"> {arguments: xxx} </span><span style="color:#008000">//</span><span style="color:#008000">创建的variable有arguments等属性</span><span style="color:#008000"><br></span><span style="color:#000000">outerFunctionContext.ScopeChain.push(variable)<br></span><span style="color:#008000">//3.</span><span style="color:#008000">实例化variable object</span><span style="color:#008000"><br></span><span style="color:#000000">outerVariable.x </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">1</span><span style="color:#000000"><br>outerVariable.Inner </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">new</span><span style="color:#000000"> Function(</span><span style="color:#000000">'</span><span style="color:#000000">y</span><span style="color:#000000">'</span><span style="color:#000000">, </span><span style="color:#000000">'</span><span style="color:#000000">return x + y</span><span style="color:#000000">'</span><span style="color:#000000">, [outerVariable, global]) </span></div><div><span style="color:#000000"> </span><span style="color:#008000">//注意上句</span><span style="color:#008000">创建Inner函数时，会传入当前的Scope Chain，即[outerVariable, global]</span></div><div><span style="color:#008000"> </span><span style="color:#008000"><span>//4.</span></span><span style="font-family:&#39;Courier New&#39;;color:#008000">确定Outer函数体内的this参数，就是新创建的函数对象。</span></div><div><span style="color:#000000"><br></span><span style="color:#008000">//</span><span style="color:#008000">最后回到globalContext中，把新建的Inner函数对象，返回给inner变量。</span><span style="color:#008000"><br></span><span style="color:#000000"><br>　　inner(</span><span style="color:#000000">2</span><span style="color:#000000">);  </span><span style="color:#008000">//</span><span style="color:#008000">最后执行的这句代码，将创建并进入InnerContext。</span></div></pre>
</div>
<p> </p>
<p><strong>初步结论</strong></p>
<p>现在已经知道，执行Outer函数时，对应的outerScopeChain的图如下，注意global对象忽略了指向所有built-in object的属性：</p>
<p>　　<img src="http://pic002.cnblogs.com/img/hongxuan20/201009/2010090516310838.png"></p>
<p>执行Inner函数时，对应的innerScopeChain的图如下：</p>
<p>　　<img src="http://pic002.cnblogs.com/img/hongxuan20/201009/2010090516315353.png"></p>
<p> </p>
<p><strong>Scope Chain的作用</strong></p>
<p>Scope chain的图出来了，那么它用来干嘛呢？执行inner函数的return x + y，会发现，我们需要两个变量，x和y。那么JavaScript将循着Scope Chain来查找，与__proto__链配合，也就是首先在innerVariable（以及其__proto__链）找x，没找到，则到outerVariable中找x，找到为1。 找y时类似。这就是Inner函数体中，可以访问得到Outer函数中定义的参数x的原理所在，不难想象，如果Outer函数中定义了局部变量z，那么z也会出现在outerVariable对象中，因此同样可以被Inner函数访问。内部函数可以引用外部函数的参数以及变量，这就是JavaScript传说中的<strong>闭包(Closure)</strong>。</p>
<p> </p>
<p>下篇将对closure进一步阐述，待续。</p><img src="http://www.cnblogs.com/CaiAbin/aggbug/1818493.html?type=1" width="1" height="1" alt=""><p>评论: 5　<a href="http://www.cnblogs.com/CaiAbin/archive/2010/09/05/1818493.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/CaiAbin/archive/2010/09/05/1818493.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/73303/">阿里巴巴：攻向eBay后院</a><span style="color:gray">(2010-09-06 12:03)</span><br>· <a href="http://news.cnblogs.com/n/73302/">新设计：树林里的办公室</a><span style="color:gray">(2010-09-06 12:01)</span><br>· <a href="http://news.cnblogs.com/n/73301/">三星公开表示将优先Android并降低WP7位置</a><span style="color:gray">(2010-09-06 11:54)</span><br>· <a href="http://news.cnblogs.com/n/73300/">GDB 7.2 发布</a><span style="color:gray">(2010-09-06 11:53)</span><br>· <a href="http://news.cnblogs.com/n/73299/">金山即将正式对外公布雷军回归消息</a><span style="color:gray">(2010-09-06 11:51)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/73223/">对女程序做梦tweet的强力回复</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>