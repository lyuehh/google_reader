---
layout: post
title:  "用Delphi控制Kinect"
date:   2011-01-05 14:49:01
author: 
categories: program
---

## 用Delphi控制Kinect
### by 
### at 2011-01-05 14:49:01
### original <http://www.cnbeta.com/articles/131508.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/85.htm"><img src="http://img.cnbeta.com/topics/x360.gif" alt="Microsoft XBOX" name="sign" align="right"></a>
        <p><b>感谢<a rel="nofollow" href="http://www.delphishare.com">盘古开发论坛</a>的投递</b><br>
好吧好吧好吧...在天空和地球上所有的迹象很清楚地说：这是一个圣诞节！所以现在是休息和娱乐的时间，绝对没有任何东西可以比和家人一起玩更好的了。一个新的玩具;-)其实是我的儿子买Xbox带的Kinect，...可以让一家大小一起娱乐！我并不想试图重新发明轮子（计算机上有很多Kinect应用），但最近我没有找到用Delphi如何控制这个令人难以置信的非常很酷的东西的任何好的例子。</p>
		<p><p>你也许认为我会就此放弃，“就这样吧“，是不是？ ;-)你们可以先看下面的（视频）和两个应用程序（二维和三维可视化），以便了解更多的技术细节。今天将不讨论黑客入侵，男孩和女孩，只有纯粹的三维虚拟现实的喜悦... :-)<br>
<br>
好了，你想尝试让这些例子并运行自己的代码？没有什么比这更简单了：<br>
<br>
环境要求：</p>
<blockquote> <br>
    *带硬件加速图形卡（支持OpenGL硬件渲染）的计算机（PC）<br>
    * Kinect控制器:-)<br>
    * Kinect Windows驱动程序 <a rel="nofollow" href="http://nuigroup.com/forums/viewthread/11249/">http://nuigroup.com/forums/viewthread/11249/</a> <br>
    *Simon J Stuart的Delphi TKinect组件取自这里：<a rel="nofollow" href="http://www.lakraven.com/2010/12/11/tkinect-delphi/">http://www.lakraven.com/2010/12/11/tkinect-delphi/</a>  （感谢你做的出色工作，老兄！）<br>
    *超快速Delphi 2D图像处理库graphics32：<a rel="nofollow" href="http://www.graphics32.org/wiki/">http://www.graphics32.org/wiki/</a> <br>
    * GLScene（Delphi OpenGL库）：<a rel="nofollow" href="http://glscene.sourceforge.net/wikka/HomePage">http://glscene.sourceforge.net/wikka/HomePage</a> <br>
</blockquote>
<p><br>
安装过程就像一个魔法。需要解决GLScene的一些小问题，但没有多复杂。<br>
重要：在您运行任何东西前 - 首先要确定你的计算机能识别Kinect设备（检查你的设备管理面板）。<br>
<br>
<a rel="nofollow" href="http://img.cnbeta.com/newsimg/110105/0835510384880787.png"><img src="http://img.cnbeta.com/newsimg/110105/0835510384880787.png" alt=""></a><br>
<br>
唉，几乎忘记了：利用从这里我的应用程序的源文件：<br>
<br>
    *二维数据可视化：<a rel="nofollow" href="http://itsecuritylab.eu/files/kinect/kinect_delphi_2d.zip">http://itsecuritylab.eu/files/kinect/kinect_delphi_2d.zip</a><br>
    *三维数据可视化：<a rel="nofollow" href="http://itsecuritylab.eu/files/kinect/kinect_delphi_3dpoints.zip">http://itsecuritylab.eu/files/kinect/kinect_delphi_3dpoints.zip</a><br>
<br>
运行所有的东西<br>
<br>
现在你可以尝试编译和测试这两个应用程序。关于如何使用这些应用程序，您已经在视频中看到很多的细节。预编译的EXE文件已经j加入了ZIP包，这可以为您提供方便。所以这些到底是什么，我们有：<br>
<br>
二维数据可视化<br>
<br>
这个实验显示如何收集，处理和在屏幕上绘制Kinect的数据。额外的挑战是编写一个函数，能够“跟踪水滴” - 对类似像素的屏幕区域。就是需要跟踪你的手，手指，鼻子或任何你想跟踪的东西。这是个令人惊讶的作品！<br>
<br>
注：这个DOF功能，选择一定范围内的3D像素 - 是一个应用程序的功能，而不是Kinect硬件部分。<br>
<br>
我也希望你能原谅我这样eeee ... “不典型”的方式，让他们知道像素颜色深度数据的转换过程：RGB - &gt;HLS - &gt; [自定义功能] - &gt;[0 .. 255]<br>
<br>
<a rel="nofollow" href="http://img.cnbeta.com/newsimg/110105/08355411973334397.jpg"><img src="http://img.cnbeta.com/newsimg/110105/08355411973334397.jpg" alt=""></a><br>
<br>
三维数据可视化<br>
<br>
你可以看到我的房间（实际上也是你的房间）在虚拟的三维立体画面抖动，含有微小的彩色小点:-)你喜欢我的Xmass树？<br>
<br>
<a rel="nofollow" href="http://img.cnbeta.com/newsimg/110105/08355621131589214.jpg"><img src="http://img.cnbeta.com/newsimg/110105/08355621131589214.jpg" alt=""></a><br>
<br>
所以你看，Delphi这么好（重要！）也容易使用的语言（懒人编码适用），因此，即使是非常复杂的应用程序你只是需要几个小时就可以写好。现在连我也希望更多的人将开始用Kinect玩耍，做一些很酷的事情，[让更多的快乐来到今年圣诞节，等等，等等]。记住：你是主宰。阿门。 :-)<br>
<br>
特别感谢Simon J Stuart 和他的TKinect Delphi组件<br>
和Jet Noir (<a rel="nofollow" href="http://soundcloud.com/jet-noir">http://soundcloud.com/jet-noir</a>)为她的录影带配音！<br>
<br>
它让我知道你喜欢那些疯狂的应用程序，...有一个不错的圣诞节， 祝新年快乐！ :-)<br>
<br>
原文连接<br>
<a rel="nofollow" href="http://itsecuritylab.eu/index.php/2010/12/27/kinect-and-delphi/">http://itsecuritylab.eu/index.php/2010/12/27/kinect-and-delphi/</a><br>
<br>
译文连接<br>
<a rel="nofollow" href="http://www.delphishare.com/index.asp?id=2130">http://www.delphishare.com/index.asp?id=2130</a><br>
<br>
欢迎访问 盘古开发论坛(<a rel="nofollow" href="http://www.delphishare.com">http://www.delphishare.com</a>) 论坛提供delphi/java/.Net/PHP/C等开发语言交流平台，并有大量优质软件开发岗位可以参考，欢迎前来灌水。</p></p></div>