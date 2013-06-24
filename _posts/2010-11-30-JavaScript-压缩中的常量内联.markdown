---
layout: post
title:  "JavaScript 压缩中的常量内联"
date:   2010-11-30 23:24:50
author: lifesinger
categories: program
---

## JavaScript 压缩中的常量内联
### by lifesinger
### at 2010-11-30 23:24:50
### original <http://lifesinger.org/blog/2010/11/const-inline-in-js-compress/>

<p>两年前，NCZ 在 <a href="http://www.slideshare.net/nzakas/extreme-javascript-compression-with-yui-compressor">Extreme JavaScript Compression With YUI Compressor</a> 里提到：</p>
<blockquote><p>
Best Optimization = Identifier Replacement
</p></blockquote>
<p>标志符替换（Identifier Replacement）在 YUI Compressor 里非常重要。<br>
下面用 CC 表示 Closure Compiler, YC 表示 YUI Compressor.</p>
<h3>本地文件大小</h3>
<p>为了帮助 YC 有效进行标志符替换，下面这段代码 <code>toogle.js</code>:</p>
<pre>
function toggle(element) {
    if(YAHOO.util.Dom.hasClass(element, 'selected')) {
        YAHOO.util.Dom.removeClass(element, 'selected');
        return false;
    } else {
        YAHOO.util.Dom.addClass(element, 'selected');
        return true;
    }
}
</pre>
<p>推荐写成 <code>toogle2.js</code>:<span></span></p>
<pre>
function toggle(element) {
    var Dom = YAHOO.util.Dom,
        SELECTED = 'selected',
        ret = false;

    if(Dom.hasClass(element, SELECTED)) {
        Dom.removeClass(element, SELECTED);
    } else {
        Dom.addClass(element, SELECTED);
        ret = true;
    }
    return ret;
}
</pre>
<p>源码和压缩后的文件大小对比：<br>
<img src="http://lifesinger.org/blog/wp-content/uploads/2010/11/cc-vs-yc-filesize.png" width="270" height="195"><br>
很明显，用 YC 压缩的文件最小。这是因为用 CC 压缩时，会将常量 <code>SELECTED = 'selected'</code> 内联回去 <code>toggle2-cc-min.js</code>：</p>
<pre>
function toggle(a){var b=YAHOO.util.Dom,c=false;
if(b.hasClass(a,"selected"))b.removeClass(a,"selected");
else{b.addClass(a,"selected");c=true}return c};
</pre>
<h3>伟大的 gzip</h3>
<p>从上面看起来，CC 将常量内联的做法很恼人。好在真实环境下，绝大部分服务器都开启了 gzip 压缩。来看一张图：<br>
<img src="http://lifesinger.org/blog/wp-content/uploads/2010/11/cc-vs-yc-gzip.png" width="680" height="156"><br>
很囧，<code>toggle2-yc-min.js</code> 最大。<br>
更囧的是，toggle2 的压缩版本，比原始 toggle 的压缩版本都大。<br>
折腾了半天，居然帮了倒忙！</p>
<p>产生这种结果的原因是：gzip 算法里，对重复字符做了处理。重复字符越多，压缩比越大。<code>toggle2.js</code> 增加了代码，减少了 <code>toggle.js</code> 的重复字符。对于该特例来说，我们的确帮了倒忙。注意一般情况下，<code>toggle2.js</code> 的写法还是有益的。</p>
<h3>CC 的常量内联</h3>
<p>CC 的常量内联，有时会导致压缩后，文件大小暴增，比如将<br>
<a href="http://lifesinger.github.com/lab/2010/cc-inline/test.js">http://lifesinger.github.com/lab/2010/cc-inline/test.js</a><br>
用 CC 压缩成<br>
<a href="http://lifesinger.github.com/lab/2010/cc-inline/test-cc-min.js">http://lifesinger.github.com/lab/2010/cc-inline/test-cc-min.js</a></p>
<p>文件大小对比：</p>
<pre>
test.js           7.2K
test-cc-min.js    21.7K
test-yc-min.js    2.8K
</pre>
<p>看起来 CC 糟糕透了。不过看下 gzip 后的大小，心里平静很多：</p>
<pre>
test.js          1.2K
test-cc-min.js   750B
test-yc-min.js   1K
</pre>
<p>远没有想象中的糟糕。</p>
<h3>CC 常量内联的代价</h3>
<p>从下载传输量上看，即便像上面这样如此极端的代码，CC 的常量内联也没增加下载量，甚至能减少下载量。但从浏览器内存占用上看：<br>
<img src="http://lifesinger.org/blog/wp-content/uploads/2010/11/cc-vs-yc-memory.png" width="532" height="90"></p>
<p>浏览器 ungzip 后，耗费的内存差异还是非常明显的。CC 的常量内联碰上变态代码时，还是会给浏览器带来一点小杯具的-.-</p>
<h3>一切皆权衡</h3>
<p>在 Google Closure Compiler 的 <a href="http://code.google.com/closure/compiler/faq.html">答记者问</a> 里，有个挺有意思的回答：</p>
<blockquote><p>
Q: Does the compiler optimize for speed?</p>
<p>A: In most cases smaller code is faster code, since download time is usually the most important speed factor in web applications. Optimizations that reduce redundancies speed up the run time of code as well.
</p></blockquote>
<p>显然，对于常量内联来说，更多的是为了提高代码运行时的速度。因为有 gzip 保驾护航，常量内联并不会增加多少下载时间。对浏览器内存占用的增加，非极端情况下，一般也不会成为问题。</p>
<h3>写在最后</h3>
<p>从上面看起来，<code>var SELECTED = 'selected'; </code> 这种写法是没必要的。<br>
从压缩角度讲，的确没必要。</p>
<p>不过从代码维护角度讲，还是非常有必要的。<br>
这样可以避免将某处写成 <code>'seleted'</code>, 直到调试了半天才发现。</p>
<p>一切皆权衡。</p>