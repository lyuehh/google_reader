---
layout: post
title:  "promise api 与应用场景"
date:   2012-02-07 17:34:05
author: 
categories: program
---

## promise api 与应用场景
### by 
### at 2012-02-07 17:34:05
### original <http://yiminghe.iteye.com/blog/1396751>

<p><a href="http://wiki.commonjs.org/wiki/Promises">promise</a> 是 commonjs 社区中提出的异步规范，其简洁直观的 api 使得异步读值操作更易于理解和使用，主要 api 包括：</p>
<p> </p>
<h1>API</h1>
<p> </p>
<h2>Defer</h2>
<p> </p>
<p>功能实现者调用 Defer() 后产生 Defer 对象，它包括 promise 属性以及 resolve 和 reject 方法</p>
<p> </p>
<h3>promise 对象</h3>
<p> </p>
<p>功能调用者通过调用 promise 的 then 方法添加成功回调和失败回调函数。多次调用 then 添加的回调函数最终“并行”执行，互相独立。</p>
<p> </p>
<h3>resolve/reject</h3>
<p> </p>
<p>功能实现者通过 resolve/reject 来通知 promise 对象成功与否并传递指定的参数给调用者的回调函数来执行。</p>
<p> </p>
<h4>note：</h4>
<p> </p>
<p>通过分离 Defer 对象和 promise 对象，可以达到功能实现者和调用者的权限分离，符合 <a href="http://en.wikipedia.org/wiki/Object-capability_model">Object-capability-model</a> 从而更有利于系统的安全。</p>
<p> </p>
<h3>all</h3>
<p> </p>
<p>通过 all 方法生成一个新的 promise 对象，通过该对象可以监控一批 promise 对象，只有当全部 promise 都成功时才使得新的 promise 对象成功，否则只要有一个 promise 对象失败那么新生成的 promise 就算做失败。</p>
<p> </p>
<h3>when</h3>
<p> </p>
<p>调用 when 可以统一获取 promise 对象和非 promise 对应的值，屏蔽异步的差异。</p>
<p> </p>
<h1>api 使用</h1>
<p> </p>
<h2>then</h2>
<p> </p>
<h3>1. 统一的回调注册</h3>
<p> </p>
<p>then 方法提供了一个统一的回调注册接口，无论注册时该异步 promise 是否已经成功，都会保证回调函数会被及时调用。</p>
<p> </p>
<p>例如：</p>
<p> </p>
<p>可以成功前注册：</p>
<p> </p>
<p> </p>
<pre name="code">var defer= S.Defer();

defer.promise.then(function(v){
  alert(v===1);
});

setTimeout(function(){
  defer.resolve(1);
},5000);</pre>
 
<p>也可以在成功后注册（仍然会被调用）</p>
<p> </p>
<p> </p>
<pre name="code">var defer= S.Defer();
defer.resolve(1);

setTimeout(function(){
  defer.promise.then(function(v){
    alert(v===1);
  });
},5000);</pre>
 
<h3>2. 链式操作</h3>
<p> </p>
<p>then 会返回一个新的 promise 对象，那么在这个返回对象上可以继续调用 then 方法，而参数值则是上一个 then 方法的返回值：</p>
<p> </p>
<p> </p>
<pre name="code">var defer = S.Defer();

defer.promise.then(function(v){
  alert(v===1);
  return v+1;
}).then(function(v2){
  alert(v2===2);
});

defer.resolve(1);
</pre>
 
<h3>3. 嵌套链式操作</h3>
<p> </p>
<p>更进一步：回调函数也可以返回一个 promise 对象，那么 then 返回的 promise 对象则会等待回调函数返回的 promise 对象成功后才算成功，并且最后一个 then 的回调函数的参数为 导致前一个 then 回调函数返回的 promise 对象成功 的值：</p>
<p> </p>
<p> </p>
<pre name="code">var defer = S.Defer();

defer.promise.then(function(v){
   alert(v===1);
   var d2= S.Defer();
   setTimeout(function(){
       d2.resolve(2);
   },1000);
   return d2.promise;
}).then(function(v2){
  alert(v2===2);
});

defer.resolve(1);</pre>
 
<h2>all</h2>
<p> </p>
<p>使用 all 也可以达到嵌套调用的效果（同时等待多个 promise 成功），并可一次性得到导致所有 promise 成功的值：</p>
<p> </p>
<p> </p>
<pre name="code">var defer = S.Defer();

var defer2 = S.Defer();

setTimeout(function(){
 defer2.resolve(2); 
 defer.resolve(1);
},1000);

S.Promise.all([defer.promise,defer2.promise]).then(function(vs){
  alert(vs[0]===1);
  alert(vs[1]===2);
});</pre>
<p> </p>
<p> </p>
<h2>when</h2>
<p> </p>
<p>使用 when 可以不加区别得对待 promise 对象和非 promise 对象，通过成功回调可以一致得获取最终结果：</p>
<p> </p>
<p>
</p>
<pre name="code">function check(p){
  S.Promise.when(p,function(v){
     alert(v===1);
  });
}

var defer=S.Defer();
defer.resolve(1);

check(1); // =&gt; alert(true)

check(defer.promise); //=&gt; alert(true);</pre>
 
<p> </p>
<h1>使用场景</h1>
<p> </p>
<h2>domReady </h2>
<p> </p>
<p>domReady 是个异步过程，具备以下特征使得它非常适合使用 promise 来实现：</p>
<p> </p>
<p>1. domReady 添加回调时可能该事件已经触发过。</p>
<p> </p>
<p>2. 多个 domReady 添加的回调并不需要前后依赖，并且要求其中一个回调失败并不影响其他回调（即回调间独立）</p>
<p> </p>
<p>那么内部实现即可将 S.ready(fn) 的调用转交给内部的 readyDefer.promise.then。那么当浏览器 domready 时只需简单调用 readyDefer.resolve(S) 即可.</p>
<p> </p>
<h2>io/ajax</h2>
<p> </p>
<p>io/ajax 一般也是异步 io，通过 promise 实现 io，那么就可以摆脱必须将处理逻辑统统写在一处的限制：</p>
<p> </p>
<p> </p>
<pre name="code">var r=S.io({...});

r.then(function(v){
  // logic 1
});


r.then(function(v){
 // logic 2 
});</pre>
<p> </p>
<p>并且通过嵌套以及 all 也可以很容易支持多个系统间的相互时序依赖。</p>
<p> </p>
<h4>串行：</h4>
<p> </p>
<p> </p>
<pre name="code">var r=S.io({..});

r.then(function(v){

   return S.io({..,data:{v2:v+1}})

}).then(function(v2){
  // logic v2
})</pre>
 
<h4>并行：</h4>
<p> </p>
<p> </p>
<pre name="code">S.Promise.all([S.io({url:&#39;u1&#39;}),S.io({url:&#39;u2&#39;})]).then(function(vs){
  alert(&quot;u1+u2 =&gt; &quot; + (vs[0]+vs[1]));
});</pre>
 
<h2>Module Loader</h2>
<p> </p>
<p>同 io 类似通过用嵌套以及 all 来管理多个 module 代码文件的串行以及并行加载，可以更加清晰得组织代码结构。</p>
<p> </p>
<p> </p>
<p>Refer:</p>
<p> </p>
<p><a href="http://docs.kissyui.com/kissy/docs/symbols/KISSY.Defer.html">KISSY.Defer API</a> &amp; <a href="http://docs.kissyui.com/kissy/docs/symbols/KISSY.Promise.html">KISSY.Promise API</a></p>
<p> </p>
<p><a href="http://wiki.commonjs.org/wiki/Promises">http://wiki.commonjs.org/wiki/Promises</a></p>
<p> </p>
<p><a href="http://en.wikipedia.org/wiki/Futures_and_promises#Read-only_views">http://en.wikipedia.org/wiki/Futures_and_promises#Read-only_views</a></p>
<p> </p>
<p><a href="http://en.wikipedia.org/wiki/Object-capability_model">http://en.wikipedia.org/wiki/Object-capability_model</a></p>
<p> </p>
<p><a href="https://github.com/kriskowal/q">https://github.com/kriskowal/q</a></p>
<p> </p>
<p><a href="http://www.sitepen.com/blog/2010/05/03/robust-promises-with-dojo-deferred-1-5/">http://www.sitepen.com/blog/2010/05/03/robust-promises-with-dojo-deferred-1-5/</a></p>
<p> </p>
<p><a href="http://dojotoolkit.org/documentation/tutorials/1.6/deferreds/">http://dojotoolkit.org/documentation/tutorials/1.6/deferreds/</a></p>
<p> </p>
<p><a href="http://api.jquery.com/category/deferred-object/">http://api.jquery.com/category/deferred-object/</a></p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
              
              <br><br>
              <span style="color:red">
                <a href="http://yiminghe.iteye.com/blog/1396751#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://yiminghe.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>