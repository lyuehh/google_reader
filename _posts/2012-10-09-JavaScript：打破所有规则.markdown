---
layout: post
title:  "JavaScript：打破所有规则"
date:   2012-10-09 12:47:29
author: 
categories: program
---

## JavaScript：打破所有规则
### by 
### at 2012-10-09 12:47:29
### original <http://blog.jobbole.com/29061/?utm_source=rss&utm_medium=rss&utm_campaign=javascript%25ef%25bc%259a%25e6%2589%2593%25e7%25a0%25b4%25e6%2589%2580%25e6%259c%2589%25e8%25a7%2584%25e5%2588%2599>

<p>来源：<a href="http://www.cnblogs.com/ziyunfei/archive/2012/10/07/2713505.html">紫云飞的博客</a>，@紫云妃</p>
<p>来自Twitter的前端工程师<a href="http://javascriptweblog.wordpress.com/">Angus Croll</a>，在柏林举办的JSConf会议上，进行了题为”<a href="http://2012.jsconf.eu/speaker/2012/08/27/break-all-the-rules.html">Break all the Rulez</a>“的演讲。主要讲了一些我们通常认为是错误的不该使用的东西，其实是有用的。Angus Croll 演讲用的讲稿（<a href="https://speakerdeck.com/u/anguscroll/p/keynoteberlin">链接</a>），远在美国的JavaScript之父看后也说同意其中大部分观点（看来还是有问题？）。</p>
<p>下面我把要点简单翻译一下,不做扩展解释.</p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2012/10/2012100710575370.png" rel="lightbox[29061]" title="JavaScript：打破所有规则"><img title="JavaScript：打破所有规则" src="http://blog.jobbole.com/wp-content/uploads/2012/10/2012100710575370.png" alt="JavaScript：打破所有规则" width="573" height="113"></a></p>
<p><strong>with语句</strong></p>
<p><strong>为什么不去使用它?</strong></p>
<p>1.意外的运行结果,可能隐式创建全局变量</p>
<p>2.闭包作用域解析过多消耗</p>
<p>3.后期编译</p>
<p>有人说,ES5的严格模式可以防止隐式创建全局变量(不用var),这样能减少with的一个问题.但是…</p>
<p>严格模式也不能使用with啊.</p>
<p><strong>为什么说它是有用的?</strong></p>
<p>1.构建<span><a href="http://blog.jobbole.com/12749/" title="浏览器">浏览器</a></span>开发者工具</p>
<div>
<pre>//Chrome Developer Tools
IS._evaluateOn =
  function(evalFunction, obj, expression) {
    IS._ensureCommandLineAPIInstalled();
    expression =
      &quot;with (window._inspectorCommandLineAPI) {\
        with (window) { &quot; + expression + &quot; } }&quot;;
    return evalFunction.call(obj, expression);
}</pre>
</div>
<p>2.模拟块级作用域</p>
<div>
<pre>//是的,还是这个老掉牙的问题
var addHandlers = function(nodes) {
  for (var i = 0; i &lt; nodes.length; i++) {
    nodes[i].onclick =
      function(e) {alert(i);}
  }
};</pre>
</div>
<div>
<pre>//你可以通过在外面包一个函数来解决
var addHandlers = function(nodes) {
  for (var i = 0; i &lt; nodes.length; i++) {
    nodes[i].onclick = function(i) {
      return function(e) {alert(i);};
    }(i);
  }
};</pre>
</div>
<div>
<pre>//或者使用&#39;with&#39;来模拟块级作用域
var addHandlers = function(nodes) {
  for (var i = 0; i &lt; nodes.length; i++) {
    with ({i:i}) {
      nodes[i].onclick =
        function(e) {alert(i);}
    }
  }
};</pre>
</div>
<p><strong>eval语句</strong></p>
<p><strong>为什么不去使用它?</strong></p>
<p>1.代码注入</p>
<p>2.无法进行闭包优化</p>
<p>3.后期编译</p>
<p><strong>为什么说它是有用的?</strong></p>
<p>1. JSON.parse不可用的时候</p>
<p>有人在Stack Overflow上说:</p>
<p>“JavaScript的eval是不安全的,使用json.org上的JSON解析器来解析JSON”</p>
<p>还有人说:</p>
<p>“不要使用eval来解析JSON!用道格拉斯写的json2.js!”</p>
<p>可是:</p>
<div>
<pre>// From JSON2.js

if (/^[\],:{}\s]*$/
  .test(text.replace(/*regEx*/, &#39;@&#39;)
  .replace(/*regEx*/, &#39;]&#39;)
  .replace(/*regEx*/, &#39;&#39;))) {
  j = eval(&#39;(&#39; + text + &#39;)&#39;);
}</pre>
</div>
<p>2.浏览器的JavaScript控制台都是用eval实现的</p>
<p>在Webkit控制台或JSBin中执行下面的代码</p>
<div>
<pre>&gt;(function () {
    console.log(String(arguments.callee.caller))
})()

function eval() {
    [native code]
}</pre>
</div>
<p>John Resig说过:</p>
<p>“eval和with是被轻视的,被误用的,被大部分JavaScript<span><a href="http://blog.jobbole.com/821/" title="程序员的本质">程序员</a></span>公然谴责的,但如果能正确使用的话,可以用它们写出一些奇妙的,无法用其他功能实现的代码”</p>
<p><strong>Function构造函数</strong></p>
<p><strong>为什么说它是有用的?</strong></p>
<p>1.代码运行在可预见的作用域内</p>
<p>2.只能动态创建全局变量</p>
<p>3.不存在闭包优化的问题</p>
<p><strong>用在了什么地方?</strong></p>
<p>1. jQuery的parseJSON</p>
<div>
<pre>// jQuery parseJSON 

// Logic borrowed from http://json.org/json2.js
if (rvalidchars.test(data.replace(rvalidescape,&quot;@&quot;)
  .replace( rvalidtokens,&quot;]&quot;)
  .replace( rvalidbraces,&quot;&quot;))) {

  return ( new Function( &quot;return &quot; + data ) )();
}</pre>
</div>
<p>2.Underscore.js的字符串内插</p>
<div>
<pre>//from _.template

// If a variable is not specified,
// place data values in local scope.
if (!settings.variable) 
  source = &#39;with(obj||{}){\n&#39; + source + &#39;}\n&#39;;

//..

var render = new Function(
  settings.variable || &#39;obj&#39;, &#39;_&#39;, source);</pre>
</div>
<p><strong>==运算符</strong></p>
<p><strong>为什么不去使用它?</strong></p>
<p>1.强制将两边的操作数转换为相同类型</p>
<p><strong>为什么说它是有用的?</strong></p>
<p>1.强制将两边的操作数转换为相同类型</p>
<p>2.undefined == null</p>
<div>
<pre>//这样写是不是很麻烦
if ((x === null) || (x === undefined))

//完全可以下面这样写
if (x == null)</pre>
</div>
<p>3.当两边的操作数类型明显相同时使用</p>
<div>
<pre>typeof thing == &quot;function&quot;;   //typeof运算符肯定返回字符串
myArray.length == 2;          //length属性肯定返回数字
myString.indexOf(&#39;x&#39;) == 0;   //indeOf方法肯定返回数字</pre>
</div>
<p>真值不一定==true,假值不一定==false</p>
<div>
<pre>if (&quot;potato&quot;) {
  &quot;potato&quot; == true;  //false
}</pre>
</div>
<p><strong>Array构造函数</strong></p>
<p><strong>为什么不去使用它?</strong></p>
<p>1.new Array()也是evil的?JS<code>Lint也推荐使用[].</code></p>
<p><strong>为什么说它是有用的?</strong></p>
<div>
<pre>//From prototype.js extension of 
//String.prototype
function times(count) {
  return count &lt; 1 ?
    &#39;&#39; : new Array(count + 1).join(this);
}
&#39;me&#39;.times(10); //&quot;memememememememememe&quot;</pre>
</div>
<p><strong>其他</strong></p>
<p><strong></strong>不要扩展原生的原型对象</p>
<p>(es 5 shims都这么干)</p>
<p>在for/in遍历时总要使用hasOwnProperty</p>
<p>(在没有扩展对象原型的前提下没有必要这么做)</p>
<p>把所有的var语句放在顶部</p>
<p>(for语句还是放在初始化表达式中好)</p>
<p>要在调用函数之前先声明函数</p>
<p>(在优先考虑实现细节时使用)</p>
<p>不要使用逗号运算符</p>
<p>(在使用多个表达式时的时候可以使用)</p>
<p>使用parseInt时总要把第二个参数指定为10</p>
<p>(除非字符串以‘0’或‘x’开头,否则没必要)</p>
<p><strong>译者注</strong></p>
<p>上面说了这么多,我自己也想到一个被误解的东西,那就是escape.网上有不少人说:<a href="http://203.208.46.1/#q=%22%E4%B8%8D%E8%A6%81%E4%BD%BF%E7%94%A8escape%22%20">“不要使用escape”</a>.</p>
<p><strong>为什么说它是有用的?</strong></p>
<p>1.escape转义的字符更多,有时候需要转义后两个函数不转义的字符.</p>
<table border="1" cellpadding="4">
<tbody>
<tr>
<th style="text-align:left">ASCII char</th>
<th style="text-align:left">escape()</th>
<th style="text-align:left">encodeURI()</th>
<th style="text-align:left">encodeURIComponent()</th>
</tr>
</tbody>
<tbody>
<tr>
<td align="center">!</td>
<td align="center">%21</td>
<td align="center">!</td>
<td align="center">!</td>
</tr>
<tr>
<td align="center">#</td>
<td align="center">%23</td>
<td align="center">#</td>
<td align="center">%23</td>
</tr>
<tr>
<td align="center">$</td>
<td align="center">%24</td>
<td align="center">$</td>
<td align="center">%24</td>
</tr>
<tr>
<td align="center">&amp;</td>
<td align="center">%26</td>
<td align="center">&amp;</td>
<td align="center">%26</td>
</tr>
<tr>
<td align="center">‘</td>
<td align="center">%27</td>
<td align="center">‘</td>
<td align="center">‘</td>
</tr>
<tr>
<td align="center">(</td>
<td align="center">%28</td>
<td align="center">(</td>
<td align="center">(</td>
</tr>
<tr>
<td align="center">)</td>
<td align="center">%29</td>
<td align="center">)</td>
<td align="center">)</td>
</tr>
<tr>
<td align="center">+</td>
<td align="center">+</td>
<td align="center">+</td>
<td align="center">%2B</td>
</tr>
<tr>
<td align="center">,</td>
<td align="center">%2C</td>
<td align="center">,</td>
<td align="center">%2C</td>
</tr>
<tr>
<td align="center">/</td>
<td align="center">/</td>
<td align="center">/</td>
<td align="center">%2F</td>
</tr>
<tr>
<td align="center">:</td>
<td align="center">%3A</td>
<td align="center">:</td>
<td align="center">%3A</td>
</tr>
<tr>
<td align="center">;</td>
<td align="center">%3B</td>
<td align="center">;</td>
<td align="center">%3B</td>
</tr>
<tr>
<td align="center">=</td>
<td align="center">%3D</td>
<td align="center">=</td>
<td align="center">%3D</td>
</tr>
<tr>
<td align="center">?</td>
<td align="center">%3F</td>
<td align="center">?</td>
<td align="center">%3F</td>
</tr>
<tr>
<td align="center">@</td>
<td align="center">@</td>
<td align="center">@</td>
<td align="center">%40</td>
</tr>
<tr>
<td align="center">~</td>
<td align="center">%7E</td>
<td align="center">~</td>
<td align="center">~</td>
</tr>
</tbody>
</table>
<p>2.将字符串转换为UTF8编码,通常用在base64的时候.</p>
<p><code>escape相当于是在utf16编码的字符串上转义,<code>encodeURIComponent</code>相当于是先把utf16的字符串转换成utf8编码后再escape.</code></p>
<p><code>encodeURIComponent</code>(str) === escape(UTF16ToUTF8(str))</p>
<p>可以推导出UTF16ToUTF8(str) === <code>unescape( encodeURIComponent( str )</code></p>
<p>然后就能用在base64编码的时候,比网上看到的那些简单多了吧.注意btoa和atob有兼容问题.</p>
<pre>function base64Encode(str) {
    return btoa(unescape(encodeURIComponent(str)));
}

function base64Decode(str) {
    return decodeURIComponent(escape(atob(str)));
}</pre>
<p> </p>
<p> </p>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/28884/" title="JavaScript：运算符">JavaScript：运算符</a></li><li><a href="http://blog.jobbole.com/28840/" title="基于 HTML5 构建 Web 操作系统">基于 HTML5 构建 Web 操作系统</a></li><li><a href="http://blog.jobbole.com/26554/" title="CoffeeScript：以优美方式编写JavaScript代码">CoffeeScript：以优美方式编写JavaScript代码</a></li><li><a href="http://blog.jobbole.com/26859/" title="JavaScript: 数据类型">JavaScript: 数据类型</a></li><li><a href="http://blog.jobbole.com/26101/" title="田永强：优秀的JavaScript模块是怎样炼成的">田永强：优秀的JavaScript模块是怎样炼成的</a></li></ul>