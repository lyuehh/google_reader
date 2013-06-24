---
layout: post
title:  "引入JavaScript虚拟机的语言：CoffeeScript 1.0、StratifiedJS、利用C/C++的Emscripten和Python"
date:   2010-12-30 10:36:23
author: 
categories: program
---

## 引入JavaScript虚拟机的语言：CoffeeScript 1.0、StratifiedJS、利用C/C++的Emscripten和Python
### by 
### at 2010-12-30 10:36:23
### original <http://news.cnblogs.com/n/86742/>

<p>　　JavaScript虚拟机现在发展非常迅速，而且也不断变得普适化 - 这是语言设计者和实现者都喜闻乐见的事情。期望运行于JVM或者.NET平台的语言也吸引了很多目光，但是最近一些新的语言还是主要围绕JavaScript做文章。</p>
<p>　　JavaScript的主要缺点就是语言本身，所以开发者着手创建一种<a href="http://www.infoq.com/news/2009/09/javascript-compilation-target">能够编译JavaScript的新语言</a>。</p>
<p>　　<a href="http://www.infoq.com/news/2010/08/coffeescript">CoffeeScript</a>刚刚发布了1.0版，这门语言希冀于能够在整理JavaScript的基础上加入一些有用的特性。CoffeeScript编译器是使用CoffeeScript语言本身编写，可以在编译的时候格式化JavaScript代码。</p>
<p>　　它是使用空格缩进而不是花括号来分隔代码段 - 这个功能引起了社区不小的争论，虽然其他语言，例如CSS3扩展<a href="http://sass-lang.com/">Sass</a>，<a href="http://haml-lang.com/">Haml</a>等也是这样做的。</p>
<p>　　CoffeeScript已经开发了近一年时间，但是直到最近才为一些项目所采用。<a href="http://arstechnica.com/apple/news/2010/11/introducing-the-ars-technica-reader-for-ipad.ars">ArsTechnica的iPad应用</a>就是使用HTML和CoffeeScript编写，在<a href="http://www.infoq.com/news/2010/02/phonegap-status">PhoneGap</a>的帮助下作为原生应用运行。37Signal也发布了<a href="http://chalk.37signals.com/unsupported">Chalk</a>，这是一个运行在iPad上的白板应用，并且使用了例如Cache Manifest这样的HTML5特性来支持离线运行。它同样也<a href="http://samisamhuri.blogspot.com/2010/11/37signals-chalk-dissected.html">使用了CoffeeScript</a>。</p>
<p>　　<a href="http://www.infoq.com/articles/stratifiedjs">StratifiedJS</a>是JavaScript的一个超集，它加入了一些并发相关的关键字来解决异步编程的冗长拖沓。可以阅读关于<a href="http://www.infoq.com/articles/stratifiedjs">StratifiedJS以及浏览器实现Apollo背后的动机的采访</a>来获取更多信息。</p>
<p>　　<a href="http://code.google.com/p/emscripten/">Emscripten</a>项目使用了另外一种方法。它尝试将使用LLVM编译器的语言引入到JavaScript虚拟机中。LLVM可以将代码编译成比特码，然后Emscripten将其翻译成JavaScript。它使用一个堆数组和一个自定义的malloc版本在某种程度上支持手动内存管理。</p>
<p>　　Emscripten也可以翻译C和C++代码，我们可以看到它的主页上已经有了一些已经翻译好的C和C++库的demo。不仅仅是这两种语言，在主页上我们也可以看到<a href="http://syntensity.com/static/python.html">Python</a>和<a href="http://syntensity.com/static/lua.html">Lua</a>也有一些已经翻译完毕的运行时demo。</p>
<p>　　不只是这些新语言和运行时，已有语言仍然可以翻译成JavaScript，例如<a href="http://github.com/johang88/haskellinjavascript">Haskell</a>，Steve Dekorte的<a href="https://github.com/stevedekorte/oia">lo语言</a>也可以在<a href="http://www.skulpt.org/">Skulpt</a>项目的帮助下翻译成Python。</p>
<p>　　仍然需要时间来证明哪种语言将会是最有用和最成功的。但是，有一件事情是肯定的，那就是：即便Emscripten这样的解决方案能够流行起来的话，JavaScript VM、JIT和GC开发者仍然需要不断地改进性能。</p>
<p>　　查看英文原文：<a href="http://www.infoq.com/news/2010/12/languages-on-javascript">Languages Come to  Javascript VMs: CoffeeScript 1.0, StratifiedJS, C/C++ with Emscripten, Python</a></p><p><br>　　相关新闻：<br>　　· <a href="http://news.cnblogs.com/n/80016/">用JavaScript来写Gameboy模拟器</a><span style="color:gray">(2010-11-06)</span><br>　　· <a href="http://news.cnblogs.com/n/58579/">不再限于页面脚本 JavaScript挺入服务器端开发语言行列</a><span style="color:gray">(2010-03-12)</span><br>　　· <a href="http://news.cnblogs.com/n/66370/">10个绝妙的HTML5，CSS和Javascript示例</a><span style="color:gray">(2010-06-16)</span><br>　　· <a href="http://news.cnblogs.com/n/84975/">Hotmail 活动视图平台新应用类型 – 允许邮件内执行 JavaScript</a><span style="color:gray">(2010-12-17)</span><br>　　· <a href="http://news.cnblogs.com/n/83523/">微软制做了一个恶意JavaScript检测工具</a><span style="color:gray">(2010-12-05)</span><br></p><p>　　本文链接：<a href="http://news.cnblogs.com/n/86742/">http://news.cnblogs.com/n/86742/</a></p><img src="http://news.cnblogs.com/news/rssclick.aspx?id=86742" width="1" height="1" alt="">