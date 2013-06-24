---
layout: post
title:  "那些被遗漏的Objective-C保留字"
date:   2013-04-29 20:22:00
author: 
categories: program
---

## 那些被遗漏的Objective-C保留字
### by 
### at 2013-04-29 20:22:00
### original <http://blog.devtang.com/blog/2013/04/29/the-missing-objc-keywords/>

<p><img src="http://blog.devtang.com/images/forget.jpeg"></p>

<p>今天翻到很久以前自己在网易博客上写的<a href="http://tangqiaoboy.blog.163.com/blog/static/116114258201110133108545/">这篇文章</a>，惊奇地发现自己都忘记了里面的一些内容。所以我又重新学习了一下，然后改了改内容，挪到这里。</p>

<h2>前言</h2>

<p><a href="http://www.amazon.cn/s?ie=UTF8&amp;search-alias=books&amp;field-author=Steffen%20Itterheim">Steffen Itterheim</a>是<a href="http://www.amazon.cn/Learn-Iphone-and-Ipad-Cocos2d-Game-Development-The-Leading-Framework-for-Building-2D-Graphical-and-Interactive-Applications-Itterheim-Steffen/dp/1430233036/ref=sr_1_1?ie=UTF8&amp;qid=1321168092&amp;sr=8-1">《Learn Iphone and Ipad Cocos2d Game Development》</a>作者。cocos2d和cocos2d-x现在已成为著名的游戏开发引擎。在AppStore上有超过100个游戏是基于Cocos2D。</p>

<p>Steffen Itterheim在<a href="http://www.learn-cocos2d.com/2011/10/complete-list-objectivec-20-compiler-directives">他的博客</a>中总结了 Objective-C 2.0 所有的编译器保留字，并且对这些保留字做了介绍和使用示例。这些保留字如下：</p>

<pre>
@class
@defs
@protocol @required @optional @end
@interface @public @package @protected @private @property @end
@implementation @synthesize @dynamic @end
@throw @try @catch @finally
@synchronized @autoreleasepool
@selector @encode
@compatibility_alias
@”string”
</pre>


<p>我把这些保留字过了一遍，发现很少用到的有 @dynamic @defs @encode @compatibility_alis，所以就给大家介绍一下这几个关键字吧。</p>




<h2>@dynamic</h2>

<p>@dynamic 是相对于 @synthesize的，它们用样用于修饰 @property，用于生成对应的的getter和setter方法。但是@ dynamic表示这个成员变量的getter和setter方法并不是直接由编译器生成，而是手工生成或者运行时生成。</p>

<p>示例如下：</p>

<span></span><div><table><tr><td><pre><span>1</span>
<span>2</span>
<span>3</span>
<span>4</span>
<span>5</span>
<span>6</span>
<span>7</span>
</pre></td><td><pre><code><span><span>@implementation</span> <span>ClassName</span>
</span><span><span>@synthesize</span> <span>aProperty</span><span>,</span> <span>bProperty</span><span>;</span>
</span><span><span>@synthesize</span> <span>cProperty</span><span>=</span><span>instanceVariableName</span><span>;</span>
</span><span><span>@dynamic</span> <span>anotherProperty</span><span>;</span>
</span><span>
</span><span><span>// method implementations</span>
</span><span><span>@end</span>
</span></code></pre></td></tr></table></div>


<h2>@defs</h2>

<p>@defs 用于返回一个Objective-C类的struct结构，这个struct与原Objective-C类具有相同的内存布局。就象你所知的那样，Objective-C类可以理解成由基本的C struct加上额外的方法构成。</p>

<p>示例代码如下：</p>

<span></span><div><table><tr><td><pre><span>1</span>
</pre></td><td><pre><code><span><span>struct</span> <span>{</span> <span>@</span><span>defs</span><span>(</span> <span>NSObject</span><span>)</span> <span>}</span>
</span></code></pre></td></tr></table></div>


<p>你可能会想，什么情况下才会需要使用这个关键字。答案是涉及非常底层的操作或优化的时候才会用到。像如这篇讨论<a href="http://www.mulle-kybernetik.com/artikel/Optimization/opti-3-imp-deluxe.html">Objective-C如何做缓存优化</a>的文章中，就用到了该关键字。</p>

<h2>@encode</h2>

<p>@encode 是用于表示一个类型的字符串，对此，苹果有专门的<a href="http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html">介绍文档</a></p>

<p>示例如下：</p>

<span></span><div><table><tr><td><pre><span>1</span>
<span>2</span>
<span>3</span>
<span>4</span>
<span>5</span>
<span>6</span>
<span>7</span>
<span>8</span>
<span>9</span>
<span>10</span>
</pre></td><td><pre><code><span><span>-</span><span>(</span><span>void</span><span>)</span> <span>aMethod</span>
</span><span><span>{</span>
</span><span>    <span>char</span> <span>*</span><span>enc1</span> <span>=</span> <span>@encode</span><span>(</span><span>int</span><span>);</span>                 <span>// enc1 = &quot;i&quot;</span>
</span><span>    <span>char</span> <span>*</span><span>enc2</span> <span>=</span> <span>@encode</span><span>(</span><span>id</span><span>);</span>                  <span>// enc2 = &quot;@&quot;</span>
</span><span>    <span>char</span> <span>*</span><span>enc3</span> <span>=</span> <span>@encode</span><span>(</span><span>@selector</span><span>(</span><span>aMethod</span><span>));</span>  <span>// enc3 = &quot;:&quot;</span>
</span><span>
</span><span>    <span>// practical example:</span>
</span><span>    <span>CGRect</span> <span>rect</span> <span>=</span> <span>CGRectMake</span><span>(</span><span>0</span><span>,</span> <span>0</span><span>,</span> <span>100</span><span>,</span> <span>100</span><span>);</span>
</span><span>    <span>NSValue</span> <span>*</span><span>v</span> <span>=</span> <span>[</span><span>NSValue</span> <span>value:</span><span>&amp;</span><span>rect</span> <span>withObjCType:</span><span>@encode</span><span>(</span><span>CGRect</span><span>)];</span>
</span><span><span>}</span>
</span></code></pre></td></tr></table></div>


<h2>@compatibility_alis</h2>

<p>@compatibility_alis 是用于给一个类设置一个别名。这样就不用重构以前的类文件就可以用新的名字来替代原有名字。</p>

<p>示例如下：</p>

<span></span><div><table><tr><td><pre><span>1</span>
</pre></td><td><pre><code><span><span>@</span><span>compatibility_alias</span> <span>AliasClassName</span> <span>ExistingClassName</span>
</span></code></pre></td></tr></table></div>


<h2>@autoreleasepool</h2>

<p>@autoreleasepool 是用于ARC下代替 NSAutoreleasePool的保留字，我把它写在这里，是想告诉那些以为ARC慢的同学，在苹果的<a href="http://developer.apple.com/library/ios/#releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html">这篇官方文档</a>中有提到， @autoreleasepool 比 NSAutoreleasePool快6倍。当然，文档中也提到，ARC下不止Autorelease Pool的实现变快了，retain和release也快很多。如果你还没有在工程中使用ARC，推荐看看我的<a href="http://blog.devtang.com/blog/2013/03/27/should-we-use-arc/">《是否应该使用ARC》</a>。</p>

<h2>Cheat Sheet</h2>

<p>有一个热心者根据他的博文，制作了一张<a href="http://maniacdev.com/cheatsheetobjccd.pdf">《Objective-C 2.0保留字速查表》</a>，对于新手来说，把这张速查表打印出来，对于熟悉现在的保留字还是很有用的，它的下载地址是：<a href="http://maniacdev.com/cheatsheetobjccd.pdf">http://maniacdev.com/cheatsheetobjccd.pdf</a></p>

<p>五一节到了，祝大家节日快乐！</p>