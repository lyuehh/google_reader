---
layout: post
title:  "Objective-C 语法之特性"
date:   2013-04-10 10:59:34
author: Rhino
categories: program
---

## Objective-C 语法之特性
### by Rhino
### at 2013-04-10 10:59:34
### original <http://golanger.cn/?p=1100>

<p><strong>特性：</strong></p>
<div>@property声明的形式是:</div>
<div>     @property (attributes) type name;</div>
<div>attributes 描述了如何编写访问器：</div>
<div></div>
<div>1. assign, retain和copy这些属性影响到合成的setter如何构建。</div>
<div></div>
<div>     a. assign直接将新值赋值给你的特性。它是非对象特性的唯一选择。</div>
<div></div>
<div>     b. 对于在引用计数下的对象特性，assign创建了一个弱引用。</div>
<div></div>
<div>     c. ratain要求编译器@synthesize你的访问器，它创建一个等价于下面代码的setter</div>
<pre>
- (void) setName: (NSString *) newName
{
    [newName retain];
    [name release];
    name = newName;
}
</pre>
<div>     注意：在垃圾收集下，retain与assign相同，并且我们不需要使用它，除非编写必须在引用计数</div>
<div>     和垃圾收集下都能工作的框架代码。</div>
<div></div>
<div>     d. 如果使用copy并且要求编译器来@synthesize你的访问器，它创建一个等价于下面的setter</div>
<pre>
- (void) setName: (NSString *) newName
{
    NSString *tem = name;
    name = [newName copyWithZone: nil];
    [tmp release];
}
</pre>
<div>     注意：要使用copy，特性的类型必须是能够复制的类。类必须通过实现copyWithZone: 而采用NSCopying协议。</div>
<div></div>
<div>     e. 如果没有显示指定assign, retain或copy，编译器假设一个默认值assign。这对于基础类型不是问题，然而，对于引用计数下的对象特性，如果你没有指定三者之一，那么将会得到一个编译器警告。</div>
<div></div>
<div>2. readwrite和readonly</div>
<div></div>
<div>     a. 默认为readwrite，该特性的值可以设置也可以获取，两个访问器都合成。</div>
<div></div>
<div>     b. 如果特性声明为readonly，只有getter合成。</div>
<div></div>
<div>3. nonatomic</div>
<div></div>
<div>     a. 默认是atomic，表示该方法是线程安全的，但是没有提供atomic关键字。如果指定了nonatomic，则编译器合成访问器时不考虑线程安全性。</div>
<div></div>
<div>     b. 对于非对象特性或者使用垃圾收集时，getter是一条简单的返回语句，setter是一次简单的赋值，并且总是atomic的。</div>
<div></div>
<div>     c. 合成的setter和getter使用锁来防止其他线程中断设置或获取。</div>
<div></div>
<div>     d. 锁在性能方面付出了代价，如果你不打算编写多线程代码，则应该将自己的特性声明为nonatomic的，然后，合成的访问器不会使用锁。</div>
<div></div>
<div>4. setter=name和getter=name允许你提供其他的名称。</div>
<div></div>