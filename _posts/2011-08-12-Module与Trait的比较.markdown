---
layout: post
title:  "Module与Trait的比较"
date:   2011-08-12 12:50:42
author: 
categories: program
---

## Module与Trait的比较
### by 
### at 2011-08-12 12:50:42
### original <http://hax.iteye.com/blog/1126601>

最近我多次提及module和trait。
<br>
<br>粗看，我们可以发现它们有一定的相似处。两者其实都是为了组织代码单元，并在代码上施加更多的约束，且约束的方式有点类似。
<br>
<br>目前典型的module是定义require和exports，即需要哪些包（所提供的类和函数），和输出哪些类和函数。
<br>
<br>而trait是定义一组方法，并声明这些方法所依赖的方法（即require某些方法）。比如 IterationTrait 可定义 map, filter, reduce, reduceRight 等方法，并声明需要 forEach 方法。这也意味着前些方法内部可能是由forEach方法实现的。
<br>
<br>
<br>如果看过Ruby语言的设计，就可以理解module和trait的相似性。组合一个trait，就相当于引入一个module。不过我个人并不赞同这样的设计，因为这种相似性只是结构上的。
<br>
<br>从概念上说，module和trait两者的共通处是都要强制明确所有的耦合点。区别在于，trait的目的是提供类实现的组合和复用机制，因此其更强制内聚性。module则不带有这个目的。
<br>
<br>
<br>通常，在module中用到的和输出的函数，都应该是无副作用的（不引起外界可观察到的状态变化）。类，也可视为一种无副作用的函数，因为状态变化被封装到类所产生的对象实例中了，而对象实例是跨模块隔绝的（即对于不同module中创建的对象实例，它们的方法调用只影响它们各自的状态，而不会互相影响）。反过来，要让会引起状态变化的函数也跨模块隔绝相当困难（这意味着被调用函数必须知道它在哪个模块中被调用，这样才能为不同模块保存互相独立的状态）。
<br>
<br>这方面的反例是单例（Singleton）。单例通常是跨模块全局的。无正当理由不应使用。
<br>另一个例子是对JS内建对象的prototype的修改。目前的各类模块系统（包括NodeJS）都是不会隔绝的。也就是说，prototype的修改是全局性的，而非局域性的。这也是对内建对象进行prototype扩展必须非常谨慎的根本原因。
<br>
<br>而trait通常也应是无状态的。如果有状态，则当前的习惯是使用闭包作为trait的产生器。如果一个trait需要组合其他有状态trait，则它自身通常也随之变为有状态的，也需要套一个闭包作为产生器，产生器接受参数并将参数通过其要组合的trait的产生器传递进去。也就是说，状态是通过产生器被注入进去的。
<br>
<br>我本人在一些特殊约束下（如性能考量，因为在产生大量对象时，如果多层trait组合需要大量闭包调用，在IE这样的浏览器中会慢到不可接受），也采用一些折中方式。例如在trait上定义init方法，并通过init方法来初始化状态。当多个具有init方法的trait组合到一起，手工resolve之——实际就是将多个init组合起来。最终init方法应在最终对象实例化后被调用。
<br>
<br>
<br>当前JavaScript的module系统的设计基本上已经逐渐明晰，其实用性也得到验证。不过trait仍然有很大的发展空间。例如是否应结合type/guard检测？是否trait的手工resolve要求太苛刻而应允许部分妥协（像mixin那样有直接的override机制）？是否应支持私有状态（private）加约定的构造器来取代闭包产生器？诸如此类的问题，可能要等待更多的实践来回答。
<br>
<br>
<br>
<br>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hax.iteye.com/blog/1126601#comments" style="color:red">已有 <strong>1</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hax.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>