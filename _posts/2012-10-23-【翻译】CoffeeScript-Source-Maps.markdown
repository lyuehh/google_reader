---
layout: post
title:  "【翻译】CoffeeScript Source Maps"
date:   2012-10-23 16:09:19
author: island205
categories: program
---

## 【翻译】CoffeeScript Source Maps
### by island205
### at 2012-10-23 16:09:19
### original <http://island205.com/2012/10/23/%e3%80%90%e7%bf%bb%e8%af%91%e3%80%91coffeescript-source-maps/>

<p><em>作者：<a href="http://ryanflorence.com/2012/coffeescript-source-maps/">Ryan Florence</a> </em></p>
<p>Michael Ficarra创建的“better CoffeeScript compiler”的<a href="http://www.kickstarter.com/projects/michaelficarra/make-a-better-coffeescript-compiler">kickstarter</a>很成功，而且现在还反馈了一个巨大的红利支持者——<a href="http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/">source maps</a>，CoffeeScript调试，作为不把CoffeeScript用在浏览器中最流行也是最强的反对意见，现在已经解决90%了。</p>
<h2>安装CoffeeScriptRedux</h2>
<p>从Github上把它clone下来：</p>
<div>
<pre><code>    $ git clone git://github.com/michaelficarra/CoffeeScriptRedux.git
</code></pre>
</div>
<p>安装依赖：</p>
<div>
<pre><code>    $ cd CoffeeScriptRedux
    $ make deps
</code></pre>
</div>
<p>运行测试，保证没啥问题：</p>
<div>
<pre><code>    $make test
</code></pre>
</div>
<h2>把你的文件编译为JS吧</h2>
<p>CoffeeScriptRedux没有使用与CofffeeScript一样的接口，所以可能这会与你使用的稍微有点不一样。</p>
<p>从CoffeeScriptRedux目录开始：</p>
<div>
<pre><code>    $ ./bin/coffee --js -i /path/to/test.coffee
</code></pre>
</div>
<p>这会创建一个文件：<code>/path/to/test.js</code>。</p>
<h2>创建Source Map文件</h2>
<div>
<pre><code>    $ ./bin/coffee --source-map -i /path/to/test.coffee &gt; /path/to/test.js.map
</code></pre>
</div>
<p>为了干成这事，我不得不安装了source-map：</p>
<div>
<pre><code>    $ npm install source-map
</code></pre>
</div>
<h2>把Source Map告诉浏览器</h2>
<p>你需要告诉浏览器source map文件在哪里，可以往文件里添加一行注释，你可以编辑<code>test.js</code>，或者直接运行：</p>
<div>
<pre><code>    $ (echo; echo &#39;//@ sourceMappingURL=test.js.map&#39;) &gt;&gt; /path/to/test.js
</code></pre>
</div>
<h2>打开Chrome开启Source Map功能</h2>
<p>打开控制台，点击右边的齿轮按钮，勾选“Enable Source Maps”。</p>
<p><img src="http://pic.yupoo.com/island205/CtcnPtKr/medish.jpg" alt="chrome" width="640" height="418"></p>
<h2>“That’s it!”</h2>
<p>打开一个带有<code>test.js</code>的页面，查看<code>web inspector</code>的“Sources”，你的CoffeeScript就在那里，你可以设置断点，鼠标划过变量能够看到它们的值（译者：噢噢噢噢），诸如此类！</p>
<p>下面是一个我故意打破映射的实现：</p>
<p><a href="http://ryanflorence.com/2012/coffeescript-source-maps/demo.zip">下载示例文件</a></p>
<p><img src="http://pic.yupoo.com/island205/CtcnPuR9/medish.jpg" alt="map" width="640" height="449"></p>
<h2>那，那10%没有解决的是什么？</h2>
<p>就是，你无法在console中直接输入CoffeeScript。这里有一个CoffeeConsole chrome插件，但是在断点的地方它什么都不知道啊 。（译者：说不准可以使用<code>：//@sourceUrl=&lt;path&gt;</code>解决？）</p>