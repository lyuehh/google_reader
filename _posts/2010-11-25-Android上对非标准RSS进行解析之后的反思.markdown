---
layout: post
title:  "Android上对非标准RSS进行解析之后的反思"
date:   2010-11-25 21:01:43
author: 
categories: program
---

## Android上对非标准RSS进行解析之后的反思
### by 
### at 2010-11-25 21:01:43
### original <http://www.javaeye.com/topic/824894>

这几天做了一个解析RSS的小东西时碰到了些问题，在这里纪录一下。
<br>
<br>1.先说明一下碰到了问题是什么。
<br>最开始本想做个很简单的小东西，在ANDROID下面用SAX解析RSS，
<br>没想到SAX对SINA博客之类的用了[CDATA]的RSS解析得不完整，很多只解析出了一句。
<br>（ps,不知道SINA这种RSS是不是标准的，也许这居于XML的东西根本就没个标准）
<br>另外，SAX对非标准的RSS解析得更差，
<br>这里的‘非标准RSS'是指没用[CDATA]直接在 description里面放上HTML TAG的那种。
<br>
<br>具体的问题描述我引用下面一段话
<br><div>引用</div><div>
<br>SAX处理完Element事件后, 紧接着会继续读取,并且交由characters进行处理内容.Character处理普通字符没有任何问题, 不会诱发任何监听事件. 而当遇到&lt; [ ] 等字符时,又会触发一个新的characters事件, 也就是说, 如果你仅仅在 Characters 事件中进行处理保存数据(保存到一个变量中)的话, 是没有任何问题的, 只是遇到的时候, 发生了多次的Characters, 而你的代码上对事件的处理方式是一样的, 都是保存到变量中, 所以,变量中仅仅会保留最后一次赋值的数据. 最后一份数据正常情况下均是\n或者空格. 所以造成了,SAX无法读取CDATA数据的表象.
<br></div>
<br>出自：<a href="http://www.lshine.com/index.php/2010/07/sax_cdata/comment-page-1/#comment-7226">http://www.lshine.com/index.php/2010/07/sax_cdata/comment-page-1/#comment-7226</a>
<br>这个博客是在刚才在重新搜索时找到的，他的分析和我之前判断的一样。
<br>（这个SAX问题我还是没解决，显示的内容是比以前多了，但还是不完整）
<br>
<br>当然，如果具体Parser过程有疑问，可以看这个SAX官方网站，看看原理，看看源码：
<br><a href="http://www.saxproject.org/about.html">http://www.saxproject.org/about.html</a>
<br>（SAX最后一次更新是SAX 2.0.2 (sax2 r3),在27-April 2004…不知道ANDROID里面的是不是这个版本…)
<br>
<br>
<br>2.再说一下怎么解决问题。
<br>SAX不行就换别的，DOM和PullXmlParser都可以。
<br>这里先介绍一个：
<br><a href="http://zhoujianghai.javaeye.com/blog/755749">http://zhoujianghai.javaeye.com/blog/755749</a>
<br>他的这个比我的好，我看sdk和那些Tutorials快一个月了，写个东西还不能得心应手，
<br>真是差距啊……
<br>
<br>人家总结的也好：
<br><div>引用</div><div>
<br>1）RSS的文件结构和含义
<br>2）解析xml文件的几种方式
<br>3）android应用的结构和MVC模式   （！）
<br>4）activity的生命周期，
<br>      activity之间通过intent通信，
<br>      handler消息机制，
<br>     自定义ListView，
<br>      LinearLayout、
<br>      RelativeLayout、
<br>      FrameLayout布局界面，
<br>      WebView显示网页。
<br>      android数据存储（SQLite3）的相关操作，
<br>      菜单和对话框、Toast的使用，
<br>      android的测试驱动，                    （！）
<br>      uses-permission权限配置
<br></div>
<br>这些，我花了一个月都像是在盲人摸象，完全没个纲领去系统的学习。
<br>对我而言，还应该加上多线程之类的比较基础的东西……
<br>真的反思一下了……
<br>
<br>最后贴个东西：
<br>1.Android XML Parser Performance，性能分析。留着备用。
<br><a href="http://www.developer.com/ws/article.php/10927_3824221_2/Android-XML-Parser-Performance.htm">http://www.developer.com/ws/article.php/10927_3824221_2/Android-XML-Parser-Performance.htm</a>
<br>
<br>2.SAX和PullxmlParser的代码，我写的东西太差，就不拿出来丢人现眼了……
<br><a href="http://blog.chinaunix.net/u2/85805/showart_1678985.html">http://blog.chinaunix.net/u2/85805/showart_1678985.html</a>
<br>
<br>over.
          
          <br><br>
          作者: <a href="http://lovext.javaeye.com">lovext</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/824894" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>