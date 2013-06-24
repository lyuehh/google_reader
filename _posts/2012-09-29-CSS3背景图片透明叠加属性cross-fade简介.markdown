---
layout: post
title:  "CSS3背景图片透明叠加属性cross-fade简介"
date:   2012-09-29 16:00:56
author: 张 鑫旭
categories: program
---

## CSS3背景图片透明叠加属性cross-fade简介
### by 张 鑫旭
### at 2012-09-29 16:00:56
### original <http://www.zhangxinxu.com/wordpress/2012/09/css3-background-image-cross-fade/>

<p>by <a href="http://www.zhangxinxu.com/">zhangxinxu</a> from <a href="http://www.zhangxinxu.com/">http://www.zhangxinxu.com</a><br>
本文地址：<a href="http://www.zhangxinxu.com/wordpress/?p=2671">http://www.zhangxinxu.com/wordpress/?p=2671</a></p>
<p><a href="http://www.mobilexweb.com/blog/iphone-5-ios-6-html5-developers">据说</a>iOS6系统(iPhone5)增加了两个CSS3属性，一个是CSS3 filters – CSS3滤镜，在“<a href="http://www.zhangxinxu.com/wordpress/?p=2547">CSS将图片转换成黑白</a>”一文中就介绍过CSS3 grayscale滤镜；另外一个是CSS3 Cross-fade – CSS3交叉淡入淡出。</p>
<p>毕竟有IE滤镜这厮，所以前者看上去可能不那么显生；那后者Cross-fade是？</p>
<p><strong>用法实例</strong><br>
用法如下：</p>
<div>
<pre>background-image: -webkit-cross-fade(url(1.jpg), url(2.jpg), 50%);</pre>
</div>
<p>官方<a href="http://www.w3.org/TR/2011/WD-css3-images-20110217/#cross-fade-function">草案</a>表达式为：</p>
<div>
<pre><dfn>&lt;image-combination&gt;</dfn> = cross-fade( &lt;image&gt;, &lt;image&gt;, &lt;percentage&gt; )</pre>
</div>
<p>两个图片地址，外加一个透明度百分比。</p>
<p>这个percentage是作用在第二张图片上的，可以让后面一站图片(2.jpg)半透明，然后产生图片透明度叠加效果。类似这样的：<br>
<a href="http://codepen.io/edge0703/pen/Hridk"><img alt="CSS3 cross-fade产生的效果截图 张鑫旭-鑫空间-鑫生活" src="http://image.zhangxinxu.com/image/blog/201209/2012-09-29_153503.jpg" title="CSS3 cross-fade产生的效果截图" width="412" height="214"></a></p>
<p>有个疑问？交叉淡入淡出效果中的透明度是两张图片都作用？还是仅作用于后面一张？？</p>
<p>对此，我专门制作了一个测试demo，您可以狠狠地点击这里：<a href="http://www.zhangxinxu.com/study/201209/css3-background-image-cross-fade.html">CSS3 cross-fade属性透明度作用对象测试</a></p>
<p><img alt="cross-fade与opacity作用对比效果测试 张鑫旭-鑫空间-鑫生活" src="http://image.zhangxinxu.com/image/blog/201209/2012-09-29_154215.jpg" title="cross-fade与opacity作用对比效果测试" width="304" height="514"></p>
<p>上面效果使用的是<code>cross-fade</code>, 后面效果是通过修改后一张图片的<code>opacity</code>透明度值实现。可以看到，在相同的透明度上，两者的效果是一样的。因此可以得出：<strong>cross-fade中的透明度值是只作用于后面一张图片上！</strong></p>
<p><strong>兼容性</strong><br>
目前，仅Chrome以及Safari 6支持该CSS3属性；IE10以及FireFox浏览器是否支持该属性还不得而知。</p>
<p>因此，本文的内容纯当扩充眼界，增加见识。基本上无法再实际项目中应用。</p>
<p><strong>结语</strong><br>
好久没有些这么短的文章了，祝大家中秋国庆快乐，玩得好，吃得好，安安全全，健健康康！！</p>
<p>最后，和平使者爱凸鳗压阵：<br>
<img alt="" src="http://image.zhangxinxu.com/image/blog/201209/aotuman.jpg" title="奥特曼" width="480" height="360"></p>
<p>原创文章，转载请注明来自<a href="http://www.zhangxinxu.com/">张鑫旭-鑫空间-鑫生活</a>[<a href="http://www.zhangxinxu.com/">http://www.zhangxinxu.com</a>]<br>
本文地址：<a href="http://www.zhangxinxu.com/wordpress/?p=2671">http://www.zhangxinxu.com/wordpress/?p=2671</a></p>
<p>（本篇完）</p>
<div>有话要说，点击<a href="http://www.zhangxinxu.com/wordpress/2012/09/css3-background-image-cross-fade/#response">这里</a>发表评论。</div>