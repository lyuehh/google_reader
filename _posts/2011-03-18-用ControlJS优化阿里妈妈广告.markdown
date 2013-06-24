---
layout: post
title:  "用ControlJS优化阿里妈妈广告"
date:   2011-03-18 14:58:20
author: 灵玉
categories: program
---

## 用ControlJS优化阿里妈妈广告
### by 灵玉
### at 2011-03-18 14:58:20
### original <http://ued.taobao.com/blog/2011/03/18/controljs-alimama/>

<blockquote><p>长时间以来阿里妈妈的广告载入策略一直存在些问题，很多页面也因为阻塞式的广告载入而被拉低性能，影响用户体验。毕竟阿里妈妈广告的渲染依赖于诸多嵌套的document.write。ControlJS的目标就是解决js的阻塞式渲染，因此灵玉急不可耐想与同仁们共同去揭秘ControlJS……</p></blockquote>
<p><a href="http://stevesouders.com/bio.html">Steve Souders</a>在2010年12月份发布了<a href="http://stevesouders.com/controljs/">ControlJS</a>项目，该项目是让开发者更好的去控制javascript文件的下载和执行，从而提高了页面脚本的加载速度。</p>
<p>Steve提出了一个非常具有创造性的思想，就是预先异步下载javascript文件而不解析执行，直到需要的javascript处理时才去真正的执行。这一点得到了很多人的关注与验证。<a href="http://www.nczonline.net/about/">Nicholas Zaka</a>也因ControlJS引发了很多思考，并分析了<a href="http://stevesouders.com/controljs/">ControlJS</a>和<a href="http://labjs.com/">LABjs</a> 的区别所在，详细内容可以阅读<a href="http://www.nczonline.net/blog/2010/12/21/thoughts-on-script-loaders/">Thoughts on script loaders</a>和<a href="http://www.nczonline.net/blog/2011/02/14/separating-javascript-download-and-execution/">Separating JavaScript download and execution</a>。Steve使用3篇博闻详细介绍了<a href="http://stevesouders.com/controljs/">ControlJS</a>：<a href="http://www.stevesouders.com/blog/2010/12/15/controljs-part-1/">异步加载</a>、<a href="http://www.stevesouders.com/blog/2010/12/15/controljs-part-2/">延迟执行</a>、<a href="http://www.stevesouders.com/blog/2010/12/15/controljs-part-3/">重写document.write</a>。</p>
<ul>
<li>
<h2><strong>ControlJS的原理</strong></h2>
</li>
</ul>
<p><a href="http://ued.taobao.com/blog/wp-content/uploads/2011/03/controlJS.jpg"><img src="http://ued.taobao.com/blog/wp-content/uploads/2011/03/controlJS.jpg" alt="" width="436" height="357"></a></p>
<p><strong><span></span>异步加载</strong></p>
<p>ControlJS本身是异步进行加载的，首先将script的标签type属性值更改为浏览器无法识别的类型，这样浏览器不会认为这是一个脚本。本身异步加载的ControlJS执行时开始遍历type=”text/cjs”的script标签(包括内嵌脚本)，如果存在”DATA-CJSSRC”属性将创建IMAGE或者OBJECT对象(依赖浏览器而选择)，去异步预下载脚本文件并缓存文件，直到window.onload时解析并执行javascript，同时第二次去遍历遗漏的script标签。具体操作可看<a href="http://stevesouders.com/controljs/examples/async.php?t=1300427794">Async WITH ControlJS</a></p>
<p><strong>延迟执行</strong></p>
<p>浏览器在解析执行javascript阶段是阻塞任何操作的，这时的浏览器处于假死状态，Steve分析了美国的Alexa前10名网站的脚本初始需要加载执行情况，发现只有29％是需要页面加载时初始化解析执行的，而其他71%的脚本是在触发交互时才会执行，压缩后这些脚本平均加载是229 kB，未压缩是500KB，这是个大量的数据。</p>
<p>我们希望的结果是在页面加载中不去解析执行javascript，只是提前预下载好文件。例如通用的点击弹出二级导航，用户有可能永远都没有点击导航的行为，此时导航的功能脚本根本毫无用处。但是人们在点击导航时不希望等太久javascript的执行，所以ControlJS会提前下载文件，这样javascript只是解析执行，不会花时间放到下载文件上。代码一目了然，具体操作可看<a href="http://stevesouders.com/controljs/examples/menu.php?t=1300288303">Menu WITH ControlJS</a></p>
<p><strong>重写document.write</strong></p>
<p>在默认情况下这些异步脚本都是在window.onload解析执行，如果此时脚本调用window.write方法，将导致一个不希望发生的问题，就是整个页面被window.write的输出内容替换，所有页面内容将被删除，ie下将处于停滞状态。产生问题的原因是由于在document被加载完后调用docuemnt.write方法时将会自动去触发<a href="https://developer.mozilla.org/en/document.write">document.open</a>,写入任何处于打开状态的doucment都将会会替换整个页面的内容。这便导致目前为止所有异步脚本无法延迟document.write的问题，ControlJS的处理方法是重写document.write，如下：</p>
<pre>CJS.docwriteOrig = document.write;
document.write = CJS.docwrite;</pre>
<p>ControlJS创建一个dom元素(span)，将其插入当前被解析执行的script标签之前，并且设置SPAN的innerHTML的值为docuemnt.write的内容。具体操作可看<a href="http://stevesouders.com/controljs/examples/docwrite.php?t=1300427794">document.write WITH ControlJS</a></p>
<ul>
<li>
<h2><strong>用ControlJS</strong>优化阿里妈妈广告</h2>
</li>
</ul>
<p>对于现在大部分的广告形式都是采用document.write方式写入，无法将这些内容异步处理是开发者非常头疼的问题。ControlJS可以为我们解决这类烦恼。</p>
<p><a href="http://ued.taobao.com/blog/wp-content/uploads/2011/03/ad-demo.jpg"><img src="http://ued.taobao.com/blog/wp-content/uploads/2011/03/ad-demo.jpg" alt="" width="745" height="252"></a><br>
没有应用ControlJS的网络图。DEMO可以看<a href="http://chesihui.github.com/ad-demo.html">http://chesihui.github.com/ad-demo.html</a></p>
<p><a href="http://ued.taobao.com/blog/wp-content/uploads/2011/03/control-demo.jpg"><img src="http://ued.taobao.com/blog/wp-content/uploads/2011/03/control-demo.jpg" alt="" width="485" height="211"></a><br>
应用ControlJS优化后的网络图。DEMO可以看<a href="http://chesihui.github.com/ControlJS-demo.html">http://chesihui.github.com/ControlJS-demo.html</a></p>
<ul>
<li>
<h2><strong>ControlJS的局限性</strong></h2>
</li>
</ul>
<p>ControlJS存在一个问题是在document.write中多层嵌套script标签时，页面仍然存在触发document.open的问题。查看源代码发现在执行完一个javascript后都会恢复document.write的原生方法：</p>
<pre>document.write = CJS.docwriteOrig;</pre>
<p>动态脚本的异步加载，同样使得document.write方法也是异步执行，因此不能恢复document.write的原生功能。复现的情况如<a href="http://chesihui.github.com/docwrite.htm">DEMO</a>。注释这段脚本虽然解决了不触发window.open的问题，但是同样的异步加载执行导致无法正确定位广告写入的位置。对于阿里妈妈广告设置alimama_type=”i”的时候，载入图片广告是根据多层document.write实现的，只能正确渲染最后一个图片广告。复现如<a href="http://chesihui.github.com/docwrite-ad.htm">DEMO</a>。</p>
<p>因为ControlJS的异步加载不存在任何依赖顺序，所有脚本都是并行加载执行，如果你的页面存在太多依赖关系，ControlJS将不会适合你的项目。</p>
<p>最后总结ControlJS为我们做了什么事，利弊还需要自己去权衡：<br>
1、异步下载所有脚本<br>
2、同时处理内嵌与外链脚本<br>
3、延迟脚本的执行直到页面被渲染完<br>
4、允许脚本只下载不执行<br>
5、解决了异步脚本中存在document.write的问题<br>
6、ControlJS本身是异步加载</p>