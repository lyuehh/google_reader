---
layout: post
title:  "飞秋lua版：luafeiq0.1.0发布"
date:   2011-01-31 16:51:00
author: Kevin Lynx
categories: program
---

## 飞秋lua版：luafeiq0.1.0发布
### by Kevin Lynx
### at 2011-01-31 16:51:00
### original <http://www.cppblog.com/kevinlynx/archive/2011/01/31/139646.html>

<p><font size="2">继上次捣鼓出了<a href="http://www.cppblog.com/kevinlynx/archive/2011/01/23/139187.html">飞秋的群聊协议</a>后,鉴于年底没啥事情做,就用lua写了个简单的协议兼容的IM。本来开始让</font></p>
<p><font size="2">另一个同事在iptux的基础上修改的，结果大概是因为iptux的代码不是那么容易修改，就不了了之了。这个</font></p>
<p><font size="2">刚发布的<a href="http://code.google.com/p/luafeiq/">luafeiq</a>功能非常简单，仅支持与飞秋（包括大部分兼容IP messager的IM）进行单聊，群</font><font size="2">消息的</font></p>
<p><font size="2">收发，简易的消息盒子（暂存未读消息）。因为选的库都是跨平台的，所以很容易的luafeiq也是跨</font><font size="2">平台的，</font></p>
<p><font size="2">最主要的是我想在linux下使用。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">之所以选用lua，一方面是想练练lua，另一方面则是因为开发效率。前段时间在<a href="http://kevinlynx.javaeye.com/">android下写了些java代码</a>，</font></p>
<p><font size="2">用java写代码觉得甚为爽快（当然算不了完美）。这几天写了千把行的lua（也许有3K行，未统计过），</font></p>
<p><font size="2">感觉也不错。综合来说，这些高级语言的很多好用的语法特性，例如闭包（closure），垃圾回收，都提高</font></p>
<p><font size="2">了不少写代码的速度。当然，lua于我而言也算不上完美的语言。例如我经常因为变量敲错字母，而在运行时</font></p>
<p><font size="2">才暴露nil错误。这也许可以通过诸如IDE之类的工具在写代码的时候就给予提示。lua 在遇到一个符号时，</font></p>
<p><font size="2">默认地将其处理为全局的。关于这个语法特性早有人提出不爽，只能说大家设计的准则不一样。（在我们</font></p>
<p><font size="2">项目里，我直接改写了全局变量的metatable，从而防止策划随意定义全局变量）</font></p>
<p><font size="2"></font> </p>
<p><font size="2">再来谈谈实现过程中的一些琐事。因为飞秋也算是IP messager协议的兼容实现，很多通信除了可以使用</font></p>
<p><font size="2">抓包软件分析外，还可以直接通过IP messager的源码来了解。所以，基础通信协议的实现过程也比较</font></p>
<p><font size="2">简单。飞秋与飞秋之间发送私聊消息是经过加密的。其加密过程也不简单，更重要的是，我并不想浪费太</font></p>
<p><font size="2">多时间在这上面。后来发现其实可以通过上线消息里某个标志位表明自己不需要加密。这个标志就是消息头</font></p>
<p><font size="2">里的option。上线广播出去的消息里一旦表明自己不加密，那么以后和飞秋通信也就不需要解密了。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">发送私聊消息时，消息里会携带一个消息ID。这个ID可以通过任意算法生成，例如直接取time的值。接收到</font></p>
<p><font size="2">对方的消息时，需要取出该ID，然后加入回应消息。对方收到回应消息后，就知道自己发送成功。这个过程</font></p>
<p><font size="2">算是ip messager在UDP上做的消息可靠验证，过程也比较简单。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">群聊消息在之前提到过，是通过UDP多播实现。我们可以接收所有群的消息。如果之前已经处于某个群里，</font></p>
<p><font size="2">那么一旦你上线后（广播上线消息），你就可以直接在这个群里发言。但如果你之前不在这个群里，则</font></p>
<p><font size="2">可以通过多播一个加入群的消息，然后就可以不请自来地在这个群里发言。详细的消息值和实现都可以从</font></p>
<p><font size="2">luafeiq的代码里读到（message_sender.lua）。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">在linux下接收windows上的飞秋消息，是需要做字符编码转换的。因为luafeiq使用IUP作为UI库，IUP在</font></p>
<p><font size="2">linux下使用GTK作为底层实现，默认全部是UTF8编码。luafeiq里我自己写了个lua库，用于编码转换。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">话说IUP作为一个UI库，还是比较不错的。正如其介绍文档里所说，学习曲线低，基本上看一会文档，就可以</font></p>
<p><font size="2">直接使用了。luafeiq使用的IUP版本至少需要3.0以上。当初在linux下为了安装IUP3.3，基本花了4个小时</font></p>
<p><font size="2">时间，各种奇怪的没多大意义的错误信息。后来换成3.2版本，居然一下子就和谐了，无限怨念。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">luafeiq目前放在googlecode的版本，可以说是一个很不负责任的版本。早上我才刚把字符编码转换的代码</font></p>
<p><font size="2">调试好。今天已经请假，家里就一台电脑，也就测试不了这个字符编码转换是否真的能正常工作。我在</font></p>
<p><font size="2">windows下dump了些字符，看上去能正常功能。明天得回老家过春节，上不了网，索性就提前发布了。</font></p>
<p><font size="2"></font> </p>
<p><font size="2">luafeiq项目地址：<a title="http://code.google.com/p/luafeiq/" href="http://code.google.com/p/luafeiq/">http://code.google.com/p/luafeiq/</a></font></p><img src="http://www.cppblog.com/kevinlynx/aggbug/139646.html" width="1" height="1"><br><br><div align="right"><a style="text-decoration:none" href="http://www.cppblog.com/kevinlynx/">Kevin Lynx</a> 2011-01-31 16:51 <a href="http://www.cppblog.com/kevinlynx/archive/2011/01/31/139646.html#Feedback" style="text-decoration:none">发表评论</a></div>