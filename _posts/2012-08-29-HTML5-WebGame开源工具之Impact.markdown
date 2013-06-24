---
layout: post
title:  "HTML5 WebGame开源工具之Impact"
date:   2012-08-29 11:04:44
author: 杨玉廷
categories: program
---

## HTML5 WebGame开源工具之Impact
### by 杨玉廷
### at 2012-08-29 11:04:44
### original <http://hp.dewen.org/?p=1349>

<p>随着浏览器（PC/Mobile）功能的不断增强，特别是近几年来<a href="http://www.dewen.org/topic/23">HTML5</a>系列技术，<a href="http://www.dewen.org/topic/21">CSS3</a>及JS引擎的快速发展，使用传统的<a href="http://www.dewen.org/topic/338">Web前端开发</a>技术来开发网页已经成为了一个新的趋势。就我个人个遇到的几个框架及近期别人推荐的进行一些分析。主要针对使用HTML+CSS+<a href="http://www.dewen.org/topic/11">JavaScript</a>技术来开发游戏的技术做一分类，其它第三方语言实现的中间件平台只在最后简要介绍。</p>
<div style="text-align:center;margin-bottom:5px;margin-top:5px"><img src="http://hp.dewen.org/wp-content/uploads/2012/08/Impact-JavaScript.png" alt="HTML5 WebGame开源工具之Impact" title="HTML5 WebGame开源工具之Impact" border="0"></div>
<p><span></span></p>
<p>HTML <a href="http://www.dewen.org/topic/424">Web Games</a>的游戏开发，目前主要有以下三类方式：</p>
<ul>
<li>纯DOM+CSS的传统方式实现游戏</li>
<li>Canvas/WebGL &amp; CSS3 实现游戏</li>
<li>SVG方式开发游戏</li>
</ul>
<p>当然，介于这三类之间的混合方式也有人已经在尝试，类似于目前比较火的Hybird Native Web App的理念，毕竟，只要能快速，<a href="http://hp.dewen.org/?p=1349">高效解决问题的技术才是好技术</a>。</p>
<p>由于HTML5技术这两年的迅速普及，大多数入门的游戏开发入门者，主要接受的是使用<a href="http://hp.dewen.org/?p=1349">Canvas技术</a>来开发游戏。但对于使用纯DOM+CSS的传统技术组合开发游戏与Canvas开发游戏二者性能上的比较，很多资深开发者们，不停地做着性能测试，来支持自己的观点。主要是基于以下考虑：canvas提供的API太基础了，游戏开发者如果使用canvas，就意味着放弃了CSS这个浏览器“亲生的”样式工具。于是接着就涌现出了很多基于canvas的库，来辅助开发者快速开发游戏。但这些canvas库的封装性能来渲染图像的性能与使用DOM+CSS进行图片渲染的性能的PK目前还未有定论。就个人的测试，小游戏开发方面性能差别可以忽略。前端界有一个大拿做了一个js动画库，基于同一套代码生成基于canvas,webgl,css 各自渲染的效果，很不错(<a href="http://labs.hyperandroid.com/static/caat/">http://labs.hyperandroid.com/static/caat/</a>)。</p>
<p>下面对于目前比较流行的一些游戏工具库进行介绍。</p>
<p>ImpactJS是目前最强大的一个js 2D游戏开发平台，当然是商业版的(<a href="http://impactjs.com/">http://impactjs.com</a>)，（国内有一个山寨者做了一个<a href="http://www.kilofox.net/">http://www.kilofox.net/</a>， 直接拿人家的源码来卖，太TMD不要脸了)。不管是新手还是资深的开发者，使用这个库都可以快速开发出来游戏。作者很nice,半年前我以研究者的身份买了一份进行了测试，基本上照着示例不到十分钟就做出来了一个带简单AI的小游戏.</p>
<p>从平台本身的角度，这款框架有以下特性：</p>
<ul>
<li>良好的跨平台性，目前作者发布了测试版的iOSImpact，通过 OpenGL 直接将JavaScript 源代码的执行结果渲染到屏幕上。这完全跳过了 iPhone 的手机 Safari，避免了这个浏览器固有的一些问题。iOSImpact完成之后，期待AndroidImpact出来，哈哈。</li>
<li>目前市面上最好的Web版2D关卡编辑器Weltmeister，快速地可以对游戏进行界地图编辑</li>
<li>强大的游戏调试菜单，很像我们调试JS时使用的firebug, 快速地看到每一个游戏实体的更新，渲染FPS等。</li>
<li>作者的更新很迅速，可以在github上查看到他非常勤奋地在推进这个框架，论坛中的开发者们也很活跃。目前有一个专门收集使用impactjs的游戏网站<a href="http://www.pointofimpactjs.com/">http://www.pointofimpactjs.com/</a>。</li>
<li>第三方工具的支持，如与Appmobi这家强大的移动游戏开放平台商的合作。Appmobi提供了专门的impactjs版的SDK供开发者开发游戏。与打包工具phonegap等的结合，与著名物理引擎Box2D的结合等。</li>
</ul>
<p>查看ImpactJS本身的代码架构，可以看到，它包括了制作一款游戏的各个方面：</p>
<ul>
<li>核心的模块定义，动态加载，扩展了原生JS的类似于underscore.js的工具类库，系统整体FPS控制等，资源预加载。</li>
<li>逻辑处理 包括游戏场景控制器，活动对象控制，多层地图控制，计时器及输入检测</li>
<li>声音资源的处理（前景声音，背景声音），在HTML5相关的浏览器厂商还在为标准争论的时候，这是我目前已知的解决声音播放的比较好的一款JS游戏库</li>
<li>特效处理 基本所有的游戏库都会有这方面的处理，不过好像在这块ImpactJS做得还不够好。</li>
</ul>
<p>四月的时候，Jesse Freeman 出了一本<a href="http://book.douban.com/subject/7906798/">《Introducing HTML5 Game Development》</a>的书，全程引导开发者用 Impact 创建游戏。它涵盖了从建立工作环境到游戏打包发布的所有内容。横向对比而言，ImpactJS算是目前相当不错的一款基于canvas的游戏引擎。</p>
<p><b>德问编程社交问答：<a href="http://www.dewen.org/">http://www.dewen.org/</a><br>
最新问题：<a href="http://www.dewen.org/questions">http://www.dewen.org/questions</a></b></p>
<p><b>本文来源：<a href="http://blessdyb.iteye.com/blog/1664615">HTML5 WebGame开源工具之impactjs</a></b></p>