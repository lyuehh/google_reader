---
layout: post
title:  "处理 JavaScript 异常的一个想法"
date:   2013-03-18 11:38:44
author: 童海波
categories: program
---

## 处理 JavaScript 异常的一个想法
### by 童海波
### at 2013-03-18 11:38:44
### original <http://blog.jobbole.com/36084/?utm_source=rss&utm_medium=rss&utm_campaign=%25e5%25a4%2584%25e7%2590%2586-javascript-%25e5%25bc%2582%25e5%25b8%25b8%25e7%259a%2584%25e4%25b8%2580%25e4%25b8%25aa%25e6%2583%25b3%25e6%25b3%2595>

<p>来源：<a href="http://sofish.de/2144">Sofish</a></p>
<p>可能由于网络、<span><a href="http://blog.jobbole.com/12749/" title="浏览器">浏览器</a></span>问题、缓存等原因，可能导致线上执行 js 的时候与开发环境并不一样，会抛出异常。js 异常基本上是前端开发工程师的家常便饭。如何记录，并使用它，却很少人关注。最近在考虑一个想法，基本上涉及到两步：收集和使用。</p>
<p><strong>一、收集</strong></p>
<p>对于 error 收集这一块，还是比较方便的，因为在各浏览器中都有一个接口：<code>window.onerror</code>：</p>
<pre>window.onerror = function(errorMessage, scriptURL, lineNumber) {
  alert(errorMessage, scriptURL, lineNumber)
}</pre>
<p>甚至中提供了 Stack Trace，比如在 <code>try/catch</code> 中还提供了 <code>e.stack</code>（各个浏览器不同，可以使用 <a href="https://github.com/eriwen/javascript-stacktrace">eriwen/javascript-stacktrace</a> 这个兼容库），试一下下面这段代码：</p>
<pre>try {
  fn()
} catch(e) {
  alert(e.stack)
}</pre>
<p>所以收集这些错误还是比较方便的，这里需要注意的事，使用 <code>window.addEventListener('error', callback, isBubble)</code> 中<code>callback</code> 的第一个参数并不是 <code>event</code>，而是一个 <code>Error object</code>。这样的话，为了方便，使用 <code>window.onerror</code> 是一个不错的选择，但通过 dot 操作符监听的事件是可以重载的，并且这段监听脚本理论上是放在所有 js 最前面的，所以需要考虑其中的风险。</p>
<p><strong>二、使用</strong></p>
<p>之前在支付宝的时候，线上 js 报错会变成一个邮件，发给前端开发 team，每个人自己认领、解决。其实这是一个不错的选择，也解决了最基本的问题：立即响应，修掉。不过也存在着一个问题，如果避免同样的错误？我的初步想法是这样的：</p>
<ul>
<li>以 URL 为单元，记录同一个页面的报错：方便统一解决</li>
<li>记录错误包括：Page URL, User Agent, Script URL, Error Message 和 Line Number</li>
<li>每个错误解决后，都可以在一个地方写解决方案，看到的人可以评论、加分，最终会存档起来，作为一个知识库，并用有方便的 api 可以使用这些知识库的内容</li>
<li>在开发的时候，相同页面 <code>window.onerror</code> 的时候，通过插件，分析 Error Message 识别出类型，加上 URL 的判断，给予开发者提醒前人犯过的错误</li>
<li>开发者可以订阅知识库上某些标签，自动接收邮件（当然也可以根据文件注释、mapping 等方式做更好的配对）</li>
</ul>
<p>为什么这样做？主要是为了解决下面的一些问题：</p>
<ul>
<li>形成知识库，开发者可以从中得到学习，特别是新人</li>
<li>工具保证效率的提升和避免重复错误重复解决</li>
<li>订阅保证通知更具针对性</li>
</ul>
<p><strong>三、注意点</strong></p>
<p><strong>1. 收集的时候使用 POST 发送</strong></p>
<p>有时候 Error Message 可能会比较长，而浏览器的 URL 长度是有限制的，如果存的错误不多的话，可以考虑用 GET 发送，但通常来说 POST 可以把所有数据都发送到后台。</p>
<p><strong>2. 何时发送数据</strong></p>
<p>建议在触发 <code>onerror</code> 的时候发送。在第一次有这个想法的时候，尝试着在 <code>onbeforeonload</code> 的时候发送，但 POST 请求还没 open 就已经被浏览器中断了。</p>
<p><strong>3. 存于数据库以哪个作为索引比较好？</strong></p>
<p>通常来说以 URL 可能会比较适合多数网站。但像百姓网、淘宝等 UGC 比较多的网站，可能需要变通一下以记录 URL。毕竟不同帖子不同 URL 都是同一套代码。</p>
<p>那以 Error 作为索引呢？其实无论是那种，看自己需求选择吧。</p>
<p><strong>4. 是否记录所有错误</strong></p>
<p>这个也比较合适根据需求来看。百姓网有各种乱七八糟的报错可能都是来自到 baidu / Google 的 ad 外链。</p>
<p><strong>四、结语</strong></p>
<p>目前初步实现了一个收集的工具（<a href="https://github.com/sofish/stacktrace.js">sofish/stacktrace.js</a>）和<a href="http://sofish.de/2144#hash_store">存储方式</a>（以 URL 为索引），是否继续，还需要时间和进一步考虑，先发出来，抛砖引玉。</p>
<p><strong>五、附录</strong></p>
<pre>&lt;?php
 
$url = new Url();
$page = $url-&gt;post(&#39;page&#39;);
 
if(!$page) return;
 
class ErrorTrace extends MongoData {
 
    // MongoData 中没有，区别 http://stackoverflow.com/a/7958954
    public function findOne($obj) {
        return $this-&gt;connection-&gt;findOne($obj);
    }
};
 
$store = new ErrorTrace();
$fields = array(
  &#39;url&#39; =&gt; $url-&gt;post(&#39;url&#39;),
	&#39;message&#39; =&gt; $url-&gt;post(&#39;message&#39;),
	&#39;line&#39; =&gt; $url-&gt;post(&#39;line&#39;),
	&#39;ua&#39; =&gt; $url-&gt;post(&#39;ua&#39;),
);
 
$index = array(&#39;page&#39; =&gt; $page);
$hash = md5(json_encode($fields));
 
// 不重复记录一个错误，所有错误记录在同一个 URL 下
if($field = $store-&gt;findOne($index)) {
    if(isset($field[$hash])) return;
    return $store-&gt;setAttr(new Query(&#39;page&#39;, $page), $hash, $fields);
}
 
$store-&gt;page = $page;
$store-&gt;$hash = $fields;
$store-&gt;save();
 
?&gt;</pre>
<h4>相关文章</h4>
<ul>
<li><a href="http://blog.jobbole.com/18291/"><img src="http://blog.jobbole.com/wp-content/uploads/2011/11/Java-programming-language-logo.jpg" alt="异常处理的最佳实践"></a><a href="http://blog.jobbole.com/18291/">异常处理的最佳实践</a></li>
<li><a href="http://blog.jobbole.com/844/"><img src="http://blog.jobbole.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/14.jpg" alt="异常的代价"></a><a href="http://blog.jobbole.com/844/">异常的代价</a></li>
<li><a href="http://blog.jobbole.com/30230/"><img src="http://blog.jobbole.com/wp-content/uploads/2013/02/java-black-1-150x150.jpg" alt="Java异常处理的陋习展播"></a><a href="http://blog.jobbole.com/30230/">Java异常处理的陋习展播</a></li>
<li><a href="http://blog.jobbole.com/18513/"><img width="150" height="150" src="http://blog.jobbole.com/wp-content/uploads/2012/04/snippets-150x150.jpg" alt="50个jQuery代码段帮你成为更出色的JS开发者"></a><a href="http://blog.jobbole.com/18513/">50个jQuery代码段帮你成为更出色的JS开发者</a></li>
<li><a href="http://blog.jobbole.com/12042/"><img src="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" alt="JavaScript需要Blocks"></a><a href="http://blog.jobbole.com/12042/">JavaScript需要Blocks</a></li>
<li><a href="http://blog.jobbole.com/36327/"><img src="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" alt="我眼中的技术高手"></a><a href="http://blog.jobbole.com/36327/">我眼中的技术高手</a></li>
<li><a href="http://blog.jobbole.com/24602/"><img width="150" height="150" src="http://blog.jobbole.com/wp-content/uploads/2012/07/Explaining-What-JavaScript-is-to-Non-programmers-150x150.jpg" alt="向非程序员解释 JavaScript"></a><a href="http://blog.jobbole.com/24602/">向非程序员解释 JavaScript</a></li>
<li><a href="http://blog.jobbole.com/31213/"><img src="http://blog.jobbole.com/wp-content/uploads/2013/02/687474703a2f2f617564756e6f2e6769746875622e636f6d2f68656164747261636b722f6578616d706c65732f6d656469612f66616365747261636b696e675f7468756d626e61696c2e706e67-150x134." alt="headtrackr：一个头部/脸部追踪的JavaScript库"></a><a href="http://blog.jobbole.com/31213/">headtrackr：一个头部/脸部追踪的JavaScript库</a></li>
<li><a href="http://blog.jobbole.com/1313/"><img src="http://blog.jobbole.com/wp-content/plugins/wordpress-23-related-posts-plugin/static/thumbs/16.jpg" alt="14个Google地图的JavaScript资源"></a><a href="http://blog.jobbole.com/1313/">14个Google地图的JavaScript资源</a></li>
<li><a href="http://blog.jobbole.com/1254/"><img src="http://blog.jobbole.com/wp-content/uploads/2011/11/node.js-logo.png" alt="Node.js 究竟是什么？"></a><a href="http://blog.jobbole.com/1254/">Node.js 究竟是什么？</a></li>
</ul>
<p><a href="http://blog.jobbole.com/36084/">处理 JavaScript 异常的一个想法</a>，首发于<a href="http://blog.jobbole.com">博客 - 伯乐在线</a>。</p>