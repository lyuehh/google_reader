---
layout: post
title:  "javascript 框架功能大剖析"
date:   2012-03-26 08:14:00
author: 司徒正美
categories: program
---

## javascript 框架功能大剖析
### by 司徒正美
### at 2012-03-26 08:14:00
### original <http://www.cnblogs.com/rubylouvre/archive/2012/03/26/2416556.html>

<p>javascript框架有什么功能，这个不是框架作者随意决定，这与人们经常用JS做什么事直接相关。
javascript框架是从common.js这样工具函数集演变过来，最重要的特征是实用。由于框架作者肯定是从
一线开发人员过来的，这个他心里有数，再结合主流框架的功能调查，就万无一失了。</p>
<p><strong><a href="http://jquery.com/">jQuery</a></strong></p>
<p>jQuery强在它专注于DOM操作的思路一开始就是对的，以后就是不断在兼容性，性能上进行改进。</p>
        <ul>
            <li>ajax  数据交互（1.5来自社区）</li>
            <li>attributes 属性操作，共分className, 表单元素的value值，属性与特征四大块。</li>
            <li>callbacks  函数列队</li>
            <li>core  种子模块，命名空间，链式结构，domReady，多库共存。</li>
            <li>css 样式操作，引入DE大神的两个伟大的hacks，基本上解决精确获取样式的问题。</li>
            <li>data 数据存取。</li>
            <li>deferred 异步列队（三合一版的函数列队）</li>
            <li>dimensions 元素尺寸的设置读取（来自社区）。</li>
            <li>effects 动画引擎</li>
            <li>event 事件系统（基于DE大神的事件系统与社区的两个插件）</li>
            <li>exports AMD系统(被RequireJS作者说服加几行代码支持其东东)</li>
            <li>manipulation 节点的操作</li>
            <li>offset 元素offsetTop(Left)的设置读取（来自社区）</li>
            <li>queue 列队模块（deferred与data的合体）。</li>
            <li>sizzle 从右到左进行解析的选择器引擎。</li>
            <li>support 特征侦测</li>
            <li>traversing 元素遍历。</li>
        </ul>
        <p>jQuery经过多年的发展，拥有庞大的插件与完善的BUG提交渠道，因此可以通过社区的力量不断完善自身。</p>
<p><strong><a href="http://www.prototypejs.org/">Prototype.js</a></strong></p>
<p>早期的王者，它分为四大部分:</p>
        <ul>
            <li>语言扩展</li>
            <li>DOM扩展</li>
            <li>AJAX部分</li>
            <li>废弃部分（新版本使用其他方法实现原有功能）</li>
        </ul>
        <p> Prototype.js的语言扩展覆盖面非常广，包括所有基本数据类型及从语言借鉴过来的“类”。
            其中Enumerable 只是一个普通的方法包， ObjectRange，PeriodicalExecuter，Template
            则用Class类工厂生产出来的。Class类工厂来自社区贡献，强大无比，
            与mootools的类工厂不相伯仲。</p>
        <p>DOM部分分成五块。dom.js花了好大劲在IE下模拟Node, Element等对象，
            并在Element原型上进行扩展。event.js就是跨浏览器的多投事件系统与domReady，
            在早些年先进到不得了，那个observe，fire，stopObserving的命名真是太好了。
            form.js是专门处理表单。layout是专门处理样式。selector.js是用于关联选择器引擎，
            现在竟然用死对头的Sizzle——shit， 真是落魄！</p>
         <p> AJAX部分是重头戏，早期框架最大的卖点，里面N个对象都是用类工厂制造的，并存在多层的继承关系。</p>
        <p><strong><a href="http://mootools.net/">mootools</a></strong></p>
        <p>比Prototype.js的入侵性更强，但由于API设计得非常优雅，官网上一大堆优质插件， 强大的团队， 因此才没有在原型扩展的反对浪潮中没落。</p>
        <ul>
            <li>Core 自1.3起,所有数据类型或原生对象都封装成Type类型,Type方法可以说是其第一类工厂</li>
            <li>Type 对Number, Object(原Hash), String, Array, Function进行扩展, 事件对象的封装</li>
            <li>Brower 检测浏览器与OS的类型与版本, Flash版本, XMLHTTP对象的创建方法</li>
            <li>Class 第二类工厂</li>
            <li>Slick mootools的新一代选择器引擎，支持CSS3高级伪类。</li>
            <li>Element 对元素的创建,克隆,插入,移除,样式操作,事件绑定，事件代理进行封装</li>
            <li>Request 数据交互</li>
            <li>Fx 动画引擎</li>
            <li>Utilities 对cookie, domReady, JSON, Flash(Swiff)提供便捷的工具方法</li>
        </ul>
        <p><strong><a href="http://rightjs.org/">RightJS</a></strong></p>
        <p>又一个在原型上进行扩展的框架，不过自创了许多东东。</p>
        <ul>
            <li>core 提供一个类工厂, Observer类, Option对象</li>
            <li>lang对array, function, json, math, number, object, regexp, string进行扩展与修复</li>
            <li>dom 提供各种dom 操作, 选择器寻找, 事件绑定与代理, cookie, domReady</li>
            <li>fx 动画引擎</li>
            <li>xhr 数据交互</li>
            <li>olds 对旧式IE的各种兼容</li>
        </ul>
        <p><strong><a href="http://mochi.github.com/mochikit/">MochiKit</a></strong></p>
        <p>一个python风格的框架，以前能进世界前十名的。现在作者跑去当CTO了。</p>
        <ul>
            <li>Base 提供命名空间, isXXX系列, 将操作符变成函数,还有N多工具方法。</li>
            <li>Async 数据交互以及从python引进Deferred(异步列队)。</li>
            <li>Color 颜色类型转换。</li>
            <li>DOM  节点的增删改查。</li>
            <li>Format 字符串与数字的格式化。</li>
            <li>DateTime 时间的格式化。</li>
            <li>DragAndDrop 拖放组件。</li>
            <li>Iter 一系列迭代器。</li>
            <li>Logger 调试用的日志。</li>
            <li>LoggingPane Logger带UI的升级版。</li>
            <li>MochiKit 用document.write引进框架的所有JS文件。</li>
            <li>Position 取得元素位置的相关方法。</li>
            <li>Selector 选择器引擎。</li>
            <li>Signal 事件系统。</li>
            <li>Sortable 排序组件。</li>
            <li>Style 样式操作。</li>
            <li>Test 单元测试。</li>
            <li>Visual 动画引擎。</li>
        </ul>
        <p><strong><a href="http://www.hatena.ne.jp/js/Ten/Ten.js">Ten</a></strong></p>
        <p>日本著名博客社区Hatena的javascript框架, 由日本顶尖高手amachang开发,核心大致于2008年完工。
            受Prototype.js影响， 但一点侵入性也没有，是最早期以命名空间为导向的框架的典范。 </p>
        <ul>
            <li>Ten对象，作为框架的基点，所有模块作为其属性进行扩展。</li>
            <li>Ten.Class， 类工厂， 与Prototype.js1.6引入的类工厂很像，但比它早。</li>
            <li>Ten.Function, Ten.Array， 两个方法包，提供一些工具方法。
                除了这两个，Ten的其他模块都是由类工厂制造出来。</li>
            <li>Ten.JSONP，估计世界上最早装备JSONP的几个javascript框架之一了。</li>
            <li>Ten.XHR，基于XMLHttpRequest的数据交互。</li>
            <li>Ten.Observer，提供订阅发布机制， Prototype.js的泊来物。</li>
            <li>Ten.Event，Ten.EventDispatcher这两个类分别对事件对象与多投事件进行封装。</li>
            <li>Ten.DOM DOM的增删改查，里面有一个addObserver提供domReady功能。</li>
            <li>Ten.Element 用于创建元素。</li>
            <li>Ten.Cookie cookie的操作。</li>
            <li>Ten.Selector，Ten._Selector，Ten._SelectorNode与Ten.querySelector共同构建其选择器引擎。</li>
            <li>Ten.Color 颜色的转换。</li>
            <li>Ten.Style 样式的设置。</li>
            <li>Ten.Geometry 元素位置的侦测。</li>
            <li>Ten.Position 相当于一个Point类。</li>
            <li>Ten.Logger 日志组件。</li>
            <li>Ten.Browser 浏览器嗅探。</li>
            <li>Ten.Deferred 由另一个日本顶尖高手cho45发明的异步列队，
                日本人对此异步列队的密集研究讨论持续了三年多，可见这东西非常NB。</li>
        </ul>
    <p><strong><a href="https://github.com/RubyLouvre/mass-Framework">mass Framework</a></strong></p>
        <p>一个模块化，以大模块开发为目标，jQuery式的框架。里面涉及的HTML5新API数量，估计除了纯净的手机框架外，无人能敌。</p>
        <ul>
            <li>
                mass.js 种子模块 提供类型识别 模块加载 糅合机制 日志 高速化判定 domReady 简单的事件绑定与移除 多库共存 多版本共存。
                别看这么多功能，其实本模块体积是非常小的。
            </li>
            <li>lang.js 提供几个isXXX方法，parseXXX方法，以及一个语言链对象。此对象能对字符串，数字，对象，类数组对象进行优雅的链式操作，
                相当于把Underscore.js这个库整进去，但两者实现机理完全不一样。</li>
            <li>lang_fix.js 补丁模块, 提供ECMA262v5大部分新API的支持, 并修复IE一些BUG。</li>
            <li>support.js 特征嗅探</li>
            <li>class.js  类工厂</li>
            <li>query.js 第五代选择器引擎Icarus， 兼容CSS3高级伪类与jQuery所有自定义伪类。</li>
            <li>node.js 提供一个节点链对象（与jQuery对象的API 95%兼容， 像wrap等不常用方法被剔除了）</li>
            <li>data.js 数据缓存</li>
            <li>css.js 相当于jquery的css dimensions offset的三合一加强版，因为它支持CSS3的transform2D。 </li>
            <li>css_fx.js 补丁模块， 将对旧式IE的兼并代码独立出去。 </li>
            <li>target.js 对事件对象进行封装，并提供自定义事件机制。</li>
            <li>event.js 事件绑定与事件代理。 </li>
            <li>flow.js 操作流，提供比异步列队更强大的处理异步的机制。</li>
            <li>attr.js 属性操作，同jQuery。</li>
            <li>ajax 数据交互。</li>
            <li>fx 动画引擎。</li>
        </ul>
        <p>经过细节比较，我们很易得出以下结论</p>
        <ul>
            <li>选择器，domReady, ajax是现代框架的标配。</li>
            <li>动画引擎，除非你的框架像Prototype.js那样拥有像script.aculo.us这样顶级的动画框架做后盾，最好也加上。</li>
            <li>DOM操作是重中之重，节点的遍历，样式操作，属性操作也属于它的范畴，是否细分看你框架的规模。由于完全模块化了，
                mass Framework基至可以将旧式IE的兼容代码独立出去。</li>
            <li>现在主流的事件系统都支持事件代理了。</li>
            <li>对基本数据类型的操作是必须的，像jQuery还是不得不提供trim, camelCase, each, map等方法。
                像Prototype.js等侵入式框架则可以肆无忌惮地在原型上添加camelize等好用方法。</li>
            <li>类型的判定必不可少，常见形式是isXXX系列。</li>
            <li>brower sniff已死， feature detect当立。</li>
            <li>异步列队等处理回调的方案的流行</li>
            <li>即使不专门提供一个类工厂，也应该存在一个叫extend或mixin的方法对对象进行扩展。
jQuery虽然没有类工厂，但在jQuery ui中也不得不整一个，可见其重要性。</li>
           <li>自从jQuery搞鼓出一个叫noConflict的方法，新兴的小库们都带此方法，以求狭缝中生存。</li>
           <li>许多框架是非常重视cookie的操作。</li>
        </ul>
<p>最后唠叨一下，当你成为高手时，一定已经在写或写过框架了。这就像一位剑豪，必然开创自己的流派。为什么前端的工资普遍不如后端呢，
正是因为前端JSer的平均水平实在太低了，大多数人不是科班出身， 还懒得要命， 写几行代码还上网找插件， 扣人家代码， 
具备写框架功能的人实在太少了。PHP框架数量上千个，天天在增加。java, C, C#的人不但玩框架，还玩编译器，制造语言去了。
而能玩ruby, python的人基本上衣食无忧， 闲得蛋疼，不说了。就前端，特别是中国前端的人的整体素质最差， 既然你只会jQuery， 
老板随便拉几个后端学上一两天也会jQuery，要你何用？！本段话是针对“不要重复造轮子”的人说的。其实老外也没有说过这句话，
人家是说，“不要重复发明轮子”。为了不成为前端攻城师当炮灰， 大家努力搞鼓个框架出来当前端架构师吧。</p><img src="http://www.cnblogs.com/rubylouvre/aggbug/2416556.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/rubylouvre/archive/2012/03/26/2416556.html">本文链接</a></p>