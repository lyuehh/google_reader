---
layout: post
title:  "unified event model"
date:   2011-10-14 23:02:08
author: 
categories: program
---

## unified event model
### by 
### at 2011-10-14 23:02:08
### original <http://yiminghe.iteye.com/blog/1197169>

<p>为了处理原生事件在各个浏览器下的兼容性，上次提出了<a href="http://yiminghe.iteye.com/blog/1073547">应对原生事件的事件模型</a>，但随着web的多样性以及新功能需求，常常需要自行实现 ui 组件，例如 <a href="http://yiminghe.iteye.com/blog/1174320">树</a>，<a href="http://yiminghe.iteye.com/blog/921096">按钮</a>，<a href="http://docs.kissyui.com/docs/html/demo/component/menu/index.html">菜单</a>等，实现时推荐尽量和原生组件（select ， input 等）保持一致，那么也需要暴露事件，不过这时是自定义事件，如果想把模拟 ui 组件做到和原生控件完全一致的体验，那么自定义事件也要具备和原生事件一样的能力。</p>
<p> </p>
<h1>自定义事件</h1>
<p> </p>
<p>自定义事件其实是 <a href="http://en.wikipedia.org/wiki/Observer_pattern">observer-listener</a> 模式的通俗叫法，本质上和浏览器的事件模型是一致的，由于之前为了应对兼容性问题在客户端已经重新实现了一遍原生事件模型，那么就可以把自定义事件也集成到这个模型中，形成统一的事件模型。</p>
<p> </p>
<h1>区别点</h1>
<p> </p>
<p>自定义事件由于完全由应用控制，那么就和原生事件有一些区别：</p>
<p> </p>
<h2>1. 不需要关联到 dom 节点</h2>
<p> </p>
<p>     虽然原生事件可以由自己完全实现一套，但是最终仍然要调用 addEventListener/attachEvent 对dom节点进行关联（接受浏览器事件通知），而自定义事件则完全避免了这一步。</p>
<p> </p>
<h2>2. 需要添加冒泡 api</h2>
<p> </p>
<p>    原生事件冒泡不需要模型自己控制，浏览器自会处理，模型只要提供接口操作浏览器 api 控制冒泡行为即可（stopPropagation）。而自定义事件由模型完全控制，那么模型就需要提供冒泡 api，并且可以指定该事件是否可以冒泡以及事件触发源对象的上一级冒泡对象，当触发自定义事件时，模型需要根据冒泡链依次向上触发，并根据是否调用了 stopPropagation 来停止事件传播。</p>
<p> </p>
<h2>3. 不需要默认事件阻止 api</h2>
<p> </p>
<p>    一般来说原生事件都会附带一些默认行为（a 页面跳转，submit 表单提交等），模型也会提供相应的阻止 api （preventDefault）。而自定义事件则完全不用考虑这个了。</p>
<p> </p>
<h2>4. 事件的程序触发方式简单（programmatic trigger）</h2>
<p> </p>
<p>    自定义事件的触发比较简单，只要注意自行冒泡即可。而原生事件的程序触发则比较麻烦，虽然可以通过构建原生事件对象交给浏览器dispatch，可这种方式兼容性问题很大（change 等事件根本没法触发），最好的方式还是由模型自己触发，这时需要考虑的是冒泡以及默认事件行为触发。</p>
<p> </p>
<p> </p>
<h1>区分</h1>
<p> </p>
<p>以上的区别点模型需要在适当的时候进行分支处理，而判断关键就在于当前对象是否是原生对象（是否具有原生事件绑定接口）：</p>
<p> </p>
<p> </p>
<pre name="code">obj.addEventListener||obj.attachEvent</pre>
 
<h1>最终 api</h1>
<p> </p>
<p>最终模型暴露出来的 api 应该是：</p>
<p> </p>
<p>
</p>
<pre name="code">obj.on(name,fn,context)

obj.detach(name,fn,context)

obj.addBubbleTarget(otherObj) // 自定义事件专属

obj.configBubble(name,isBubble); // 自定义事件专属

obj.fire(name,data)</pre>
 
              
              <br><br>
              <span style="color:red">
                <a href="http://yiminghe.iteye.com/blog/1197169#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://yiminghe.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>