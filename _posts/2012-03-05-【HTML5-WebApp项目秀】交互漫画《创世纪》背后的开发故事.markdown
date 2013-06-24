---
layout: post
title:  "【HTML5 WebApp项目秀】交互漫画《创世纪》背后的开发故事"
date:   2012-03-05 14:34:03
author: ling
categories: program
---

## 【HTML5 WebApp项目秀】交互漫画《创世纪》背后的开发故事
### by ling
### at 2012-03-05 14:34:03
### original <http://www.webapptrend.com/2012/03/2034.html>

<p>导读：Disney 《创世纪》是一个交互式在线漫画，探索了使用HTML5以及现代Web技术讲述故事的新方式。它采用了HTML5 Canvas以及audio tags，将纸面上的故事更为鲜活。这篇文章揭示了震撼的HTML5体验网站Disney TRON: Legacy Digital Book Site背后的故事，制作团队为何放弃CSS3，转投canvas技术，如何在不影响HD图片质量同时降低带宽，又是如何解决浏览器间时间粒度问题的。</p>
<p>感谢Disney 和 Vectorform的精诚合作，只花了大概一个月时间就开发出新的<a href="http://disneydigitalbooks.go.com/tron/">Disney TRON: Legacy Digital Book Site</a>，这是一个构建于用IE9硬件加速的HTML5之上的一个应用。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/1.png"><img title="1" src="http://www.webapptrend.com/wp-content/uploads/2012/03/1.png" alt="" width="514" height="292"></a></p>
<p><span></span>在这篇文章中我想与大家分享一下这个项目的一些“幕后故事”，尤其是我们在这个项目中受到的教训和学到的一些经验。我要特别感谢来自于<a href="http://www.vectorform.com/">Vectorform</a>的创意总监（Creative Director）Ken Disbennett以及首席开发人员（Lead Developer）Alex Barkan能与我们分享他们关于项目的经验和想法。</p>
<h4>把漫画从纸面上搬到web上来（Ken）</h4>
<p>一切都要从漫画书开始。项目的目标是利用HTML5的能力来更好地展现漫画，而又不会失去传统漫画应用的原滋原味。我们想要web上呈现的漫画中的每一幅图都鲜活生动，就像它们在打印版中一样，让人们有一种逐步穿越故事的感觉。因此，我们确定将这个应用开发成一个线性的、时间轴类型的应用是最合适的。每一幅图都做了特殊处理，以强调画中的动作。应用保持了从左到右的阅读方式，保持了传统漫画阅读的感觉，没有页面翻页带来的中断。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/2.png"><img title="2" src="http://www.webapptrend.com/wp-content/uploads/2012/03/2.png" alt="" width="514" height="297"></a></p>
<p>构建这个应用的原材料是一些高分辨率的Photoshop源文件，这些材料以书页或者章节的形式组织起来。大概花了5天时间小心将每一幅图中的人物和必要的元素从每幅图的背景中中剥离开来。我们给场景绘制了底色以保证一种无缝观感。然后每一幅图以顺序方式重组。最后每个元素都被分隔开来并导出到不同状态来生成最后的动画。</p>
<h4>选择最佳的底层技术（Alex）</h4>
<p>我们大概花了5天时间去构建几个原型系统来看哪个技术是最适于应用于这个项目并能在不同浏览器上都能提供最好的结果。</p>
<p>最初的想法是使用CSS3（特别是 <a href="http://msdn.microsoft.com/en-us/ie/ff468705.aspx#_CSS3_2D_Transforms">CSS3 2D Transforms</a>）。我们使用CSS3建立了几个测试来模拟这个项目中需要的交互，其中最难的地方在于既保持交互性，又不影响性能的情况。我们试了好几种方法，比如使用纯Javascript、jQuery animation、应用绘图、DOM以及CSS优化，但是，所有这些方法，都不能在各浏览器中达到我们所需要的60帧/秒（FPS）。</p>
<p>由于不满意CSS在这个应用中的性能表现，我们转而使用<a href="http://msdn.microsoft.com/en-us/ie/ff468705.aspx#_HTML5_canvas">HTML5 &lt;canvas&gt;</a>。从我们之前的项目开始（<a href="http://www.foursquareplayground.com/">Foursquare HTML5 Playground</a>），我们就建立起一个新的原型利用HTML5来处理浏览器性能问题，这个原型表现平稳，在低配置的浏览器中也能达到60FPS；我们可以只使用基本的viewport-clipping功能就处理上万的漫画设置以及提供数以百计的32-bit RGBA PNG。使用HTML5的灵感来源于我们使用具有图像分割能力的<a href="http://dev.w3.org/html5/2dcontext/#dom-context-2d-drawimage">drawImage()</a> canvas方法（下面将有更详细的介绍）来添加animated sprites时。我们添加了好几百个sprite character，并经过了大量测试，经历了各种漫画设置。</p>
<p>很显然，IE9中的HTML5 &lt;canvas&gt;改变了这个游戏（其他浏览器中也有所改变）！</p>
<h4>在降低带宽的同时又不降低质量（Alex）</h4>
<p>带宽从一开始就是一个亟待考虑的问题。我们希望得到高清图像：很多的像素点、高比特率、平滑的透明度。为了能达到视差效果，我们需要叠加好几个层，否则图像看起来不会比GIF格式的图像好。PNG是显然的选择，尽管它占用更多带宽。幸运地是，John Einselen (Vectorform 的艺术总监)提出了一个易于使用的工具<a href="http://www.libpng.org/pub/png/apps/pngquant.html">pngquant</a>。所有的浏览器现在都支持<a href="http://en.wikipedia.org/wiki/Portable_Network_Graphics">PNG/8</a>，这是PNG的一个少见的变种，RGB和Alpha值可以在同一个8-bit channel中存储，这可以让我们使用好几个bits的alpha值来实现更平滑的alpha-blending，又可以让图片文件大小减少一半！我们需要通过实验来找到这种量化在何处是能使用的，又不会给图像带来太多失真。</p>
<p>One trick we learned was a split-compromise between 8-bit RGBA and 32-bit RGBA. You bake an image as two separate layers: base texture and glows. Then compress both as 8-bit RGBA. This gives a lot of bits for smooth glows (think lamp posts in fog) but cuts out 16-bit worth of data per RGB triplet. The result is lower total file size than a single 32-bit RGBA png, and higher quality than a single 8-bit RGBA! Here’s the car and its glow from the first screen.</p>
<p>我们使用的一种方法是在8-bit RGBA 和32-bit RGBA做折中。你可以将一幅图看做两个单独的层：基本的文本线条（base texture） 和光照（glows）。然后将这两个层都压缩成8-bit RGBA。这带来了平滑的光照（想象雾中的灯光），但是却在每个RGB层减少了16bit的数据。最后处理过的图像比单独的32bit的RGBA png要小，但比8-bit RGBA有更高的图像质量。下面的图就是处理以后的车以及光照的效果图。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/3.png"><img title="3" src="http://www.webapptrend.com/wp-content/uploads/2012/03/3.png" alt="" width="295" height="287"></a></p>
<p>我们需要处理的图片有好几层，并且都使用了不同的混合模式，还有一些需要填补缺失的背景。我们的设计师，Ken以及John，需要将提供的图片在Photoshop中转换成常用的混合模式（normal-blending modes）才能让它们在web浏览器中正常显示。他们还填补了必要的背景，并且充分利用了可用的像素点，之后我们对这些像素点进行了重采样，以便让画面更清爽。我们需要一个固定大小的目标屏幕以便让动画有更好的视觉效果。在项目的整个过程中，这一直都是我们需要解决的核心问题，即无论浏览器大小或者鼠标灵敏度如何，都需要有平滑的动画，并可以通过背景图像预加载器加载。</p>
<h4>同步与反应速度问题（Alex）</h4>
<p>所有技术都会遇到的一个问题是同步。这个问题的复杂之处在于不同浏览器有着不同的timer resolution ，会有或大或小的边缘误差（a bigger or smaller margin of error）。因此，在向屏幕上绘图的时候，可以同时向帧缓存（framebuffer）中绘图。为了防止视觉误差，我们需要对setTimeout() draw callback进行微调并与60hz的重绘进行匹配。总体上说，我期待这个会容易些。我期待能在W3C中看到关于<a href="http://lists.w3.org/Archives/Public/public-web-perf/2011May/0015.html">requestAnimationFrame</a>的讨论的变革。</p>
<p>最后要说的是，IE9的硬件加速给了我们很深的印象：Canvas 2D提供的实际的图像表现优异，在其他浏览器上它也提供了很好的结果，尽管在某些低端机上有<a href="http://blogs.msdn.com/b/ie/archive/2011/04/26/understanding-differences-in-hardware-acceleration-through-paintball.aspx">一些瑕疵</a>。</p>
<h4>使用Developer Tools管理代码</h4>
<p>为了保持应用的灵活性，并且让其更容易维护——整个playground被划分成13个不同“页面”（和原书中保持一致）。页面在应用启动时就预先读入缓存并在水平轴上平铺开。每个页面都定义了它自己的展现方式和交互逻辑，这些都是相对于X坐标而言的（在启动时配置好的）。</p>
<p>在这个应用中，只有一个drawing循环（experience.js中的<em>Draw()</em>）来绘出所有屏幕上的可见画面（以及调试信息）。</p>
<p>建议：在你查看代码的时候，你可以在 <a href="http://msdn.microsoft.com/en-us/ie/aa740478">Developer Tools</a>中按F12来启用“Format JavaScript”，从而使Javascript更具有可读性。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/4.png"><img title="4" src="http://www.webapptrend.com/wp-content/uploads/2012/03/4.png" alt="" width="566" height="178"></a></p>
<h4>Debug Mode调试模式</h4>
<p>很多页面都包含有调试信息，这些信息在开发过程中是很有用的。特别的一点是，当你在你的键盘上按“d”的时候，你可以显示性能帧计数器。如果你按F12以及Console tab的话会有额外的信息发送到Console中。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/5.png"><img title="5" src="http://www.webapptrend.com/wp-content/uploads/2012/03/5.png" alt="" width="451" height="312"></a></p>
<h4>图像预加载</h4>
<p>在第一个页面加载的时候，每个页面都会通过experience.js的preloader class同步请求所需资源，preloader用于下载并将资源排序。</p>
<p>在加载阶段，页面上会有一个漂亮的旋转圈出现，它是通过sprite animation技术实现的。这个动画是在图像中定义的，是一个有限步的循环旋转的圆环（为了显示效果起见，它的旋转角度是小于90度的）。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/6.png"><img title="6" src="http://www.webapptrend.com/wp-content/uploads/2012/03/6.png" alt="" width="509" height="15"></a></p>
<p>当我们在Canvas中画这个图像的时候，我们改变绘图窗口，每次只在窗口中显示一个状态，并根据需要增加图片偏移。</p>
<pre>// draw loading indicator</pre>
<pre>var size = 128;</pre>
<pre>var frameNumber = Math.round(30 * mLoadingProgressSmooth);</pre>
<pre>var frameOffsetY = frameNumber * 128;</pre>
<pre>surface.drawImage(Preloader.SpinnerLayer1, 0, frameOffsetY, 128, 128, x - 64, y - 64, 128, 128);</pre>
<p>我们如何知道这个图像在其他一些资源之前先准备好呢？很简单。我们在下载其他资源之前先下载它，并尽力让它保持很小的规模。</p>
<h4>保持原始字体</h4>
<p>在这个应用中，保持和动漫书原作中的一样的字体是很关键的部分。在研究过WOFF格式以后，我们决定使用 <a href="http://www.fontsquirrel.com/">FontSquirrel.com</a>来将WOFF字体打包（使用其他格式作为后备），并生成我们所需要的@font-face代码片段。</p>
<pre>@font-face</pre>
<pre>{</pre>
<pre>    font-family: 'BadaBoomBBRegular';</pre>
<pre>    src: url('fonts/badaboombb-webfont.eot');</pre>
<pre>    src: url('fonts/badaboombb-webfont.eot?iefix') format('eot'),</pre>
<pre>         url('fonts/badaboombb-webfont.woff') format('woff'),</pre>
<pre>         url('fonts/badaboombb-webfont.ttf') format('truetype'),</pre>
<pre>         url('fonts/badaboombb-webfont.svg#webfontGYspKv93') format('svg');</pre>
<pre>    font-weight: normal;</pre>
<pre>    font-style: normal;</pre>
<pre>}</pre>
<h4>IE9站点锁定（IE9 site pinning）</h4>
<p>接下来还要略微谈一下我们是如何在IE9中进一步提升用户体验的。这个站点还可以放到Windows7的taskbar中（只需要将图标拖到taskbar中就可以了）。我们使用 <a href="http://buildmypinnedsite.com/">BuildMyPinnedSite.com</a>来创建了这个站点的高分辨率图标（分别包括16×16, 24×24, 32×32以及64×64像素大小）。</p>
<pre>&lt;link rel=&quot;shortcut icon&quot; type=&quot;image/x-icon&quot; href=&quot;favicon.ico&quot; /&gt;</pre>
<pre>&lt;meta name=&quot;application-name&quot; content=&quot;Tron: Legacy HTML5&quot; /&gt;</pre>
<pre>&lt;meta name=&quot;msapplication-tooltip&quot; content=&quot;Enter Tron: Legacy&quot; /&gt;</pre>
<pre>&lt;meta name=&quot;msapplication-navbutton-color&quot; content=&quot;#00CCFF&quot; /&gt;</pre>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/7.png"><img title="7" src="http://www.webapptrend.com/wp-content/uploads/2012/03/7.png" alt="" width="438" height="42"></a></p>
<h4>测试以及互操作性</h4>
<p>为了保证站点能正常显示，你的浏览器需要支持HTML5 &lt;canvas&gt;、HTML5 &lt;audio&gt;以及 CSS3 WOFF。在我们的测试中可以看到，尽管这个应用在IE9中才有最好表现，但它在Firefox、Safari、Chrome 以及Opera都有较好展现。我们使用 <a href="http://blogs.msdn.com/b/ie/archive/2010/04/14/same-markup-writing-cross-browser-code.aspx">feature detection</a>技术来检测浏览器是否过劳而无法运行这个页面。</p>
<pre>var canvas = document.createElement('canvas');</pre>
<pre>if (!canvas.getContext) {</pre>
<pre>    // The browser doesn’t support HTML5 Canvas</pre>
<pre>}</pre>
<p>原文链接：<a href="http://windowsteamblog.com/ie/b/ie/archive/2011/06/03/behind-the-scenes-of-disney-tron-legacy-digital-book-site.aspx">Behind the Scenes of Disney TRON: Legacy Digital Book Site</a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Lanyrd是如何应用HTML5创建出优秀的移动Web应用的" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F02%2F1784.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2034.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/02/14/15448515.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Lanyrd是如何应用HTML5创建出优秀的移动Web应用的</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="再议HTML5离线浏览" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2011%2F12%2F1000.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2034.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/01/05/13493586.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">再议HTML5离线浏览</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="HTML5视频发展状况" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F02%2F1613.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2034.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/02/01/14601806.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">HTML5视频发展状况</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="初探：通过Application Cache实现HTML5离线浏览" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2011%2F12%2F977.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2034.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/01/05/13493596.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">初探：通过Application Cache实现HTML5离线浏览</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="4" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table>