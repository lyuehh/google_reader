---
layout: post
title:  "nodejs 异步之 Timer &amp;Tick 篇"
date:   2011-09-13 21:45:13
author: 爱多
categories: program
---

## nodejs 异步之 Timer &amp;Tick 篇
### by 爱多
### at 2011-09-13 21:45:13
### original <http://cnodejs.org/blog/?p=2661>

<div><strong>Timer：</strong></div>
<div><strong></strong><br>
在前端开发中，我们 经常会使用setTimeout 函数组，这组函数其实不属于语言标准，他们只是extentsion ，在浏览器中，他们属于 BOM（浏览器对象扩展），即它的确切定义为：window.setTimeout ,和window.alert , window.open 等函数处于同一层次。<span></span></div>
<div><a href="http://code.google.com/p/doctype/wiki/WindowSetTimeoutMethod">http://code.google.com/p/doctype/wiki/WindowSetTimeoutMethod</a></div>
<div>
<p>你可以在浏览器的控制台监测window 对象 ，或查看chromium 的实现 :<br>
<a href="http://codesearch.google.com/#OAMlx_jo-ck/src/third_party/WebKit/Source/WebCore/page/DOMWindow.h">http://codesearch.google.com/#OAMlx_jo-ck/src/third_party/WebKit/Source/WebCore/page/DOMWindow.h</a></p>
<p>在nodejs ，当然也要实现自己的Timer ，在nodejs 的src 目录下，timer.h/cc 是对 libev 中的 ev_timer 进行了一层浅包装，在src/node.js 文件中，把这组函数放置于全局范围中（相当于浏览器中window）</p>
<pre>startup.globalTimeouts = function() {
  global.setTimeout = function() {
    var t = NativeModule.require('timers');
    return t.setTimeout.apply(this, arguments);
  };
  global.setInterval = function() {
    var t = NativeModule.require('timers');
    return t.setInterval.apply(this, arguments);
  };
...</pre>
<p>通过我们对libev的分析，我们已经知道，在libev中队timer的 添加/删除/更新 操作都是 O(lg(n)) ,<br>
（libev 分析 <a href="http://cnodejs.org/blog/?p=2489">http://cnodejs.org/blog/?p=2489</a>）</p>
<p>但其实在我们的日常开发中，以网络套接字为例，我们往往会对其设置timeout，即如果一段时间（120s 或 60s）此套接字没有活动事件发生，我们就关闭它（这种技巧非常重要，否则由于一些内部或外部的原因，很容易出现描述符占用过多的现象），注意到，我们的超时时间是一个统一的值，这时候，我们可以采用如下算法：(lib/_linklist.js 真实实现)</p>
<p><a href="http://cnodejs.org/blog/wp-content/uploads/2011/09/link.png"><img src="http://cnodejs.org/blog/wp-content/uploads/2011/09/link.png" alt="" width="346" height="365"></a></p>
<p>当我们在不同的时刻多次 调用 setTimeout(fn ,1000) 时，我们把所有的这些事件仅由一个timer 来管理， 这些事件被放到一个双向链表中，其顺序自然而然的就是按时间来排序的，当某一时刻，timer 触发时，会依次的从prev链表头中取出节点，进行检查， 如果超时则触发…<br>
（参见lib/timer_legacy.js 由于libev无法在windows上 很好的工作，所以nodejs项目组又对libev，libeio 作了一层封装 ，称为libuv, 使用了传统的方法文件以_legacy 结尾，使用了libuv 的以_uv 结尾，如net_legacy.js ,net_uv.js ,timers_legacy.js  ,timers_uv.js 等,）</p>
<p>当我们需要更改某异步事件时，比如，我们添加超时控制的socket上发生了read/ write 事件，这时候，我们需要重新reset 计时，我们仅需要把这个事件从链表中转移到链表尾部即可<br>
(参见 lib/net_legacy.js)</p>
<p>删除时的情况与此类似，所以在此场景下，仅耗费一个ev_timer ，而我们的所有异步事件的添加/删除/更新 都是 O(1) 复杂度.</p>
<p>注意，在lib/timer_*.js  中，当超时值&lt;= 0 时是不做优化的,</p>
<pre>exports.exports.setTimeout = function(callback, after) {
  if (after &lt;= 0) {
    // Use the slow case for after == 0
   timer = new Timer();
   timer.ontimeout = callback;
  else{
    ...
  }</pre>
<p>同时setInterval 函数也是没做优化的.</p>
<p><strong>nextTick :</strong></p>
<p>nextTick是个很有意思的东西，它其实是一个prepare watcher 来实现的，在libev中的event loop 的每次迭代，在nodejs 中就叫做 “Tick” ，而在libev 中，支持prepare watcher ，它在每次迭代开始时触发  ，在src/node.cc 中 我们看到类似代码（v4.9为例）：</p>
<pre>ev_prepare_init(&amp;node::prepare_tick_watcher, node::PrepareTick);
ev_prepare_start(EV_DEFAULT_UC_ &amp;node::prepare_tick_watcher);
ev_unref(EV_DEFAULT_UC);</pre>
<p>在函数 PrepareTick 中 会调用一个回调函数，此函数 _tickCallback 在 src/node.js 中：</p>
<pre>startup.processNextTick =function() {
  var nextTickQueue = [];
  process._tickCallback = function() {
  ...
  }
  process.nextTick = function(callback) {
    nextTickQueue.push(callback);
    process._needTickCallback();
   };
};</pre>
<p>当我们调用API  process.nextTick 时，其实就是向nextTickQueue  这个闭包队列中添加一个函数，当下次事件循环开始时，会自动触发prepare_tick_watcher ,调用我们设定的函数。<br>
注意，上面有个问题，那就是如果没有机会进行下次事件循环，比如，此时没有实质性的watcher可供监测了，这时事件循环就会退出 ，为了避免着这种情况, nextTick  函数中会调用一次 process._needTickCallback() ,这个函数会使tick_spinner （一个idle watcher）处于活跃状态 , 来防止事件循环的退出。</p>
<p>由nextTick 原理我们可知，当我们添加一个待处理事件时，其复杂度为O(1) ,但如果你用setTimeout(fn ,0) 的话，如我们在libev分析中所讲，其一进一出均为 O(lg(n)) , 所以nodejs官方文档讲 ：</p>
<blockquote><p>On the next loop around the event loop call this callback. This is not a simple alias to setTimeout(fn, 0), it’s much more efficient.</p></blockquote>
<p>nextTick 确实高效的多，诚不我欺也!</p>
<p><strong>小节：<br>
</strong>通过以上分析 ，我们至少得出2条粗浅的结论：<br>
#1  如果可能的话，调用setTimeout时，尽量使用相同的超时值<br>
#2  尽量用process.nextTick 来代替 setTimeout(fn ,0)</p>
</div>
<div style="border:1px solid #ccc;font-size:14px;margin:27px auto">
<div style="padding:7px 10px;border-bottom:1px dotted #ccc">关于作者</div>
<div style="overflow:hidden">
<div style="text-align:left;line-height:23px">
<div style="padding:5px 10px">
<div style="margin:2px 0;height:23px;overflow:hidden;font-size:14px"><a href="http://42qu.com/-10032144" style="text-decoration:none">爱多</a>, 游民</div>
<div>码奴，求收留，求包养，求打赏，求…</div>
</div>
</div>
</div>
<div style="text-align:right;border-top:1px dotted #ccc;padding:10px">
<div style="float:left"><a href="http://10032144.42qu.com/link/27124" style="margin-right:7px;text-decoration:none">豆瓣</a> <a href="http://10032144.42qu.com/link/27125" style="margin-right:7px;text-decoration:none">新浪微博</a> </div>
<div><a href="http://42qu.com/-10032144/pay?title=nodejs+%E5%BC%82%E6%AD%A5%E4%B9%8B+Timer+%26amp%3BTick+%E7%AF%87&amp;url=http%3A%2F%2Fcnodejs.org%2Fblog%2F%3Fp%3D2661&amp;rel=blog" style="text-decoration:none;margin-right:17px">向文章付费</a> <a href="http://42qu.com/-10032144/pay?title=nodejs+%E5%BC%82%E6%AD%A5%E4%B9%8B+Timer+%26amp%3BTick+%E7%AF%87&amp;url=http%3A%2F%2Fcnodejs.org%2Fblog%2F%3Fp%3D2661&amp;cid=1&amp;rel=blog" style="text-decoration:none;margin-right:16px">请作者吃饭</a></div>
</div>
</div>