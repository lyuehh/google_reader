---
layout: post
title:  "[原]详图实证：再谈JavaScript的语源问题"
date:   2011-08-03 09:12:14
author: aimingoo
categories: program
---

## [原]详图实证：再谈JavaScript的语源问题
### by aimingoo
### at 2011-08-03 09:12:14
### original <http://blog.csdn.net/aimingoo/article/details/6654742>

 
<p align="left"><strong>【本文发表于《程序员》2011.03期】</strong></p>
<p>　</p>
<p>有两个错误的观点，其一是“JavaScript在语源上继承自Cmm”。这个错误的观点主要的来自于以下途径（部分）：</p>
<ul>
<li>2002年10月7日的《Wired Magazine（连线杂志）》的一份名为“Mother Tongues”的图；</li><li>O’Reilly公布的“The History of Programming Languages图；</li><li>Levenez.com公布的“Computer Language History”；</li><li>……</li></ul>
<p>第二个错误的观点，即“Nombas公司的Espresso Pages（浓咖啡版网页）以及其内置的脚本（CEnvi，Cmm语言的开发环境）是WWW上首次使用的脚本语言”。这个错误的观点主要来自于：</p>
<ul>
<li>在Nombas网站中关于Cmm、CEniv以及ScriptEase等技术与产品的一篇介绍文字；</li><li>Brent Noorda先生（Nombas公司总裁）关于发布Espresso Pages的新闻组消息。</li></ul>
<p>这些明显是不太靠谱的一面之词。但是进一步援引这些内容的结果是：错误的观点被一再重述，似乎已经快要变成事实了，例如《JavaScript高级程序设计》一书已经白底黑字地将这些内容记述在了“JavaScript简史”之中。</p>
<p>本文将简明而完整地厘清上述观点。</p>
<p><strong>　</strong></p>
<h3>一、真相：Netscape早在Espresso Pages发布之前就实现了主要的WEB开发特性</h3>
<p>首先，这些问题的一部分相当容易说清楚，例如“是不是Espresso Pages首次在WEB中使用了脚本语言”。因为关于Espresso Pages的这个新闻组消息还有着明确的时间（1995.11.27）（*1）：</p>
<p><img alt="Espresso pages的新闻" src="http://hi.csdn.net/attachment/201108/2/1804_13122780842ILy.png"></p>
<p>那么，Netscape Navigator 2.0中对脚本的支持又是如何出现的呢？考察NN2此前发布的各个Beta版就可以发现(*2)：至迟到NN2 Beta2，网页内嵌脚本的概念就相当清晰，并且实现完整了。我们先看看在1995.10.10发布的NN2 Beta1这个版本。它能做什么呢？它其实已经具备了标签内联脚本功能，如下图所示：</p>
<p><img alt="NN2 Beta1中的JavaScript" src="http://hi.csdn.net/attachment/201108/2/1804_1312278085LZPj.png" height="395" width="627"></p>
<p>相对完整的脚本特性，例如&lt;script&gt;标签与function函数等，则要等到NN beta2才支持，其发布时间为1995.11.04：</p>
<p><img alt="NN2 Beta2中的JavaScript" src="http://hi.csdn.net/attachment/201108/2/1804_13122780855n1N.png" height="470" width="656"></p>
<p>上面的示例说明了beta2中的脚本已经开始支持在函数内通过this引用来得到window对象，而frames、self这些成员在这时的window对象中就已经存在，这些事实上就是最最原始的DOM。</p>
<p>所以尽管Nombas是抢在了Netscape Navigator 2.0正式发布（1996.01.23，JavaScript 1.0随该版本发布）之前，公开了在NN2上嵌入第三方脚本引擎的方式来实现的Espresso Pages，但是与我们今天讨论的WEB开发相关的一切，在这之前、在NN Beta2中就已成事实了。而这个Netscape Navigator Beta2的发布时间，比Espresso Pages的发布早了近一个月。</p>
<p>《JavaScript高级程序设计》一书中不但认为Espresso Pages是WWW上“首次使用脚本语言的标志”，还继续讲道：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>相信当初的Nombas公司不太可能意识到，他们这种网页中嵌入脚本的想法会在很大程度上左右未来因特网的发展。</strong></p>
</blockquote>
<p>这段“文字游戏”很容易让读者将“网页嵌入脚本”的设想归功于Nombas，以及具体的那个脚本语言Cmm。但如同上面所说的，这种设想并非源自Espresso Pages。这些想法的确左右了因特网的发展，但是没有任何证据显示Nombas与这一起源存在关系。</p>
<p>　<strong></strong></p>
<h3>二、真相：Cmm与JavaScript完全无关，而ScriptEase不过是追随者</h3>
<p>Cmm的确是一种脚本语言，比JavaScript出现得早很多。但这个脚本语言并不是嵌入式的，它需要一个名为CEniv的可执行程序，来运行这种扩展名为.cmm的脚本代码文件。因为Cmm一开始并没有设计为嵌入式语言，所以这些称为Espresso Pages的东西其实是一些可供下载的Demos，使用者需要安装CEnvi的某个版本，并通过一些配置来使Demo能在NN2 beta中使用。原文是：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>but for now these demos use our CEnvi for Windows application as a helper. Instructions are on the page for how to configure, including a download of a demo of the Cmm interpreter.</strong></p>
</blockquote>
<p>现在已经再也找不到这些下载和操作指南了。不过我们这里并不想考察Espresso Pages的效果，而是想说清他当时所使用的Cmm语言与JavaScript语言有没有什么关系。因为当时JavaScript正在开发之中，而Cmm已经出现了三年之久，那么JavaScript的语言设计是否参考了Cmm呢？</p>
<p>目前可以下载到的CEnvi包括以下版本：</p>
<blockquote dir="ltr" style="margin-right:0px">
<div>
<p>CEnvi 1.0                        – 1993.08.09</p>
<p>CEnvi 1.008                     – 1993.12.21</p>
<p>CEnvi 1.009 for DOS/Win/OS2    – 1994.11.29</p>
<p>CEnvi 2.0 for DOS/Win/OS2      – 1995.03.29</p>
<p>CEnvi 2.10 for DOS/Win/NT/OS2  – 1995.10.17</p>
<p>CEnvi 2.11 for DOS             – 1996.02.20</p>
</div>
</blockquote>
<p>在NN2之前的是CEnvi 2.0，NN2正式发布之后则有CEnvi 2.11。我们只需要考察这其中的Cmm语言的特性以及其进化的路线，就可以了解这一阶段中二者可能存在的相互影响。根据Cmm的语言手册(*3)，其主要的发展为：</p>
<ul>
<li>1.0 ~ 2.0：添加了字符串中的转义字符(Escape Sequences for Characters)</li><li>2.0 ~ 2.1：无变化</li><li>2.1 ~ 2.11：无变化</li></ul>
<p>也就是说，Cmm语言从1993年随CEnvi发布一直到1996年2月，下面的语言特性并没有任何实质性的变化：</p>
<ul>
<li>与C语言相似的运算符（包括位运算、布尔运符和三元”?:”运算符等）、注释、字符串表达方式；</li><li>有Byte、Integer、Float等数据类型，不需要预声明变量，变量在第一次赋值时创建并关联类型；</li><li>有数组与字符串类型，数组不需要预定义长度或元素类型；字符串是字符类型的数组，可以通过数组下标存取；可以通过”...”方式，或用{...}声明一系列单个字符的方式来声明字符串；</li><li>有结构(Structures)数据类型，用“.”字符存取其字段；</li><li>有if、while、do、switch以及for等等语句，有GOTO语句；</li><li>可以声明函数，但不需要用function关键字开始，也不需要声明参数类型，参数个数必须是确定的。</li><li>有#include与#define等预声明语句；在任意函数之外的代码为初始化代码，其声明的变量为全局变量；在初始化代码之后执行的第一个函数为main(ArgCount, ArgStrings)。</li></ul>
<p>可见Cmm语言的多数概念来自于C语言，而与JavaScript所体现的面向对象特性毫无关系。例如Cmm的数组是一个独立的数据类型，取得数组的长度应使用GetArraySpan(aArr)函数，而不是取数组这个对象的一个属性aArr.length。通过对Cmm语言特性的详细分析可见：即使JavaScript与Cmm存有一些相似的部分，那么最多也是其借鉴自C语言的表达式运算、语句语法等等，而这些并不足以成为“JavaScript语源自Cmm”的证据。</p>
<p>而在这个话题中的另一个角色，是名为“ScriptEase”的脚本语言，它也是最早符合ECMAScript 262标准的语言之一——简单的说，它也是一个JavaScript语言。而且，Nombas公司正是因此将Cmm与JavaScript挂上关系，因为他们声称：ScriptEase就是Cmm，只不过是换了个名字。</p>
<p>真的只是换了一个名字这样简单吗？</p>
<p>此前我们说过，Cmm的最后一个可见的发布包含在CEnvi 2.11中，发布时间是1996.02.20，其语言特性与上述列表一致（没有变化）。这个语言是什么时候更名的呢？答案是：晚于1997.02.06。因为在1997年4月至6月间，Nombas公司发布了CEnvi的3.0版本，在这个版本中首次使用到ScriptEase这个名字，其中的名为HISTORY的文件——该文件被打包的时间（令人惊奇地）是1997.02.06——描述这次更名的缘由：</p>
<p><img alt="CEnvi更名成ScriptEase" src="http://hi.csdn.net/attachment/201108/2/1804_1312278086I709.png"> </p>
<p>那么在这个版本中的ScriptEase又具有什么样的语言特性呢？答案是：与Cmm 1.0~2.11相比仍然没有变化。对比该ScriptEase 3.0发布包中的语言手册《The ScriptEase Language》与Cmm 2.11中对应文档可知：数组、字符串、函数、GOTO语句等等一切，仍跟Cmm中一模一样。</p>
<p>但是这个原来叫Cmm的语言摇身一变，改名叫ScriptEase了。现在，请看下面这样的代码（语言手册对比，左侧为ScriptEase 3.0 - 1997.04.15文档，右侧为Cmm 2.11 – 1996.02.20文档）：</p>
<p><img alt="没有变化：对比Cmm与ScriptEase实际的代码" src="http://hi.csdn.net/attachment/201108/2/1804_1312278086J7pP.png" height="280" width="695"></p>
<p>相信任何一个看过JavaScript代码的人都会明白，这个名为Cmm/ScriptEase的语言，所采用的函数声明的语法绝非JavaScript语法，也绝对不可能符合ECMAScript所定义的规范。而此时已是1997年，JavaScript 已经发布了两年，ECMAScript标准已即将发布（1997.06）。虽然现在的Cmm已经更名为ScriptEase，但在其语言手册中仍然找不到Object这样的词汇，代码上仍然使用着最初的函数声明语法。</p>
<p>那么，这样的一门语言又是何时摇身一变，以至于被误认为“JavaScript的语源”的呢。这一时间还要推进到半年之后，也就是1997年12月。这时ScriptEase发布了它的4.0版，在目前可以找到的一个名为“ScriptEase:WebServer Edition 4.01 – 1997.12.08”版本中（准确的说，我并没有找到3.0至4.0的全部发布过程），终于有了一份手册，开始采用如下的语法：</p>
<p><img alt="ScriptEase很晚才正式地支持JavaScript的语法" src="http://hi.csdn.net/attachment/201108/2/1804_1312278087LhP0.png" height="194" width="620"></p>
<p>正是在这份ScriptEase官方手册中，写道：</p>
<p> <img alt="ScriptEase的发布" src="http://hi.csdn.net/attachment/201108/2/1804_13122780878AY2.png" style="width:616px;height:102px"></p>
<p>现在这一切还需要解释吗？以下两个言论：</p>
<ul>
<li>ScriptEase在从3.0到4.0的过程中，采用了ECMAScript标准，进而实现了JavaScript的一个版本；和</li><li>JavaScript语源自Cmm，继承了后者语言特性</li></ul>
<p>之中，后者是何等的站不住脚？！然而，它正是这样地画在了O’Reilly的图册里(*4)：</p>
<p><img alt="O&#39;Reilly导致了更大范围的误解" src="http://hi.csdn.net/attachment/201108/2/1804_1312278088sjlN.png" height="409" width="680"></p>
<p>　</p>
<h3>三、真相：Nombas公司的广告做得太好</h3>
<p>大概是2001年前后，Nombas官方网页对其ScriptEase的介绍中，使用着一份名为“History of ScriptEase and JavaScript”的文档(*5)，但这个网页使用着与该文档标题完全不同的TITLE图片：</p>
<p><img alt="Nombas官方：名不符实的图片" src="http://hi.csdn.net/attachment/201108/2/1804_13122780959uiZ.png" style="width:642px;height:264px"></p>
<p>这篇文章的序言写得中规中矩。大意是说1992年到1993年间，Nombas开发了一个Cmm语言。不过这里讲到了该语言是“embeddable scripting language”，如前所述，这时的Cmm并没有嵌入式的特性。但是从后来Nombas总是同时发布CEniv的不同操作系统平台版本来看，Cmm语言是采用了跨平台的语言（例如C）编译的。而“Cmm随CEnvi发布不同平台版本”并不能作为它“具有嵌入特性”的充分证据。再退一步来看，即使Cmm具备了嵌入式语言特性，可作为独立模块来嵌入到CEnvi发布，也并不表明它可以嵌入到后来的Netscape
 Navigator中，因为后者在当时是闭源的，没有可能让CEnvi来“嵌入”。</p>
<p>然而这一点，在《JavaScript高级程序设计》中被写成：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>When the popularity of Netscape Navigator started peaking, Nombas developed a version of CEnvi that could be embedded into web pages.</strong></p>
<p><strong>（在Netscape Navigator受到人们的狂热追捧之际，Nombas公司开发了能够嵌入到Web页面中的CEnvi版本。）</strong></p>
</blockquote>
<p>可惜这并非事实。关于这一点，Nombas的原文是：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>When Netscape's first commercial browsers were released we made a version of CEnvi that could handle short scripts embedded within web pages.（当Netscape的第一个商业浏览器发布时，我们创建了一个嵌入在WEB页面中的、能够操作简短脚本的CEnvi版本。）</strong></p>
</blockquote>
<p>Nombas并没有在这里指明“first commercial browsers”是Netscape的哪一个具体版本。因此这段文字很容易让人误解为NN 1.0发布的时候，CEnvi就已经可以实现网页内嵌脚本了。而事实上，所谓的“CEnvi定制版本”根本就没有出现过，即使曾经有过，也不过是Nombas在NN 2.0 beta中玩的一些小手脚。进一步地说，这时的Cmm、CEnvi以及后文中的&quot;Espresso Pages&quot;，都只是Nombas在NN 2.0 Beta发布阶段的一些技术尝试而已。</p>
<p>然而，这篇Nombas的原文又继续谈论“网页内嵌脚本”的一些特点。例如：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>By embedded scripts within the page we allowed the client side to handle processing, rather than making all dynamic interaction happen on the server. This brought immediate client-side interaction with the user.（使用网页中的嵌入脚本，我们允许客户端控制一些东西，这比让所有动作发生在服务器端更好。这强化了客户端与用户之间的交互。）</strong></p>
</blockquote>
<p>这些特性描述也就仅仅是对这类技术的一个概述而已，并不表明什么有“原创”、“独创”或“初创”的意义（注意这段文字的未尾使用了“句号”）。但是接下来，Nombas耍了一个小花招，在文章中说：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>The advantages of client-side handling were made obvious by Nombas&#39; &quot;Espresso Pages&quot;, and Netscape soon began work on their own version, which they called LiveScript, and then renamed to JavaScript just before its final release.（Nombas的&quot;Espresso
 Pages&quot;使客户端控制的优势更加明显，接下来（and?）Netscape很快在他们自己的版本上开始工作，他们称之为LiveScript，然后就在正式版本之前更名为JavaScript。）</strong></p>
</blockquote>
<p>注意，“Netscape很快在他们自己的版本上开始工作”之前的“and”，这里可以理解为“接下来”，或者“并且”，或者仅仅是语气上的连贯，但无论如何，这此前一个“逗号”便暗示了“their own version（他们自己的版本）”与前面所述的一切存在“必然的”关系。然而如本文此前所述的：</p>
<ul>
<li>“Espresso Pages”中网页内嵌脚本的想法，在NN Beta2中就已经成熟了，而后者早前了一个多月就发布了，谈不上受到了“Espresso Pages”的影响。</li></ul>
<p>同样的，正是由于这个“and”以及逗号，使得LiveScript与后面的JavaScript“变成了”Cmm的一个后来者，被错误地解读成了“存在语源上的关系”。而本文此前的分析说明：</p>
<ul>
<li>事实上是Cmm受到了JavaScript的影响：ScriptEase在从3.0到4.0的过程中，采用了ECMAScript标准，进而实现了JavaScript的一个版本。</li></ul>
<p>正是Nombas原文中的措辞，不可避免地让人认为LiveScript是基于&quot;Espresso Pages&quot;思想的（或受到其影响的）一个版本，进而发展成为JavaScript。但是事实上：</p>
<ul>
<li>JavaScript最早被称为Mocha（魔卡），这是项目的代码名。这个名字一直用到NN2.0 beta 2发布之前（95.11.04）——包括在beta 1中弹出的错误框上，还可以看到Mocha的名字。</li><li>NN2.0 beta2发布的时候使用了LiveScript这个内部名称。</li><li>随后Netscape与SUN共同发布声明（1995.12.04），正式启用了JavaScript这个名字。直到NN2.0 beta4发布时（1995.12.20），其文档与软件界面中才开始统一使用JavaScript这个名字。</li></ul>
<p>这一切，与“Espresso Pages”是否出现，有什么样的关系吗？——毫无关联！</p>
<p>　</p>
<h3>四、最后的定论，Brendan Eich的澄清</h3>
<p>在Wiki Talk中保留着Brendan Eich的一段对话(*6)，足以为Cmm与JavaScript之间的关系做最终的澄清。</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>Hello. I first met Brent Noorda in late 1996, when Netscape </strong><a href="http://cgi.netscape.com/newsref/pr/newsrelease289.html"><strong>brought</strong></a><strong> JavaScript to ECMA for standardization. I had never heard of NOMBAS or its
 products before then. When I created JS in May 1995 (in about ten days for the core language implementation; the rest of that year was consumed by the DOM and browser embedding work), my influences were awk, C, HyperTalk, and Self, combined with management
 orders to &quot;make it look like Java.&quot;</strong></p>
</blockquote>
<p>其一是与Cmm/Nombas的关系：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>我第一次见到Brent Noorda是在1996年底，……在此之前我从未听说过NOMBAS或它的产品。</strong></p>
</blockquote>
<p>其二是JS语言的创建过程，以及大概的时间：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>在1995年5月开始创建JS的时候（大约10天用于核心语言实现，其它的时间主要是用在DOM以及浏览器嵌入方面）。</strong></p>
</blockquote>
<p>其三是关于JS语源的最终结论：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>我的影响主要来自于awk、C、HyperTalk和Self，以及主管们所要求的“使它看起来像Java一点”。</strong></p>
</blockquote>
<p>关于第三点，JavaScript“像”Java，而不是“语源自”Java，可以由《JavaScript Working Document 1.0》(1996.01.22)，以及其原始的、Netscape公司的官方文档《JavaScript Authoring Guide》文档中的原文(*7)：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>The JavaScript language resembles java, …</strong></p>
</blockquote>
<p>以佐证。JavaScript在语法上与Java、Awk和Perl都有一定“形似”，这可以由《JavaScript Language Specification V1.1》（1996.11.18，Brendan Eich参与编订）文档中的原文(*8)：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>Borrows most of its syntax from Java, but also inherites from Awk and Perl</strong></p>
</blockquote>
<p>以佐证。最后，关于Brendan Eich所言：</p>
<blockquote dir="ltr" style="margin-right:0px">
<p><strong>my influences were awk, C, HyperTalk, and Self</strong></p>
</blockquote>
<p>其中的关键语言特性——原型继承、动态类型和动态数据绑定等，并未出现在Netscape Navigator 2.0及其中的JavaScript 1.0中，因此这里不再详究。</p>
<p>　</p>
<p><strong>　</strong></p>
<p><strong>参考：</strong></p>
<p>*1：<a href="http://embedded.eecs.berkeley.edu/Respep/Research/dds/Hyper-RNweb/0052.html">http://embedded.eecs.berkeley.edu/Respep/Research/dds/Hyper-RNweb/0052.html</a></p>
<p>*2：关于Netscape Navigator 2.0，其Beta版的发布时间采用安装包文件的打包时间，其正式版的发布时间采用官方公告时间。</p>
<p>*3：关于Cmm语言的特性与版本变更的相关结论，是通过对照各个CEnvi版本中的Cmm语言手册（CEnvi Demo Manual, Chapter 2: Cmm Language Tutorial）而得出的。</p>
<p>*4：<a href="http://oreilly.com/news/languageposter_0504.html">http://oreilly.com/news/languageposter_0504.html</a></p>
<p>*5：<a href="http://web.archive.org/web/20040618115018/http://www.nombas.com/us/scripting/history.htm">http://web.archive.org/web/20040618115018/http://www.nombas.com/us/scripting/history.htm</a></p>
<p>*6：<a href="http://en.wikipedia.org/wiki/Talk:ECMAScript#Origins_of_LiveScript">http://en.wikipedia.org/wiki/Talk:ECMAScript#Origins_of_LiveScript</a></p>
<p>*7：<a href="http://tecfa.unige.ch/guides/js/js10.pdf">http://tecfa.unige.ch/guides/js/js10.pdf</a></p>
<p>*8：<a href="http://www.planetpdf.com/codecuts/pdfs/tutorial/jsspec.pdf">http://www.planetpdf.com/codecuts/pdfs/tutorial/jsspec.pdf</a> </p>

            <div>
                作者：aimingoo 发表于2011-8-2 18:12:14 <a href="http://blog.csdn.net/aimingoo/article/details/6654742">原文链接</a>
            </div>
            <div>
            阅读：8040 评论：6 <a href="http://blog.csdn.net/aimingoo/article/details/6654742#comments">查看评论</a>
            </div>