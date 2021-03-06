---
layout: post
title:  "重构之美－浴火重生的火凤凰CSS3【前传：Gradient】"
date:   2010-10-08 03:31:00
author: 爆牙齿
categories: program
---

## 重构之美－浴火重生的火凤凰CSS3【前传：Gradient】
### by 爆牙齿
### at 2010-10-08 03:31:00
### original <http://www.cnblogs.com/yuntian/archive/2010/10/08/1827993.html>

<p>作者: <a href="http://www.cnblogs.com/yuntian/">爆牙齿</a> 发表于 2010-10-08 03:31 <a href="http://www.cnblogs.com/yuntian/archive/2010/10/08/1827993.html">原文链接</a> 阅读: 1771 评论: 7</p><p>叮叮叮，铛铛铛，上课了上课了，同学们挨个坐好，爆老师要点名啦！</p>
<p>额，在上课之前先说一下，本套课程所有图例或示例包括以后涉及到的动画，我都将使用CSS3来设计，不采用一张图片，所以请使用Chrome浏览器。否则你看到乱七八糟的东西或一片空白，<b style="color:#fe280e">I Don't Care！</b></p>
<p><b>一、Gradient是什么东西？</b></p>
<p>这个问题比较初级，不过初级也能有高级的说法和理解，且听我的忽悠～</p>
<p>我记得曾经在读《硅谷革命》的时候，乔帮主说圆角矩形无处不在，然后在那个技术尚不成熟的年代，追求完美的帮主定义下圆角矩形，并运用到了苹果的所有设计上。是的，圆角确实很普及，但是直角矩形的设计依然存在，且不说1%幅度的视觉直角矩形，就是0幅度的纯直角矩形也是大量存在的，而且还不少，随便就能找出例子来，比如书本。比如，三角形，额，你能给三角形加一点圆角上去么？</p>
<p>但是色彩，却是没有纯色的存在。也许你会说纯色的设计无处不在。是，纯色设计确实普遍，但是真正进入你的眼睛，你看见的，你感受到的，你体验到的，却不会是纯色，为什么？？？</p>
<p>光啊！你生活在一个漫射光的世界里，只要有光，色彩就不会纯净，最多无限的接近纯净。如果你要说没有光的时候就是纯色了，纯黑。呵呵，是这样吗？真正没有光的时候，你根本都不存在对色彩的感知。没有白色，何为黑色？</p>
<p>所以，我要说：真正无处不在的，是渐变，因为，光，无处不在。</p>
<p>网页设计，我们经常使用到光使用到渐变，长久以来这些都处理为图片，而图片天生的弊端使得我们非常小心谨慎的使用。我们大量使用水平或垂直的规则渐变而回避掉圆形的斜线的合成的不规则渐变，因为只有规则渐变才能平铺，因为不规则渐变存为8bit的png会产生色阶，存为24bit的png文件很大，而存为jpg则因失去精度而模糊，找不到平衡点。于是充斥在页面上的是大量的垂直的规则的渐变，但这显然和真实情况下光的漫射、叠加、混合相去甚远。</p>
<p>而CSS3的Gradient，就是这个平衡点，尽管它仍不够完美。</p>
<p><b>二、Linear Gradient【线性渐变——点到点的渐变】</b></p>
<p>这很简单也很容易理解，就是从起点到终点的直线渐变。在这条线上你可以加入色彩的断点。代码为：</p>
<p><i style="font:14px arial">-webkit-gradient(linear,<b style="color:#aad010;font:14px arial"><u>0% 0%</u></b>,<b style="color:#2a8bbe;font:14px arial"><u>0% 100%</u></b>,from(#FFF),<b style="color:#ff7f00;font:14px arial">color-stop(0.5,#999)</b>,to(#000));</i></p>
<p>绿色为起点坐标，蓝色为终点坐标，橙色为断点位置（0.5表示50%）与色彩。以下为几种线性渐变示例：</p>
<p style="color:#fff;font:12px arial;background-image:-webkit-gradient(linear,0% 0%,100% 0%,from(#2a8bbe),to(#fe280e))">
左上（0% 0%）到右上（0% 100%）即从左到右水平渐变：<br>background-image:-webkit-gradient(linear,<b><u>0% 0%,100% 0%,</u></b>from(#2A8BBE),to(#FE280E));</p>
<p style="color:#fff;font:12px arial;background-image:-webkit-gradient(linear,0% 0%,0% 100%,from(#2a8bbe),to(#fe280e))">
左上到左下即从上到下垂直渐变：<br>background-image:-webkit-gradient(linear,<b><u>0% 0%,0% 100%,</u></b>from(#2A8BBE),to(#FE280E));</p>
<p style="color:#fff;font:12px arial;background-image:-webkit-gradient(linear,0% 0%,100% 100%,from(#2a8bbe),to(#fe280e))">
左上到右下即斜线渐变：<br>background-image:-webkit-gradient(linear,<b><u>0% 0%,100% 100%,</u></b>from(#2A8BBE),to(#FE280E));</p>
<p style="color:#fff;font:12px arial;background-image:-webkit-gradient(linear,0% 0%,100% 0%,from(#2a8bbe),,,to(#fe280e))">
水平渐变，33%处为绿色，66%处为橙色：<br>background-image:-webkit-gradient(linear,0% 0%,100% 0%,from(#2A8BBE),<b><u>color-stop(0.33,#AAD010),color-stop(0.33,#FF7F00),</u></b>to(#FE280E));</p>
<p>留意没？渐变是作为background-image存在，那么就可以做一些通用处理，比如同样的渐变光加不同的背景色：</p>
<p style="color:#fff;font:12px arial;border-color:#2a8bbe;background:-webkit-gradient(linear,0% 0%,0% 100%,from(rgba(255,255,255,0.7)),,,to(rgba(255,255,255,0.1))) #2a8bbe">
background:-webkit-gradient(linear,0% 0%,100% 0%,from(rgba(255,255,255,0.7)),color-stop(0.5,rgba(255,255,255,0.2),color-stop(0.5,rgba(255,255,255,0)),to(rgba(255,255,255,0.1))) <b><u>#2A8BBE</u></b>;</p>
<p style="color:#fff;font:12px arial;border-color:#aad010;background:-webkit-gradient(linear,0% 0%,0% 100%,from(rgba(255,255,255,0.7)),,,to(rgba(255,255,255,0.1))) #aad010">
background:-webkit-gradient(linear,0% 0%,100% 0%,from(rgba(255,255,255,0.7)),color-stop(0.5,rgba(255,255,255,0.2),color-stop(0.5,rgba(255,255,255,0)),to(rgba(255,255,255,0.1))) <b><u>#AAD010</u></b>;</p>
<p style="color:#fff;font:12px arial;border-color:#ff7f00;background:-webkit-gradient(linear,0% 0%,0% 100%,from(rgba(255,255,255,0.7)),,,to(rgba(255,255,255,0.1))) #ff7f00">
background:-webkit-gradient(linear,0% 0%,100% 0%,from(rgba(255,255,255,0.7)),color-stop(0.5,rgba(255,255,255,0.2),color-stop(0.5,rgba(255,255,255,0)),to(rgba(255,255,255,0.1))) <b><u>#FF7F00</u></b>;</p>
<p style="color:#fff;font:12px arial;border-color:#ff7f00;background:-webkit-gradient(linear,0% 0%,0% 100%,from(rgba(255,255,255,0.7)),,,to(rgba(255,255,255,0.1))) #fe280e">
background:-webkit-gradient(linear,0% 0%,100% 0%,from(rgba(255,255,255,0.7)),color-stop(0.5,rgba(255,255,255,0.2),color-stop(0.5,rgba(255,255,255,0)),to(rgba(255,255,255,0.1))) <b><u>#FE280E</u></b>;</p>
<p>试试拉伸浏览器的宽度，渐变不错吧，色彩不错吧，嘿嘿，那可是英来网的CI色彩体系。</p>
<p>线性渐变很简单，本不打算说的，算热热身吧，今天的重头戏是接下来的硬骨头：Radial Gradient。</p>
<p><b>三、Radial Gradient【径向渐变——圆到圆的渐变】</b></p>
<p>在讲解这个属性前，先要咬文嚼字一下，Radial是什么意思？这很关键！</p>
<p>相关的中文翻译有两种：1、径向。2、放射。我不用Photoshop，所以不清楚Photoshop中Radial是如何翻译的，又是如何设置的。在Firework中，Radial被译为放射，其设置也是放射，一根线为半径，从圆心到圆周渐变。为什么我要特意这样咬文嚼字一下？很长一段时间我都没搞清楚这种渐变该怎么设置，前天我给我们的手绘妹妹讲解这个属性的时候，在她的提示下，我才发现为什么我一直搞不清楚这种渐变是怎么实现的即便我已经使用多次了，她反过来给我上了一课，让我明白了原理。美术专业的敏感是不一样，当我给她说两个圆的渐变时，她一下就理解了。最后我发现我搞不清楚的根本原因就在于这个词的理解上，我是按照Firework的放射渐变在理解，按照圆心到圆周这样的起点终点渐变方式在理解，这是一个天大的错误。</p>
<p><b style="color:#fe280e">So，记住了，这不是从一个点到一个点的渐变，而从一个圆到一个圆的渐变。不是放射渐变而是径向渐变。</b>好了，下面我说说什么叫圆到圆的渐变，你现在可以先自己想象一下再接着往下看。其实只要不陷在点到点的渐变上，这个看起来复杂的属性其实很好理解的，当我从该死的点到点跳出来后一下就理解了。</p>
<p>首先来看代码，从一个圆到另一个圆的渐变，同样你可以在过程中加入色彩断点，代码为：</p>
<p><i style="font:14px arial">-webkit-gradient(radial,<b style="color:#aad010;font:14px arial"><u>50 50,50,</u></b><b style="color:#2a8bbe;font:14px arial"><u>50 50,0,</u></b>from(black),<b style="color:#ff7f00;font:14px arial"><u>color-stop(0.5,red),</u></b>to(blue))</i></p>
<p>绿色是起始圆的圆心坐标和半径，蓝色是目标圆的圆心坐标和半径，红色是断点的位置和色彩。这里我提醒一下，和放射由内至外不一样，径向由外到内的渐变，刚好相反。好了，就这段代码你能想象出来这是一个什么渐变吗？首先这是两个同心圆，外圆半径为50px，内圆半径为0，那么就是从黑色到红色再到蓝色的正圆形渐变。下面就是这段代码的效果：</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,50 50,0,from(black),,to(blue))">backgroud:-webkit-gradient(radial,50 50,50,50 50,0,from(black),color-stop(0.5,red),to(blue));</p>
<p>同心圆是最简单的，也是最容易产生点到点误解，因为一看就是从黑色到红色再从红色到蓝色，一条直线。实际上不是这样的，现在我小改一下，仍是同心圆，不过内圆半径改为10px。</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,50 50,10,from(black),,to(#blue))">backgroud:-webkit-gradient(radial,50 50,50,50 50,<b><u>10</u></b>,from(black),color-stop(0.5,red),to(blue));</p>
<p>有没有问题？我问一个：为什么中间有一片纯蓝色？如果你疑惑，那么你仍是用放射思路去看的。现在我用纯色来指明，渐变的范围在哪里。</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,50 50,10,from(black),,,,,,to(blue))">background:-webkit-gradient(radial,50 50,50,50 50,10,from(black),color-stop(0.1,white),color-stop(0.5,white),color-stop(0.5,red),color-stop(0.6,white),color-stop(0.9,white),to(blue));</p>
<p>白色区域是Radial的渐变范围，红色断点在白色的50%间。有没有搞清楚？现在我再改变一下，不再是同心圆了，内圆圆心向右20px偏移。</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,70 50,10,from(black),,to(#blue))">backgroud:-webkit-gradient(radial,50 50,50,<b><u>70</u></b> 50,10,from(black),color-stop(0.5,red),to(blue));</p>
<p>明白没？如果还没明白，回到白色示例来看：</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,70 50,10,from(black),,,,,,to(blue))">background:-webkit-gradient(radial,50 50,50,70 50,10,from(black),<b><u>color-stop(0.1,white),color-stop(0.5,white)</u></b>,color-stop(0.5,red),<b><u>color-stop(0.6,white),color-stop(0.9,white),</u></b>to(blue));</p>
<p>再不明白，我就没辙啦！<b style="color:#fe280e">一句话总结：所谓径向渐变，所谓圆到圆的渐变，就是指外圆周到内圆周的渐变。</b>如果这个明白了，那么下面这个图就应该明白为什么会产生了：</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,89 50,10,from(black),,to(#blue))">backgroud:-webkit-gradient(radial,50 50,50,<b><u>89</u></b> 50,10,from(black),color-stop(0.5,red),to(blue));<br>外圆与内圆几乎内切，1px。</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,90 50,10,from(black),,to(#blue))">backgroud:-webkit-gradient(radial,50 50,50,<b><u>90</u></b> 50,10,from(black),color-stop(0.5,red),to(blue));<br>外圆与内圆内切。</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,91 50,10,from(black),,to(#blue))">backgroud:-webkit-gradient(radial,50 50,50,<b><u>91</u></b> 50,10,from(black),color-stop(0.5,red),to(blue));<br>内圆刚刚超出外圆1px，产生了两个交点和切线。</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,150 50,10,from(black),,to(#blue))">backgroud:-webkit-gradient(radial,50 50,50,<b><u>150</u></b> 50,10,from(black),color-stop(0.5,red),to(blue));<br>内圆已经离开外圆。</p>
<p>看看当内圆离开外圆时的白色范围：</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,150 50,10,from(black),,,,,,to(blue))">background:-webkit-gradient(radial,50 50,50,70 50,10,from(black),<b><u>color-stop(0.1,white),color-stop(0.5,white)</u></b>,color-stop(0.5,red),<b><u>color-stop(0.6,white),color-stop(0.9,white),</u></b>to(blue));</p>
<p>好了，更多的情况我就不继续了，自己可以去试验。</p>
<p>了解了原理，我们来做一个来自顶部的漫射光，开盏灯，嘿嘿：</p>
<p style="padding-left:110px;font:12px arial;color:#fff;height:100px;background:-webkit-gradient(radial,50 50,50,50 1,0,from(black),to(white))">backgroud:-webkit-gradient(radial,50 50,50,50 1,0,from(black),to(white));</p>
<p>贴一张我的设计，你懂得，只有一个div，没有任何图片，一个月前做的，当时还是稀里糊涂，效果现在看来并不好：</p>
<p><img src="http://images.cnblogs.com/cnblogs_com/yuntian/49141/o_2010100802.png"></p>
<p>再贴一个实际应用场所，半个月前做的，有很多进步了：</p>
<p><img src="http://images.cnblogs.com/cnblogs_com/yuntian/49141/o_2010100801.png"></p>
<p>额，绝密的<a href="http://www.englive.cn">英来网</a>正式版界面发生泄漏事件鸟……So，不能继续了，下课下课！</p>
<p>稍等，下课前还有两件事：1、布置家庭作业。2、口水不能忘了喷。</p>
<p><b>四、家庭作业</b></p>
<p>实现一个椭圆形的径向渐变。</p>
<p><b>五、口水话</b></p>
<p>目前渐变还没有完全成熟，最麻烦的是Firefox和Webkit的实现写法差别太大，IE9 Beta也尚未提供支持。渐变类型也仅仅限于线性和径向，且径向渐变不能使用百分比来定义。还有一个问题，在没有GPU的情况下，大面积径向渐变（比如500X500）对渲染性能的影响极其大，甚至大过多个页面内容的叠加！这是我国庆节才发现的，当大面积使用径向渐变的时候，我们的所有交互动画都变得不流畅了。但是当GPU打开的时候，就没任何问题，这也从另外一个角度说明了GPU的重要性和它在可见的未来里所拥有的地位：那是必须的！但是目前Chrome的GPU在渲染上问题相当多，我正在就GPU的各种CSS3渲染问题和Chrome团队联系和沟通。</p>
<p>虽然还有很多很多问题，还有很多很多不足，还有很多很多限制，但是它已经可以解决很多问题了，尤其在减少图片的使用下，尤其在显示速度上，没有等待图片下载的过程，没有闪烁，直接渲染呈现，体验相当好。而且代码矢量，维护性通用性那根本不是图片能比拟的。</p>
<p>由于忙于<a href="http://www.englive.cn">英来网</a>正式版的开发，所以根本没有时间来对自己围绕在HTML5上的技术应用和探索和经验心得进行整理成文。国庆期间花了一天时间把这篇文章整理出来了，算是个先行版，所谓前传。之后我又将扑到产品的开发上，离上线不远了，而<a href="http://www.englive.cn">英来网</a>正式版上线之时就是《重构之美》第四部正式提笔连载之时。</p>
<p>下课！</p>
<hr>
<p>贴个招聘：英来网招聘HTML5 JS交互开发两名，一名偏前端，一名偏后端。<br><br>
工作方式：兼职。<br>
工作内容：基于JQuery进行纯JS交互开发。<br>
工作地点：杭州。（最好城西）<br>
其他要求：<br>
1、善于沟通，你主要是和我打交道。<br>
2、对新技术要敏感，你发现没，我很前卫的，如果我说一起攻克web worker，你都不知web worker为何物，那搞个铲铲。<br>
3、技术能力要过硬，最好三年以上JS开发经验，比如我说一起攻克web socket，结果你一个泡都冒不出来，那搞个铲铲。<br>
4、偏前端的JS交互开发要熟悉Web标准，对结构和样式敏感，要不我们道不同不相为谋。<br>
5、偏后端的JS交互开发要熟悉Java和数据库设计。<br>
你将面对的是：<br>
1、一个高尚而伟大的行业：教育。（真的，我一直很高兴自己选择了这样绝对昂首挺胸的行业，我做了一件正确的事，剩下的就是找到正确的方法，当一个好老师！想到这点，我就神经质的激动万分然后努力工作。）<br>
2、一个追求自由和完美，神经质的领导：爆牙我。<br>
3、一个激情四射的团队。<br>
4、一个不排斥并最大限度拥抱新技术拥抱未来的项目。<br>
5、一个匪夷所思的产品。<br><br>

和我一起体验创业，一起打造一个惊心动魄的应用，把未来带入现在，把梦想照进现实。<br><br>

待遇及回报：面议。<br>
联系方式：zhengminlive@gmail.com（请附简历，欢迎畅谈）
</p>




<img src="http://www.cnblogs.com/yuntian/aggbug/1827993.html?type=1" width="1" height="1" alt=""><p>评论: 7　<a href="http://www.cnblogs.com/yuntian/archive/2010/10/08/1827993.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/yuntian/archive/2010/10/08/1827993.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76645/">Facebook十大里程碑事件：今年7月用户达5亿</a><span style="color:gray">(2010-10-08 22:22)</span><br>· <a href="http://news.cnblogs.com/n/76644/">Zynga首席设计师解密FrontierVille成功原因</a><span style="color:gray">(2010-10-08 22:10)</span><br>· <a href="http://news.cnblogs.com/n/76643/">华东代理商与谷歌沟通未果 再发公开信声讨</a><span style="color:gray">(2010-10-08 22:09)</span><br>· <a href="http://news.cnblogs.com/n/76642/">阿联酋与RIM签署和解协议 暂不禁止黑莓服务</a><span style="color:gray">(2010-10-08 22:08)</span><br>· <a href="http://news.cnblogs.com/n/76640/">专访Twitter新任CEO：公司继续保持独立</a><span style="color:gray">(2010-10-08 21:55)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/76477/">远离.NET</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>