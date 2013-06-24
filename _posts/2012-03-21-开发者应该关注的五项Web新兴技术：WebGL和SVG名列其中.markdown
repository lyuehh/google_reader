---
layout: post
title:  "开发者应该关注的五项Web新兴技术：WebGL和SVG名列其中"
date:   2012-03-21 18:11:47
author: Ray_tiger
categories: program
---

## 开发者应该关注的五项Web新兴技术：WebGL和SVG名列其中
### by Ray_tiger
### at 2012-03-21 18:11:47
### original <http://www.webapptrend.com/2012/03/2234.html>

<h1><span style="font-size:13px;font-weight:normal"><strong><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/htmlnewt.jpg"><img title="htmlnewt" src="http://www.webapptrend.com/wp-content/uploads/2012/03/htmlnewt-150x150.jpg" alt="" width="150" height="150"></a>Bruce Lowson</strong><strong>是Opera</strong><strong>开放web</strong><strong>标准的撰写人员之一，一些没有包含在HTML5</strong><strong>之内的浏览器技术十分奇妙，包括WebGL</strong><strong>和SVG</strong><strong>，作者希望通过本文与共同爱好者们分享。</strong></span></h1>
<p>最近一位HTML5专家Rich Clark（作者的好朋友）为大家做了一个HTML5 APIs的简介，在文章中为大家指向了一个令人迷惑的网页（web平台：浏览器技术http://platform.html5.org/），其中包含两个很长的专栏和小正文并提及到一些让人感到迷茫的技术，例如“window.crypto.getRandomValues”和“DOM Mutation observer”。</p>
<p>别担心，咱们不去管那些啦，因为有些还远远没有完成呢，在浏览器中见到它们还要等一阵子。然而，其它已经在浏览器中，或者距离您很近，或者马上就要出现。人们可能将称之为“HTML5”，尽管它们并不是。其实，它们都属于令人激动的新兴Web技术（New Exciting Web Technology），值得每个开发者关注。</p>
<p><span></span></p>
<h2><strong>WebGL</strong></h2>
<p>WebGL是一种基于Web的Graphic库，由非盈利组织Khronos运营,目前结合HTML5&lt;canvas&gt;元素广泛应用在3D图形开发中。</p>
<p>学习WebGL比较困难，因为它是底层开发——它运行在GPU上面，而且它实际上是一个OpenGL的JavaScript port，是一种游戏开发者使用的已经长期建立的API集。WebGL的主要受众是哪些已经拥有丰富OpenGL经验的游戏开发者，他们可以通过WebGL为web平台编写游戏。</p>
<p>好在有很多资源可以帮助您学习WebGL，这些资源不仅仅是关于游戏开发的，还有很多奇幻的图形、视觉和音乐视频等方面。作者个人比较推荐的是：</p>
<ul>
<li>Introduction to WebGL。<a href="http://dev.opera.com/articles/view/an-introduction-to-webgl/">http://dev.opera.com/articles/view/an-introduction-to-webgl/</a>作者Luz Caballero，简介可以获得的各种库。</li>
<li>Raw WebGL 101。<a href="http://dev.opera.com/articles/view/raw-webgl-part1-getting-started/">http://dev.opera.com/articles/view/raw-webgl-part1-getting-started/</a>适合那些不使用库的用户。</li>
<li>Learning WebGL。<a href="http://learningwebgl.com/">http://learningwebgl.com/</a>一个非常好的引导网站。</li>
<li>WebGL 101。http://www.youtube.com/watch?v=me3BviH3nZc一个由Erik Moller制作的介绍视频（2.5小时）。</li>
<li>See Emberwind。http://operasoftware.github.com/Emberwind/一个由Erik Moller做的WebGL游戏port，您可以深入Github或看代码。</li>
</ul>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/11.jpg"><img title="1" src="http://www.webapptrend.com/wp-content/uploads/2012/03/11-1024x754.jpg" alt="" width="614" height="452"></a></p>
<p> </p>
<p>WebGL目前在所有桌面浏览器（发布版和开发频道）中都支持，除了IE10（微软表示不支持）。对于移动产品来说，已经在Opera Mobile 12中发布了，最终会出现在Android，BlackBerry Playbook 2.0，Nokia N900，SonyEricsson Xpertia Android Phones等以及Firefox移动浏览器中。</p>
<h2><strong>SVG</strong></h2>
<p>SVG（Scalable Vector Graphics）已经在Opera，Firefox，Chrome中存在多年了，但是直到IE9开始支持它之后才渐渐变得主流一些它在HTML5&lt;Canvas&gt;的光环下显得有点暗淡，尽管SVG和HTML5&lt;canvas&gt;是面向不用应用的不同工具。</p>
<p>Canvas2D可以迅速paint图形到屏幕上面，这一点很犀利。但是其全部功能就是paint了，没有内存来做那些（位置，顶层或其他）其他功能。如果您需要那种book-keeping工作，就只能自己用JavaScript实现，因为Canvas2D不会把DOM保存到内存中，也正因为如此Canvas2D速度快，十分适合第一人称射击类应用。</p>
<p>与Canvas2D不同，SVG在您需要保存DOM的时候就给力了。使用JavaScript，所有的Objects都可以移动并且与动画无关。您可以试试Daniel Davis做的复古类SVG游戏Inbox Attack（http://people.opera.com/danield/svg/inbox-attack.svg）来体验一下，并且看看源代码来了解如何完成动画效果。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/21.jpg"><img title="2" src="http://www.webapptrend.com/wp-content/uploads/2012/03/21.jpg" alt="" width="597" height="384"></a></p>
<p>因为shape和path是用Markup来描述的，所以他们可以用CSS来定型。与&lt;canvas&gt;不同，text在SVG中保持text格式并且更加的灵活，更加可扩展，更加易于访问。在Canvas中，text变成了像素，就像Photoshop中的图形text。</p>
<p>SVG最强大的特性是它基于矢量，这样您的插图，图形和UI图标等都是矢量图了，这样无论是在50英寸的电视屏还是手机屏幕桌面上，看上去感觉都是一样的清晰。在当今这样一个web应用无处不在的时代，SVG图形甚至可以包括媒体查询（http://my.opera.com/ODIN/blog/2009/10/12/how-media-queries-allow-you-to-optimize-svg-icons-for-several-sizes），可以是响应式的，可以根据不同的目标设备做尺寸的调整。</p>
<p>综上所述，在最新的桌面浏览器中SVG已经能被广泛支持了。在移动产品方面的支持总体上来说也很好，以及预期在Android 3.0版本之前原生浏览器也会支持它了。</p>
<p>Daniel Davis有一些SVG 介绍性的资源（http://my.opera.com/tagawa/blog/learning-svg），作者个人也推荐一本免费的电子书：Learn SVG（http://www.learnsvg.com/book-learnsvg/），您也可以看看《SVG or Canvas？Choosing Between the Two》（http://dev.opera.com/articles/view/svg-or-canvas-choosing-between-the-two/）来了解二者的区别。</p>
<h2><strong>getUserMedia</strong></h2>
<p>不像那些被错误地称为HTML5的API，getUserMeida（在下文中简写为gUM）有个相对正当的理由：起初它是HTML5&lt;device&gt;元素，之后它改名了然后离开了W3C WebRTC规范集合。</p>
<p>gUM允许访问用户的摄像头和麦克风，本来是在WebRTC规范中在浏览器中进行P2P视频会议的，当gUM拥有了其他的用途，就离开了WebRTC。</p>
<p>摄像头的访问最终在Opera12安卓版，Opera桌面实验室和Google Chrome Canary里面实现了，不过Opera和Chrome都还没有实现麦克风的接入。</p>
<p>W3C规范依然在用，所以Opera和Webkit有不同的语法规范，这样的麻烦被一个叫做The gUM Shield（https://gist.github.com/f2ac64ed7fc467ccdfe3）的小JavaScript片段搞定了。如果您想更深入地了解这方面请看作者的另一篇文章：It’s Curtains for Marital Strife Thanks to getUserMedia（<a href="http://html5doctor.com/getusermedia/">http://html5doctor.com/getusermedia/</a>）</p>
<p>当视频从设备开始流传输的时候，源数据可以被做成变成了&lt;video&gt;元素，如果需要的话还可以被定为到屏幕外面，然后拷贝到&lt;canvas&gt;里面进行所需要的操作。Paul Neave写的《HTML5 变成玩具！》（http://neave.com/webcam/html5/）为了方便操作把流媒体数据拷贝到WebGL中。作者在.net 杂志的226话有采访他的报导（<a href="http://www.netmagazine.com/shop/magazines/april-2012-226">http://www.netmagazine.com/shop/magazines/april-2012-226</a>）。</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/3.jpg"><img title="3" src="http://www.webapptrend.com/wp-content/uploads/2012/03/3.jpg" alt="" width="559" height="392"></a></p>
<p>如果想把web app的功能做得像native app，gUM需要做很多的工作。试了一下Neaver的gum和WebGL 在Opera Mobile 12上面的demo，感觉和平台独有的app一样富有响应式并且很时髦。当在浏览器产品中其功能被广泛应用的时候，作者语言会有很多基于web的QR代码阅读者以及很多增强现实的应用。</p>
<h2><strong>File APIs</strong></h2>
<p>W3C File APIs允许JavaScript访问本地文件，其中最常用的API是FileReader，可以从Opera，FireFox，IE10平台等的预览版看到（不包括Safari）。</p>
<p>这一份W3C规范“为了在web应用中提供API来代表文件对象，以及编程选择和访问数据”。例如：你可以上传文件到浏览器中，并本地查找相关信息（例如文件名，尺寸，类型）而不需要到服务器端。您也可以打开文件，操作内容，这样可以加强基于浏览器的应用的交互性，用起来更像是本地应用。</p>
<p>另一个常用的用途是使传统的图像上传兑换狂更具有Web2.0特色：通过允许在浏览器内部的Drag and Drop，而不是本地文件系统中改变。</p>
<p>您可以通过使用一个普通的&lt;input type=”file”&gt;开始，然后循序渐进地提高。HTML5 Drag&amp;Drop支持特征检测，如果存在的话就使用&lt;div&gt;替换&lt;input&gt;，那就是您的drag图像目标了。当图像被drag到目标的时候，使用File Reader API来显示一个指甲盖大小的图像。您可以看一下Remy Sharp的demo（http://html5demos.com/file-api）。</p>
<p>还有很多写文件和操作文件系统的规范，不过这些对目前的跨浏览器应用来说还不太够：</p>
<p>W3C文件API：（http://dev.opera.com/articles/view/the-w3c-file-api/）非常基础的介绍。</p>
<p>开发文件系统API：（http://www.html5rocks.com/en/tutorials/file/filesystem/）HTML5 Rocks文章，（仅限Chrome）。</p>
<h2><strong>Feature-detecting, progressive enhancement and upgrade messages（特征检测，渐进式增强和消息通知）</strong></h2>
<p>诚然，在没有那些奇幻的API的时候，大家总是试图使用渐进式增强和HTML语义的方法让网站照常工作。然而有时候却不能这样，例如Paul Neaver的《HTML5变成玩具》中，如果gUM和WebGL现在不存在的话，其网站不能有什么补救措施了，整个网站的核心都没了。</p>
<p>在这样的情况下有两种典型的惯例：要么是显示一条消息说“你的浏览器太垃圾了，塞油哪啦”或者说“你必须用Chrome6/Firefox 4/Opera10等[插入能支持你应用的浏览器]才能访问”。第一种方法又没用又粗鲁，没有建议和补救措施；第二种方法是个临时办法，因为六个月之内所有浏览器可能都能支持你现在使用的技术了，让你在网站上留下的信息过时：例如您写的解决方案是建议使用Firefox4来访问，可是半年后用户安装着Firefox7回来访问你的页面了，这可就真的没救了。</p>
<p>如果您真的不能使用渐进式增强，那么就用新型的HTML 5 Please API吧（http://api.html5please.com/）。这是Jon Neal，Divya Manian和其他几位大虾创作的。通过使用它，可以先查询caniuse.com然后返回一个最新（能支持你的新特性的）的浏览器版本列表。</p>
<p>如果您已经做了一个需要Canvas或WebSQL DB技术的DEMO或者网站，恐怕你已经处在一个这样的尴尬境地了：您只是在告诉访问者们他们的浏览器不咋地。但是您不能只推荐他们使用一个能支持这些特性的浏览器来补救，例如“找个支持WebRTC性能的浏览器再来吧”，这样对于大家都没啥效果。</p>
<p>HTML5 Please API把开发人员的语言（和特性）翻译成用户能理解的语言（浏览器）。通过调用这个API你就可以得到一些HTML返回值来告诉访问者，或者返回一个带有相关数据的JSON对象（包括浏览器Logo及下载介绍等信息）。这样您可以根据不同的客户来显示不同的补救信息了。</p>
<p>使用这种方式最令人欣慰的是：如果所有新特性在客户当前浏览器的升级版都能支持的情况下，Please API值建议访客对浏览器升级，而不是让访客单纯为了访问你这个页面而更换浏览器。效果图如下：</p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/41.png"><img title="4" src="http://www.webapptrend.com/wp-content/uploads/2012/03/41.png" alt="" width="563" height="75"></a></p>
<p><strong>结束语：</strong></p>
<p>正如您所看到的，大量的令人惊喜的新技术正在接踵而至，您着手研究上述某项技术的时候恐怕又要担心更新鲜的技术到来了吧。希望您开发得愉快，请记得让您所开发的应用在尽可能多的浏览器上面测试一下。</p>
<p>———————————————————————————————</p>
<p>原文作者：Bruce Lawson 2012年3月19日</p>
<p>图片整理：Rob Winter ；HTML5阐述： Mike Brennan</p>
<p>原文地址：<a href="http://www.netmagazine.com/features/developers-guide-new-exciting-web-technologies">http://www.netmagazine.com/features/developers-guide-new-exciting-web-technologies</a></p>
<p><a href="http://www.webapptrend.com/wp-content/uploads/2012/03/0.jpg"><img title="0" src="http://www.webapptrend.com/wp-content/uploads/2012/03/0.jpg" alt="" width="50" height="50"></a>关于Bruce Lawson ：支持Opera开放标准，专注于HTML5，窗口小部件和接入性。他是一位HTML5专家，与Remy Sharp共同编写了《Introducing HTML5》。他的个人网站：<a href="http://brucelawson.co.uk/">http://brucelawson.co.uk/</a></p>
<p>———————————————————————————————–</p>
<p>译者：范小虎</p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Firefox上Web开发工具库一览" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2011%2F12%2F589.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2234.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/01/05/13493795.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Firefox上Web开发工具库一览</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="质疑：Mobile Web开发者准备好了吗？" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2331.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2234.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/03/30/18461542.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">质疑：Mobile Web开发者准备好了吗？</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Financal Time产品主管谈FT Web App开发" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2071.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2234.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/03/06/16621005.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Financal Time产品主管谈FT Web App开发</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="移动应用开发火热，开发者移步Appcelerator（上）" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fwww.webapptrend.com%2F2011%2F11%2F392.html&amp;from=http%3A%2F%2Fwww.webapptrend.com%2F2012%2F03%2F2234.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2012/01/05/13493850.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">移动应用开发火热，开发者移步Appcelerator（上）</font>
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