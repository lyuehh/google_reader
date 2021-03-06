---
layout: post
title:  "JavaScript的未来方向之观察"
date:   2011-07-12 02:53:39
author: 
categories: program
---

## JavaScript的未来方向之观察
### by 
### at 2011-07-12 02:53:39
### original <http://hax.iteye.com/blog/1122103>

最近每次去杭州，都有爱民做东道，这次吃羊排不亦乐乎。席间不免聊起技术话题，爱民问我对JavaScript的未来方向的看法，特别是像Class、Inheritence等静态特性是否会引入语言？
<br>
<br>这确实是一个令人关心的问题。很长一段时间以来，ES4的难产和分歧让人始终对JavaScript的未来感觉蒙有一层面纱，即使ES5.1都已经发布，我们仍不免心存疑虑。
<br>
<br>不过，根据我的观察，从ES5所做的语言的改进，到Harmony的一些草案，加上一些JS社区领军人物的blog和speech作为印证，都证明JavaScript的未来方向已经是比较明朗的。
<br>
<br>任何一门语言的演化都是一件困难的事情，特别JS这样一门已经10年历史的语言，而且是Web的唯一编程语言，其历史包袱是很重的。所以不难得出这样的原则：在修复bug和引入新特性的同时必须保持实践上的兼容性。此外，在标准制定上参考和吸取社群的实践经验已经成为了共识。对于JS语言来说，可资参考的，比如CommonJS，比如各大前端JS库，比如Coffeescript，比如Python/Smalltalk/Ruby等其他语言。
<br>
<br>
<br>具体来看ES的发展方向大概是以下三个方面：
<br>
<br>
<br>1. API扩展。如JSON、数组上的forEach等迭代方法、RegExp的增强等。这个部分相对比较简单，主要是从厂商标准（特别是Mozilla的JavaScript扩展），一些事实标准JS库（如JSON）中吸取API，不赘述。
<br>
<br>
<br>2. 使得ES成为一种真正的通用编程语言，而不是仅仅依附于host环境的脚本语言，最主要就是取消host/native对象的分野，让<strong>ES本身也可以实现平台对象</strong>（比如DOM接口）。
<br>
<br>ES5在这方面的努力是加入了get/set accessor以及Object.defineProperty等一系列特性。
<br>
<br>在此之前，开发者是无法做出类似innerHTML这样的属性接口的（除了使用厂商特定的扩展）。开发者也无法控制属性是否只读、是否可枚举、是否可删除（一个直接后果是对Object.prototype的扩展成为了一个罪大恶极的举动）。ES5弥补了这方面的缺陷。
<br>
<br>不过这只是第一步。仍然有一些host接口是无法实现出来的，例如类Array的接口（各种Collection的下标存取），或可动态变化的接口（比如localStorage/sessionStorage接口）。
<br>
<br>而Harmony引入了Proxy（动态代理），提供了完整的元编程能力。由此，几乎所有的host对象接口都可以直接由JavaScript实现出来。【Proxy的强大当然不止于此，另文阐述。】也就是说，这让JS开发者，特别是JS库的开发者获得了与平台（浏览器）提供商同等的能力。<strong>可以想象，在拥有这样的能力之后，库的发展和创新会进入一个新的阶段</strong>，我们（JS开发者）完全不必受制于平台提供商（那些Bottleneck的C++程序员们！）并且它能反过来推动平台API和标准API的演进。
<br>
<br>Harmony（以及HTML5相关标准）引入更多类型如整数、二进制数组等，也可视为属于这一方向的努力。
<br>
<br>
<br>3. 适应于programming-in-the-large的（PITL）需要。这包括去除一些语言陷阱【对于小规模编程，规避语言陷阱比较容易，但是在PITL，语言陷阱的危害就可能很要命】，提供“防御性”编程的支持，提升语言的鲁棒性，加强复杂度的管理能力。
<br>
<br>一个例子是ES5引入了Object.freeze()/seal()等方法。这使得<strong>保持invariants约束</strong>成为可能，这对于大规模编程的帮助是非常大的。
<br>
<br>ES5的strict模式也主要是为这个目的而生。因为要保持向前兼容，所以有些明知道是有问题的特性无法剪除。为此，引入strict模式。strict模式实际上是一个更鲁棒更安全更易于分析（因此开发工具可提供更多帮助，性能也有更大优化可能）的语言子集。
<br>
<br>不过strict模式基本上是一个只做减法（以及fix一些错误）的方式，这样的好处是最大限度的保持了语言的统一性，语言委员会的成员也能很容易达成一致。坏处是提升仍然不够。例如strict修复了ES3在lexical scope上的漏洞（包括全局变量曝露、with、eval("var ...")等），或者说是剪除了好玩但危险的dynamic scope特性。但是ES5-strict仍然不够lexical，那就是var仍然只是function scope，而没有block scope。这导致一些常见的陷阱（如for循环里的closure如果需要引用单次循环中产生的对象，必须引入额外一层function）。这个问题留待Harmony去解决，即引入let/const。
<br>
<br>Module、类和继承、可见性控制等特性都属于PITL的需求。从目前情况来看，这些特性将来一定会有，问题是只是<strong>以何种形式实现</strong>。
<br>
<br>所以回到爱民的问题，我认为，问题的核心并不在于这些特性是否更靠近“静态语言”，而是这些特性<strong>是否有助于PITL</strong>（programming in the large）。如前述Proxy特性实际是非常动态的特性，但是也有助于PITL（比如元编程使得更容易创建DSL，而DSL对于问题域的抽象提升显然有助于PITL）。Traits特性也是一个首先在诸如smalltalk这样的动态语言上试验的新特性，但要引入ES的原因也可归结为PITL需求（比传统多继承和mixin更好的OO形态）。
<br>
<br>还有就是语法的改进。在有些草案里，类不过就是语法糖（也就是特定命令序列的“声明式”化或者说静态化）。虽然在PITL中，局部的语法因素通常不是最重要因素，但好的语法能更好的呈现语义，减少语法噪音，提高代码可读性，并连带提高代码可维护性和编程生产率，编程规模越大这方面累计的效果就越大。
<br>
<br>
<br>最后，必须承认，在面对众多精彩纷呈的harmony提案和strawman草案时，我也深感眼花缭乱，力不从心。不过历史的车轮是不可阻挡的，JS的进化是必然的，JS开发者只有保持一个开放的心态，主动去了解和学习新技术，才能在即将到来的新潮流中游刃有余。
<br>
<br>
<br>
<br>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hax.iteye.com/blog/1122103#comments" style="color:red">已有 <strong>5</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hax.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>