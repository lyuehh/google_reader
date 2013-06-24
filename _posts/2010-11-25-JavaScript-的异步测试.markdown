---
layout: post
title:  "JavaScript 的异步测试"
date:   2010-11-25 23:50:40
author: lifesinger
categories: program
---

## JavaScript 的异步测试
### by lifesinger
### at 2010-11-25 23:50:40
### original <http://lifesinger.org/blog/2010/11/js-async-test/>

<p>有这样一段代码：</p>
<pre>
var O = {
    data: 1,
    fn: function(callback) {
        setTimeout(function() {
            O.data++;
            callback();
        }, 100);
    },
    fn2: function() {
        O.data--;
    }
};
</pre>
<p>现在要测试 O 的两个方法是否正常工作。我们采用自主开发的测试框架 STF：</p>
<pre>
/* STF - stupid test framework v0.1 */
function test(desc, fn) {
    console.log(&#39;START test &#39; + desc);
    fn &amp;&amp; fn();
    console.log(&#39;END test &#39; + desc);
}
function equals(actual, expected) {
    console.log(actual === expected ? &#39;  passed&#39; : &#39;  failed&#39;);
}
</pre>
<p>有了以上方法，很容易写出测试用例：<br>
<span></span></p>
<pre>
test('fn', function() {
    O.data = 1;
    O.fn(function() {
        equals(O.data, 2);
    });
});
test('fn2', function() {
    O.data = 1;
    O.fn2();
    equals(O.data, 0);
});
</pre>
<p>一运行，输出的 log 顺序很乱，有个用例还失败了：</p>
<pre>
START test fn
END test fn
START test fn2
  passed
END test fn2
  failed
</pre>
<h3>都是异步惹的祸</h3>
<p>对上面的结果稍加分析可以看出，一切都是 setTimeout 惹的祸。<br>
只要被测试的代码里有异步操作，就有可能引起上面的问题。作为一个非常有前景的 STF 测试框架，究竟怎样才能支持异步测试呢？</p>
<p>思路很简单，如果没有异步，都是同步运行，那就很好测试了。因此立刻推出 STF v0.2:</p>
<pre>
// STF 里增加 sleep 方法
function sleep(ms) {
    var t = (new Date()).getTime();
    while((new Date()).getTime() - t &lt; ms);
}
</pre>
<p>将 O.fn 的测试用例修改为：</p>
<pre>
test('fn', function() {
    O.data = 1;
    O.fn(function() {
        equals(O.data, 2);
    });
    sleep(200);
});
</pre>
<p>满怀期望一运行，杯具，log 还是乱的，依旧失败，囧。<br>
分析 sleep 的实现，很明显忽略了 js 单线程的特点，这样实现，是很难搞定的。</p>
<h3>解铃还需系铃人</h3>
<p>中国有部很有名的电影：《圆环套圆环》，导演和情节我全忘了，但这片名的精髓可以用来解决异步测试。<br>
有了实现思路，就好办了。立刻升级 STF 到 v0.9:</p>
<pre>
/* STF - stupid test framework v0.9 */
var blocks = [];
function test(desc, fn) {
    blocks.push(function() { console.log('START test ' + desc); });
    fn();
    blocks.push(function() { console.log('END test ' + desc); });
}
function runs(fn) {
    blocks.push(fn);
}
function waits(ms) {
    blocks.push(ms);
}
function equals(actual, expected) {
    console.log(actual === expected ? '  passed' : '  failed');
}
function run(fns) {
   var b;
    fns = fns || blocks;
    while((b = fns.shift())) {
        if(typeof b !== 'number') {
            b();
        } else {
            setTimeout(function() {
                run(fns);
            }, b);
            break;
        }
    }
}
</pre>
<p>核心思想是将测试代码封装成一个个 block 块，然后依次执行。碰到异步操作时，后续 block 块都放到异步操作的回调里。这样“圆环套圆环”，就能保证“同步”执行了。</p>
<p>测试用例修改为：</p>
<pre>
test('fn', function() {
    runs(function() {
        O.data = 1;
        O.fn(function() {
            equals(O.data, 2);
        });
    });
    waits(200);
});
test('fn2', function() {
    runs(function() {
        O.data = 1;
        O.fn2();
        equals(O.data, 0);
    });
});
run();
</pre>
<p>来看运行结果：</p>
<pre>
START test fn
  passed
END test fn
START test fn2
  passed
END test fn2
</pre>
<p>非常棒，一切如预期。</p>
<p>注意，以上 STF 框架最多仅是 v0.9, 离正式版 v1.0 还差了至少一光年距离。考虑更完善、功能更强大的 JS 测试框架，推荐 Jasmine. 本文仅是对 Jasmine 异步测试原理的简陋模拟。</p>
<p>Jasmine 相关链接：<a href="http://github.com/pivotal/jasmine">源码</a> | <a href="http://pivotal.github.com/jasmine/">文档</a></p>
<p>打造一个测试框架不难，难的是思路和想法，以及疯狂的细节和对完美的追求。<br>
希望这篇文章能让你对 Jasmine 的异步测试原理有个初步了解。</p>
<p>晚安，睡觉。</p>