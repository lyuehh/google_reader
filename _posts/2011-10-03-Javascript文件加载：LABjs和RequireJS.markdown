---
layout: post
title:  "Javascript文件加载：LABjs和RequireJS"
date:   2011-10-03 11:51:47
author: 
categories: program
---

## Javascript文件加载：LABjs和RequireJS
### by 
### at 2011-10-03 11:51:47
### original <http://www.ruanyifeng.com/blog/2011/10/javascript_loading.html>

<p>传统上，加载Javascript文件都是使用&lt;script&gt;标签。</p><p>就像下面这样：</p>

<blockquote>

<p>　　&lt;script type=&quot;text/javascript&quot; src=&quot;example.js&quot;&gt;&lt;/script&gt;</p>

</blockquote>

<p><img src="http://image.beekka.com/blog/201110/bg2011100301.png"></p>

<p>&lt;script&gt;标签很方便，只要加入网页，浏览器就会读取并运行。但是，它存在一些严重的缺陷。</p>

<p>　　<strong>（1）严格的读取顺序。</strong>由于浏览器按照&lt;script&gt;在网页中出现的顺序，读取Javascript文件，然后立即运行，导致在多个文件互相依赖的情况下，依赖性最小的文件必须放在最前面，依赖性最大的文件必须放在最后面，否则代码会报错。</p>

<p>　　<strong>（2）性能问题。</strong>浏览器采用&quot;同步模式&quot;加载&lt;script&gt;标签，也就是说，页面会&quot;堵塞&quot;（blocking），等待javascript文件加载完成，然后再运行后面的HTML代码。当存在多个&lt;script&gt;标签时，浏览器无法同时读取，必须读取完一个再去读取另一个，造成读取时间大大延长，页面响应缓慢。</p>

<p>为了解决这些问题，可以使用DOM方法，动态加载Javascript文件。</p>

<blockquote>

<p>　　function loadScript(url){</p>

<p>　　　　var script = document.createElement("script");</p>

<p>　　　　script.type = "text/javascript";</p>

<p>　　　　script.src = url;</p>

<p>　　　　document.body.appendChild(script);</p>

<p>　　}</p>

</blockquote>

<p>这样做的原理是，浏览器即时创造出一个&lt;script&gt;标签，然后&quot;异步&quot;读取Javascript文件。这样不会造成页面堵塞，但会造成另外一个问题：这样加载的Javascript文件，不在原始的DOM结构之中，因此在DOM-ready（DOMContentLoaded）事件和window.onload事件中指定的回调函数对它无效。</p>

<p>外部函数库<a href="http://labjs.com/">LABjs</a>和<a href="http://requirejs.org/">RequireJS</a>，可以帮助我们更有效地管理Javascript加载。 </p>

<p>下面根据<a href="http://msdn.microsoft.com/en-us/scriptjunkie/ff943568">ScriptJunkie</a>的文章，举一个最简单的例子，来说明这两个函数库的基本用法。更高级的用法，请参阅它们的文档。</p>

<blockquote>

<p>　　&lt;script src=&quot;script1.js&quot;&gt;&lt;/script&gt;</p>

<p>　　&lt;script src=&quot;script2-a.js&quot;&gt;&lt;/script&gt;</p>

<p>　　&lt;script src=&quot;script2-b.js&quot;&gt;&lt;/script&gt;</p>

<p>　　&lt;script type=&quot;text/javascript&quot;&gt;</p>

<p>　　　　initScript1();</p>

<p>　　　　initScript2();</p>

<p>　　&lt;/script&gt;</p>

<p>　　&lt;script src=&quot;script3.js&quot;&gt;&lt;/script&gt;</p>

<p>　　&lt;script type=&quot;text/javascript&quot;&gt;</p>

<p>　　　　initScript3();</p>

<p>　　&lt;/script&gt;</p>

</blockquote>

<p>上面这段代码，将依次加载4个javascript文件：script1.js、script2-a.js、script2-b.js和script3.js。在加载完前三个文件后，运行两个函数initScript1()和initScript2()；加载完第四个文件后，再运行函数initScript3()。</p>

<p>下面，用LABjs对其进行改写：</p>

<blockquote>

<p>　　&lt;script src=&quot;LAB.js&quot;&gt;&lt;/script&gt;</p>

<p>　　&lt;script type=&quot;text/javascript&quot;&gt;</p>

<p>　　　　$LAB</p>

<p>　　　　　.script("script1.js").wait()</p>

<p>　　　　　.script("script2-a.js")</p>

<p>　　　　　.script("script2-b.js")</p>

<p>　　　　　.wait(function(){</p>

<p>　　　　　　　initScript1();</p>

<p>　　　　　　　initScript2();</p>

<p>　　　　　})</p>

<p>　　　　　.script("script3.js")</p>

<p>　　　　　.wait(function(){</p>

<p>　　　　　　　initScript3();</p>

<p>　　　　　});</p>

<p>　　&lt;/script&gt;</p>

</blockquote>

<p>首先，$LAB对象替代了&lt;script&gt;标签，然后.script()方法表示加载Javascript文件，不带参数的.wait()方法表示立即运行刚才加载的Javascript文件，带参数的.wait()方法也是立即运行刚才加载的Javascript文件，但是还运行参数中指定的函数。</p>

<p>这里需要注意的是，可以同时运行多条$LAB链，但是它们之间是完全独立的，不存在次序关系。如果你要确保一个Javascript文件在另一个文件之后运行，你只能把它们写在同一个链操作之中。只有当某些脚本是完全无关的时候，你才应该考虑把它们分成不同的$LAB链，表示它们之间不存在相关关系。</p>

<p>接下来是requireJS的改写：</p>

<blockquote>

<p>　　&lt;script src=&quot;require.js&quot;&gt;&lt;/script&gt;</p>

<p>　　&lt;script type=&quot;text/javascript&quot;&gt;</p>

<p>　　　　require([</p>

<p>　　　　　　"script1.js",<br>
　　　　　　"script2-a.js",<br>
　　　　　　"script2-b.js",<br>
　　　　　　"script3.js"</p>

<p>　　　　　],</p>

<p>　　　　　function(){</p>

<p>　　　　　　initScript1();<br>
　　　　　　initScript2();<br>
　　　　　　initScript3();</p>

<p>　　　　　}</p>

<p>　　　　);</p>

<p>　　&lt;/script&gt;</p>

</blockquote>

<p>require()接受两个参数，第一个数组表示所要加载的Javascript文件，第二个是加载完成后所要运行的回调函数。原生的require()不支持按次序加载，所以四个Javascript文件到底先加载哪个，无法事前知道，require()只保证这四个文件全部加载完成之后，才会运行所指定的回调函数。</p>

<p>如果按次序加载对你很重要，你可以使用官方提供的<a href="http://requirejs.org/docs/api.html#order">order插件</a>。</p>

<p>（完）<br>
</p><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;border:1px solid #d3d3d3;margin:1em;background-color:#aad2f0;border-radius:10px"><h3>文档信息</h3>
<ul>
<li>版权声明：自由转载-非商用-非衍生-保持署名 | <a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2011/10/javascript_loading.html">http://www.ruanyifeng.com/blog/2011/10/javascript_loading.html</a></li>
<li>最后修改时间：2011年10月28日 08:56</li>
<li>付费支持（<a href="http://www.ruanyifeng.com/blog/2011/05/my_google_adsense_is_disabled.html" style="text-decoration:underline">说明</a>）：<a href="https://mai.alipay.com/p.htm?id=2011101500701706"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" alt="支付宝担保交易" style="border:none;vertical-align:middle"></a> | <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=2.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="paypal" style="border:none;vertical-align:middle"></a> </li>
</ul></div><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;margin:1em;border-radius:10px"><p></p></div>