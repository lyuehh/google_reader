---
layout: post
title:  "Stage3D vs WebGL 性能较量 （补充了评论的翻译）"
date:   2011-10-28 19:55:52
author: admin
categories: program
---

## Stage3D vs WebGL 性能较量 （补充了评论的翻译）
### by admin
### at 2011-10-28 19:55:52
### original <http://www.hiwebgl.com/?p=535>

<p>你想在浏览器里开发一些疯狂的次时代3D图形应用吗？目前有两个好的可用选项：Flash 11的Stage3D和JavaScript原生的WebGL。</p>
<p><a href="http://www.adobe.com/devnet/flashplayer/stage3d.html">Stage3D</a>是Flash的API，支持GPU硬件加速的3D图形。</p>
<p><a href="http://en.wikipedia.org/wiki/WebGL">WebGL</a>是一项开放的Javascript 3D图形标准，直接嵌入到浏览器中。</p>
<div><p>目录</p><ul><li><a href="http://www.hiwebgl.com/#Demo">1 Demo和源代码</a></li><li><a href="http://www.hiwebgl.com/#Demo-2">2 搭建Demo</a></li><li><a href="http://www.hiwebgl.com/#i">3 代码比较</a></li><li><a href="http://www.hiwebgl.com/#i-2">4 支持平台</a></li><li><a href="http://www.hiwebgl.com/#i-3">5 性能比较</a></li><li><a href="http://www.hiwebgl.com/#i-4">6 总结</a></li><li><a href="http://www.hiwebgl.com/#i-5">7 补充评论翻译</a></li></ul></div>
<h1><span>Demo和源代码</span></h1>
<p>为了比较两者的性能，我搭建了一双简单的Demo，其中展示了100个半透明的立方体，还有额外的混合，以及两个点光源。显然这个demo不能全面的比较两者，但是从两个相同的3D场景中可以看出两者的性能。</p>
<p><a href="http://airtightinteractive.com/demos/flash/stage3d/cubes01/">Stage3D Demo</a>  |   <a href="http://airtightinteractive.com/demos/flash/stage3d/cubes01/cubes01_stage3d.zip">源代码</a></p>
<p><a href="http://airtightinteractive.com/demos/js/cubes01/">WebGL Demo</a>    |   <a href="http://airtightinteractive.com/demos/js/cubes01/cubes01_webgl.zip">源代码</a></p>
<p>如果你运行时的帧率很低，试着缩小浏览器窗口。对于Stage3D的Demo，查看debug面板中的DRIV项是不是显示“OpenGL”或“Software”。</p>
<h1><span>搭建Demo</span></h1>
<p>我在Stage3D的Demo中使用了Away3D 4框架， 在WebGL的Demo中使用了Three.js框架。</p>
<p><a href="http://away3d.com/comments/away3d-4-0-alpha-release-broomstick">Away3D 4</a>是最流行的Flash 3D引擎的最新版本，基于Stage3D构建。Away3D有一个不错的<a href="http://away3d.com/forum">线上社区</a>，并且一系列的<a href="http://away3d.com/livedocs/away3d-core-fp11/">API文档</a>都维护的很好。Away3D 4目前还处于alpha版本，一些特性还缺失，比如不支持粒子系统，而且滤镜和着色器的支持也很有限。你可以用<a href="http://www.adobe.com/devnet/flashplayer/articles/creating-games-away3d.html">这个教程</a>来开始学习Away3D 4。</p>
<p><a href="https://github.com/mrdoob/three.js/blob/master/README.md">Three.js</a>是一个开源WebGL 3D引擎，有大量积极的开发者在持续不断的为其增加新的特性。网上也有<a href="http://fhtr.org/BasicsOfThreeJS/#1">大量的教程</a>来学习Three.js。</p>
<h1><span>代码比较</span></h1>
<p>Away3D和Three.js的语法以及模型逻辑都非常相似。比如创建一个立方体，在Away3D中你可以这么写：</p>
<pre>var cube:Cube = new Cube(material, 100,100,100);

view.scene.addChild(cube);</pre>
<p>而在Three.js中，你可以写成：</p>
<pre>var geometry = new THREE.CubeGeometry(100, 100, 100);

var cube = new THREE.Mesh(geometry, material);

scene.addObject(cube);</pre>
<h1><span>支持平台</span></h1>
<p>为了实现硬件加速，Stage3D需要Flash 11和最新的显卡。如果Stage3D不能使用硬件渲染，将会回滚使用软件渲染，这会慢5到10倍。</p>
<p>WebGL不需要插件就可以实现硬件加速，支持Chrome、Firefox、Safari和Opera浏览器（但是不支持IE，为微软欢呼吧！）。在Windows系统上的Chrome中，WebGL使用<a href="http://blog.chromium.org/2010/03/introducing-angle-project.html">ANGLE</a>将WebGL转换为DirectX，这提供了很好的性能。</p>
<p>Stage3D在Mac上使用OpenGL，在Windows上使用DirectX。它对显卡的要求非常严格。你可以在<a href="http://kb2.adobe.com/cps/921/cpsid_92103.html">这里</a>找到不支持Stage3D的GPU列表。你可以<a href="http://www.mcfunkypants.com/2011/flash11-stage3d-tutorial-handling-init-errors/">手动查询</a>是否处于软件渲染模式，然后做出对应的解决措施。</p>
<p>Stage3D一个很大的优点就是可以在IE上运行，尽管在我的体验中，因为软件渲染的缘故，它慢的不能再慢了。在Mac上，Stage3D和WebGL都会转换为OpenGL运行，所以性能上没有什么太大的区别。</p>
<p>现在两者都不能运行于移动设备上（iOS / Android），但是在未来两者都值得期待。</p>
<h1><span>性能比较</span></h1>
<p>我在MacBook Pro（显卡是NVIDIA GeForce GT 330M）和一个中等配置的安装了Windows 7的Dell笔记本（显卡是Intel集成显卡）上测试了这两个Demo。在你机器上的性能将会有所不同。</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="189"></td>
<td valign="top" width="189">Stage3D</td>
<td valign="top" width="189">WebGL</td>
</tr>
<tr>
<td valign="top" width="189">Mac / Chrome</td>
<td valign="top" width="189">60 FPS (OpenGL)</td>
<td valign="top" width="189">60 FPS</td>
</tr>
<tr>
<td valign="top" width="189">Mac / Firefox</td>
<td valign="top" width="189">60 FPS (OpenGL)</td>
<td valign="top" width="189">60 FPS</td>
</tr>
<tr>
<td valign="top" width="189">Mac / Safari</td>
<td valign="top" width="189">60 FPS (OpenGL)</td>
<td valign="top" width="189">60 FPS</td>
</tr>
<tr>
<td valign="top" width="189">Windows / Chrome</td>
<td valign="top" width="189">8 FPS (Software)</td>
<td valign="top" width="189">50 FPS</td>
</tr>
<tr>
<td valign="top" width="189">Windows / Firefox</td>
<td valign="top" width="189">8 FPS (Software)</td>
<td valign="top" width="189">无法运行</td>
</tr>
<tr>
<td valign="top" width="189">Windows / IE</td>
<td valign="top" width="189">12 FPS (Software)</td>
<td valign="top" width="189">无法运行</td>
</tr>
</tbody>
</table>
<p>总的来看，Stage3D的Demo初始化的更慢一些，帧率也比WebGL的Demo要低一些。看来Stage3D能提供更有趣的颜色混合和高光。</p>
<p><strong>更新——</strong>很多评论说都可以在Windows下Stage3D跑到60FPS。以上的数字不代表大众范围的机器配置，是指我在我的机器上的测试结果。我的PC笔记本用的是集成GPU，因此不支持Stage3D的硬件渲染模式。这是一个中等配置的笔记本，我买了还不到6个月，因此也许可以代表大众的机器。</p>
<h1><span>总结</span></h1>
<p>Stage3D和WebGL都是在浏览器中创建交互3D内容的优秀技术。Away3D和Three.js提供了令人意料之外如此相似的API。</p>
<p>在Mac上，Stage3D和WebGL的性能都很不错。在Windows上，Stage3D的性能很可怜；而WebGL则在Chrome中运行的非常不错，但是其他浏览器都无法运行。</p>
<p>Stage3D的软件渲染模式是一个不错的注意，它会在不被支持的硬件上显示3D内容。问题在于性能实在是太慢了。与其在性能这么差的情况下去展示什么东西，是不是可以直接考虑用其他的非3D的方式展示呢？</p>
<p>因为Chrome在Mac和Windows上都是免费的，所以你可以考虑一下使用WebGL来获得更好的性能以及更多的用户。也许我们会开始见到越来越多的提示写着“请使用Chrome访问本站”。</p>
<p> </p>
<p>HiWebGL翻译整理，转载请注明出处。</p>
<p>原文地址：<a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/">http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/</a></p>
<p> </p>
<h1><span>补充评论翻译</span></h1>
<div>
<p align="left"><strong>Philippe</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6081">October 28, 2011 at 4:22 am</a></p>
</div>
<p align="left">我在Win上所有的浏览器里Stage3D和WebGL都能跑到固定的60FPS，全屏1680×1050。这玩意儿很依赖显卡和显卡驱动。</p>
<div>
<p align="left"><strong>Ricardo Sanchez</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6082">October 28, 2011 at 4:36 am</a></p>
</div>
<p align="left">很有趣的文章，我想知道作者你做这个性能测试用的Mac和Windows机器的显卡</p>
<p align="left"><strong><a href="http://www.airtightinteractive.com/">Felix Turner</a></strong> <em>says:</em></p>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6101">October 28, 2011 at 12:13 pm</a></p>
<p align="left">我加上，显卡信息了。</p>
<div>
<p align="left"><strong><a href="http://mrdoob.com/">Mr.doob</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6083">October 28, 2011 at 4:48 am</a></p>
</div>
<p align="left">你用不同配置的机器来测试是很没有道理的！这是我在我的MacMini上的测试结果，显卡是Nvidia GeForce 9400M。</p>
<p align="left">Linux / Chrome – Stage3D: 4FPS (Software) WebGL: 60FPS<br>
Linux / Firefox – Stage3D: 3FPS (Software) WebGL: 30FPS</p>
<div>
<p align="left"><strong><a href="http://mrdoob.com/">Mr.doob</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6084">October 28, 2011 at 4:50 am</a></p>
</div>
<p align="left">另外，“scene.addObject(cube)”已经被弃用了，现在应该写成“scene.add(cube)” ^^</p>
<div>
<p align="left"><strong><a href="http://www.derschmale.com/">David Lenaerts</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6085">October 28, 2011 at 4:59 am</a></p>
</div>
<p align="left">呵呵，一个没有系统配置的评测？ 看起来你Windows上的DirectX驱动好像有问题（或者你的显卡被列入黑名单了）从传统角度看，Stage3D在Windows上运行的效果应该比在Mac上好。</p>
<p align="left">而且，你可以优化一下代码来获得更多速度，比如创建新的网格对象而不是立方体对象（立方体对象并不自动共享同一个集合体实例）。</p>
<p align="left">// once<br>
var referenceCube:Cube = new Cube(material, 100,100,100);</p>
<p align="left">// repeatedly<br>
var mesh:Mesh = new Mesh(material, referenceCube.geometry);<br>
view.scene.addChild(mesh);</p>
<p align="left">Anyway, this was for backwards compatibility with 3.6. Primitive creation is slated for heavy refactoring before beta so wasting geometry instances isn’t possible anymore</p>
<div>
<p align="left"><strong><a href="http://mrdoob.com/">Mr.doob</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6087">October 28, 2011 at 5:23 am</a></p>
</div>
<p align="left">天啊！Android版Firefox不支持WwebGL。（实际上Android版Firefox是部分支持WebGL的，此处为反讽。）虽然我没怎么测试。我觉得移动设备上的着色器需要一些微调才行（比如精确度）。</p>
<p align="left"><strong><a href="http://www.airtightinteractive.com/">Felix Turner</a></strong> <em>says:</em></p>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6102">October 28, 2011 at 12:33 pm</a></p>
<p align="left">不知道为什么我的Firefox在Windows上不能跑WebGL。</p>
<div>
<p align="left"><strong>Bart</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6088">October 28, 2011 at 5:24 am</a></p>
</div>
<p align="left">我觉得Stage3D的软件渲染模式并不是硬件3D加速的回滚，而是为了代替旧的Flash 3D引擎（Away3D 3、PaperVision等等），它拥有完整API（使用框架）并有清晰的途径获得高速硬件支持。在支持硬件范围这么广的情况下，软件渲染模式下相应的性能调整可以引申去做很多有用的事情。</p>
<p align="left">而且它在每台电脑上、每个浏览器中都可以工作，只要这个有自动更新的插件就行（对比一下，安装一个全新的浏览器）。也许运行的不好，但是至少可行。</p>
<p align="left">（和我们这些技术人员比起来），真正的用户：我在我老爹老娘的电脑上也可以看一些相当不错的3D效果（尽管他们根本不知道什么新版本什么乱七八糟的）。</p>
<div>
<p align="left"><strong>LightMan</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6089">October 28, 2011 at 5:27 am</a></p>
</div>
<p align="left">我在我的iMac上测试了，配置是Lion, 3.06GHz core 2 duo, 4GB RAM, NVIDIA GeForce 9400 256 MB，窗口最大化，分辨率是1920×1080，使用了最新版的Flash Player 11</p>
<p align="left">Firefox:<br>
WebGL运行的非常流畅，53 FPS，54% CPU使用率 （平均值）<br>
Stage3D运行的非常糟糕，FPS计数器都不听使唤啦！我猜应该是8FPS但是很卡。CPU占用率14-17%，Firefox失去响应，很长时间后才关闭了这个标签页。</p>
<p align="left">Chrome:<br>
WebGL运行的非常流畅，30 FPS，30% CPU占用率 (平均值)<br>
Stage3D再一次运行的非常坑爹，FPS计数器又不听使唤了。我脑补了一下应该是23FPS，而且很卡，CPU占用率14-17%，Chrome完全失去响应，还有iMac也是，我只好选择了强制退出。</p>
<p align="left">Safari:<br>
开启WebGL后，它运行的非常流畅，就像在Chrome里一样，30 FPS，30% CPU占用率 (平均值)<br>
Stage3D，还是和Chrome和Firefox一样。看起来Flash引擎应该有什么问题，或者是用的软件渲染，我不知道，但是它上面明明写着Driver: OpenGL…</p>
<p align="left">所以结论很清楚，至少在Mac上，WebGL要比Flash好！</p>
<div>
<p align="left"><strong>Corey</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6090">October 28, 2011 at 5:49 am</a></p>
</div>
<p align="left">“在Windows上，Stage3D的性能很可怜”？？你在搞笑吧？你只说了软件模式下的帧数，那硬件加速下的帧数跑哪里去了？</p>
<p align="left"><strong><a href="http://www.airtightinteractive.com/">Felix Turner</a></strong> <em>says:</em></p>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6104">October 28, 2011 at 12:36 pm</a></p>
<p align="left">这已经足够公平了！就像我说的，“你机器上的性能可能会发生变化。我文章里也改了说法了：“在Windows上，如果你有支持的硬件，Stage3D的性能还不错，否则就很可怜。”</p>
<div>
<p align="left"><strong><a href="http://www.lab4games.net/zz85/blog">zz85</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6091">October 28, 2011 at 6:48 am</a></p>
</div>
<p align="left">我很好奇为什么你Windows的Firefox不能跑WebGL呢？</p>
<p align="left">在我的windows 7 (32bit) 桌面电脑 (intel E8400) ，集成显卡(intel g41)，FULL HD分辨率。</p>
<p align="left">Chrome:<br>
stage3d   15 fps<br>
webgl   30fps<br>
FF:<br>
stage3d: 15fps<br>
webgl: 35fps</p>
<p align="left">在我的macbook air上，分辨率是1440×900,<br>
FF:<br>
都跑到了 60fps</p>
<p align="left">Chrome:<br>
Stage 3d: 60fps<br>
Webgl: 35fps (很奇怪，我把窗口宽度缩小100像素，就能跑到60fps了)</p>
<div>
<p align="left"><strong><a href="http://blog.fathah.net/">Fathah Noor</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6092">October 28, 2011 at 6:51 am</a></p>
</div>
<p align="left">一下是我在我电脑上测试的结果</p>
<p align="left">————————————————————–<br>
操作系统: Microsoft Windows 7 Ultimate<br>
CPU: DualCore Intel Core i3 550, 3200 MHz (24 x 133)<br>
内存: 3959 MB (DDR3-1333 DDR3 SDRAM)<br>
显卡: NVIDIA GeForce GTS 450 (1024 MB)<br>
————————————————————–<br>
Windows / Chrome: Stage3D – 12 FPS (Software) – 62 FPS (DirectX9) | WebGL – 61 FPS<br>
Windows / Firefox: Stage3D – 12 FPS (Software) – 62 FPS (DirectX9) | WebGL – 62 FPS<br>
Windows / IE: Stage3D – 16 FPS (Software) – 62 FPS (DirectX9) | WebGL – 不能运行<br>
Windows / Opera: Stage3D – 24 FPS (Software) – 62 FPS (DirectX9) | WebGL – 不能运行</p>
<p align="left"><strong>Andrei</strong> <em>says:</em></p>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6103">October 28, 2011 at 12:35 pm</a></p>
<p align="left">Opera不能运行是因为它用的语法和Javascript的语法稍有不同，有点像ECMAScript的语法。</p>
<div>
<p align="left"><strong>daniel</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6093">October 28, 2011 at 7:18 am</a></p>
</div>
<p align="left">“在Windows上，Stage3D的性能很可怜”这简直是大错特错！我在Windows上的所有浏览器里，Stage3D都能跑到60FPS。另外，WebGL的成绩才相当可怜，只有不到35FPS。</p>
<p align="left">测试机：Windows XP 64-bit sp3, GF 7600GS, FF7,IE8,Opera11,Chrome14</p>
<div>
<p align="left"><strong>CS</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6095">October 28, 2011 at 8:23 am</a></p>
</div>
<p align="left">当你在Windows上测试Stage3D的时候应当配置好GPU，不要用软件渲染模式（它是给配置很差没有GPU的电脑用的，比如集成显卡）。Flash在这里之所以表现的这么差都是因为你的破电脑和烂配置导致的。牛逼的Flash在Windows上通常都会跑到60FPS！</p>
<div>
<p align="left"><strong>nystep</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6096">October 28, 2011 at 8:28 am</a></p>
</div>
<p align="left">这是一个很有趣的测试，谢谢！</p>
<p align="left">你试过在Windows上的Firefox中开启WebGL了吗？还是一直无法运行？</p>
<p align="left">这儿有一个如何在Firefox中开启WebGL支持的教程：</p>
<p align="left"><a href="http://support.mozilla.com/fr/questions/720087">http://support.mozilla.com/fr/questions/720087</a></p>
<p align="left">你应该在“about:config”页面中开启“webgl.prefer-native-g”选项，这样就会默认用direct3d运行WebGL了。再次感谢。</p>
<div>
<p align="left"><strong>Harry</strong><strong> </strong><em>says:</em><strong></strong></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6097">October 28, 2011 at 9:04 am</a></p>
</div>
<p align="left">“在Windows上，Stage3D的性能很可怜”？！我的Windows上，在Chrome、Firefox、IE上都能跑到60FPS！你肯定是什么地方搞错了……</p>
<div>
<p align="left"><strong>Mike K</strong><strong> </strong><em>says:</em><strong></strong></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6099">October 28, 2011 at 10:41 am</a></p>
</div>
<p align="left">很有趣的对比！如果再来测试下使用不同的AS3/JS库，结果是否相似就好了。下面是我的测试结果：</p>
<p align="left">硬件: Dell OptiPlex 745, Intel Core2 CPU 6400 2.13 GHz (2 CPUs), 4 GB RAM<br>
操作系统: Windows 7 Enterprise SP1, 32 bit</p>
<p align="left">所有测试都是在小窗口（800×480像素）和全屏（1440×900像素）下测试的</p>
<p align="left">Stage 3D: DirectX9<br>
IE 9: 58 小窗口, 24 全屏<br>
Chrome 15: 58小窗口, 25全屏<br>
Firefox 7: 59小窗口, 27全屏</p>
<p align="left">Safari 5: 60小窗口, 25全屏<br>
Opera 11.52: 58小窗口, 24全屏</p>
<p align="left">WebGL:<br>
IE 9: “你的浏览器不支持WebGL”<br>
Chrome 15: 43小窗口, 32 全屏<br>
Firefox 7: “你的浏览器不支持WebGL”<br>
Safari 5.1.1: “你的浏览器不支持WebGL”<br>
Opera 11.52: “你的浏览器不支持WebGL”</p>
<div>
<p align="left"><strong>Eduardo</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6100">October 28, 2011 at 11:56 am</a></p>
</div>
<p align="left">大哥们，这里都是Apple的五毛吗？哈哈，开个玩笑……真的。</p>
<p align="left">如果你们说的是移动领域，那么说WebGL比Stage3D性能好也许是对的。但是在桌面和笔记本上，没有显卡就不能渲染3D内容啦？我的笔记本是intel 3000集成显卡，太TM丢人了，还有i5处理器。WebGL还没有Stage3D跑的好。请发一下硬件加速下的两者的性能测试！而且是在集成浏览器里！这非常关键！因为一半以上的实际用户不会使用纯Chrome和Firefox。</p>
<div>
<p align="left"><strong>Daniel N</strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6105">October 28, 2011 at 1:01 pm</a></p>
</div>
<p align="left">在我的Mac上很有趣的是，它们都能在Chrome中跑到60FPS，但是如果你同时运行这两个测试，Stage3D会抢走WebGL的资源，Stage3D还是60FPS，但是WebGL就下降到了12FPS。我猜（也许是错的），Stage3D用某周手段获取到了显卡端的更高的优先级。</p>
<div>
<p align="left"><strong><a href="http://www.tom-carden.co.uk/">Tom Carden</a></strong> <em>says:</em></p>
</div>
<div>
<p align="left"><a href="http://www.airtightinteractive.com/2011/10/stage3d-vs-webgl-performance/#comment-6108">October 28, 2011 at 3:02 pm</a></p>
</div>
<p align="left">如果你一直只能跑在软件渲染模式下，那你真得好好检查一下你的Flash的设置了。右键单击，选择“设置”，最左边的选项卡里选中“开启硬件加速”。我在几个版本之前关闭了这个选项，所以对最近稳定版的Flash的性能有所影响。</p>
<p> </p>