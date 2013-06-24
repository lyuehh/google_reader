---
layout: post
title:  "从 RequireJS 到 SeaJS（1）"
date:   2011-10-24 14:41:20
author: lifesinger
categories: program
---

## 从 RequireJS 到 SeaJS（1）
### by lifesinger
### at 2011-10-24 14:41:20
### original <http://lifesinger.wordpress.com/2011/10/24/comparing-requirejs-with-seajs-1>

<p>RequireJS 发布 1.0.0 了，很值得关注，看是否有可借鉴之处。<br>
本次比较不涉及具体实现代码，主要比较两者的 API 设计。</p>
<p><a href="http://requirejs.org">http://requirejs.org</a><br>
<a href="http://seajs.org">http://seajs.org</a></p>
<h2>从定位谈起</h2>
<p>首先，从 requirejs.org 首页可以得知，RequireJS 的定位是：</p>
<p><strong>RequireJS 是一个 JavaScript 文件和模块加载器，特别为浏览器优化，同时也可运行在 Rhino 和 Node 环境中。</strong></p>
<p>SeaJS 的定位是：</p>
<p><strong>SeaJS 是一个适用于浏览器端的 JavaScript 模块加载器。</strong></p>
<p>不要小看这两句话，任何类库/框架的定位，或者说愿景/目标，最终会决定该类库/框架的方方面面。比如：</p>
<p>RequireJS 的定位中，除了是模块加载器，还是文件加载器，这决定了 RequireJS 需要实现类似 LABjs 等 script loader 的功能。LABjs 的核心功能是异步加载脚本并提供运行的依赖等待：</p>
<p><pre>
$LAB
   .script(&quot;a.js&quot;)
   .script(&quot;b.js&quot;).wait()
   .script(&quot;c.js&quot;);
</pre></p>
<p>为了提供类似功能，RequireJS 开发了 order 插件：<br>
<span></span><br>
<pre>
require([&quot;order!one.js&quot;, &quot;order!two.js&quot;, &quot;order!three.js&quot;], function () {
    //This callback is called after the three scripts finish loading.
});
</pre></p>
<p>order 功能是 JavaScript 文件加载器必备的功能，RequireJS 有文件加载器的定位，因此才有了 order 插件的设计。</p>
<p>而对于 SeaJS 来说，定位为很纯粹的模块加载器，因此不需要考虑普通脚本的 order 加载，如果需要脚本加载功能，直接和 LABjs 一块用就好：</p>
<p><pre>
define(function(require) {
  var $LAB = require('labjs/2.0.3/lab.js');

   $LAB
      .script('a.js')
      .script('b.js').wait()
      .scrpt('c.js');
);
</pre></p>
<p>SeaJS 推荐用组合的思路解决问题：LABjs + SeaJS = JavaScript 文件和模块加载器。</p>
<p>上面是 SeaJS 的狭义定位，SeaJS 还有一个广义定位：</p>
<p><strong>SeaJS 是浏览器端的 NodeJS.</strong></p>
<p>在广义定位下，SeaJS 包括以下部分：</p>
<ol>
<li>一套模块书写格式。
<li>一个适用于浏览器端的模块加载器。
<li>一组默认推荐的模块。
<li>类似 npm 的包管理工具。
<li>调试等开发者工具。
<li>等等。
</li></li></li></li></li></li></ol>
<p>狭义定位对应 github 中的：<a href="https://github.com/seajs/seajs">seajs/seajs</a><br>
广义定位对应 github 中的：<a href="https://github.com/seajs">seajs</a></p>
<p>RequireJS 和 SeaJS 狭义定位上的差异，将直接导致两者之间的最佳使用场景和 API  设计差异，后续博文会进一步提及。</p>
<p>SeaJS 广义定位中的定语很重要，是“浏览器端的”，这将直接决定 SeaJS 和 NodeJS 的差异，后续博文会进一步提及。</p>
<h2>兼容性</h2>
<p>requirejs.org 首页还列举了 RequireJS 支持的浏览器，这和 SeaJS 没什么差异。其实两者涉及到的 DOM 操作都不多，大部分代码是纯 JavaScript 代码，兼容性上都很不错。</p>
<h2>参考文章</h2>
<ul>
<li><a href="http://blog.getify.com/2010/12/on-script-loaders/">On Script Loaders
</a></li></ul>
<div></div>