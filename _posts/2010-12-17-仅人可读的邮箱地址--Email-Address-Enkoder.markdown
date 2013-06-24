---
layout: post
title:  "仅人可读的邮箱地址--Email Address Enkoder"
date:   2010-12-17 08:00:00
author: Joey.D.Darko
categories: program
---

## 仅人可读的邮箱地址--Email Address Enkoder
### by Joey.D.Darko
### at 2010-12-17 08:00:00
### original <http://foobar.me/2010/12/17/jin-ren-ke-du-de-you-xiang-email-address-enkoder/>

<p>在很久很久以前…邮箱是可以明文放在页面里的…</p>

<p>但是后来有了什么蜘蛛啦(spider)爬虫啦(crawler)的东西,可以在页面上抓到你的email地址给他们也许用心不良的主人,很有可能导致邮箱被一些垃圾邮件骚扰,的确是个困扰阿.</p>

<p>办法是想出来的,然后就有人想出来用<code>#</code>或者<code>AT</code>来代替<code>@</code>符号,或者直接用图片等的方式让机器爬虫不能识别邮箱地址.这些方法都多少有些让真人的用户多几步操作和麻烦.</p>

<p>现在有了个更好的方式解决,先看效果:</p>




<p>似乎是一样的,但是在源代码里上面的邮箱是这样显示的:</p>

<pre><code>:::javascript
    var x=&quot;function f(x){var i,o=\&quot;\&quot;,ol=x.length,l=ol;while(x.charCodeAt(l/13)!&quot; +
    &quot;=116){try{x+=x;l+=l;}catch(e){}}for(i=l-1;i&gt;=0;i--){o+=x.charAt(i);}return &quot; +
    &quot;o.substr(0,ol);}f(\&quot;)09,\\\&quot;?39(\\\&quot;\\\\330\\\\020\\\\Rt\\\\330\\\\330\\\\7&quot; +
    &quot;20\\\\030\\\\020\\\\5600\\\\620\\\\600\\\\200\\\\120\\\\200\\\\410\\\\010\\&quot; +
    &quot;\\330\\\\IB700\\\\400\\\\200\\\\220\\\\410\\\\620\\\\420\\\\L520\\\\610\\\\&quot; +
    &quot;:3(?4&gt;\\\&quot;(f};o nruter};))++y(^)i(tAedoCrahc.x(edoCrahCmorf.gnirtS=+o;721=%&quot; +
    &quot;y;i=+y)09==i(fi{)++i;l&lt;i;0=i(rof;htgnel.x=l,\\\&quot;\\\&quot;=o,i rav{)y,x(f noitcnu&quot; +
    &quot;f\&quot;)&quot;                                                                        ;
    while(x=eval(x));
</code></pre>

<p>被用javascript加密了,最终显示的效果都一样,只是机器想简单的抓邮箱是抓不到了,cool~huh?</p>

<p>生成这段代码的是HIVELOGIC.com的一个在线服务,地址在这里,方便童鞋们使用:</p>

<p><a href="http://hivelogic.com/enkoder/form">http://hivelogic.com/enkoder/form</a></p>