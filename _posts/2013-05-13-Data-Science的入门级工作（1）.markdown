---
layout: post
title:  "Data Science的入门级工作（1）"
date:   2013-05-13 03:05:39
author: DiamRem
categories: program
---

## Data Science的入门级工作（1）
### by DiamRem
### at 2013-05-13 03:05:39
### original <http://ibeca.me/data-science-entry-level-1/>

<p>最近看到豆瓣的“冒牌HR”<a href="http://www.douban.com/people/flycondor/status/1157215404/">挑灯看剑</a>和一亩三分地的<a href="http://www.1point3acres.com/what-is-data-science-analytics/">Warald老大</a>都写了关于Data Science的一些东西。我也来凑个热闹好了，不过我的出发点是用自己做过的一些项目来让大家有个直观的感受——data science到底做些什么。当然我不是正宗的data scientist，统计、数据挖掘和机器学习的积累都……只有一点点，所以我主要做的是入门级的工作，数据处理（data manipulation）和数据探索（data exploration），外加我自己比较喜欢的数据可视化（data visualization）。如果你想看到的是通过分析来做预测（prediction）、推荐（recommendation）等等，这篇博客可能让你失望。</p>
<p>如果文中有说错和不完全的地方，还请各位指正。</p>
<p>基本上所有跟数据有关的工作，首先要做的不是收集数据更不是计算。第一步，也是最重要的一步是，你要通过数据来回答什么问题。“Twitter上的中文用户和微博上的中文用户讨论的话题是不是一样的”？这就是一个很具体的问题，要回答这个问题，你需要去找Twitter和微博的数据，然后再确定用什么方法来分析。这也是一个典型的统计学理假设再检验的模式。像“明天会不会下雨？”或者“奥巴马会在那几个州选票领先？”这样更加庞大而泛泛的问题，你可能需要先把他们具体化到类似于“根据去年这个时候人们购买雨伞的数量来判断明天会不会下雨”，或者“根据Twitter上的geo tag和tweet内容来分析奥巴马再哪个州有优势”这一类的问题。</p>
<p>总之，先要确定研究问题，然后通过它来确定需要的数据和分析方法。</p>
<p>下面是两个具体例子。</p>
<p>项目1，微博和Twitter。</p>
<p>这个项目是2011年下半年开始的，持续了几个月左右。第一部分可以在<a href="http://ibeca.me/twitter_weibo_top_comparison/">这里看到</a>。第一部分主要是做的网络关系分析（network analysis），通过分析社交关系网来探索两个平台上用户的特征。完成第一部分之后，我想进一步的分析一下两个平台在内容上的差异。</p>
<p>首先是收集数据。Twitter的数据好弄，我们学院有一个API，几个学生写了小脚本不断的在爬。我的工作就是从这些已经爬好的数据里挑出中文的tweet。主要方法就是通过encoding，具体代码在<a href="https://github.com/diamrem/twitter_chn">github</a>。微博上则通过stream api来收集公共时间轴的数据。整个过程大概持续了一个月，收集的总量现在不太记得了，大概是十几或者几十个GB级别的文本文件。</p>
<p>上面说了，我想回答的问题是两个平台上人们分享、谈论的内容有没有差异。很自然的，我想到了用语言学里的常见方法——建立“词汇模型”（languange model）。大概的思路就是找出两个平台上用户所使用的高频词汇，如果高频词汇差别很大（在统计上呈显著），那么可以断言两个平台上人们在说不一样的东西。由于我们面对的是中文，所以涉及到一个分词的问题。</p>
<p>比如有人有天发了这样一个推“结婚的和尚未结婚的”。那我应该是把它分成“结婚－的－和－尚未－结婚－的”，还是“结婚－的－和尚－未－结婚－的”？这种对于你我来讲很简单的问题对于计算机来说则很头疼。做好中文分词这本身就是一个很难且庞大的话题。我自然不会自己去发明一套分词算法，“善假于物”是做data的人必须知道的技巧。我当然是找一个好用的分词库了。Smallseg是我用的一个python的分词库，另外现在还有个库叫jieba貌似也不错。</p>
<p>分好词以后，记得把stop word去掉。语言学里有个理论是出现屏率太高的词本身没有多少语义分析上的意义，比如中文里的“的地得”、“和”，还有“你我他”等等。如果不把这些词去掉，那么每一个词汇库看起来都可能差不多。</p>
<p>以上，就是所谓的数据处理（data manipulation）的过程了。总结起来这个过程就是把原始数据（raw data）给转换成自己所需要的数据格式。</p>
<p>做好数据处理之后，在微博和Twitter上抓取到的内容就被分解成了一个个的词汇，所谓建立词汇模型，就是计算某一个词汇在整个词汇数据里出现的概率，里面具体牵扯到一个叫Maxim Likelihood的方法理论。上文我还提到需要比较两个词汇库的差异性，这里面用到的则是统计上的T检验（T-test）。</p>
<p>具体分析过程就不在这里赘述了，挑几个有意思的可视化给大家看看吧。</p>
<p>首先是高频词：</p>
<p><img style="display:block;margin-left:auto;margin-right:auto" title="Screen Shot 2013-05-12 at 11.57.30 AM.png" alt="Screen Shot 2013 05 12 at 11 57 30 AM" src="http://ibeca.me/wp-content/uploads/2013/05/Screen-Shot-2013-05-12-at-11.57.30-AM.png" width="400" height="377" border="0"></p>
<p>图中的横轴是300个高频词，红色的点某个词在Twitter上出现的概率，蓝绿色的点是微博的，这里高频的定义是出现屏率大于0.0005。从图中可以看出，同一个流行词，在Twitter和微博上出现的概率有很大的差别。如果我们把这些词汇按照其在Twitter上出现的概率排序，就会得到另外一个图：</p>
<p><img style="display:block;margin-left:auto;margin-right:auto" title="Screen Shot 2013-05-12 at 11.59.09 AM.png" alt="Screen Shot 2013 05 12 at 11 59 09 AM" src="http://ibeca.me/wp-content/uploads/2013/05/Screen-Shot-2013-05-12-at-11.59.09-AM1.png" width="400" height="411" border="0"></p>
<p>这个可视化里可以看到，在Twitter上经常出现的流行词，在Weibo上不一定是流行的，反之亦然。</p>
<p>然后我还追踪了单独的某个话题，在两个平台上出现的屏率，并以该话题在wikipedia上被编辑的次数作为参考。去年春天薄督闹得满城风雨，下图是就是“薄熙来”这个名字在Twitter和微博上出现的频率，这个图并不用怎么分析就能看出两个平台的差异性：</p>
<p><img style="display:block;margin-left:auto;margin-right:auto" title="Screen Shot 2013-05-12 at 11.59.40 AM.png" alt="Screen Shot 2013 05 12 at 11 59 40 AM" src="http://ibeca.me/wp-content/uploads/2013/05/Screen-Shot-2013-05-12-at-11.59.40-AM1.png" width="400" height="409" border="0"></p>
<p>去年同时还发生了另外一件事，那就是舒淇被挖了老底，我也研究了一下“舒淇”这个名字被提及的概率：</p>
<p><img style="display:block;margin-left:auto;margin-right:auto" title="Screen Shot 2013-05-12 at 12.01.42 PM.png" alt="Screen Shot 2013 05 12 at 12 01 42 PM" src="http://ibeca.me/wp-content/uploads/2013/05/Screen-Shot-2013-05-12-at-12.01.42-PM.png" width="400" height="412" border="0"></p>
<p>大概可以得到的结论是，两个平台对这种话题都挺有兴趣的。但是，微博上话题的兴起似乎比Twitter晚一些。</p>
<p>另外，这里用Wikipedia上相应词条做对比的原因是我假设当一个话题有突发新闻（breaking news）时，它在的Wikipedia上的页面会有一个编辑高峰期。这个曲线正好可以被用来作为一个参考曲线。</p>
<p>（下一篇我会讲关于Wikipedia编辑记录的数据处理、探索和可视化）</p>