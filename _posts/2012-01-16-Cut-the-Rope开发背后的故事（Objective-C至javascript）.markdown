---
layout: post
title:  "Cut the Rope开发背后的故事（Objective-C至javascript）"
date:   2012-01-16 00:49:12
author: Dr.H
categories: program
---

## Cut the Rope开发背后的故事（Objective-C至javascript）
### by Dr.H
### at 2012-01-16 00:49:12
### original <http://item.feedsky.com/~feedsky/Phonekr/~8620091/597091175/6726073/1/item.html>

<p>或许大家都知道了这个剪绳子的软件已经移植到了html5平台，并且因为更大的屏幕显示效果更佳而得到很多好评。但是从c语言到javascript中的转换过程有什么故事呢？<a href="http://www.phonekr.com/wp-content/uploads/2012/01/cut-the-rope.jpg"><img title="cut-the-rope" src="http://www.phonekr.com/wp-content/uploads/2012/01/cut-the-rope.jpg" alt="" width="600" height="394"></a><span></span></p>
<h2>（没有玩过？<a href="http://www.phonekr.com/www.cuttherope.ie">点我呗</a>）</h2>
<h2>分享的目的：首先这篇文章说明javascript的性能程度不是很低（webOS只要向好的方向发展，未来就会很好，不存在什么代码效率太慢没有前途的问题）。再者给开发者们看看，无论是入门还是老鸟，这篇文章都值得一读，给予开发过程中的更多灵感。</h2>
<p> </p>
<p><strong>Cut the Rope</strong> 是一款人见人爱的小游戏。所以我们有了个点子，即通过使用HTML5将这款游戏放到网上以便让更多的人能接触到它。</p>
<p>为了做到这一点，微软的IE团队和<strong>ZeptoLab</strong>团队（游戏的开发者）以及<strong>Pixel Lab</strong>的专家们合作以便将<strong>Cut the Rope</strong> 的网络版本实现。最终效果要能将游戏毫不失真地翻译成网络版本，并且能展示出HTML5的强大功能：画布提供的绘图、基于浏览器的音频和视频、CSS3风格以及WOFF字体的个性。</p>
<p>我们觉得HTML5版本的游戏让网络更有趣了，同时，它也展示了IE的上一个版本因支持开放标准而显示出的长处。因此，我们想要分享一些开发 <strong>Cut the Rope</strong> 过程中的技术细节，以便帮助构建你自己的HTML5站点，并最终为Windows 8 Store做准备！</p>
<h2></h2>
<h2><strong>从</strong><strong>Objective-C</strong><strong>到</strong><strong> JavaScript</strong></h2>
<p>在将<strong>Cut the Rope</strong>应 用到一个新平台上的时候，我们希望我们保留了这个游戏的独特的物理特性、动作以及风格。所以，在开始的时候，我们想要改写iOS版本<span style="color:#ff0000">（而不是重写）。</span>我们 仔细分析了用Objective-C写成的原始版本，发现工程量很大，并且很复杂。iOS版本包含大约15，000行代码（不包括库！）代码中最复杂的部 分是动作、动画以及绘图引擎。它们本身就很复杂，使事情变得更为复杂的是这三块之间耦合度很高，并经过了大量优化。</p>
<p><span style="color:#ff0000">这是个令人生畏的工作：要将这些代码在浏览器上实现，而又不丧失原先的独特个性以及很高的质量。为了完成这个工作，我们赌上了Javascript。</span></p>
<p><span style="color:#ff0000">在 过去，Javascript一直被人看做是速度很慢的语言。坦白讲，这种说法在最初的确是对的。老的Javascript引擎是为处理简单的“脚本” （scripting）类型的工作设计的（如它的名字所示）。然而，在现在，Javascript引擎已经经过大量优化了。整合进just-in- time等功能以后，Javascript执行速度已经可以接近底层代码执行速度了。</span></p>
<p>除此以外，我们知道使用Javascript编程不同于——并且需要的思维方式也不同于——用编译型语言编程。当我们将这个游戏从Objective-C改写过来的时候，我们知道应该充分利用Javascript编程的不同以及优点。</p>
<p><span style="color:#ff0000">一 个明显的例子是在Javascript中缺少structs</span>。Structs是相关属性的轻量级的聚合。使用Javascript对象来聚合一系列属性是 很容易的，但是这和使用一个合适的struct是很不同的。第一个不同是一旦structs被赋值给一个变量或则传递到一个函数的时候，它都会被复制。因 此，一个使用如Objective-C这样的编译型语言编写的函数可以修改以参数形式传过来的struct的值，而又不改变原来调用函数中的值。即使是在 同一个函数中，将一个不同的值赋给struct也会将值复制一遍。而Javascript对象，是通过引用传递的。所以一个函数修改了一个对象参数的时 候，原调用函数也能看到这个变化。</p>
<p><span style="color:#ff0000">一个用来模仿structs的简单的方式是每当将Javascript对象作 为赋值对象或者参数传递的对象时都创建一个副本对象。</span>在底层语言（native languages）中，使用structs的开销是很小的。但在Javascript中创建一个对象则会有很大开销，因此我们要非常小心，减少创建对象 的次数。特别是在赋值的时候，我们尽可能地复制单个属性，而不是创建一整个新的对象实体。</p>
<p><span style="color:#ff0000">另一个例子是 Objective – C代码库面向对象的本质。</span>与传统的基于对象的继承不同，JavaScript提供了基于原型属性（Prototype property）的继承。这是对基于对象的继承的一个高度简化的形式，与传统的Objective – C这样面向对象的语言不兼容。幸运的是，有各种各样的类库，可以帮助你写的面向对象编程（OOP）风格的JavaScript代码，我们使用的类库是一个非常简单的由John Ressig写的。 （需要注意的是，ECMAScript5，最新版本的JavaScript的规范，也提供了对一些类的支持，但我们选择不使用它，因为我们对该版本的语言不熟悉，而我们的开发进度非常紧张）。</p>
<p>除了将代码从Objective-C改到Javascript，<span style="color:#ff0000">我们还需要将图像代码从OpenGL改到HTML5的Canvas API</span>。总体上说，这一切都进行地很顺利。Canvas是一个很快的渲染表面，特别是在一个API由硬件加速的浏览器中（比如IE9）。</p>
<p> </p>
<div>
<dl>
<dt><img src="http://www.webapptrend.com/wp-content/uploads/2012/01/notes-aliasing-thumb.jpg" alt="" width="220" height="220"></dt>
<dd>一个使用帆布API完成的aliased lines来画绳子的例子。</dd>
</dl>
</div>
<p>令人惊讶地是，我们遇到了好几个地方，都是Canvas比在移动版本Cut the Rope中使用的OpenGL ES提供了更多功能的。一个例子是画<strong>anti-alias lines</strong>。 在使用OpenGL画anti-aliased lines的时候，需要将一条线镶嵌到一个三角形地带中，并且将末端的浑浊部分褪色以完成透明化。而在HTML5的canvas中，anti- aliasing lines的绘制是由line API自动完成的。这意味着我们实际上需要从OpenGL版本中去掉一些代码。将OpenGL代码中的三角形顶点数组解约掉可以提供更好性能。</p>
<p>最 终，我们有几乎15，000行代码在浏览器中执行（它已经被最小化了，所以如果你在浏览器中查看源代码的时候，你会看到少得多的代码）。考虑到这么多代码 带来的<span style="color:#ff0000">复杂性</span>，Denis Morozov（ZeptoLab开发部门的总监，the Director of Development at ZeptoLab）在开始的时候问了一个问题：<span style="color:#ff0000">HTML5能给我们我们所需要的速度和性能吗？</span></p>
<p>为了回答这个问题，我们创建了一个早期的“性能”里程碑，在这里，我们集中精力去得到游戏运行时难度最高部分的最小版本。也就是说，我们想要看一下绳子看起来是什么样子的，以及我们是否能在浏览器中处理复杂的物理引擎。</p>
<h2></h2>
<h2><strong>Performance</strong><strong>性能</strong><strong></strong></h2>
<p>项 目开始以后三个星期，我们最终有了物理和绘图引擎的基本部分，以及一个简单的用于启动动画的计时器。现在，我们可以在游戏场景中呈现出一些绳索，一颗星 星，以及一个Om Nom sprite。不错的进步！第四周的时候，我们加入了一些基本的和鼠标的互动，这样，我们就能真正开始玩游戏了。我们在开发的过程中一直都在测试性能，并 且希望ZeptoLab的团队能够给我们一些反馈。</p>
<p>当我们把这些代码和ZeptoLab分享的时候，他们对这些 代码在浏览器中的性能表现感到惊喜（尤其是游戏的速度和平滑度）。说句实话，我们一直都提着一口气呢。我们希望Javascript能快点，因为物理计算 非常复杂，并且有实时性要求。这是一个很好的例子，证明了人们过去认为Javascript很慢的观点实际上是错的。<span style="color:#ff0000">最新的Javascript引擎是非 常快的。</span></p>
<p>在这个项目中，我们在IE9中预览了游戏。当你加载了游戏的时候，<span style="color:#ff0000">IE9的Chakra JavaScript引擎在一个后台线程中将代码进行了预编译——就像一个编译器编译Objective-C 或者 C++一样。然后，它实时将编译过的代码（字节码）发送给游戏线程去执行。执行速度几乎和本地执行速度一样。令人惊讶的是，这样的性能是来自于 Javascript引擎，我们不需要在代码中做任何特殊处理。</span></p>
<p> </p>
<div>
<dl>
<dt><img src="http://www.webapptrend.com/wp-content/uploads/2012/01/notes-framerates-thumb.jpg" alt="" width="220" height="220"></dt>
<dd>项目早期帧率检测结果（注意帧率上限是60FPS）</dd>
</dl>
</div>
<p>我们在Javascript上打的赌成功了，因此，我们将注意力转向了硬件和浏览器。由于IE的硬件加速堆栈以及我们在 <strong>Disney Tron</strong>和其他一些HTML5站点上积累的经验，我们对于游戏在测试机器上的完美表现毫不担心。我们很轻易地达到了上限60 FPS（帧/每秒）。但是，我们想要确认游戏在其他硬件和其他浏览器上也能表现得很好。下面是我们经过一些初步测试后所看到的结果。</p>
<p>根 据测试数据，我们将30 FPS（帧/每秒）设置为最小阈值。当浏览器速度低于这个阈值的时候，我们将会通知用户。他们仍然可以玩这个游戏，但是我们会通知他们在游戏中他们可能会 感觉到一些迟缓现象。这确保了这个游戏能支持不同硬件和软件，并且提供给玩家我们所能提供的最好体验。</p>
<p>我们想要指出两件事情。第一件事，这个游戏的现有版本在桌面PC机和Mac机上使用鼠标玩时效果是最好的。我们还没有加入对触屏输入的只支持，但在未来的版本中，我们会考虑这一点。</p>
<p>第二件事，现有的Chrome版本（version 16）有一些已经为大家所知的和媒体播放相关的问题， 使得Cut the Rope中的声音飘忽不定。我们进行了深入调研，试图用不同格式（包括WebM）重新编码媒体文件，但是没有找到一个合适的格式或者MIME配置或者其他 任何方案来有效解决这个问题。这个问题看起来是浏览器的bugs以及已经为大家所知的问题。更重要的是，尽管声音时断时续，游戏玩起来还是非常有意思的。 考虑到这一点，我们一方面可以说IE9的用户能免费得到一个很棒的应用，另一方面，Chrome以及一些Firefox用户可能会遇到一些声音上的问题， 但他们会注意到我们退回使用了一个flash插件来确保声效和音乐都能正常工作。</p>
<h2></h2>
<h2><strong>工具</strong><strong></strong></h2>
<p><span style="color:#ff0000">关于HTML5的一件很好的事情是你不需要学习一门新的语言来利用这项新技术的强大功能。如果你知道并且懂得Javascript，那么你就能实现一个现代浏览器所能实现的所有功能。你甚至可以创建一个像这个游戏一样的你自己的游戏！</span></p>
<p><strong>代码编辑器以及开发环境</strong><strong></strong></p>
<p> </p>
<div>
<dl>
<dt><img src="http://www.webapptrend.com/wp-content/uploads/2012/01/notes-visualweb-thumb.jpg" alt="" width="220" height="161"></dt>
<dd>Visual Web Developer 2010 Express可以免费下载使用，是一款很棒的编辑器，即使是对有经验的Web开发者来说也是如此。</dd>
</dl>
</div>
<p> </p>
<div>
<dl>
<dt><img src="http://www.webapptrend.com/wp-content/uploads/2012/01/notes-profiler-thumb.jpg" alt="" width="220" height="260"></dt>
<dd>分析器截图，图中内容是对Calc2PointBezier函数中花费的不合比例的时间（Calc2PointBezier函数是用来计算绳子每节的位置）。</dd>
</dl>
</div>
<p>有一些很好的免费工具，可以让我们更容易地使用Javascript和HTML5。我们的大多数开发工作都是在Visual Web Developer 2010（“快捷”版本可以在这里免 费获得）中完成的。这是一个非常健壮的Web编辑器，带有Javascript以及CSS自动完成功能。更好的一点是：它是免费的！我们在 Windows7的IE9上完成了我们的大多数测试，并且我们也时不时在Firefox、Chrome、Safari以及IE10的开发者尝鲜版。总体上 说，主流浏览器对于我们所使用的HTML5的特性都有比较一致的实现。在大多数情况下，我们在IE9上测试通过的特性在其他地方也运行得一样好。</p>
<p><strong>清查我们的资源加载器（</strong><strong>Resource Loader</strong><strong>）！</strong><strong></strong></p>
<p>Cut the Rope有一个非常独特的细节化的视觉风格——有很多图片、音频和视频，并且花费也很小。最终达到的效果就是这个游戏比一般的网站要大很多。综合起来说， 它大概有6MB（而一般的网站是200-300K）。这些多媒体资源要花费一段时间才能下载，而如果资源没有下载到位，我们看不到网页上的内容，我们是无 法开始游戏的。在一个典型的网页中，如果你缺掉了一两幅图，它仍然是可以运行的，但在HTML5的帆布API（drawImage）中，如果图像无法获取 的话，这一API就会崩溃。</p>
<p>为了解决这个问题，我们想要创建一个资源加载器，用来下载页面所需要的所有内容，并且当下载完成后，给我们一个好的反馈。这一点小代码能做很多很棒的事情：</p>
<p>1.它屏蔽了不同浏览器之间对下载处理的不同以及它们告知你进度的方式的不同。</p>
<p>2. 它能让你决定事物下载的顺序（你可能会想要先下载大文件，或者你想要在下载游戏图形之前先下载所有菜单图像）。</p>
<p>3.最终，它可以智能提醒你事物的到达，这样就可以通知用户进度情况，甚至可以开始部分游戏。</p>
<p>创 建这些类型的库是很难做好的。由于我们对于这些库的效果感到十分满意，因此我们想要分享我们的资源加载器的代码给你。最终的成果形式是PxLoader， 一个Javascript的资源加载器库，你可以使用它为HTML5应用、游戏、站点制作预加载器。它是开源免费的。你可以从页面顶端抓取它，或者点击这里。</p>
<p><strong>IE</strong><strong>中的性能工具</strong><strong></strong></p>
<p>另 外一个在开发过程中不可或缺的工具是IE9中的Javascript分析器（JavaScript Profiler）。分析器能让你发现你的代码中的热点以及瓶颈。在我们第一次做性能评估的时候，我们发现在一些机器上我们一直困在了20或者30帧/每 秒，这使得我们几乎要放弃了。</p>
<p>我们做了一些最初的代码检查，但是什么都没有检查出来。我们使用分析器加载了游 戏，发现我们在satisfyConstraints()函数上花了太多时间。这个函数是用来计算有关绳子的一些物理性质的数字。我们用来改写的 Objective-C版的实现是用递归实现的，递归每加深一层，就会传递一个新的对象。</p>
<p>通过Microsoft的一些指导，我们决定将递归函数替换成一个“解开”的循环版本。结果是惊人的。我们在每一个浏览器中都看到了10倍以上的性能提升。坦白说，如果没有IE9的分析器工具，我们永远都不可能发现这一点。、</p>
<p>本文转载自：<a href="http://www.webapptrend.com/">webapptrend</a></p>
<p>原文链接：<a href="http://origin.cuttherope.ie/dev/">http://origin.cuttherope.ie/dev/</a></p>
<p>译文链接：<a href="http://www.webapptrend.com/2012/01/1477.html">http://www.webapptrend.com/2012/01/1477.html</a></p>
<div style="float:left">
<div>
	<a></a>
	<a></a>
	<a></a>
	<a></a>
	<a></a>
	<a href="http://www.jiathis.com/share/"></a>
	<a></a>
</div>

</div>Dr.H For <a href="http://www.phonekr.com/cut-the-rope%e5%bc%80%e5%8f%91%e8%83%8c%e5%90%8e%e7%9a%84%e6%95%85%e4%ba%8b%ef%bc%88objective-c%e8%87%b3javascript%ef%bc%89.html">Cut the Rope开发背后的故事（Objective-C至javascript）</a>|2012年01月|<a href="http://www.phonekr.com/cut-the-rope%e5%bc%80%e5%8f%91%e8%83%8c%e5%90%8e%e7%9a%84%e6%95%85%e4%ba%8b%ef%bc%88objective-c%e8%87%b3javascript%ef%bc%89.html#comments">0 评论</a>|
分类:<a href="http://www.phonekr.com/play/skills" title="查看 技巧 中的全部文章" rel="category tag">技巧</a>
<hr>
<a href="http://feed.phonekr.com">Feed订阅</a>锋客网将遵循<a href="http://creativecommons.org/licenses/by-nc-sa/3.0/deed.zh">创作共有</a>版权协议,非商业性。转载©<a href="http://www.phonekr.com">锋客网|Phonekr</a>原创内容时请注明出自<a href="http://www.phonekr.com">锋客网|Phonekr</a>.
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">锋客推荐：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Veer简单的拆解.目的:看看Veer的电池是否可拆解!?" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwww.phonekr.com%2Fveer-simple-dismantling-objective-to-see-whether-the-battery-can-be-disassembled-veer.html&amp;from=http%3A%2F%2Fwww.phonekr.com%2Fcut-the-rope%25E5%25BC%2580%25E5%258F%2591%25E8%2583%258C%25E5%2590%258E%25E7%259A%2584%25E6%2595%2585%25E4%25BA%258B%25EF%25BC%2588objective-c%25E8%2587%25B3javascript%25EF%25BC%2589.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/05/11/8297191.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Veer简单的拆解.目的:看看Veer的电池是否可拆解!?</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="修复&quot;You must eject the device before using Photos&quot;问题" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwww.phonekr.com%2Ffix-u0026quotyou-must-eject-the-device-before-using-photosu0026quot-problem.html&amp;from=http%3A%2F%2Fwww.phonekr.com%2Fcut-the-rope%25E5%25BC%2580%25E5%258F%2591%25E8%2583%258C%25E5%2590%258E%25E7%259A%2584%25E6%2595%2585%25E4%25BA%258B%25EF%25BC%2588objective-c%25E8%2587%25B3javascript%25EF%25BC%2589.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/07/08/16627285.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">修复&quot;You must eject the device before using Photos&quot;问题</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Enyo开发的应用证实确实可以和webOS 1.4.5兼容" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwww.phonekr.com%2Fenyo-confirmed-the-development-of-the-application-can-webos-1-4-5-compatible.html&amp;from=http%3A%2F%2Fwww.phonekr.com%2Fcut-the-rope%25E5%25BC%2580%25E5%258F%2591%25E8%2583%258C%25E5%2590%258E%25E7%259A%2584%25E6%2595%2585%25E4%25BA%258B%25EF%25BC%2588objective-c%25E8%2587%25B3javascript%25EF%25BC%2589.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/04/22/6196704.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Enyo开发的应用证实确实可以和webOS 1.4.5兼容</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="修复”You must eject the device before using Photos”问题" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwww.phonekr.com%2Fplay%2Ffix-u0026quotyou-must-eject-the-device-before-using-photosu0026quot-problem.html&amp;from=http%3A%2F%2Fwww.phonekr.com%2Fcut-the-rope%25E5%25BC%2580%25E5%258F%2591%25E8%2583%258C%25E5%2590%258E%25E7%259A%2584%25E6%2595%2585%25E4%25BA%258B%25EF%25BC%2588objective-c%25E8%2587%25B3javascript%25EF%25BC%2589.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/07/08/16627285.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">修复”You must eject the device before using Photos”问题</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="软件开发商wingedchariot宣布将开发webOS版本儿童读物" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwww.phonekr.com%2Fsoftware-developers%25E3%2580%2591-%25E3%2580%2590flying-wagon-version-announced-the-development-of-childrenu002639s-books-webos.html&amp;from=http%3A%2F%2Fwww.phonekr.com%2Fcut-the-rope%25E5%25BC%2580%25E5%258F%2591%25E8%2583%258C%25E5%2590%258E%25E7%259A%2584%25E6%2595%2585%25E4%25BA%258B%25EF%25BC%2588objective-c%25E8%2587%25B3javascript%25EF%25BC%2589.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/04/12/5498492.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">软件开发商wingedchariot宣布将开发webOS版本儿童读物</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="5" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table>
<p><small>Feed enhanced by <a href="http://planetozh.com/blog/my-projects/wordpress-plugin-better-feed-rss/">Better Feed</a> from  <a href="http://planetozh.com/blog/">Ozh</a></small></p><img src="http://www1.feedsky.com/t1/597091175/Phonekr/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/Phonekr/~8620091/597091175/6726073/1/item.html" border="0" height="0" width="0">