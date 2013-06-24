---
layout: post
title:  "音乐你的生活：Android QQMusic 设计实录"
date:   2010-11-26 13:48:29
author: 
categories: program
---

## 音乐你的生活：Android QQMusic 设计实录
### by 
### at 2010-11-26 13:48:29
### original <http://news.cnblogs.com/n/82447/>

<p> 在android平台上开发音乐播放器是一个全新的挑战。这次通过android QQ music项目实战，总结出一些方法和经验，希望能够对移动平台的设计尤其是多媒体这块提供一些有价值的参考和帮助。</p>
<p><strong>一、引入产品定义描述（Application  Definition  Statement） </strong></p>
<p> 相信大多数设计师都有过这样的经历：在产品设计过程中，设计师和产品经理不断pk，各抒己见，甚至闹得脸红耳赤、拍桌翻脸，最后项目总结时又因达不到“理想目标“双方深深自责，纷纷表示”缺少交流“。彼此不断pk，交流绝对足够，只是我们缺少有效沟通的工具。如果能在早期建立共识，口水仗爆发的频率就会越少，真正花在产品上的时间也就越多。这一次，我们决定更早地切入，在最开始的产品规划层面，引入“产品定义描述”（关于ap- plication definition statement，请参考《iPhone Human Interface Guidelines》）。</p>
<p><a href="http://cdc.tencent.com/?attachment_id=3235"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image001" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134742622.jpg" border="0" alt="clip_image001" width="630" height="175"></a></p>
<p> 产品定义描述（product definition statement），着重解决三大问题：解决方案、目标受众、产品优势。针对And-roid QQ music，我们与产品经理建立了一个统一的认知：</p>
<p>· 解决方案：移动场景听歌</p>
<p>· 目标用户：android平台用户，大部分是上班族</p>
<p>· 产品优势：随身携带的歌曲列表（收藏夹）和丰富的运营歌曲库</p>
<p><strong>二、细分场景，从场景推导需求，从需求推导设计</strong></p>
<p> 从源头的战略层面上建立了统一的认知，我们接下来开展的工作就有根据了。既然核心是解决“移动场景听歌“的问题，我们必须首先弄清楚什么是”移 动场景“？移动环境和PC环境差异甚大，碎片时间的使用更为突出。用户一天是怎么活动的呢？每次拿起手机听音乐都是什么时间？上班的公车上、走路、晚上睡觉前？在这些点上用户都会做些什么？简单的脑暴可以罗列各个可能的使用场景，但还比较粗糙。场景的构想需要建立在高度的认知上，这时候用研的切入显得相当 关键，用户特征、喜好、使用习惯…对用户越了解，场景就越能贴近真实、越能发现更多的细节。这些都为后续的设计决策提供了有力的依据。</p>
<p><a href="http://cdc.tencent.com/?attachment_id=3239"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image002" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134750774.jpg" border="0" alt="clip_image002" width="621" height="516"></a></p>
<p> 客观构建的场景有很多有意思的发现：</p>
<p>1. 在播放本地歌曲时，用户挑选第一首歌往往比较犹豫，但对后续播放的歌曲却不太在意；</p>
<p>2. 对播放模式，中国的用户会认为这是一种单选的模式选择，而非“开关”组合之意；</p>
<p>3. 抖动换歌很有趣，但是用户几乎不会用到，“谁会在逛街走路时摇手机呢？”</p>
<p> 从场景仔细分析和推敲，很容易就可以明确产品的需求，对应上述三个发现，需求如下：</p>
<p>1. 提供“马上听歌”引导用户直接听歌；</p>
<p>2. 三态按钮实现“列表循环、单曲循环、随机播放”三种模式切换；</p>
<p>3. 忽略抖动换歌这种华而不实的特性。</p>
<p><strong>三、敏捷原型设计，适应与借鉴并行</strong></p>
<p> 用研的输出以及前面的ADS定义为功能筛选和设计取舍提供了强有力的决策依据。在需求框架大致决定的时候，我们接下来就进行方案设计了。正所谓“一图胜千言”，原型有时候会比面面俱到的文档更直观。不同的阶段，我们会进行不同精度的原型设计：</p>
<p><a href="http://cdc.tencent.com/?attachment_id=3240"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image003" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134753749.jpg" border="0" alt="clip_image003" width="598" height="299"></a></p>
<p> 在原型迭代的过程中，需要注意android平台的UI尴尬。与其他平台（iPhone、symbian、windows mobile）不同，an-droid更加开放，不同的厂商不同的ROM版本界面规范并不统一。如果简单地将其他平台的设计规范移植过来，产品体验可能会和整个系统“格格不入”。借鉴了android平台的一些优秀app（比如twitter和new york times）的设计策略，我们决定“适应与借鉴并行“：在大的基础体验上，保证与系统体验一致，尊重平台使用习惯；在细节体验上，尽量克服系统的操作困 难，保证局部体验的流畅。</p>
<p><strong>四、视觉方案</strong></p>
<p><strong> </strong>在整个设计过程中，我们尝试了多套风格方案，比如“梦幻光影”、“清爽夏日”、“木质桌面”“蓝色海洋”等等， 由于beta1开发时间有限，最后选择了偏深绿色主色调的“梦幻光影”作为默认皮肤，这里奉上其他未曾谋面的方案，供参考。</p>
<p><a href="http://cdc.tencent.com/?attachment_id=3241"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image004" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134756901.jpg" border="0" alt="clip_image004" width="618" height="344"></a></p>
<p><strong>4.1 </strong><strong>主要界面模块划分</strong></p>
<p> 视觉方案的第一步是对界面各模块和控件进行作出符合审美原理和需要的的合理划分和尺寸设定，这里重点会落在正在播放界面和歌曲列表界面两块，虽 然两个界面功能和承载的信息都不相同，但是在造作上两个界面跳转和切换是紧密关联的，在各模块划分和空间比例上两个需要统筹安排，一方面考虑上下同样尺寸的“标题行”和底部“控制行 / tab行”一方面要考虑歌曲列表界面的列表选择合适的行高和行数。在保证每一行歌曲信息能够有足够的空间显示的同时，还得保证一屏里面的歌曲行数是整数行。</p>
<p><a href="http://cdc.tencent.com/?attachment_id=3242"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image005" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134757957.jpg" border="0" alt="clip_image005" width="606" height="404"></a></p>
<p> 从上图区域划分和比例可以看出，播放器界面，封面/歌手图片的中心点在整个界面的高度是296，相对于整个界面的高480，这一比例基本是处在0.618：1的黄金分隔点上。</p>
<p><strong>4.2 </strong><strong>正在播放界面</strong></p>
<p><a href="http://cdc.tencent.com/?attachment_id=3244"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image006" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134757298.jpg" border="0" alt="clip_image006" width="597" height="249"></a></p>
<p> 用户使用音乐播放器，最大的关注点一般会落在正在播放/播放器界面上，这块在视觉上是整个产品的核心区域，在很大程度上代表了整款软件的品质、品位和风格信息表达。而专辑封面/歌手头像又是正在播放界面的视觉中心，因此这块在设计上花了比较重的笔墨，用细腻的光照效果与晶莹剔透的质感精心营造了一种符合我们QQ音乐在手机移动操作平台上的气质的效果。</p>
<p><strong>4.3 </strong><strong>系统菜单icon</strong></p>
<p><strong><a href="http://cdc.tencent.com/?attachment_id=3245"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image007" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134758479.jpg" border="0" alt="clip_image007" width="583" height="216"></a></strong></p>
<p> 菜单选项icon是另外一个视觉信息传达比较重要的地方，对整体风格的形成起着重要作用，这里做了两种效果的尝试，一种<strong>A</strong>方案：是空心边框形式，看起来弹出菜单很轻盈，一屏6个选项带文字不会觉得拥挤，缺点是单个图标的轮廓有的地方不连贯，有可能会造成辨识度降低。另外最后选用的方案<strong>B</strong>是 实心剪影的形式，这种表现方式整个icon看起来很整体，比较饱满，识别度较高，不过如果一屏图标过多的话可能会稍有拥挤的感觉，但如果以缩小的方式处 理，对于手指触摸操作来说是不合理的，这里icon作了圆角处理和镂空实心均衡化处理，能在视觉感受上弱化一下可能会产生的拥挤感。</p>
<p><strong> </strong></p>
<p><strong>4.4 </strong><strong>出现“水波纹”的问题</strong></p>
<p><strong><a href="http://cdc.tencent.com/?attachment_id=3246"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image008" src="http://pic003.cnblogs.com/2010/34358/201011/201011261347592.jpg" border="0" alt="clip_image008" width="611" height="187"></a></strong></p>
<p> 色彩显示效果上Android系统的手机由于硬件和技术上的限制和问题，会在某些情况下产生令人抓狂的水波纹问题，在尝试的过程中大致总结了容易造成水波纹的渐变使用，如上图左边两个图块：如果使用规则的径向渐变，程序贴图后极容易出现水波纹，另一种情况是如右边的两图块：渐变色差值过小，就是 说渐变特别的细腻不显著的话也很容易在切图程序贴图后出现明显的水波纹。所以后面在配色和设计过程中可以据此避免尽量这样的情况出现。</p>
<p><strong>4.5 </strong><strong>最终选用方案:</strong></p>
<p><a href="http://cdc.tencent.com/?attachment_id=3247"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border:0px" title="clip_image009" src="http://pic003.cnblogs.com/2010/34358/201011/20101126134800824.jpg" border="0" alt="clip_image009" width="615" height="342"></a></p>
<p><strong>五、后记</strong></p>
<p> 这是我们第一次在android平台上的尝试，为了解决以前传统设计流程带来的问题，我们在项目开始之初就引入了 ADS，在移动场景分析中又得到了用研同学的大力支持，从抽象到具体，从概念到实现，一步一步的开展变得有依有据，对产品，对设计，都是获益良多。</p>
<p> 本项目涉及到两地三方的合作，沟通成本比较大，严重感谢团队的每一位成员，感谢大家全心全意的付出。</p>
<p> 临尾还是希望来句厚颜无耻的大声吆喝：也许，它不是最华丽的播放器，但一定最懂你:在路上，在等候的车站，在前往神秘的美好旅途中…QQ音乐，音乐你的移动生活。</p><p><br>　　相关新闻：<br>　　· <a href="http://news.cnblogs.com/n/82337/">苹果大幅扩张领地，20亿地皮变成水果园</a><span style="color:gray">(2010-11-25)</span><br>　　· <a href="http://news.cnblogs.com/n/80170/">Chrome 即将登陆 Android 平台？</a><span style="color:gray">(2010-11-08)</span><br>　　· <a href="http://news.cnblogs.com/n/79763/">造梦移动互联网</a><span style="color:gray">(2010-11-04)</span><br>　　· <a href="http://news.cnblogs.com/n/81735/">Android的分化问题迫使Rovio开发多版本的愤怒小鸟</a><span style="color:gray">(2010-11-20)</span><br>　　· <a href="http://news.cnblogs.com/n/81374/">诺基亚CEO奉行独立自主战略 不用Android平台</a><span style="color:gray">(2010-11-17)</span><br></p><p>　　本文链接：<a href="http://news.cnblogs.com/n/82447/">http://news.cnblogs.com/n/82447/</a></p><p>　　<a href="http://job.cnblogs.com">程序员找工作，就在博客园</a></p><p>　　<a href="http://a4.yeshj.com/rd/34138/">每天10分钟，轻松学外语</a></p><img src="http://news.cnblogs.com/news/rssclick.aspx?id=82447" width="1" height="1" alt="">