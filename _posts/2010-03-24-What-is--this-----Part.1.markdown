---
layout: post
title:  "What is 'this'? - Part.1"
date:   2010-03-24 22:54:09
author: 明城
categories: program
---

## What is 'this'? - Part.1
### by 明城
### at 2010-03-24 22:54:09
### original <http://www.gracecode.com/archives/3018/>

<p>呃，这次的标题有些“装逼”，首先我们从道题目开始。</p>

<p>这是道有趣的题目：考虑下面的代码，考虑 console.log 输出的值：</p>

<pre>var x = 10;
var foo = {
    x: 20,
    bar: function () {
        var x = 30;
        return this.x;
    }
};

console.log(
    foo.bar(),             // 1.
    (foo.bar)(),           // 2.
    (foo.bar = foo.bar)(), // 3.
    (foo.bar, foo.bar)()   // 4.
);</pre><p><tt>-- Split --</tt></p>

<p>实际上，题目真正意思求 console.log 中依次四条语句的值。为了方便讲述，上面的语句分别标注为 1-4。</p>

<p>首先，是“1.”这条语句相对来说比较明朗（其实我们也经常这样写）。调用 foo 的 bar 方法，因此 bar 的 this 指向（作用域）为 foo，因此就等于是取 foo 上面的 x 属性（也就是 foo.x）的值，那么语句返回的值就是 20 。</p>

<p>然后是“2.”这条语句。我们可能对 Grouping Operator （也就是“()”）犹豫不决，那 么我们找找 <a href="http://bclary.com/2004/11/07/#a-11.13.1" title="http://bclary.com/2004/11/07/#a-11.13.1">ECMA 中相关定义</a>：</p>

<pre>11.1.6 The Grouping Operator

The production PrimaryExpression : ( Expression ) is evaluated as follows:

    1. Evaluate Expression. This may be of type Reference.
    2. Return Result(1).

NOTE 

This algorithm does not apply GetValue to Result(1). The principal 
motivation for this is so that operators such as delete and typeof 
may be applied to parenthesised expressions.</pre><p>因此，由于 foo.bar 是个引用“Reference”，所以使用组操作符 <tt>(foo.bar)</tt> 返回 的引用是和 foo.bar 是一样的。</p>

<p>如果还不理解，那么考虑下面的表达式：</p>

<pre>console.log(foo.bar === (foo.bar))</pre><p>因此，</p>

<pre>(foo.bar)();</pre><p>返回的也是 foo.x 的值，也就是 20 。</p>

<p>目前为止上面的前两个语句的解释，想必已经了解。细心的读者会发现，其实题目的重点是在 3.  和 4. 两条语句。</p>

<p>要解释后面的两个语句恐怕这篇文章会塞不下，请留给我点喝水的时间，我们下篇继续。</p>

<p><tt>-- To be continued --</tt></p>

<p>PS, <a href="http://www.gracecode.com/archives/3019/" title="http://www.gracecode.com/archives/3019/">Part 2. 在这里</a></p>