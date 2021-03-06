---
layout: post
title:  "从 RequireJS 到 SeaJS（5）"
date:   2011-10-30 23:03:01
author: lifesinger
categories: program
---

## 从 RequireJS 到 SeaJS（5）
### by lifesinger
### at 2011-10-30 23:03:01
### original <http://lifesinger.wordpress.com/2011/10/30/comparing-requirejs-with-seajs-5/>

<p>这是该系列的最后一篇。</p>
<p>作为模块加载器，最核心的是：模块书写格式。从表层看，RequireJS 和 SeaJS 最大的差异就是各自默认推荐的模块书写格式不同。</p>
<p>由于 RequireJS 支持 Simplified CommonJS Wrapper, 一定程度上可以说 SeaJS 是 RequireJS 的一个子集。甚至可以认为：<strong>SeaJS 是 RequireJS 中的  The Good Parts.</strong>（RequireJS 作者肯定不会同意，不过从社区中可以看出很多人很喜欢 Simplified CommonJS Wrapper 格式，这与 SeaJS 遵循的 Wrappings 基本一致。）</p>
<h2>功能差异</h2>
<p>从功能上讲，RequireJS 很丰富。下面这些功能是 RequireJS 有，而 SeaJS 没有的：</p>
<ol>
<li>domReady 插件</li>
<li>i18n 插件</li>
<li>order 插件</li>
<li>通过 r.js 支持 packages</li>
<li>支持 Web Worker</li>
<li>支持 Rhino</li>
</ol>
<p>这些功能，SeaJS 现在没有，以后也不打算提供。因为 SeaJS 的定位是：<strong>浏览器端的模块加载器</strong>。</p>
<p>SeaJS 也有一些非常好用的功能，RequireJS 却没有的：</p>
<ol>
<li>映射插件，可用来做 <a href="http://lifesinger.wordpress.com/2011/07/24/online-local-debug/">在线本地调试</a></li>
<li>通过 map 配置，对版本和时间戳提供更方便的管理</li>
<li>通过 preload 配置，可以预加载模块</li>
<li>更开放的插件机制，并默认提供 plugin-json 和 plugin-less</li>
<li>支持 css 文件的加载</li>
<li>由于延迟执行机制，可以很便捷地探测 404 等错误请求</li>
</ol>
<h2>场景差异</h2>
<p>在使用场景上，两者之间的差异不小。</p>
<p>RequireJS 的例子一直是单项目，类库和业务脚本都放在一起：</p>
<pre>
project
-- scripts
   -- require-jquery.js
   -- main.js
   -- sub
      -- some.js
   ...
-- project.html
</pre>
<p> <br>
这种使用场景下，RequireJS 和 SeaJS 半斤八两，彼此彼此。</p>
<p>但上面的组织方式不是 SeaJS 推荐的。SeaJS 推荐通用类库独立存放：</p>
<pre>
libs
--seajs/1.0.2/sea.js
--jquery/1.6.4/jquery.js
--underscore/1.2.1/underscore.js
...

project-assets
-- main.js
-- sub
   -- some.js
...

project
-- project.html
</pre>
<p> <br>
这样，不同项目之间，可共用 libs. libs 的版本维护也很方便。比如：在 requirejs 里，如果用到 jquery, 当 jquery 更新时，得重新打包 require-jquery.js. 在 SeaJS 里则不需要更新 sea.js, 只需要更新配置中的 alias.</p>
<p>SeaJS 很适合大型网站，伸缩性很好，能适应的场景很广。这受益于 SeaJS 的广义定位：<strong>浏览器端的 NodeJS</strong>.</p>
<h2>写在最后</h2>
<p>RequireJS 我不是很喜欢，主要是因为其定位不纯粹，以及 API 在我看来不够优雅。但 RequireJS 的作者依旧是很让人敬佩的一个牛人。从在 CommonJS 积极讨论，到非常有激情地开发 RequireJS，以及后续坚持不懈布道，这些工作，看起来不难，实际上非常不易，<strong>很佩服这份激情、执着和毅力</strong>。</p>
<p>作为 SeaJS 的作者，我尽可能公平公正评价 RequireJS. 如果上面的文字有失偏颇，还希望你热心指正，欢迎所有真心诚恳的交流讨论。</p>
<p>（完）</p>
<br>  <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gocomments/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/comments/lifesinger.wordpress.com/607/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/godelicious/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/delicious/lifesinger.wordpress.com/607/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gofacebook/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/facebook/lifesinger.wordpress.com/607/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gotwitter/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/twitter/lifesinger.wordpress.com/607/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/gostumble/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/stumble/lifesinger.wordpress.com/607/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/godigg/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/digg/lifesinger.wordpress.com/607/"></a> <a rel="nofollow" href="http://feeds.wordpress.com/1.0/goreddit/lifesinger.wordpress.com/607/"><img alt="" border="0" src="http://feeds.wordpress.com/1.0/reddit/lifesinger.wordpress.com/607/"></a> <img alt="" border="0" src="http://stats.wordpress.com/b.gif?host=lifesinger.wordpress.com&amp;blog=38365&amp;post=607&amp;subd=lifesinger&amp;ref=&amp;feed=1" width="1" height="1">