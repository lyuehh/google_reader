---
layout: post
title:  "在Mac下使用berserkJS"
date:   2012-07-10 00:32:33
author: JerryQu
categories: program
---

## 在Mac下使用berserkJS
### by JerryQu
### at 2012-07-10 00:32:33
### original <http://www.udpwork.com/item/7676.html>

<p>上周六去杭州参加了第七届<a href="http://www.d2forum.org/d2/7/index.html">D2</a>，整个上午的前端性能监控与优化专场都非常不错，尤其是最后<a href="http://weibo.com/itapir">貘大</a>分享的berserkJS，之前正好有类似的需求，赶紧试用下～</p>
<p>通过<a href="http://tapir-dream.github.com/berserkJS/">这个页面</a>，可以获得berserkJS的全部信息，例如工具用途介绍，名称来历，Api说明，github地址，源码下载等等，这些内容请大家自己看，我就不多介绍了。</p>
<p>从github把整个项目弄到本地，/build/release有windows可用的可执行文件，只是用mac的同学没办法直接用，需要自己动手编译下，好在整个过程非常简单。先去<a href="http://qt.nokia.com/downloads">Qt官方</a>下载Mac下的sdk，我下载的是这个<a href="http://qt.nokia.com/downloads/sdk-mac-os-cpp-offline">760MB的dmg</a>，双击安装。接着用装好的Qt Creator打开src目录下的berserkjs.pro项目文件，不要急着编译。左侧“项目”选项卡下，有一些配置要改：Qt版本改成for GCC那个，构建目录也要选为项目下的任一目录。顺便提一句，貌似Mac系统默认不带GCC的，不过大家应该都装了Xcode。做完这些准备工具，理论上就可以编出Mac下可用的berserkJS.app。</p>
<p>双击berserkJS.app即可运行，但不推荐这么做。app文件是Mac标准的应用程序，其实它是目录，右键“显示包内容”可以打开这个目录包。包里有个Contents/MacOS/berserkJS文件，这才是真正可执行的二进制文件。最好把它拷出来使用，因为如果使用berserkJS.app，App.path会指到包内的berserkJS（如/Users/.../berserkJS/build/berserkJS.app/Contents/MacOS/），这样会引发不少问题。</p>
<p>berserkJS窗口会让初次见到它的人有点不知所措，四个空白框、两个按钮就是全部，没任何多余的说明，也没任何菜单项。简单介绍下：上方那个大框是webview，右键有刷新、后退和inspect，点inspect可以打开大家都熟悉的webkit调试工具（mac下这个调试窗口出现在主程序后方，而且一直被盖住没法用=&gt;_&lt;=）；左下第一个框是用来输入app js，输入完点“Run Script”或者cmd+enter就会运行，返回值显示在左下第二个框；右下的框会显示app js的日志信息。看图：</p>
<p><a href="http://st.imququ.com/uploads/2012/07/22126365-88CB-41B6-8690-51C955056B3B.png" title="点击查看大图"><img src="http://st.imququ.com/uploads/2012/07/m_22126365-88CB-41B6-8690-51C955056B3B.png"></a></p>
<p>有个概念需要特别关注下：上图左下第一个框里输入的js，我称之为app js，上方webview显示网页里的js，我称之为webview js，这两种js运行在各自沙箱里，没办法相互直接调用。为此，berserkJS提供了App.webview.execScript(sandbox func [, argObj])方法，用来在app js里调用webview js，并将app js变量传过去。通过execScript，webview js里的function返回值也可以被app js拿到。如果app调用webview的function是异步的，则需要在webview里使用__pageExtension.postMessage将返回值回传给app js。另外，相互传递的数据，必须为plain object。</p>
<p>berserkJS提供了大量的Api，为我们分析页面性能提供了很多便利。例如，下面这几行代码即可获得页面首屏渲染时间：</p>
<pre>App.webview.open('http://www.imququ.com');
// 监听首屏渲染结束事件
App.webview.addEventListener('firstScreenFinished', function(timeout, url) {
  alert(url + ' \n' + timeout);
});</pre><p>以往，统计首屏时间非常麻烦，由于没有标准事件的支持，通常需要JS埋点来实现。berserkJS提供了默认统计策略，用起来非常方便，当然，策略也可以由使用者自定义，以满足各种场景。</p>
<p>berserkJS先介绍到这里，建议大家都动手试试。我也会尝试在实际项目中使用它，有什么心得到时候再写。</p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7676.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7676.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7676.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>