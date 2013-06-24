---
layout: post
title:  "用Visual Studio实践敏捷测试 (一) 上"
date:   2010-06-24 09:23:00
author: 微软
categories: program
---

## 用Visual Studio实践敏捷测试 (一) 上
### by 微软
### at 2010-06-24 09:23:00
### original <http://www.cnblogs.com/stbchina/archive/2010/06/24/agile-testing-on-visual-studio-part-I-a.html>

<p><a href="http://www.cnblogs.com/stbchina/"><img src="http://pic.cnblogs.com/face/u99033.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/stbchina/">微软</a> 发表于 2010-06-24 09:23 <a href="http://www.cnblogs.com/stbchina/archive/2010/06/24/agile-testing-on-visual-studio-part-I-a.html">原文链接</a> 阅读: 899 评论: 0</p><p><font face="宋体">    本文为“用Visual Studio实践敏捷测试”系列文章的第一篇，主要讲述测试人员在正式进入测试阶段之前需要做的准备工作。</font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体"></font> </p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体">    敏捷软件开发是近年来谈的很多的一个话题，业界也流传着许多敏捷开发的方法：Scrum、FDD（Feature Driven Development，特性驱动开发）、TDD（Test Driven Development，测试驱动开发）、XP（eXtreme Programming，极限编程）是其中比较常见的几种。不过可惜的是我们所能找到的内容还是方法论居多。当聊起敏捷软件开发时，被问得最多的问题大概就是：具体如何在一个团队中开展敏捷软件开发呢？——如何在实际开发过程中运用敏捷思想？如何定义开发流程？以及使用什么样的工具来辅助开发流程？这些都是刚接触敏捷软件开发概念时常见的问题。</font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体"></font></p>
<p><font face="宋体">    在过去的几年中，我所在的团队从传统的瀑布开发模型向敏捷开发模型转变。我们在这个过程中，尝试了很多不同的流程、工具，逐渐摸索总结出我们自己的敏捷开发方法。我的经理Ramesh Rajagopal在《Visual Studio Team Architect 团队的敏捷软件开发》系列文中介绍了我们以Sprint作为时间单元、以Feature Crew形式分配资源的开发流程。这里我就不再赘述整个开发流程了。本文着重强调测试人员在整个敏捷开发流程中所扮演的角色、需要完成的工作以及如何应用Visual Studio 作为工具实行整个流程的。另外，本文旨在介绍一个可行的实践方案，为大家提供参考。它并不是一个最优的方案，我们自己也仍然在不断地探索尝试新的方法、工具，以期进一步优化我们的流程。</font></p>
<p><font face="宋体"></font></p>
<h3><font face="宋体"></font> </h3>
<h3><font face="宋体">从用户故事（User Story）在开始进入角色吧！</font></h3>
<p><font face="宋体">    作为一个测试人员，敏捷软件开发与传统开发模式最大的不同，就是你必须彻底改变从前那种“前半程”（设计、编码阶段）悠闲，而“后半程”（即开发人员已经把代码大体完成时）才匆忙投入战斗的工作方式——从开发周期的第一天起，你就要立刻进入测试角色！我们需要在一个短短4周左右时间的Sprint中，利用仅有的几个开发和测试人员的Feature Crew资源，完成数个（取决于实现的难易程度）用户故事（即我们一般意义上说的功能或特性），并保证在Sprint结束时通过规定的验收，并有一个可运行、可演示的版本。这就要求我们合理运用所有的时间和人力资源，不能有任何的浪费。那么，你可能要问：在一开始连代码都还没有写的情况下，测试人员能做些什么呢？答案就是：从审阅用户故事（即产品功能规范说明书）开始。</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">    通常，在我们的团队中会有两种形式的用户故事：Team Foundation Server（以下简称TFS）工作项（2010版本中新添加了用户故事工作项）和Microsoft Office OneNote（以下简称OneNote）文档。TFS是Visual Studio提供的、跟踪管理项目的利器。我们的项目经理会在TFS中创建用户故事对应的工作项，用于跟踪该用户故事的进度。而此后我们也可以在此工作项下再创建各项任务（Task）甚至子任务，并且通过各工作项的状态、预计时间、完成时间、剩余时间等了解开发进度。TFS还可以根据工作项的信息生成各种报表。</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">     而在具体描述用户故事内容或进行团队讨论时，OneNote文档就显得更加实用了。在这里，项目经理可以详细地描述该用户故事的目标、具体功能、操作流程，必要时还可以加上图片、表格等辅助说明。更重要的是OneNote支持多人同时编辑和自动同步功能，这样团队成员就可以留下自己的意见和建议，甚至可以直接在此讨论设计细节。这么做的优点不仅仅是团队成员有一个便利的讨论问题的地方，更重要的是讨论的内容其实蕴藏着丰富的信息，OneNote为这些讨论提供了一种非正式的文档，将来需要查询时也能方便的找到。</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">     介绍了两种不同表现形式的用户故事之后，让我们把话题转回本节开始的时候提出的问题上来——测试人员如何通过审阅用户故事来从第一时间开始测试。首先，测试人员应该捍卫用户的利益。在一个开发团队中，项目经理可能更加关注的是功能本身如何定义，开发人员关心的是如何实现功能，而测试人员则是最终用户利益的代表。所以，讨论功能设计不仅仅是项目经理和开发人员的任务，也是测试人员的任务。测试人员需要站在用户的角度考虑问题：这个功能对于用户而言实用么？这样的操作流程对用户来说是否方便？这样的界面设计是不是对于各种用户来说都合理（举个例子，如果我们设计某个功能是通过鼠标点击来完成的，那么就需要为只有键盘的用户考虑，他们能否通过键盘来完成相应的操作呢）？……这些可能不是我们传统意义上认为的Bug，却将实实在在的影响用户最终的使用体验——对用户而言产品的好坏不在于它有多少功能，而在于它是否好用、是否能满足其需求。</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">     其次，测试人员应该关注细节。我们在实践中发现，往往第一稿的用户故事都比较简单，仅描述主线功能而忽视了边界情况或错误情况。边界情况的处理是如何定义的？错误应该通过何种形式反馈给用户？在模棱两可的情况下产品行为应该如何定义（比如，在输入字符串长度超界时，正确的行为是仅处理合理长度的内容，还是干脆不处理抛出错误信息）？……这些细节很容易被忽略，在极端情况下甚至会出现在没有明确定义时，项目经理、开发人员和测试人员认为的“正确”的行为完全不一致！虽然我们可以在将来的实际测试阶段找出这些错误、讨论并达成共识最终纠正它们，但是在代码开始写之前就确定好细节，节省了反复修改的时间不是更好么？</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">     再次，测试人员还应该关注设计的可测试性。比如，在自动化测试中，有时产品的状态不容易捕捉——某一操作是否执行完毕可以进行下一步操作了呢？又比如，该功能需要进行压力测试，是否能有一种方法帮助测试工具创建大量数据呢？如果我们在设计产品功能的时候就考虑到这些问题，要求开发人员提供一些特殊的返回值或者API，我们之后的自动化测试工作将会轻松不少，同时也准确不少。设想一下，在开发人员宣布代码完成之后，测试人员再提出这样一个对功能没有多大影响的要求，被接受的可能性又有多大呢？</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">     上述各种测试人员与其他团队成员之间的讨论可以在OneNote上进行，可以在团队会议讨论的时候进行，也可以和团队其他成员私下交流时进行。讨论能很好的帮助完善一个用户故事的设计，也能帮助测试人员了解功能本身，以便更有针对性地制定测试计划。此时，测试人员对有哪些测试任务需要完成，心中已经大致有数了，接下来就应该为这些任务创建好TFS工作项并填上预估完成时间。这不但可以提醒自己把想到需要的测试都一一完成，更重要的是让整个团队清楚具体有哪些任务以及这些任务的工作量，这样才能更好的帮助你的项目经理管理整个Sprint乃至整个项目。</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font> </p>
<p><font face="宋体">     打起精神投入到你的测试角色中去，打响测试人员阻止Bug的第一场战斗——审阅用户故事吧！</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体">  </font></p>
<p><font face="宋体">作者：林俊彦，复旦大学计算机专业毕业后加入微软，现为微软亚太研发集团服务器与开发工具事业部（中国）的软件测试开发工程师，参与了Visual Studio及.NET Framework多个版本的开发测试工作。林俊彦还参与了《Visual Studio DSL工具特点领域开发指南》、《为软的软件测试之道》等书籍的中文翻译工作。</font></p>
<p><font face="宋体"></font></p>
<p> </p>
<p><font face="宋体"></font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体">本文精简版收录于《程序员》6月刊。</font></p>
<p><font face="宋体"></font></p>
<p><font face="宋体"> </font></p> <img src="http://www.cnblogs.com/stbchina/aggbug/1764159.html?type=1" width="1" height="1" alt=""><p>评论: 0　<a href="http://www.cnblogs.com/stbchina/archive/2010/06/24/agile-testing-on-visual-studio-part-I-a.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/stbchina/archive/2010/06/24/agile-testing-on-visual-studio-part-I-a.html#commentform">发表评论</a></p><p><a href="http://a4.yeshj.com/rd/35451/">软件研发团队管理年会(上海，7.10-7.11)</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/66918/">新版《红楼梦》网播蹊跷叫停 开播时间待定</a><span style="color:gray">(2010-06-24 18:10)</span><br>· <a href="http://news.cnblogs.com/n/66917/">纵谈Linux发行版中的十座火山</a><span style="color:gray">(2010-06-24 18:00)</span><br>· <a href="http://news.cnblogs.com/n/66916/">Apache 每年一度的路演又来中国了</a><span style="color:gray">(2010-06-24 17:51)</span><br>· <a href="http://news.cnblogs.com/n/66915/">微软重金力邀开发人员从iPhone转战WP7</a><span style="color:gray">(2010-06-24 17:34)</span><br>· <a href="http://news.cnblogs.com/n/66914/">Mozilla：Firefox将不支持本地插件</a><span style="color:gray">(2010-06-24 17:09)</span><br></p><p>编辑推荐：<a href="http://kb.cnblogs.com/page/66804/">苹果编程语言和 API 的未来</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>