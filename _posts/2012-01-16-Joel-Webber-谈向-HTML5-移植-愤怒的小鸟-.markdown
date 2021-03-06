---
layout: post
title:  "Joel Webber 谈向 HTML5 移植“愤怒的小鸟”"
date:   2012-01-16 16:41:44
author: admin
categories: program
---

## Joel Webber 谈向 HTML5 移植“愤怒的小鸟”
### by admin
### at 2012-01-16 16:41:44
### original <http://www.hiwebgl.com/?p=837>

<p>作者 <strong><a href="http://www.infoq.com/cn/author/Abel-Avram;jsessionid=266B8E7A1AD2004B7668C83BAF80DF57">Abel Avram</a> </strong>译者 <strong><a href="http://www.infoq.com/cn/author/%E9%83%91%E6%9F%AF;jsessionid=266B8E7A1AD2004B7668C83BAF80DF57">郑柯</a> </strong>发布于 2012年1月15日</p>
<p>Joel Webber是Google Web Toolkit的创建者之一，他在GOTO Aarhus 2011大会上做了名为<a href="http://www.infoq.com/news/2012/01/Angry-Birds-HTML5;jsessionid=266B8E7A1AD2004B7668C83BAF80DF57">”HTML 5中愤怒的小鸟”</a>的演讲，该演讲由InfoQ录制并发布。我们采访了Webber，想知道将“愤怒的小鸟”这款热门游戏移植到Google Chrome和HTML5上的更多细节。</p>
<p><strong>InfoQ：关于将“愤怒的小鸟”移植到HTML5上，请您提供一些技术细节吧。</strong></p>
<p><strong>JW：</strong>在GOTO的演讲里面，涵盖了大多数广泛技术层面。简单说来，游戏是用Java写的，使用Google Web Toolkit转编译成了JavaScript。在移植过程中，我们开发了一个小程序库，叫PlayN，游戏开发者可以用其完全在Java环境中工作（相比在浏览器中调试，直接在JVM上调试要方便、强大得多）。经过转编译，PlayN程序库让同样的代码可以运行在Flash和Android上，尽管Rovio目前还没有使用这个功能。</p>
<p><strong>InfoQ：您使用了哪些HTML5特性？</strong></p>
<p><strong>JW：</strong>要是说得迂腐一点，我用到了一些目前还不属于“HTML5”的规范，但常常被人们认为是其一部分。其中包括：WebGL、Canvas、CSS3（特别是仿射[affine]变换），LocalStorage、&lt;audio&gt;/WebAudio。在渲染上，有两种模式：WebGL和DOM。WebGL模式中，页面上只有一个大的&lt;canvas&gt;元素。在DOM模式中，我们对每个对象使用单独的DOM元素，这些对象包括鸟、猪、石块、背景元素等等。然后，我们使用CSS3变换来高效地给这些元素布局。之所以用这两种模式，是因为还有很多浏览器不支持WebGL，而且我们想保证所有可能的浏览器都能运行这个游戏，不仅仅是Chrome。</p>
<p>在音效方面，刚开始的时候，我们必须退而使用基于Flash的方法，因为一些浏览器对于&lt;audio&gt;标签的支持很差（包括Chrome）。后来这里修复了一下，Flash也就成为我们很少使用的倒退式方法。最终，我们相信WebAudio（<a href="https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html">https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html</a>）才是游戏中应该使用的API。</p>
<p><strong>InfoQ：移植这个游戏花了多少时间？</strong></p>
<p><strong>JM：</strong>很难给出精确答案。在Google，我们有三个人支持Rovio，但主要是用那20%的时间（有时还包括晚上和周末）。一个Rovio的工程师完成了移植的大部分主要工作。其他人后来辅助一些应用内支付工作，以及一些上线后的问题。</p>
<p><strong>InfoQ：最困难的工作是哪些？</strong></p>
<p><strong>JW：</strong>游戏的逻辑很直接。我们已经有Box2D（Angry Birds和其他很多2D物理游戏使用的引擎）可以使用，用Java开发游戏让其更易于管理（某种程度上比较大的）代码库。不过还是有一些东西需要额外的工作：</p>
<ul>
<li>资源加载：不同于大多数本地应用，Web应用通常会按需加载资源，这样才能快速启动。一般来说这是好事，但是会让应用逻辑更复杂，因为我们希望需要的资源下载完成后，每个游戏场景马上就能显示出来。这还意味着你需要对资源的请求小心排序，这样就不会在还不需要的资源上耗费带宽。</li>
<li>跨浏览器渲染：如前所述，没有什么“最好的”方式，能够在所有的浏览器中渲染游戏画面。抽象渲染逻辑，并实施两种完全不同的方法，需要大量精力和工作量。</li>
<li>让游戏能够做到每秒60帧运行，这是一个很重要的目标。垃圾回收暂停让其变得很困难，尽管我们最后还是解决了这方面的问题。</li>
</ul>
<p>在InfoQ提供的“HTML5中愤怒的小鸟”演示中，Web提到更多遇到的关于垃圾回收方面的难题，以及<a href="http://code.google.com/p/playn/">PlayN程序库</a>，还有该程序库在JVM上调试时起到的作用。</p>
<p><strong>查看英文原文：</strong><strong><a href="http://www.infoq.com/news/2012/01/Angry-Birds-HTML5;jsessionid=266B8E7A1AD2004B7668C83BAF80DF57">Joel Webber on Porting Angry Birds to HTML5</a> </strong></p>
<p>转载自<a href="http://www.infoq.com/cn/news/2012/01/Angry-Birds-HTML5">InfoQ</a></p>