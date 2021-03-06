---
layout: post
title:  "从 RequireJS 到 SeaJS（3）"
date:   2011-10-30 16:11:22
author: lifesinger
categories: program
---

## 从 RequireJS 到 SeaJS（3）
### by lifesinger
### at 2011-10-30 16:11:22
### original <http://lifesinger.wordpress.com/2011/10/30/compareing-requirejs-wth-seajs-3/>

<p>来看 RequireJS 的 API 页面：<a href="http://requirejs.org/docs/api.html">api.html</a>, 该篇博客着重分析 RequireJS 和 SeaJS 在 ID 规则上的异同。</p>
<h2>RequireJS 的 ID 规则</h2>
<p>对于模块加载器来说，模块 id 遵守的规则非常重要。</p>
<p>首先来定义几个概念。假设当前页面路径为 <code>http://t.com/a/b/c.html</code>, 则：</p>
<ol>
<li><strong>pageRoot</strong> = <code>http://t.com</code></li>
<li><strong>pageUrl</strong> = <code>http://t.com/a/b</code></li>
<li><strong>baseUrl</strong>: 在 RequireJS 里，默认情况下，baseUrl 就是 pageUrl, 除非：
<ul>
<li>通过 require.config 更改了 baseUrl</li>
<li>指定了 data-main, 比如
<pre>
&lt;script src=&quot;require.js&quot; data-main=&quot;scripts/main&quot;&gt;&lt;/script&gt;
</pre>
<p>这时 baseUrl 为 <code>pageUrl/scripts</code></p></li>
</ul>
<li><strong>paths</strong>: 表示路径的缩写，通过 config 来配置：
<pre>
require.config({ paths: { 'some': 'xxx/zzz' } })
</pre>
<p>这时，<code>some/a</code> 实际上代表 <code>xxx/zzz/a</code></p></li>
<li><strong>moduleUrl</strong>: 如果模块路径为 <code>http://t.com/path/to/a.js</code>, 则 moduleUrl = <code>http://t.com/path/to</code></li>
</li></ol>
<p>RequireJS 的 id 规则为：首先，会通过 paths 解析，解析完成后：</p>
<ol>
<li>是 <strong>Page ID</strong>：
<ul>
<li>以 <code>.js</code> 结尾的，比如 <code>some/a/b.js</code></li>
<li>以 <code>/</code> 开头的，比如 <code>/a/b</code></li>
<li>以 <code>http(s)://</code> 开头的</li>
</ul>
<p>Page ID 的解析规则与 <code>script src="xxx"</code> 中的 xxx 一样。</p></li>
<li>是 <code>some/module</code> 这种 <strong>Top-Level ID</strong>, 会解析成 <code>baseUrl/some/module.js</code></li>
<li>是 <code>./some/module</code> 这种 <strong>Relative ID</strong>, 会解析成 <code>moduleUrl/some/module.js</code></li>
</ol>
<h2>SeaJS 的 ID 规则</h2>
<p>文档：<a href="http://seajs.com/docs/module-identifier.html">module-identifier.html</a></p>
<p>SeaJS 的 ID 规则可以总结为：</p>
<ol>
<li><strong>省略约定</strong>：<code>.js 后缀可以省略。 <code>require('path/to/a.js')</code> 和 <code>require('path/to/a')</code> 是一样的。</code></li>
<li><strong>环境相关</strong>: 除了 Top-Level ID 始终相对 baseUrl 来定位，其他 ID 都是相对当前环境来定位。</li>
</ol>
<p>举例如下：</p>
<pre>
// 在 http://t.com/test.html 中：
seajs.use(['./a', 'b', 'c.js', '/d', 'http://x.com/e']);

// 会下载
// http://t.com/a.js
// baseUrl/b.js
// baseUrl/c.js
// http://t.com/d.js
// http://x.com/e.js

// 在 http://cdn.com/path/to/t.js 中：
define(function(require) {
  require('./a');
  require('b');
  require('c.js');
  require('/d');
  require('http://x.com/e');
});

// 会下载
// http://cdn.com/path/to/a.js
// baseUrl/b.js
// baseUrl/c.js
// http://cdn.com/d.js
// http://x.com/e.js
</pre>
<p> </p>
<p>可以看出，在 SeaJS 里，ID 可以分成两类：</p>
<ol>
<li><strong>Top-Level ID</strong>: <code>some/module</code> 这种，会根据 baseUrl 来定位。</li>
<li><strong>Context ID</strong>: 除了 <code>some/module</code> 之外的所有形式，会根据当前环境来定位。</li>
</ol>
<h2>ID 规则对使用体验的影响</h2>
<p>RequireJS 里有 Page ID 的概念，和浏览器解析 src 的规则保持一致。<br>
SeaJS 里则有省缺 .js 后缀的约定，和 CommonJS 以及 NodeJS 的约定保持一致。</p>
<p>这个取舍，使得 RequireJS 的 ID 更接近文件路径，SeaJS 的 ID 则更接近模块标识。<br>
这其实是由两者的定位决定的：RequireJS 想同时是文件和模块加载器，SeaJS 则只专注于模块加载器。</p>
<p>当采用 Simplified CommonJS Wrapper 格式时，RequireJS 和 SeaJS 的解析规则基本一致：<br>
<pre>
// xxx/to/a.js
define(function(require, exports, module) {
  require('a'); // baseUrl/a.js
  require('./b'); // xxx/to/b.js
  require('./b.js'); // xxx/to/b.js
});
</pre></p>
<p>RequireJS 的 ID 规则是比较容易让人混淆的，特别对于新手来说。有没有 .js 后缀，以及采用的模块书写格式，都会影响解析规则。</p>
<p><strong>SeaJS 的 ID 规则更简单纯粹，和 CommonJS 以及 NodeJS 尽量保持一致。</strong></p>
<br>  <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gocomments/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/comments/lifesinger.wordpress.com/554/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/godelicious/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/delicious/lifesinger.wordpress.com/554/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gofacebook/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/facebook/lifesinger.wordpress.com/554/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gotwitter/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/twitter/lifesinger.wordpress.com/554/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gostumble/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/stumble/lifesinger.wordpress.com/554/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/godigg/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/digg/lifesinger.wordpress.com/554/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/goreddit/lifesinger.wordpress.com/554/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/reddit/lifesinger.wordpress.com/554/"></a> <img alt="" border="0" src="http://stats.wordpress.com/b.gif?host=lifesinger.wordpress.com&amp;blog=38365&amp;post=554&amp;subd=lifesinger&amp;ref=&amp;feed=1" width="1" height="1">