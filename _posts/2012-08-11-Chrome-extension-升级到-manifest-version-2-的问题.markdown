---
layout: post
title:  "Chrome extension 升级到 manifest version 2 的问题"
date:   2012-08-11 10:54:14
author: 糖伴西红柿
categories: program
---

## Chrome extension 升级到 manifest version 2 的问题
### by 糖伴西红柿
### at 2012-08-11 10:54:14
### original <http://www.qianduan.net/chrome-extension-upgrade-to-the-manifest-version-2-2.html>

<p>最近在折腾一个 chrome extension, 虽然之前也折腾过，但是了解很浅，这次就顺带系统地了解了下。开始的时候一切都很顺利，对一个前端工程师来说，学习的成本并不是很高。</p>
<h3>问题</h3>
<p>在某天 chrome 提示升级的时候，就果断升级了（当前版本为 Version 21.0.1180.57）。升级之后，发现在 extensions 页面，有个提示：</p>
<blockquote><p>There were warnings when trying to install this extension:<br>
Support for manifest version 1 is being phased out. Please <a href="http://code.google.com/chrome/extensions/manifestVersion.html">upgrade to version 2</a>.</p></blockquote>
<p>一开始并没有想到这个版本号的升级会有多大问题，因为至少<a href="http://code.google.com/chrome/extensions/manifestVersion.html">看起来</a>并没有特别多的改动。但是实际操作起来，遇到了不少问题，还挺让人头疼的。</p>
<p>升级之后整个插件就崩溃了。经查看 console 里面的提示信息为：</p>
<blockquote><p>Uncaught Error: Code generation from strings disallowed for this context</p></blockquote>
<p>一番查询之后，发现这次版本号升级里面，实际在 extension 的安全策略方面做了较大改动.改变最大的是 <a href="http://code.google.com/chrome/extensions/contentSecurityPolicy.html">Content Security Policy (CSP)</a> 。这个改变包括，不再允许执行 inline script；不再允许使用 eval() 或者 new Function() 来进行字符串处理。</p>
<p>对 eval() 和 new Function() 的限制很悲剧，因为大多数的 javascript 模板库都用到了这两个方法。我在用的 <a href="http://api.jquery.com/category/plugins/templates/">jQuery templates</a> 就因为用到了 new Function() 而悲剧了。</p>
<p>而且没有任何方式可以改变默认的安全策略来使用这些被 CSP 禁用的方法。</p>
<blockquote><p>There is no mechanism for relaxing the restriction against executing inline JavaScript. In particular, setting a script policy that includes unsafe-inline will have no effect. This is intentional.</p></blockquote>
<h3>方案</h3>
<ol>
<li>chrome extension team 提供了一个官方的解决方案。可以使用 <a href="http://developer.chrome.com/trunk/apps/manifest.html#sandbox">sandboxed page</a>来继续使用用到了 eval() new Function() 的 javascript template library 。通过 iframe 加载它并与之通信以渲染模板。参考
<ul>
<li><a href="http://developer.chrome.com/trunk/extensions/sandboxingEval.html">Using eval in Chrome Extensions. Safely.</a></li>
<li><a href="https://github.com/GoogleChrome/chrome-app-samples/tree/master/sandbox">官方实例</a>。</li>
</ul>
</li>
<li>另外可用的方案是使用可以预编译模板的 javascript template library，目前我使用了 <a href="http://www.qianduan.net/handlebarsjs.com">handlebarsjs</a>，其他可用的还有 <a href="http://akdubya.github.com/dustjs/#dust">dust</a> <a href="https://github.com/sstephenson/eco">eco</a> 。这几个模板库都可以通过预编译模板文件来规避 eval() new Function 与 CSP 的冲突。</li>
<li>还有一个需要注意的地方是，我在插件中还用到了 <a href="http://underscorejs.org/">underscore</a> 的一些函数。因为 underscore 里面的 template() 也使用了 new Function()，所以我只能把相关的函数单独拆分出来，以避免和 CSP 的冲突。</li>
</ol>
<h3>关于 handlebarsjs</h3>
<ol>
<li>安装通过 npm 安装来使用 handlebars 的预编译
<pre><code> $ npm install handlebars -g </code></pre>
</li>
<li>把之前的模板转为 handlebars 模板以我之前使用的 jQuery template 为例，先把之前写在 html 里面以 <code>&lt;script type=&quot;text/x-jquery-tmpl&quot;&gt;</code> 标记的模板拆分成单个后缀为 handlebars 的文件。然后根据 handlebars 的语法要求，把 <code>{{= }} {{if}} {{each}}</code> 分别替换为 <code>{{}} {{#if}} {{#each}}</code> 。</li>
<li>预编译
<pre><code> $ handlebars &lt;input&gt; -f &lt;output&gt; </code></pre>
<p>handlebars 内置的那些 helper 相对来说过于简单了。比如 jQuery template 里面的 <code>{{each(i, v) items}}</code> 都需要自己写 helper 来实现。官方的说法是鼓励自己写 helper 来实现这些用法，而且 “<strong>that’s on purpose.</strong>” 。</p>
<p>可以把这些 hepler 单独写在一个 helpler.js 里面，预编译的时候，通过 -k 指定这个 helper.js 进行编译。</p>
<p>官方文档里面有提到</p>
<blockquote><p>Because you are precompiling templates, you can also apply several optimization to the compiler. The first allows you to specify a list of the known helpers to the compiler</p>
<pre><code>handlebars &lt;input&gt; -f &lt;output&gt; -k each -k if -k unless </code></pre>
<p>The Handlebars compiler will optimize accesses to those helpers for performance.</p></blockquote>
</li>
<li>使用模板模板预编译完成之后，会得到一个如 templates.js 的模板文件。只需要把 <a href="https://github.com/wycats/handlebars.js/archives/master">handlebars runtime</a> 文件和这个模板文件通过 <code>&lt;script&gt;</code>方式引入到 html 中就可以使用了。handlebars 把所有的模板挂载到全局的 <code>Handlbars.templates</code> 对象之下。每个模板都是安装模板文件的名称来命名的。如果模板为 <code>cinema.handlebars</code> 则可以通过 <code>Handlbars.templates.cinema</code> 来获取到这个模板。
<pre><code>var _cinema = { 'title': '', 'id': '', 'total': '', 'timelines': [] }; var _render = Handlbars.templates.cinema(_cinema) </code></pre>
<p><code>_render</code> 就是渲染后的模板。</p></li>
<li>handlebars 参考资料
<ul>
<li><a href="http://thinkvitamin.com/code/getting-started-with-handlebars-js/">ArticleGetting Started with Handlebars.js</a></li>
<li><a href="http://thinkvitamin.com/code/handlebars-js-part-2-partials-and-helpers/">ArticleHandlebars.js Part 2: Partials and Helpers</a></li>
<li><a href="http://thinkvitamin.com/code/handlebars-js-part-3-tips-and-tricks/">ArticleHandlebars.js Part 3: Tips and Tricks</a></li>
</ul>
</li>
</ol>
<p>以安全之名，各位 chrome extension 开发者，开始折腾吧。</p>
<h3>reference</h3>
<p>在解决问题的查找过程中，发现中文资料还是太少，幸亏发现了 <a href="http://matthewrobertson.org/blog/2012/07/10/javascript-templates-and-chromes-content-security-policy/">Javascript Templates and Chrome’s Content Security Policy</a> 这篇文章，从问题到解决方案，一应俱全，因此我这篇只是在他的基础上做了一些补充。</p>
<h3>更新</h3>
<p>关于 javascript template library，今天同事分享了一个看上去更简单的 <a href="https://github.com/jasonmoo/t.js/blob/master/t_test.html">t.js</a>。暂时先补充上，之后或许会试用下。</p>
<img src="http://feeds.feedburner.com/~r/qianduannet/~4/Sz86C1P9ovw" height="1" width="1">