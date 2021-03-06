---
layout: post
title:  "【技术宅的情人节】99种用Racket说I love you的方式"
date:   2013-03-14 15:00:00
author: 
categories: program
---

## 【技术宅的情人节】99种用Racket说I love you的方式
### by 
### at 2013-03-14 15:00:00
### original <http://www.soimort.org/posts/145>

<p><img src="http://pre.racket-lang.org/racket/collects/icons/heart.png" alt=""></p>

<p>今天是3月14日，也就是传说中的<strong>白色情人节</strong>（据说是个被表白一方向表白方回赠礼物以表示心意的好日子，蕴含着人们对天下有情人终成眷属的<a href="https://www.google.com/search?q=%E6%84%BF%E5%A4%A9%E4%B8%8B%E6%9C%89%E6%83%85%E4%BA%BA%E7%BB%88%E6%88%90%E5%85%84%E5%A6%B9">良好祝愿</a>）。</p>

<p>作为一个技术宅，应该在这一天准备什么样的礼物给妹子捏？99朵玫瑰？还是巧克力？可不要被这些<a href="http://zh.wikipedia.org/wiki/%E7%99%BD%E8%89%B2%E6%83%85%E4%BA%BA%E7%AF%80">商家营销手段</a>给骗了——就像Sheldon家Amy说的一样，Nerds们过一个真正属于自己的“No dinner, no romance, no gifts”的情人节才是正道啊；这种时候，只要像往常那样写写代码，用代码来说出“I love you”就好了。当然，为了表示诚意，我们要用99种——没错，是用99种不同的方式来写同一句话。</p>

<p>用什么语言写好捏？当然是<a href="http://pre.racket-lang.org/racket/collects/icons/heart.png">满满的都是爱</a>的<a href="http://racket-lang.org/">Racket</a>咯ヽ(♡´ω`♡)ノ</p>

<p>（先放上一段忘了从哪偷来的代码……在DrRacket里运行，就能看到一颗doki~doki跳动的红心哦）</p>
<div><pre><code><span>#lang racket</span>

<span>(</span><span>require </span><span>2</span><span>htdp/universe</span> <span>2</span><span>htdp/image</span><span>)</span>

<span>(</span><span>define </span><span>scale-factor</span> <span>10</span><span>)</span>
<span>(</span><span>define </span><span>window-size</span> <span>(</span><span>* </span><span>scale-factor</span> <span>20</span><span>))</span>

<span>(</span><span>define </span><span>(</span><span>lobe</span> <span>shape</span> <span>size</span><span>)</span> <span>(</span><span>shape</span> <span>(</span><span>* </span><span>size</span> <span>scale-factor</span><span>)</span> <span>&#39;solid</span> <span>&#39;red</span><span>))</span>
<span>(</span><span>define </span><span>•</span> <span>(</span><span>lobe</span> <span>circle</span> <span>4</span><span>))</span>
<span>(</span><span>define </span><span>••</span> <span>(</span><span>overlay/offset</span> <span>•</span> <span>(</span><span>* </span><span>7</span> <span>scale-factor</span><span>)</span> <span>0</span> <span>•</span><span>))</span>
<span>(</span><span>define </span><span>▼</span> <span>(</span><span>rotate</span> <span>180</span> <span>(</span><span>lobe</span> <span>triangle</span> <span>14</span><span>)))</span>
<span>(</span><span>define </span><span>♥</span> <span>(</span><span>overlay/offset</span> <span>••</span> <span>0</span> <span>(</span><span>* </span><span>8</span> <span>scale-factor</span><span>)</span> <span>▼</span><span>))</span>

<span>(</span><span>define </span><span>amplitude</span> <span>1/10</span><span>)</span>
<span>(</span><span>define </span><span>speed</span> <span>1/2</span><span>)</span>

<span>(</span><span>define </span><span>(</span><span>☺♥</span> <span>n</span><span>)</span>
  <span>(</span><span>overlay</span> <span>(</span><span>scale</span> <span>(</span><span>add1 </span><span>(</span><span>* </span><span>(</span><span>sin </span><span>(</span><span>* </span><span>n</span> <span>speed</span><span>))</span> <span>amplitude</span><span>))</span> <span>♥</span><span>)</span>
         <span>(</span><span>empty-scene</span> <span>window-size</span> <span>window-size</span><span>)))</span>
<span>(</span><span>animate</span> <span>☺♥</span><span>)</span> 
</code></pre></div>
<p>下面开始，进入正题。</p>

<hr>

<p>以前，穷屌丝书生喜欢发问，茴香豆的茴字有几种写法？而高富帅则会问，I love you里面的love有几种make法？不管做任何一件事情，方法上的选择总是很重要——对于敲代码的程序猿来说也一样。</p>

<p>在Python里，做任何事，通常有且仅有一种正确的方法；Pythonic教的传教士说：只有一种姿势最优雅，其他的体位都是邪恶的。但是Rubyists们却不以为然。他们是一群奉行自由主义的嬉皮，不需要Python里面那种类似<a href="http://www.python.org/dev/peps/pep-0020/">哲♂学</a>一样的教条束缚；在解决同一个最基本的问题上有时也会跟随feeling写出新奇而孑然不同的代码。</p>

<p>记得刚开始接触Ruby那会，试着<a href="https://github.com/soimort/p99#p-99-ninety-nine-prolog-problems-morts-solutions">用Ruby解过</a>那套经典的<a href="https://sites.google.com/site/prologsite/prolog-problems">P-99（99个Prolog问题）</a>。前面好几十个问题差不多都是关于list的操作：做过一遍之后立刻就感觉到，Ruby单从表达问题的灵活性上来说，的确不是现有的其他主流语言所能够比拟的——显然，作为Perl的继任者，Ruby很好地发扬光大了这一点。</p>

<p>嘛，当然，要说到表达能力，基于<a href="http://en.wikipedia.org/wiki/S-expression">抖S前入式</a>、把数据和代码结构高度统一起来的Lisp才是编程语言中真正的总攻（虽然这也让一部分Lisper养成了光说空话不干实事+吹嘘自己造车轮造得多么的好+不合作不贡献社区的<a href="http://www.soimort.org/posts/124">坏毛病</a>）。话说回来，在Lisp里，哪怕是做最简单的一件事情——构造一个list，也能凭空造出千般万般变化。不信？继续看下去。</p>

<hr>

<p>为了帮助童鞋们更好地理解Racket语言中的核心概念，Univ of Utah的CS系教授Matthew Might写了这篇<a href="http://matt.might.net/articles/i-love-you-in-racket/">《99 ways to say <code>&#39;(I love you)</code>》</a>。你不仅能够从中领略到Lisp那无与伦比的表达能力，还能借此复习一下Scheme/Racket中的那些基础知识点：lambdas，higher-order function，pattern matching，delayed evaluation，macros，等等——当然，后者才是此文的主要目的。</p>

<p>下面奉上原文翻译。（其实文字部分也就几句话）</p>

<hr>

<p>Original Article: <a href="http://matt.might.net/articles/i-love-you-in-racket/">99 ways to say &#39;(I love you)</a> by <a href="https://twitter.com/mattmight">Matthew Might</a><br>
(Chinese Translation by <strong>Mort Yao</strong>)</p>

<h1>99 ways to say <code>&#39;(I love you)</code></h1>

<p>虽说其具有本质上的简单性，有关list的那些事儿仍然会常常让Racket程序员们困惑不已。</p>

<p>除开list结构本身之外，Racket程序中各种运算的表达方式更需要花上不少时间来发掘和掌握。</p>

<p>为了更好地强调这些重点，我写了99个不超过一条推长度的Racket表达式，它们求值的结果均得到同样的一个list：<code>&#39;(I love you)</code>。</p>

<p>这些表达式中大约有一半用来展示构建或操作list的方法，剩下来的则展示了Racket中表述计算的各色方式。</p>

<p>所涉及到的关键知识点包括：</p>

<ul>
<li>用cons连接单元（cons cells）</li>
<li>字面list记号（literal list notation）</li>
<li>加点list记号（dotted list notation）</li>
<li>反引号表示list（quasiquoted lists）</li>
<li>let绑定形式（let-binding forms）</li>
<li>vector和stream（vectors and streams）</li>
<li>匿名函数（anonymous functions）</li>
<li>高阶list操作（higher-order list operations）</li>
<li>条件式（conditionals）</li>
<li>结构化模式匹配（structural pattern-matching）</li>
<li>哈希表（hash maps）</li>
<li><a href="http://en.wikipedia.org/wiki/Continuation">续延（continuations）</a></li>
<li>异常（exceptions）</li>
<li><a href="http://en.wikipedia.org/wiki/Futures_and_promises">诺言（promises）</a></li>
<li>变更（mutation）</li>
<li>宏（macros）</li>
<li>端口（ports）</li>
<li><a href="http://en.wikipedia.org/wiki/Futures_and_promises">未来（futures）</a></li>
<li>参数（parameters）</li>
</ul>

<p>以下是具体的99种方式。</p>

<p>如果读到任何一条不能理解的话，不妨在DrRacket的REPL里面尝试摸索一番，直到你彻底弄懂它。</p>

<h2>99种方式</h2>

<p>下面的每一枚Racket表达式，求值的结果均得到一个list <code>&#39;(I love you)</code>：</p>

<p><em>（原文里面没有给这99条归类，小标题是翻译君擅自加的，看的时候请注意鉴别……）</em></p>

<h3>1. 构造list的基本手段：<code>quote</code>和<code>list</code></h3>
<div><pre><code><span>;;;</span>
<span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>quote </span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>list </span><span>&#39;I</span> <span>&#39;love</span> <span>&#39;you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>list </span><span>(</span><span>quote </span><span>I</span><span>)</span> <span>(</span><span>quote </span><span>love</span><span>)</span> <span>(</span><span>quote </span><span>you</span><span>))</span>
</code></pre></div>
<h3>2. 用<code>cons</code>连接单元</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>cons </span><span>&#39;I</span> <span>(</span><span>cons </span><span>&#39;love</span> <span>(</span><span>cons </span><span>&#39;you</span> <span>&#39;</span><span>())))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>cons </span><span>&#39;I</span> <span>(</span><span>cons </span><span>&#39;love</span> <span>(</span><span>list </span><span>&#39;you</span><span>)))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>cons </span><span>&#39;I</span> <span>(</span><span>list </span><span>&#39;love</span> <span>&#39;you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>cons </span><span>&#39;I</span> <span>&#39;</span><span>(</span><span>love</span> <span>you</span><span>))</span>
</code></pre></div>
<h3>3. 加点list记号（Dotted list notation）</h3>
<div><pre><code><span>;;;</span>
<span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>.</span> <span>(</span><span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>&#39;</span><span>(</span><span>I</span> <span>.</span> <span>(</span><span>love</span> <span>.</span> <span>(</span><span>you</span><span>)))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>&#39;</span><span>(</span><span>I</span> <span>.</span> <span>(</span><span>love</span> <span>.</span> <span>(</span><span>you</span> <span>.</span> <span>())))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>&#39;</span><span>(</span><span>I</span> <span>.</span> <span>(</span><span>love</span> <span>you</span><span>))</span>
</code></pre></div>
<h3>4. 反引号表示list（Quasiquoted lists）</h3>
<div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>,</span><span>&#39;love</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;</span>
<span>(</span><span>quasiquote </span><span>(</span><span>I</span> <span>,</span><span>&#39;love</span>  <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;</span>
<span>(</span><span>quasiquote </span><span>(</span><span>I</span> <span>(</span><span>unquote </span><span>&#39;love</span><span>)</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>,`</span><span>love</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>verb</span> <span>&#39;love</span><span>])</span>
  <span>`</span><span>(</span><span>I</span> <span>,</span><span>verb</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>,</span><span>(</span><span>string-&gt;symbol </span><span>&quot;love&quot;</span><span>)</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>,</span><span>(</span><span>string-&gt;symbol </span><span>(</span><span>list-&gt;string </span><span>&#39;</span><span>(</span><span>#\l</span> <span>#\o</span> <span>#\v</span> <span>#\e</span><span>)))</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>love</span> <span>.</span> <span>(</span><span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>love</span> <span>.</span> <span>,</span><span>(</span><span>list </span><span>&#39;you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>love</span> <span>,@&#39;</span><span>(</span><span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>love</span> <span>(</span><span>unquote-splicing </span><span>&#39;</span><span>(</span><span>you</span><span>)))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>,@</span><span>(</span><span>list </span><span>&#39;love</span> <span>&#39;you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>,@</span><span>(</span><span>list </span><span>&#39;I</span> <span>&#39;love</span><span>)</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>,@&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`,&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>love</span> <span>you</span> <span>.</span> <span>,&#39;</span><span>())</span>
</code></pre></div>
<h3>5. <code>car</code>和<code>cdr</code></h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>car </span><span>(</span><span>list </span><span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>cdr </span><span>&#39;</span><span>(</span><span>Hark!</span> <span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div>
<h3>6. <code>let</code>绑定形式（<code>let</code>-binding forms）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>words</span> <span>&#39;</span><span>(</span><span>love</span> <span>you</span> <span>I</span><span>)])</span>
  <span>(</span><span>list </span><span>(</span><span>car </span><span>(</span><span>cdr </span><span>(</span><span>cdr </span><span>words</span><span>)))</span>
        <span>(</span><span>car </span><span>words</span><span>)</span>
        <span>(</span><span>car </span><span>(</span><span>cdr </span><span>words</span><span>))))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>words</span> <span>&#39;</span><span>(</span><span>love</span> <span>you</span> <span>I</span><span>)])</span>
  <span>(</span><span>list </span><span>(</span><span>caddr </span><span>words</span><span>)</span>
        <span>(</span><span>car </span><span>words</span><span>)</span>
        <span>(</span><span>cadr </span><span>words</span><span>)))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>let* </span><span>([</span><span>c</span> <span>&#39;</span><span>(</span><span>you</span><span>)]</span>
       <span>[</span><span>b</span> <span>(</span><span>cons </span><span>&#39;love</span> <span>c</span><span>)]</span>
       <span>[</span><span>a</span> <span>(</span><span>cons </span><span>&#39;I</span> <span>b</span><span>)])</span>
  <span>a</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>()</span>
  <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span> <span>not</span><span>)</span>
  <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div>
<h3>7. vector</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>vector-&gt;list </span><span>(</span><span>vector </span><span>&#39;I</span> <span>&#39;love</span> <span>&#39;you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>vector-&gt;list </span><span>#</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div>
<h3>8. stream</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>stream-&gt;list</span> <span>(</span><span>stream</span> <span>&#39;I</span> <span>&#39;love</span> <span>&#39;you</span><span>))</span>
</code></pre></div>
<h3>9. 匿名函数（Anonymous functions）</h3>
<div><pre><code><span>;;;</span>
<span>((</span><span>lambda </span><span>args</span> <span>args</span><span>)</span> <span>&#39;I</span> <span>&#39;love</span> <span>&#39;you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>((</span><span>lambda </span><span>(</span><span>one</span> <span>two</span> <span>.</span> <span>rest</span><span>)</span> <span>rest</span><span>)</span> <span>&#39;You</span> <span>&#39;believe</span> <span>&#39;I</span> <span>&#39;love</span> <span>&#39;you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>((</span><span>lambda </span><span>(</span><span>a</span> <span>c</span> <span>b</span><span>)</span> <span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>))</span> <span>&#39;I</span> <span>&#39;you</span> <span>&#39;love</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>apply </span><span>(</span><span>lambda </span><span>(</span><span>a</span> <span>c</span> <span>b</span><span>)</span> <span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>))</span> 
       <span>(</span><span>list </span><span>&#39;I</span> <span>&#39;you</span> <span>&#39;love</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>((</span><span>lambda </span><span>(</span><span>a</span> <span>b</span> <span>[</span><span>c</span> <span>&#39;you</span><span>])</span> <span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>))</span> <span>&#39;I</span> <span>&#39;love</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>((</span><span>lambda </span><span>(</span><span>#:foo</span> <span>b</span> <span>#:bar</span> <span>c</span> <span>#:baz</span> <span>a</span><span>)</span>
   <span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>))</span>
 <span>#:baz</span> <span>&#39;I</span> <span>#:bar</span> <span>&#39;you</span> <span>#:foo</span> <span>&#39;love</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>((</span><span>lambda </span><span>(</span><span>a</span> <span>b</span> <span>#:key</span> <span>[</span><span>c</span> <span>&#39;me</span><span>])</span> <span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>))</span> <span>#:key</span> <span>&#39;you</span> <span>&#39;I</span> <span>&#39;love</span><span>)</span>
</code></pre></div>
<h3>10. 柯里化（Currying）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>f</span> <span>(</span><span>λ</span> <span>(</span><span>x</span><span>)</span>
           <span>(</span><span>λ</span> <span>(</span><span>y</span><span>)</span>
             <span>(</span><span>λ</span> <span>(</span><span>z</span><span>)</span>
               <span>(</span><span>list </span><span>x</span> <span>y</span> <span>z</span><span>))))])</span>
  <span>(((</span><span>f</span> <span>&#39;I</span><span>)</span> <span>&#39;love</span><span>)</span> <span>&#39;you</span><span>))</span>
</code></pre></div>
<h3>11. <code>case-lambda</code></h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>f</span> <span>(</span><span>case-lambda </span>
           <span>[()</span> <span>&#39;I</span><span>]</span>
           <span>[(</span><span>x</span><span>)</span> <span>&#39;love</span><span>]</span>
           <span>[(</span><span>x</span> <span>y</span><span>)</span> <span>&#39;you</span><span>])])</span>
  <span>(</span><span>list </span><span>(</span><span>f</span><span>)</span> <span>(</span><span>f</span> <span>1</span><span>)</span> <span>(</span><span>f</span> <span>1</span> <span>2</span><span>)))</span>
</code></pre></div>
<h3>12. 基础list操作</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>append </span><span>&#39;</span><span>(</span><span>I</span> <span>love</span><span>)</span> <span>&#39;</span><span>(</span><span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>append </span><span>&#39;</span><span>(</span><span>I</span><span>)</span> <span>&#39;</span><span>(</span><span>love</span><span>)</span> <span>&#39;</span><span>(</span><span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>flatten</span> <span>&#39;</span><span>((</span><span>I</span><span>)</span> <span>(</span><span>love</span> <span>you</span><span>)))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>flatten</span> <span>&#39;</span><span>((</span><span>I</span><span>)</span> <span>(</span><span>love</span><span>)</span> <span>(</span><span>you</span><span>)</span> <span>()))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>reverse </span><span>&#39;</span><span>(</span><span>you</span> <span>love</span> <span>I</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>remove</span> <span>&#39;cannot</span> <span>&#39;</span><span>(</span><span>I</span> <span>cannot</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>remove-duplicates</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>love</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>take</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span> <span>not</span><span>)</span> <span>3</span><span>)</span>
</code></pre></div><div><pre><code><span>;;</span>
<span>(</span><span>take-right</span> <span>&#39;</span><span>(</span><span>I</span> <span>think</span> <span>I</span> <span>love</span> <span>you</span><span>)</span> <span>3</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>drop</span> <span>&#39;</span><span>(</span><span>She</span> <span>knows</span> <span>I</span> <span>love</span> <span>you</span><span>)</span> <span>2</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>drop-right</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span> <span>no</span> <span>more</span><span>)</span> <span>2</span><span>)</span>
</code></pre></div>
<h3>13. 高阶list操作（map、filter和reduce）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>map </span><span>(</span><span>lambda </span><span>(</span><span>x</span><span>)</span> <span>(</span><span>if </span><span>(</span><span>eq? </span><span>x</span> <span>&#39;hate</span><span>)</span> <span>&#39;love</span> <span>x</span><span>))</span>
     <span>&#39;</span><span>(</span><span>I</span> <span>hate</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>map </span><span>(</span><span>λ</span> <span>(</span><span>i</span><span>)</span> <span>(</span><span>vector-ref </span><span>#</span><span>(</span><span>love</span> <span>you</span> <span>I</span><span>)</span> <span>i</span><span>))</span>
     <span>&#39;</span><span>(</span><span>2</span> <span>0</span> <span>1</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>map </span><span>(</span><span>λ</span> <span>(</span><span>k</span><span>)</span> <span>(</span><span>hash-ref</span> <span>#</span><span>hash</span><span>((</span><span>&quot;foo&quot;</span> <span>.</span> <span>I</span><span>)</span> 
                            <span>(</span><span>&quot;baz&quot;</span> <span>.</span> <span>you</span><span>)</span>
                            <span>(</span><span>&quot;bar&quot;</span> <span>.</span> <span>love</span><span>))</span> <span>k</span><span>))</span>
     <span>&#39;</span><span>(</span><span>&quot;foo&quot;</span> <span>&quot;bar&quot;</span> <span>&quot;baz&quot;</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>map </span><span>string-&gt;symbol</span> <span>(</span><span>sort</span> <span>(</span><span>list </span><span>&quot;love&quot;</span> <span>&quot;you&quot;</span> <span>&quot;I&quot;</span><span>)</span> <span>string&lt;?</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>map </span><span>string-&gt;symbol</span> <span>(</span><span>string-split</span> <span>&quot;I-love-you&quot;</span> <span>&quot;-&quot;</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>flatten</span> <span>(</span><span>map </span><span>(</span><span>λ</span> <span>(</span><span>a</span> <span>b</span><span>)</span> <span>(</span><span>cons </span><span>a</span> <span>b</span><span>))</span>
              <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)</span>
              <span>&#39;</span><span>(()</span> <span>()</span> <span>())))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>filter</span> <span>(</span><span>lambda </span><span>(</span><span>x</span><span>)</span> <span>(</span><span>not </span><span>(</span><span>eq? </span><span>x</span> <span>&#39;cannot</span><span>)))</span> 
        <span>&#39;</span><span>(</span><span>I</span> <span>cannot</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>foldr</span> <span>cons</span> <span>&#39;</span><span>()</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>foldl</span> <span>cons</span> <span>&#39;</span><span>()</span> <span>&#39;</span><span>(</span><span>you</span> <span>love</span> <span>I</span><span>))</span>
</code></pre></div>
<h3>14. 迭代（Iterations）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>for/list</span> <span>([</span><span>word</span> <span>#</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
  <span>word</span><span>)</span>
</code></pre></div>
<h3>15. 条件式（Conditionals）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>cond</span>
  <span>[(</span><span>even? </span><span>3</span><span>)</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[(</span><span>odd? </span> <span>3</span><span>)</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>cond</span>
  <span>[(</span><span>even? </span><span>3</span><span>)</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[(</span><span>odd? </span> <span>2</span><span>)</span> <span>&#39;</span><span>(</span><span>Nor</span> <span>I</span><span>)]</span>
  <span>[</span><span>else</span>      <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>case </span><span>1</span>
  <span>[(</span><span>a</span> <span>b</span> <span>c</span><span>)</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[(</span><span>3</span> <span>2</span> <span>1</span><span>)</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div>
<h3>16. 结构化模式匹配（Structural pattern-matching）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>#t</span>
  <span>[</span><span>#f</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[</span><span>#t</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>#t</span>
  <span>[</span><span>#f</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[</span><span>_</span>  <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;you</span>
  <span>[</span><span>&#39;me</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[</span><span>x</span>   <span>`</span><span>(</span><span>I</span> <span>love</span> <span>,</span><span>x</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>foo</span> <span>bar</span><span>)</span>
  <span>[</span><span>&#39;</span><span>(</span><span>foo</span> <span>bar</span><span>)</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>I</span> <span>cannot</span> <span>lift</span> <span>you</span><span>)</span>
  <span>[(</span><span>list </span><span>&#39;I</span> <span>&#39;cannot</span> <span>_</span> <span>c</span><span>)</span> <span>`</span><span>(</span><span>I</span> <span>love</span> <span>,</span><span>c</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>2</span> <span>3</span> <span>1</span><span>)</span>
  <span>[(</span><span>list-no-order</span> <span>3</span> <span>1</span> <span>2</span><span>)</span>
   <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>love</span> <span>you</span> <span>I</span><span>)</span>
  <span>[(</span><span>list-no-order</span> <span>&#39;I</span> <span>&#39;love</span> <span>foo</span><span>)</span>
   <span>`</span><span>(</span><span>I</span> <span>love</span> <span>,</span><span>foo</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>3</span> <span>.</span> <span>4</span><span>)</span>
  <span>[(</span><span>cons </span><span>3</span> <span>4</span><span>)</span>
   <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>3</span> <span>love</span> <span>1</span><span>)</span>
  <span>[(</span><span>cons </span><span>3</span> <span>(</span><span>cons </span><span>x</span> <span>(</span><span>cons </span><span>1</span> <span>&#39;</span><span>())))</span>
   <span>`</span><span>(</span><span>I</span> <span>,</span><span>x</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>3</span> <span>love</span> <span>1</span><span>)</span>
  <span>[(</span><span>cons </span><span>3</span> <span>(</span><span>cons </span><span>x</span> <span>(</span><span>cons </span><span>1</span> <span>&#39;</span><span>())))</span>
   <span>`</span><span>(</span><span>I</span> <span>(</span><span>unquote </span><span>x</span><span>)</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>3</span>
  <span>[(</span><span>?</span> <span>symbol?</span><span>)</span> <span>&#39;</span><span>(</span><span>Not</span> <span>me</span><span>)]</span>
  <span>[(</span><span>?</span> <span>string?</span><span>)</span> <span>&#39;</span><span>(</span><span>Me</span> <span>neither</span><span>)]</span>
  <span>[(</span><span>?</span> <span>number?</span><span>)</span> <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>3</span>
  <span>[(</span><span>not </span><span>4</span><span>)</span>   <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)]</span>
  <span>[</span><span>3</span>         <span>&#39;unreachable</span><span>])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>you</span> <span>love</span> <span>I</span><span>)</span>
  <span>[</span><span>`</span><span>(</span><span>,</span><span>c</span> <span>love</span> <span>,</span><span>a</span><span>)</span>
   <span>`</span><span>(</span><span>,</span><span>a</span> <span>love</span> <span>,</span><span>c</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>We</span> <span>love</span> <span>you</span><span>)</span>
  <span>[</span><span>`</span><span>(</span><span>,</span><span>_</span> <span>.</span> <span>,</span><span>rest</span><span>)</span>
   <span>`</span><span>(</span><span>I</span> <span>.</span> <span>,</span><span>rest</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>We</span> <span>love</span> <span>you</span><span>)</span>
  <span>[</span><span>`</span><span>(</span><span>,</span><span>_</span> <span>,</span><span>rest</span> <span>...</span><span>)</span>
   <span>`</span><span>(</span><span>I</span> <span>,@</span><span>rest</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;</span><span>(</span><span>We</span> <span>love</span> <span>you</span><span>)</span>
  <span>[(</span><span>list </span><span>_</span> <span>rest</span> <span>...</span><span>)</span>
   <span>`</span><span>(</span><span>I</span> <span>,@</span><span>rest</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>#</span><span>(</span><span>1</span> <span>love</span> <span>3</span><span>)</span>
  <span>[(</span><span>vector </span><span>(</span><span>?</span> <span>number?</span><span>)</span> <span>b</span> <span>(</span><span>?</span> <span>number?</span><span>))</span>
   <span>`</span><span>(</span><span>I</span> <span>,</span><span>b</span> <span>you</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>#</span><span>hash</span><span>((</span><span>1</span> <span>.</span> <span>I</span><span>)</span> <span>(</span><span>3</span> <span>.</span> <span>you</span><span>)</span> <span>(</span><span>5</span> <span>.</span> <span>love</span><span>))</span>
  <span>[(</span><span>hash-table</span> <span>(</span><span>1</span> <span>a</span><span>)</span> <span>(</span><span>5</span> <span>b</span><span>)</span> <span>(</span><span>3</span> <span>c</span><span>))</span>
   <span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;you</span>
  <span>[(</span><span>and </span><span>x</span> <span>(</span><span>?</span> <span>symbol?</span><span>))</span> <span>`</span><span>(</span><span>I</span> <span>love</span> <span>,</span><span>x</span><span>)])</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>match</span> <span>&#39;100</span>
  <span>[(</span><span>app</span> <span>(</span><span>λ</span> <span>(</span><span>n</span><span>)</span> <span>(</span><span>- </span><span>n</span> <span>1</span><span>))</span> <span>99</span><span>)</span>
   <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>)])</span>
</code></pre></div>
<h3>17. 续延（Continuation）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>list </span><span>&#39;I</span> 
      <span>(</span><span>call/cc </span><span>(</span><span>λ</span> <span>(</span><span>cc</span><span>)</span>
                 <span>(</span><span>error </span><span>(</span><span>cc</span> <span>&#39;love</span><span>))))</span>
      <span>&#39;you</span><span>)</span>
</code></pre></div>
<h3>18. 异常（Exceptions）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>with-handlers </span><span>([</span><span>symbol?</span> <span>(</span><span>lambda </span><span>(</span><span>p</span><span>)</span>
                             <span>`</span><span>(</span><span>I</span> <span>,</span><span>p</span> <span>you</span><span>))])</span>
    <span>(</span><span>raise </span><span>&#39;love</span><span>))</span>
</code></pre></div>
<h3>19. 延迟求值（Delayed evaluation）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>problem</span> <span>(</span><span>delay </span><span>(</span><span>car </span><span>&#39;</span><span>()))])</span>
  <span>&#39;</span><span>(</span><span>I</span> <span>love</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>`</span><span>(</span><span>I</span> <span>,</span><span>(</span><span>force </span><span>(</span><span>delay </span><span>&#39;love</span><span>))</span> <span>you</span><span>)</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>letrec </span><span>([</span><span>x</span> <span>(</span><span>delay </span><span>(</span><span>list </span><span>a</span> <span>b</span> <span>c</span><span>))]</span>
         <span>[</span><span>a</span> <span>&#39;I</span><span>]</span>
         <span>[</span><span>c</span> <span>&#39;you</span><span>]</span>
         <span>[</span><span>b</span> <span>&#39;love</span><span>])</span>
  <span>(</span><span>force </span><span>x</span><span>))</span>
</code></pre></div>
<h3>20. 变更（Mutation）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>word</span> <span>&#39;know</span><span>])</span>
  <span>(</span><span>set! </span><span>word</span> <span>&#39;love</span><span>)</span>
  <span>`</span><span>(</span><span>I</span> <span>,</span><span>word</span> <span>you</span><span>))</span>
</code></pre></div><div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>word-box</span> <span>(</span><span>box </span><span>&#39;know</span><span>)])</span>
  <span>(</span><span>set-box! </span><span>word-box</span> <span>&#39;love</span><span>)</span>
  <span>`</span><span>(</span><span>I</span> <span>,</span><span>(</span><span>unbox </span><span>word-box</span><span>)</span> <span>you</span><span>))</span>
</code></pre></div>
<h3>21. 宏（Macros）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let-syntax </span><span>([</span><span>un-yoda-list</span>
              <span>(</span><span>syntax-rules </span><span>()</span>
                <span>[(</span><span>_</span> <span>c</span> <span>a</span> <span>b</span><span>)</span> <span>(</span><span>list </span><span>&#39;a</span> <span>&#39;b</span> <span>&#39;c</span><span>)])])</span>
  <span>(</span><span>un-yoda-list</span> <span>you</span> <span>I</span> <span>love</span><span>))</span>
</code></pre></div>
<h3>22. 端口（Ports）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>((</span><span>in</span> <span>(</span><span>open-input-string </span><span>&quot;I love you&quot;</span><span>)))</span>
  <span>(</span><span>cons </span><span>(</span><span>read </span><span>in</span><span>)</span>
        <span>(</span><span>cons </span><span>(</span><span>read </span><span>in</span><span>)</span>
              <span>(</span><span>cons </span><span>(</span><span>read </span><span>in</span><span>)</span> <span>&#39;</span><span>()))))</span>
</code></pre></div>
<h3>23. 未来（Futures）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>list </span><span>(</span><span>touch</span> <span>(</span><span>future</span> <span>(</span><span>λ</span> <span>()</span> <span>&#39;I</span><span>)))</span> <span>&#39;love</span> <span>&#39;you</span><span>)</span>
</code></pre></div>
<h3>24. 参数化（Parameterization）</h3>
<div><pre><code><span>;;;</span>
<span>(</span><span>let </span><span>([</span><span>a</span> <span>(</span><span>make-parameter </span><span>&quot;a&quot;</span><span>)]</span> 
      <span>[</span><span>b</span> <span>(</span><span>make-parameter </span><span>&quot;b&quot;</span><span>)]</span>
      <span>[</span><span>c</span> <span>(</span><span>make-parameter </span><span>&quot;c&quot;</span><span>)])</span>
  <span>(</span><span>parameterize </span><span>([</span><span>a</span> <span>&#39;i</span><span>]</span> <span>[</span><span>b</span> <span>&#39;love</span><span>]</span> <span>[</span><span>c</span> <span>&#39;you</span><span>])</span>
    <span>(</span><span>list </span><span>(</span><span>a</span><span>)</span> 
          <span>((</span><span>parameterize </span><span>([</span><span>b</span> <span>&#39;dislike</span><span>])</span>
             <span>(</span><span>λ</span> <span>()</span> <span>(</span><span>b</span><span>))))</span>
          <span>(</span><span>c</span><span>))))</span>
</code></pre></div>
<h2>来自其他人的贡献</h2>

<p>由<a href="http://dvanhorn.lambda-calcul.us/">David Van Horn</a>提供：</p>
<div><pre><code><span>(</span><span>define </span><span>`</span><span>λ</span> <span>(</span><span>λ</span> <span>&#39;love</span> <span>&#39;I</span> <span>&#39;you</span><span>))</span> 
<span>((</span><span>λ</span> <span>λ</span> <span>λ</span><span>)</span> 
  <span>`</span><span>(</span><span>λ</span> <span>(</span><span>⊙</span> <span>λ</span> <span>∪</span><span>)</span> <span>λ</span><span>)</span> 
   <span>`</span><span>(</span><span>λ</span> <span>(</span><span>λ</span> <span>⊙</span> <span>∪</span><span>)</span> <span>λ</span><span>)</span>
   <span>`</span><span>(</span><span>λ</span> <span>(</span><span>∪</span> <span>⊙</span> <span>λ</span><span>)</span> <span>λ</span><span>))</span>
</code></pre></div>
<p>由Bruce Bolick提供：</p>
<div><pre><code><span>(</span><span>define </span><span>(</span><span>stream-take</span> <span>s</span> <span>n</span><span>)</span>
  <span>(</span><span>if </span><span>(</span><span>= </span><span>n</span> <span>0</span><span>)</span>
      <span>&#39;</span><span>()</span>
      <span>(</span><span>cons </span><span>(</span><span>stream-first</span> <span>s</span><span>)</span>
            <span>(</span><span>stream-take</span> <span>(</span><span>stream-rest</span> <span>s</span><span>)</span> <span>(</span><span>- </span><span>n</span> <span>1</span><span>)))))</span>
<span>(</span><span>define </span><span>U</span> <span>3</span><span>)</span>
<span>(</span><span>define </span><span>I</span> <span>stream-take</span><span>)</span>
<span>(</span><span>define </span><span>♥</span> <span>(</span><span>stream</span>  <span>&#39;I</span> <span>&#39;love</span> <span>&#39;you</span><span>))</span>
<span>(</span><span>I</span> <span>♥</span> <span>U</span><span>)</span>
</code></pre></div>
<p>（原文完）</p>

<hr>

<p>（我表示诸位看官到这里可以直接无视以下内容了……看可以，请一笑了之。）</p>

<hr>

<h1>【无聊】伦家也要来一发嘛！<img src="http://static.tieba.baidu.com/tb/editor/images/qpx_n/b09.gif" alt=""></h1>

<p>因为文章里提到的99种方式都没有涉及到logic programming，我觉得很有必要写一个自己的版本。用谓词逻辑来推导出“I love you”这种设定会很有趣的不是么……</p>

<p>于是就有了下面这段蛋疼的产物（用到了<a href="http://docs.racket-lang.org/racklog/">Racklog</a>，一个仿Prolog的逻辑式编程扩展），最终的结果也是一样一样的<code>&#39;(I love you)</code>：</p>

<p><del>我要做一只合格的脑残偶像厨WOTA</del>＼(^o^)／</p>
<div><pre><code><span>#lang racket</span>
<span>(</span><span>require </span><span>racklog</span><span>)</span>

<span>(</span><span>define </span><span>%love</span>
  <span>(</span><span>%rel</span> <span>()</span>
    <span>[(</span><span>&#39;I</span> <span>&#39;AKB48</span><span>)]</span>
    <span>[(</span><span>&#39;I</span> <span>&#39;you</span><span>)]</span>
    <span>[(</span><span>&#39;you</span> <span>&#39;I</span><span>)]))</span>

<span>(</span><span>define </span><span>first-love</span>
  <span>(</span><span>%which</span> <span>(</span><span>who</span><span>)</span>
    <span>(</span><span>%and</span> <span>(</span><span>%is</span> <span>who</span> <span>&#39;AKB48</span><span>)</span>
          <span>(</span><span>%love</span> <span>&#39;I</span> <span>who</span><span>)</span>
          <span>(</span><span>%love</span> <span>who</span> <span>&#39;I</span><span>))))</span>

<span>(</span><span>define </span><span>not-that-much-but-still-love</span>
  <span>(</span><span>%which</span> <span>(</span><span>who</span><span>)</span>
    <span>(</span><span>%and</span> <span>(</span><span>%love</span> <span>&#39;I</span> <span>who</span><span>)</span>
          <span>(</span><span>%love</span> <span>who</span> <span>&#39;I</span><span>))))</span>

<span>(</span><span>define </span><span>(</span><span>♡˙︶˙♡</span><span>)</span>
  <span>`</span><span>(</span><span>I</span> <span>love,</span> <span>(</span><span>cdr </span><span>(</span><span>car </span><span>not-that-much-but-still-love</span><span>))))</span>

<span>(</span><span>♡˙︶˙♡</span><span>)</span>
</code></pre></div>
<p><img src="http://static.tieba.baidu.com/tb/editor/images/baodong/b_0024.gif" alt=""></p>