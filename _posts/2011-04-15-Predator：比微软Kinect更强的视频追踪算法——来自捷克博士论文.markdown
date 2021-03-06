---
layout: post
title:  "Predator：比微软Kinect更强的视频追踪算法——来自捷克博士论文"
date:   2011-04-15 10:58:57
author: 
categories: program
---

## Predator：比微软Kinect更强的视频追踪算法——来自捷克博士论文
### by 
### at 2011-04-15 10:58:57
### original <http://news.cnblogs.com/n/97536/>

<p>　　boycott (haha):刚刚看到了这个用来演示一种新的物体跟踪的算法的视频，它是Zdenek Kalal博士论文里的一部分。Zdenek Kalal是英国萨里大学的一个捷克学生。他演示的是他的神奇的精确定位系统，这个系统几乎可以跟踪镜头里的任何物体，只要你能看见它，并把它选中。它能做很多神情的事情。在这个视频中，他演示了通过摄像机拍摄他的手指、把他的手指选做目标。系统于是就能精确的跟踪他的手指的动作。更令人惊奇的是，这个系统能够通过分析物体的运动来完善跟踪算法。你能在很短的时间里教会它跟踪你的手指、面孔或在高速公路上狂颠的轿车。有了这套系统，我们几乎真的可以实现”Minority Report“那样的人机界面。就像微软Xbox的Kinect那样，而这个效果更好。</p>
<p>　　Kalal有12个视频来演示他的这套算法都能做什么。只要你有一个好的摄像头，把这个软件装到计算机上、平板电脑上或手机里，它就能精确的定位跟踪你的前额上的一个点、你的指尖、或你的眼睛。你把摄像头放到门外，它就能自动识别是你认识的人来了，或警告你这是个陌生人。人们不用通过手就能简单的操控计算机。这项技术应用前景广泛。</p>
<p style="text-align:center"><img src="http://pic003.cnblogs.com/2011/66372/201104/2011041510534366.png" alt=""></p>
<p>　　你可以从萨里大学的网站找到这个程序的代码，它是免费的。Kalal被授予了“Technology Everywhere”奖学金作为嘉奖。<a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/tld.html">http://info.ee.surrey.ac.uk/Personal/Z.Kalal/tld.html</a></p>
<p>　　非常精彩的视频演示：</p>
<p style="text-align:center">



</p>
<p>　　<strong>产品介绍</strong></p>
<p>　　<strong>Key Features</strong></p>
<ul>
<li>Input: video stream from single monocular camera, bounding box defining the object</li>
<li>Output: object location in the stream, object model</li>
<li>Implementation: Matlab + C, single thread, no GPU</li>
<li>No offline training stage</li>
<li>Real-time performance on QVGA video stream</li>
<li>Dependence on OpenCV library(single function)</li>
<li>Ported to Windows, Mac OS X and Linux</li>
<li>Illumination invariant features</li>
</ul>
<p>　　<strong>Free Version（免费源码可从Github下载）</strong></p>
<p>　　TLD can be downloaded for testing in a chosen application. We provide a <a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/TLD/TLD1.0_demo.zip">precompiled demo</a> (Windows) and a <a href="https://github.com/zk00006/OpenTLD">source code</a> that is released under GPL version 3.0. In short, it means that any distributed project that includes or links any portion of TLD source code has to be released with the source code under the GPL version 3.0 license or later.</p>
<p>　　<strong>Commercial Version</strong></p>
<p>　　A license has to be purchased for using TLD in a commercial project. The licencing is managed by the IP owner, the University of Surrey and the fee is subject to negotiation. Please contact the <a href="http://www.surrey.ac.uk/business/services/licensingopportunities/new/">University of Surrey</a> for further information.</p>
<p>　　<strong>More Information</strong></p>
<ul>
<li><a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2011_ict_pioneers.pdf">High-level description of TLD</a></li>
<li><a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2010_cvpr_demo_poster.pdf">Components of TLD</a></li>
<li><a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2010_cvpr_poster.pdf">Learning component of TLD</a></li>
<li><a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2010_icip_poster.pdf">Application of TLD tracker to faces</a></li>
<li>Detailed description is in the following papers: <a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2009_olcv.pdf">ICCV'09 (w)</a>, <a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2010_cvpr.pdf">CVPR'10</a>, <a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2010_icip.pdf">ICIP'10</a>, <a href="http://info.ee.surrey.ac.uk/Personal/Z.Kalal/Publications/2010_icpr.pdf">ICPR'10</a></li>
<li>Many technical questions (e.g. installation) are being discussed in the following <a href="http://groups.google.com/group/opentld">discussion group</a>.</li>
</ul>
<p>　　<strong>MITBBS网友评论</strong></p>
<p>　　lalaphin (orpheus):   MIT一直在研究这玩意儿。我早就说过了，微软做kinect最终目标并不是游戏业，而是作为将来windows应用的探路石。</p>
<p>　　kz80 (雨过天晴):不错，这学生很能下工夫. kinect也是差不多原理.光线暗了肯定就不能用了</p>
<p>　　zlike (最终幻想)：我觉得他这个没解决最关键的问题：what to track?object tracking/recognition的一个难点是如何对凭空冒出来的一副图象中判断“哪些”发生了变化（因为所有像素都有变化），而不是对预先选好的一个小区域进行跟踪。如果他这个没有那个选取的步骤，而是直接能识别移动的“有效”物体，那就可以和kinnect媲美了</p><p><br>　　本文链接：<a href="http://news.cnblogs.com/n/97536/">http://news.cnblogs.com/n/97536/</a></p><p>　　相关新闻：<br>　　· <a href="http://news.cnblogs.com/n/82868/">梁念坚：微软中国正努力引入Kinect游戏机</a><span style="color:gray">(2010-11-30)</span><br>　　· <a href="http://news.cnblogs.com/n/85436/">微软官方证实Kinect精度将提高消息纯属谣言</a><span style="color:gray">(2010-12-22)</span><br>　　· <a href="http://news.cnblogs.com/n/83812/">消息称微软体感游戏设备Kinect出现缺货</a><span style="color:gray">(2010-12-07)</span><br>　　· <a href="http://news.cnblogs.com/n/96349/">Gmail Motion ？搞个 Kinect 来就可以玩儿了！</a><span style="color:gray">(2011-04-04)</span><br>　　· <a href="http://news.cnblogs.com/n/77871/">一些重要的算法</a><span style="color:gray">(2010-10-19)</span><br></p><img src="http://news.cnblogs.com/news/rssclick.aspx?id=97536" width="1" height="1" alt="">