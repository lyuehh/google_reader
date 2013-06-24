---
layout: post
title:  "从 RequireJS 到 SeaJS（2）"
date:   2011-10-27 21:27:07
author: lifesinger
categories: program
---

## 从 RequireJS 到 SeaJS（2）
### by lifesinger
### at 2011-10-27 21:27:07
### original <http://lifesinger.wordpress.com/2011/10/27/comparing-requirejs-with-seajs-2/>

<p>从 requirejs.org 的首页出发，接下来是快速入门和下载，我们来看下载页面：<a href="http://requirejs.org/docs/download.html">Download</a></p>
<h2>产出物</h2>
<p>RequireJS 提供了三个文件：</p>
<ol>
<li>require.js  —  这是核心文件，提供 JavaScript 文件和模块加载功能
<li>require-jquery.js  — 打包了 jquery 最新版本的 require.js
<li>r.js — 优化工具，以及让 RequireJS 的模块可运行于 Node 和 Rhino 环境
</li></li></li></ol>
<p>SeaJS 提供的内容，目前也有三部分：</p>
<ol>
<li>sea.js — 模块加载器</li>
<li>modules — 可运行在 seajs 环境中的推荐模块，在这里下载 <a href="http://modules.seajs.com/">modules.seajs.com</a></li>
<li>spm 等优化和支持工具</li>
</ol>
<p>如果安装了 spm, <strong>可以用 spm 获取到 seajs 提供的一切</strong>：<br>
<pre>
$ spm install seajs   # 安装 sea.js
$ spm install jquery  # 安装 modules 中的模块
$ spm build a.js      # 压缩、打包等操作
</pre></p>
<p>SeaJS 的这种设计，是由其广义定位（<strong>浏览器端的 NodeJS</strong>）决定的。在这种设计下：<br>
<span></span></p>
<p>1). require-jquery.js 必要性不大，在 SeaJS 里，用户直接手工或用 combo 服务合并就好：<br>
<pre>
http://a.tbcdn.cn/libs??seajs/1.0.2/sea.js,jquery/1.6.4/jquery.js
</pre><br>
简单灵活，扩展性也好。需要打包其他类库时，只需更改 url 参数。</p>
<p>2). r.js 在 SeaJS 里也是不必要的。modules 项目中的模块，比如 backbone，目前<strong>可以直接运行于三种环境中</strong>：</p>
<p>在浏览器中直接使用：<br>
<pre>
&lt;script src=&quot;http://modules.seajs.com/backbone/0.5.3/backbone.js&quot;&gt;&lt;/script&gt;
</pre></p>
<p>通过 seajs 加载：<br>
<pre>
&lt;script src=&quot;http://modules.seajs.com/seajs/1.0.2/sea.js&quot;&gt;&lt;/scipt&gt;
&lt;script&gt;
seajs.use(&#39;backbone/0.5.3/backbone&#39;, function(BB) {
});
&lt;/script&gt;
</pre></p>
<p>在 node 中直接调用：<br>
<pre>
var BB = require('./libs/backbone/0.5.3/backbone');
</pre></p>
<p><strong>SeaJS 的方式接近 NodeJS，简洁优雅，致力于生态圈的形成。</strong></p>
<h2>插件</h2>
<p>RequireJS 提供了一系列插件：text, order, domReady, cs, i18n.</p>
<p>SeaJS 默认只支持 js 和 css 模块，通过 text、coffee 和 less 插件来扩展支持的模块类型。SeaJS 还提供了 map 插件，方便开发调试：<a href="http://lifesinger.wordpress.com/2011/07/24/online-local-debug/">在线本地调试大观</a>。对于 order 功能，推荐组合使用 LABjs 来实现。需要 domReady 时，则使用 jQuery 库。至于 i18n, 感觉放在模块加载框架里不太合适，可以做成独立的国际化模块。</p>
<p>插件实现机制上，RequireJS 采用的是钩子模式：在 require.js 源码中，主动判断并调用插件代码。<br>
<pre>
// require.js
function callPlugin(pluginName, depManager) {
  //...
}
</pre></p>
<p>SeaJS 的实现方式是，和原生 JavaScript 类似，暴露 prototype, 插件开发者通过给 prototype 添加新方法或覆盖原有方法来实现插件功能：<br>
<pre>
// plugin-xx.js
define(funtion(require, exports, module) {
  var Require = require.constructor;
  var Module = module.constructor;

  // 覆盖原有方法
  Require.prototype.resolve = ...

  // 添加新方法
  Module.prototype.extend = ...
});
</pre></p>
<p>这种方式的好处是，在 sea.js 的代码中，合理暴露 prototype 后，就不用怎么为插件考虑了。有了 prototype, 插件作者一定程度上可以“随心所欲”。</p>
<p>进一步了解，请阅读源码：<a href="https://github.com/seajs/seajs/tree/master/src/plugins">src/plugins</a></p>
<h2>小结</h2>
<p>SeaJS 的产出物受广义定位影响，和 RequireJS 的定位是不一样的。后续会进一步分析两者的差异。</p>
<p>插件的设计无优劣。RequireJS 的方式很简单，对插件作者的要求不高。SeaJS 的插件开发方式，好处是 sea.js 本身可以保持很纯粹，不足之处是需要插件作者对 SeaJS 比较熟悉，入门更高。</p>
<p>后续博文会分析两者在 API 和使用场景上的差异。</p>
<br>  <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gocomments/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/comments/lifesinger.wordpress.com/544/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/godelicious/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/delicious/lifesinger.wordpress.com/544/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gofacebook/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/facebook/lifesinger.wordpress.com/544/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gotwitter/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/twitter/lifesinger.wordpress.com/544/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gostumble/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/stumble/lifesinger.wordpress.com/544/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/godigg/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/digg/lifesinger.wordpress.com/544/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/goreddit/lifesinger.wordpress.com/544/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/reddit/lifesinger.wordpress.com/544/"></a> <img alt="" border="0" src="http://stats.wordpress.com/b.gif?host=lifesinger.wordpress.com&amp;blog=38365&amp;post=544&amp;subd=lifesinger&amp;ref=&amp;feed=1" width="1" height="1">