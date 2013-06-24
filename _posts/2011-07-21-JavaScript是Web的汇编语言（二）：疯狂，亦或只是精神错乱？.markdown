---
layout: post
title:  "JavaScript是Web的汇编语言（二）：疯狂，亦或只是精神错乱？"
date:   2011-07-21 08:40:28
author: 为之漫笔
categories: program
---

## JavaScript是Web的汇编语言（二）：疯狂，亦或只是精神错乱？
### by 为之漫笔
### at 2011-07-21 08:40:28
### original <http://www.cn-cuckoo.com/2011/07/21/javascript-is-assembly-language-for-the-web-ii-2531.html>

<p style="text-align:right">原文地址：<a href="http://www.hanselman.com/blog/JavaScriptIsAssemblyLanguageForTheWebPart2MadnessOrJustInsanity.aspx">JavaScript is Assembly Language for the Web: Part 2 – Madness or just Insanity?</a></p>
<p><strong>有些人认为“<a href="http://www.cn-cuckoo.com/2011/07/20/javascript-is-assembly-language-for-the-web-i-2511.html">JavaScript是Web的汇编语言</a>”完全是精神病说的话。</strong>为此，我询问了几位JavaScript权威，比如Brendan Eich（JavaScript之父）、Douglas Crockford（JSON之父），还有Mike Shaver（Mozilla技术副总裁）。以下都是从个人邮件里摘过来的，得到了以上几位的许可。</p>
<p><a href="http://en.wikipedia.org/wiki/Mike_Shaver">Mike Shaver</a>：</p>
<div style="border-left:2px solid #ddd;margin-left:1em;padding-left:1em">以前我就听说过这种比较，我认为很大程度上确实如此。但是，这种说法忽视了JS开发人员与机器之间的人机工程学方面的大量努力，因为汇编语法设计得没有那么人性化（特别是现代的汇编语言）。</div>
<p><a href="http://en.wikipedia.org/wiki/Brendan_Eich">Brendan Eich</a>：</p>
<div style="border-left:2px solid #ddd;margin-left:1em;padding-left:1em">
<p>几年前，我曾说过“JS是Web的x86”（好像是在一次JSConf上），不过我不敢说我是第一个这么说的。（Nick Thompson今年也在Hacker News中这么说过。）</p>
<p>关键在于，JS确实在按照我们想的，越来越往低级方向发展了。但它也具备高级的特性。</p>
<p>Shaver说得没错，汇编缺少可靠的宏处理器，因此不适合程序员，也不够安全。但JS可不是这样。所以，这个比喻需要加点限制条件，不然就要闹出笑话来了。</p>
<p>无论从高级函数式编程还是内存安全角这个角度看，还是从低级特性，像类型化数组以及即将成为现实的ES中类型化数组的扩展、二进制数据，等等来说，JS都是一个比汇编更加强大的编程语言。当然了，内存安全是首要的区别。</p>
</div>
<p><a href="http://en.wikipedia.org/wiki/Douglas_Crockford"><span></span>Douglas Crockford</a>：</p>
<div style="border-left:2px solid #ddd;margin-left:1em;padding-left:1em">
<p>就这个问题来说，我觉得说JavaScript是Web的虚拟机更接近一些。过去我们一直都把Java的JVM看成是Web的虚拟机，但结果呢，JavaScript才是。</p>
<p>从提供代码安全的角度说，JavaScript的解析器比JVM的字节码验证器更有效。就兑现“编写一次，到处运行”这个诺言来看，JavaScript更出色；这或许正是因为它是在较高层次上运行，才得以避免触及一些底层的棘手问题。因为它把剩下的事儿都交给图灵去解决了。</p>
<p>当然啦，也有不少人始终不肯承认JavaScript能把一切都处理好，我过去就是这样一个人。而现在我则不断地被眼前这种百花齐放的景象震撼着。</p>
</div>
<p>Brendan Eich，补充：</p>
<div style="border-left:2px solid #ddd;margin-left:1em;padding-left:1em">
<p>Doug说源代码强过字节码，说得太好了。很早以前，我的朋友，加州大学欧文分校的Michael Franz教授就指出了Java验证器O(n^4)级别的复杂性（计算机时钟周期失控，拒绝服务）。精简之后的JS呢，确实传输更便捷，而且词法/语法分析也相当快。</p>
<p>（JS）源代码作为“字节码”同样也避免了Java字节码的一个很傻的问题：冻结设计不良的Java低级形式，导致高级形式的源代码也无法解决这个问题。换句话说，Java唯恐破坏其字节码的兼容性。而这严重影响了Java内部类及其泛型的设计。——不管怎么说，Sun最后还是破坏了字节码的兼容性。
</p></div>
<p>以下是前一段时间<a href="http://nixweb.com/">Nick Thompson</a>在YCombinator说过的话：</p>
<div style="border-left:2px solid #ddd;margin-left:1em;padding-left:1em">
<p>这只是我个人的看法：我花了自己两年时间，想尽可能让JVM能够与JavaScript互通。当时的Netscape有不少人认为字节码作为移动代码的基础比较好。但Sun从头搭建了自己大而全的软件体系，把问题搞得很复杂。他们没有想让Java与其他语言沟通，更别提让Java能嵌入其他软件中了。他们的字符串处理代码都是用一种解释型语言来写，而不肯用C来玷污自己！有什么我就说什么，Netscape，这个当时Java唯一的一个大客户，在Sun眼里无非就是一个用来实现他们取代Windows梦想的工具而已。所有想用Java的人都是自己给自己找罪受。</p>
<p>在此期间，Brendan一个人干了10个工程师，外加3个客户服务人员的活儿，同时还要关注Web作者在乎的一些事，比如把JS代码混合到HTML中、即时加载、与浏览器其他模块的集成等等，此外还要协调其他浏览器厂商，以便让JS成为一个开放标准。</p>
<p>因此，今天的JS作为Web的x86汇编程序，并没有像它本该的那样完美，但你通过它真能把事情稿定（GWT就是一个最明显的例子）。这可以说是一个经典的“更差就是更好”的例子，只不过Java也就是从下往上那么看起来更好罢了。而JS则在此期间取得了相当不错的成就。要想取代它的地位可没那么容易。</p>
</div>
<p>当然，这个比喻不一定准确。JavaScript代码无论从外观到行为，肯定不像ASM。但作为一个比喻，至少可以说明：</p>
<ul>
<li>JavaScript无所不在；</li>
<li>它速度快而且越来越快；</li>
<li>JavaScript酷似低级的Web编程语言；</li>
<li>它可以通过手工编写，也可以从另一种语言编译而来。</li>
</ul>
<p>诸如此类的话题也经常在Hacker News中出现：</p>
<div style="border-left:2px solid #ddd;margin-left:1em;padding-left:1em">
“现在的JavaScript其实就是客户端的汇编语言。想改它太难了，所以得想办法开发一些工具来解决这个问题。”——<br>
<a href="http://news.ycombinator.com/item?id=2451594">jonnycat</a></div>
<p>好啦，能听到如此有见地、有深度，而又详尽的讨论，该满足了吧。亲爱的读者，你们太棒了。</p>