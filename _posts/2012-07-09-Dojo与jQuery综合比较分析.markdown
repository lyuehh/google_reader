---
layout: post
title:  "Dojo与jQuery综合比较分析"
date:   2012-07-09 21:09:09
author: 童海波
categories: program
---

## Dojo与jQuery综合比较分析
### by 童海波
### at 2012-07-09 21:09:09
### original <http://blog.jobbole.com/23517/?utm_source=rss&utm_medium=rss&utm_campaign=dojo%25e4%25b8%258ejquery%25e7%25bb%25bc%25e5%2590%2588%25e6%25af%2594%25e8%25be%2583%25e5%2588%2586%25e6%259e%2590>

<p>最近Dojo和jQuery双双发布了最新的1.8版本，有着相同版本号的两个Javascript库也有许多核心的相同之处：相同的资源加载机制AMD、相同的选择器 引擎Sizzle等。作为业界知名的Javascript库，Dojo和jQuery在各自领域有着为数众多的拥护者。不过正所谓一把钥匙开一把锁，对一个项目来说肯定有个最适合它的工具库，用对了工具才能事半功倍。所以对项目经理或是技术总监来说，工程开始前的技术选型是关键一步，本文将对Dojo和 jQuery最新版本进行一个综合比较，重点在于区分两者的适用场景，而不涉及讨论孰优孰劣。</p>
<p><strong>Licence</strong></p>
<p>Dojo和jQuery都属于活跃的开源项目，并且都使用自由度很高的开源协议，可以免费使用，没有费用和许可问题。Dojo 许可协议为BSD&amp;AFL，jQuery许可协议为MIT&amp;GPL。</p>
<p><strong>框架组成</strong></p>
<p>Dojo属于重量级开发框架，由框架核心（dojo）、基本控件库（dijit）、扩展包（dojox）组成的，三部分都是由dojo官方提供。</p>
<p>jQuery属于轻量级框架，本身仅包含框架核心，另外有一个与jQuery名下的独立开源项目jQuery UI，它提供了最常用的8个控件。</p>
<p>另外jQuery的第三方开发者基于jQuery的框架核心开发了许多扩展控件和功能。不过这些第三方插件质量参差不齐，许可方式不同，需要慎重选择和使用。当选择了多家提供的插件时，还需要注意这些插件共存的兼容性问题。</p>
<p>总体来说，jQuery以及jQueryUI源于官方开发，比第三方插件更值得信赖，jQueryUI秉承了jQuery小块灵的特点，适合Web快速开发。不过鉴于jQueryUI提供控件数量上的限制，进行对UI交互依赖较重的应用时略显力不从心。</p>
<p><strong>核心包大小</strong></p>
<p>下图是Dojo与jQuery框架核心的大小比较，压缩后的dojo核心是135K，jQuery是93K。</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2012/07/1340868081_5499.png" rel="lightbox[23517]" title="Dojo与jQuery综合比较分析"><img title="Dojo与jQuery综合比较分析" src="http://blog.jobbole.com/wp-content/uploads/2012/07/1340868081_5499.png" alt="Dojo与jQuery综合比较分析" width="483" height="187"></a></p>
<p><strong>编程风格</strong></p>
<p><strong></strong>Dojo使用面向对象编程方式，为大型应用开发提供了保证；jQuery使用函数式编程方式，开发小型应用时更加灵活快捷。</p>
<p>Javascript 自身使用原型链模拟继承，但仅仅依靠原型链模拟的类继承不能提供全面的面向对象能力。Dojo在Javascript的基础进行了面向对象能力的加强和规 范化，提供了原生Javascript不具备的面向对象编程能力，比如父类方法重载（注意，不是重写）、多继承、构造函数调用链等等，并提供一系列面向对象编程规范的函数和属性declare、inherit、declaredClass、extend等作为dojo自身的编程基础。在dojo中，所有的 UI控件都被定义为类，许多Dojo的核心功能库也被定义为类，这都是出于更好的代码重用性的考虑。</p>
<p>Javascript从本质上来说属于函数式<span><a href="http://blog.jobbole.com/tag/%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80/" title="如何选择语言和编程语言排名相关文章">编程语言</a></span>，jQuery没有改变Javascript的编程方式，使其学习成本大大降低。</p>
<p><strong>常用功能支持情况</strong></p>
<p>下图中数据来源自wiki，包含了一些流行的Javascript框架对于Web项目开发中经常会出现的功能需求的支持情况。本文仅涉及dojo与jQuery，其他框架的支持情况请看原文链接（<a href="http://en.wikipedia.org/wiki/Comparison_of_JavaScript_frameworks">http://en.wikipedia.org/wiki/Comparison_of_JavaScript_frameworks</a>)</p>
<table border="1" cellpadding="0">
<tbody>
<tr>
<td></td>
<td>
<p align="center"><strong><a title="Dojo Toolkit" href="http://en.wikipedia.org/wiki/Dojo_Toolkit">Dojo</a></strong></p>
</td>
<td>
<p align="center"><strong><a title="JQuery" href="http://en.wikipedia.org/wiki/JQuery">jQuery</a></strong></p>
</td>
</tr>
<tr>
<td><strong>版本</strong></td>
<td>                1.7.2</td>
<td>       1.7.2</td>
</tr>
<tr>
<td><strong>代码组成</strong></td>
<td>JavaScript + HTML</td>
<td>JavaScript</td>
</tr>
<tr>
<td><strong><span><a href="http://blog.jobbole.com/12749/" title="浏览器">浏览器</a></span>功能检测</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>DOM API包装</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>XMLHttpRequest</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>JSONP技术</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>接收服务器推送数据</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>其他数据格式支持</strong></td>
<td>
<p align="center">Yes: XML, HTML, CSV, ATOM</p>
</td>
<td>
<p align="center">Yes: XML, HTML</p>
</td>
</tr>
<tr>
<td><strong>拖拽功能</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>页面视觉特效</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>事件绑定</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>动作撤销/行为历史管理</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>输入验证</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>表格（Grid）控件</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>树控件</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>编辑器控件</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>自动补全输入</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>HTML生成器</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>自定义控件主题</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>Modal Dialog &amp; Resizable Panel</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugins</p>
</td>
</tr>
<tr>
<td><strong>布局控件</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugin</p>
</td>
</tr>
<tr>
<td><strong>Canvas</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td></td>
</tr>
<tr>
<td><strong>手持设备支持</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugin</p>
</td>
</tr>
<tr>
<td><strong>Accessibility</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>ARIA支持</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>可视化编程工具</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">Yes</p>
</td>
</tr>
<tr>
<td><strong>离线存储</strong></td>
<td>
<p align="center">No</p>
</td>
<td>
<p align="center">No</p>
</td>
</tr>
<tr>
<td><strong>跨浏览器的矢量绘图</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td></td>
</tr>
<tr>
<td><strong>图表控件</strong></td>
<td>
<p align="center">Yes</p>
</td>
<td>
<p align="center">With plugin</p>
</td>
</tr>
<tr>
<td><strong>Internet Explorer</strong></td>
<td>                   6+</td>
<td>           6+</td>
</tr>
<tr>
<td><strong>Mozilla Firefox</strong></td>
<td>                   3+</td>
<td>           2+</td>
</tr>
<tr>
<td><strong>Safari</strong></td>
<td>                    4</td>
<td>           3+</td>
</tr>
<tr>
<td><strong>Opera</strong></td>
<td>             10.50 only</td>
<td>           9+</td>
</tr>
<tr>
<td><strong>Chrome</strong></td>
<td>                    3</td>
<td>           1+</td>
</tr>
</tbody>
</table>
<p>由上图可见，dojo作为重量级的Javascript框架，提供了对绝大多数Web开发功能需求的支持。而jQuery除了对Web绘图功能支持不够以外，其余功能基本都可以通过引入第三方插件来提供支持。不过还是会涉及到前文提到的代码协议问题和插件间的兼容性问题。</p>
<p><strong>代码风格</strong></p>
<p>从代码风格上来看，除去jQuery标志性的“$”符号外，其实dojo与jQuery在一些常用API上的命名和参数列表基本相似。</p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2012/07/1340701999_7099.png" rel="lightbox[23517]" title="Dojo与jQuery综合比较分析"><img title="Dojo与jQuery综合比较分析" src="http://blog.jobbole.com/wp-content/uploads/2012/07/1340701999_7099.png" alt="Dojo与jQuery综合比较分析" width="548" height="322"></a></p>
<h2></h2>
<p><strong>核心代码性能</strong></p>
<p>Web应用中的性能非常关键，dojo与jQuery在核心功能上的性能指标在Javascript框架中都属优秀，那么他们之间的比较结果如何呢？本文将比较两者在DOM操作、选择器以及事件绑定这三个最常用功能上的性能指标。场景如下：</p>
<p>1．  添加500个DOM节点；</p>
<p>2．  改变添加节点的style属性；</p>
<p>3．  在500个节点中选择一部分节点改变其innerHTML；</p>
<p>4．  为每个节点绑定鼠标事件；</p>
<p>这四步对应的dojo代码如下：</p>
<pre>function dojoStep1() {
    var html = &quot;&quot;;
    for (var i = 0; i &lt; 500; i++) {
        html += &quot;&lt;div&gt;&lt;span class=test data=0&gt;&quot; + i + &quot;&lt;/span&gt;&lt;/div&gt;&quot;;
    }
    dojo.byId(&quot;container&quot;).innerHTML = html;
}    

function dojoStep2 () {
    dojo.query(&quot;.test&quot;, &quot;container&quot;).style(&quot;color&quot;,&quot;red&quot; );
}    

function dojoStep3() {
    dojo.query(&quot;#container div:nth-child(odd)&quot;).addContent(&quot;&lt;span&gt;odd row:&lt;/span&gt;&quot;);
    dojo.query(&quot;#container div:nth-child(even)&quot;).addContent(&quot;&lt;span&gt;even row:&lt;/span&gt;&quot;);
}    

function dojoStep4() {
    dojo.query(&quot;#container span&quot;).on(&quot;mouseenter,mouseleave&quot;, function(e){
        if(e.type == &quot;mouseenter&quot;){
            dojo.style(e.target, &quot;color&quot;, &quot;blue&quot;);
    }
    });
}</pre>
<p>四个步骤对应的jQuery代码如下：</p>
<pre>function jQueryStep1() {
    var html = &quot;&quot;;
    for (var i = 0; i &lt; 500; i++) {
        html += &quot;&lt;div&gt;&lt;span class=test data=0&gt;&quot; + i + &quot;&lt;/span&gt;&lt;/div&gt;&quot;;
    }
    $(&quot;#jContainer&quot;)[0].innerHTML = html;
}    

function jQueryStep2() {
    $(&quot;#jContainer .test&quot;).css({ color: &quot;blue&quot; });
}    

function jQueryStep3() {
    $(&quot;#jContainer div:even&quot;).append(&quot;&lt;span&gt;even row:&lt;/span&gt;&quot;);
    $(&quot;#jContainer div:odd&quot;).append(&quot;&lt;span&gt;odd row:&lt;/span&gt;&quot;);
}    

function jQueryStep4() {
    $(&quot;#jContainer span&quot;).hover(function () {
        $(this).css(&quot;color&quot;, &quot;red&quot;);
    }, function () {
        $(this).css(&quot;color&quot;, &quot;black&quot;);
    });
}</pre>
<p>Firefox/Chrome/IE8的测试结果如下所示：</p>
<p> </p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2012/07/1340714509_3497.png" rel="lightbox[23517]" title="Dojo与jQuery综合比较分析"><img title="Dojo与jQuery综合比较分析" src="http://blog.jobbole.com/wp-content/uploads/2012/07/1340714509_3497.png" alt="Dojo与jQuery综合比较分析" width="505" height="324"></a></p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2012/07/1340714706_1591.png" rel="lightbox[23517]" title="Dojo与jQuery综合比较分析"><img title="Dojo与jQuery综合比较分析" src="http://blog.jobbole.com/wp-content/uploads/2012/07/1340714706_1591.png" alt="Dojo与jQuery综合比较分析" width="506" height="322"></a></p>
<p style="text-align:center"><a href="http://blog.jobbole.com/wp-content/uploads/2012/07/1340714821_8160.png" rel="lightbox[23517]" title="Dojo与jQuery综合比较分析"><img title="Dojo与jQuery综合比较分析" src="http://blog.jobbole.com/wp-content/uploads/2012/07/1340714821_8160.png" alt="Dojo与jQuery综合比较分析" width="505" height="350"></a></p>
<p> </p>
<p>根据Dojo1.7.2与jQuery1.7.2几个核心函数的比较结果可见，dojo与jQuery的选择器性能相差无几，dojo略胜一筹。由于 dojo和jQuery中的选择器就是dojo基金会下的项目Sizzle.js,所以这个结果也比较合理；从DOM操作来看，dojo的性能也优于jQuery；在事件绑定方面，dojo的性能明显高于jQuery。</p>
<p><strong>总结</strong></p>
<p>Dojo在众多前台框架中，属于重量级开发框架，在面向对象支持，代码架构，多极模块加载机制，控件完整性等方面有着较为突出的特点，适用于企业级或是复杂的大型Web应用开发；jQuery属于轻量级开发框架，架构和机制相对简单，易于开发，应用广泛，适用于相对简单的Web 2.0开发。 Dojo和jQuery分别为复杂应用开发和简单应用开发设计，由此也带来二者一些技术特点的不同。从工具本身角度看，二者特点鲜明，在实际项目中，需要根据具体需求来衡量，进行工具的选择。</p>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/21136/" title="10 个 jQuery 图表插件推荐">10 个 jQuery 图表插件推荐</a></li><li><a href="http://blog.jobbole.com/18513/" title="50个jQuery代码段帮你成为更出色的JS开发者">50个jQuery代码段帮你成为更出色的JS开发者</a></li><li><a href="http://blog.jobbole.com/12203/" title="完全理解关键字this">完全理解关键字this</a></li><li><a href="http://blog.jobbole.com/11468/" title="2011年使用率增长最快的十大Web技术">2011年使用率增长最快的十大Web技术</a></li><li><a href="http://blog.jobbole.com/9743/" title="10 个jQuery/JavaScript的圣诞主题效果 ">10 个jQuery/JavaScript的圣诞主题效果 </a></li><li><a href="http://blog.jobbole.com/864/" title="16个流行的JavaScript框架">16个流行的JavaScript框架</a></li><li><a href="http://blog.jobbole.com/23563/" title="Javascript定义类（class）的三种方法">Javascript定义类（class）的三种方法</a></li><li><a href="http://blog.jobbole.com/23182/" title="jQuery 1.9和2.0版路线图问答集锦">jQuery 1.9和2.0版路线图问答集锦</a></li><li><a href="http://blog.jobbole.com/22687/" title="简讯：jQuery 2.0将不再支持IE 6/7/8">简讯：jQuery 2.0将不再支持IE 6/7/8</a></li><li><a href="http://blog.jobbole.com/22262/" title="jQuery 开启 1.8 时代，1.8 Beta 1 发布">jQuery 开启 1.8 时代，1.8 Beta 1 发布</a></li></ul>