---
layout: post
title:  "Silverlight布局,也许不止这么多(附照片墙示例及源码)"
date:   2010-10-12 15:52:00
author: hievis
categories: program
---

## Silverlight布局,也许不止这么多(附照片墙示例及源码)
### by hievis
### at 2010-10-12 15:52:00
### original <http://www.cnblogs.com/hielvis/archive/2010/10/12/1847025.html>

<p><a href="http://www.cnblogs.com/hielvis/"><img src="http://pic.cnblogs.com/face/u140818.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/hielvis/">hievis</a> 发表于 2010-10-12 15:52 <a href="http://www.cnblogs.com/hielvis/archive/2010/10/12/1847025.html">原文链接</a> 阅读: 1761 评论: 22</p><p><span style="color:#00ff00"><strong><span style="font-size:18pt">前言</span></strong></span></p>
<p>    很喜欢一种人，他们可以把一种技术分析得很透彻，由浅入深，深入浅出，不管你是初学者还是资深人士，看上去都会感觉非常舒服。</p>
<p>    但是有时候，试图去描述一个技术点是很困难的事情。</p>
<p>    开始，你觉得这个技术很有价值，你通过自己花了一定时间去学习，想要把自己的理解与心得分享，于是就打算写一篇随笔；可是当你开笔之后，突然发现描述起来很困难，虽然在你心中那个概念已经很简单了，但是却要用很多文字来描述，而且不同的技术可能有不同的“上下文”，每个人可能也有不同的“上下文”，在写了几个字之后你有点想放弃了；可是最后又不甘心，聪明的你灵机一动，做了一个Demo，大概对着图片描述一下，然后发布，让读者自己去猜吧！</p>
<p>    我想说的是，这并不算心得和学习经验，这充其量算是一篇索引或者“提醒”，读者看了想，还可以这样子，于是又不得不自己去找资料学习。因为你心中的经验没有描述出来啊。</p>
<p>    想想看，我们现在学习技术的途径是什么。可能主要是SDK之类的，其次呢，我相信你印象应该很深刻：其中有一些是别人写的你觉得非常好的文章。很多Blog尤其是国外的Blog已经成为了我们提升技术和积累经验的一种无形的方式。</p>
<p>    文化的传承有口述和媒介承载，口述精准简单，但是也许文字更能深刻的描述事物而且可以反复理解，所以好的文字是文化传载的重要方式。</p>
<p>    诚然，写技术文章也许需要一点点技巧，有些时候描述一个技术点需要篇幅，需要时间，需要耐心，更需要的是对读者负责，你能为读者带来什么？</p>
<p>    所以，写作需要用心，从构思到文字，这篇文章的价值是什么？你对技术的理解是什么，不要因为时间问题（一篇有内容的文章肯定需要花很多时间）就对你认为核心的理解一笔带过。</p>
<p>    我相信，这样才是再学习的过程，这样才对得起读者的时间和关注，这样才叫分享！</p>
<p> </p>
<p><strong><span style="color:#ff00ff;font-size:14pt">Silverlight中的布局</span></strong></p>
<p>    对于Silverlight学习来说，首先要面对的应该是布局：你得把元素放到你想摆放的位置，然后是考虑元素的层次以及可见性，之后可能你想让它动起来，就学习动画，最后理解更深入之后，可能会开发如Behavior之类的行为，或者设置复杂的控件状态，模板。</p>
<p>    很多教程是从使用Grid开始，然后是Canvas，StackPanel之类的控件，他教你怎样设置元素的位置。然后就没有下文，很少会去讲解布局的原理，不过如果是看Silverlight SDK，是能看到的。其实最好的教程就是Silverlight SDK，包括像两个不同的SL插件（即使它们在不同的浏览器窗口）之间的通信这些一般人没有注意到的特性，里面都是讲得清清楚楚。所以，建议不要花钱去买Silverlight相关的书。</p>
<p>    其实，不是能使用Grid之类的就知道了布局，Silverlight布局包含更多的东西，理解布局系统有利于更深层的理解Silverlight，从而开发更得心应手。</p>
<p>    遗憾的事初学者理解布局也许有点复杂，可能是因为其中的递归，但是我们生活中其实有很多递归系统的。我试着学习郭欣用铁路系统描述网络传输一样，也来构造这样一个场景。</p>
<p> </p>
<p><span style="color:#0000ff"><strong><span style="font-size:18pt">故事场景-巴比难题</span></strong></span></p>
<p>    我们假设巴比基金现在愿意把所有的钱捐献给某个国家的收入在该国平均收入以下的老百姓。</p>
<p>    但是他们不能自己去每个地方发放这笔资金，所以要求该国中央政府处理好这件事情，将这笔资金发放到各个省份的相关百姓手中。</p>
<p>    中央政府高度重视这件事情，但是这么大的工程应该怎样完成了，财政官员很为难？</p>
<p>    这时候负责行政的长官提出了自己的建议，中央政府觉得很好，于是采纳了该建议。</p>
<p>    行政长官的建议分为两道程序：调查和发放。</p>
<p>    <strong>调查</strong>：首先中央向各级地方行政官发出调查通知，要求各地方政府统计一下年收入在平均收入以下的人数，并要汇报每个人的差额。省级行政长官在收到通知后统计了一下省会的情况，然后把通知转发给县级行政官；县级行政官接到通知还是首先统计县城的情况，然后转发通知到各个村子。村子就是最小的行政单位了，于是村委会秘密调查了每个人的收入，并计算差额。然后上报给县级，县级在结合县里面的情况，整理一份整个县的报告给省级，省级重新整理，上报给中央政府。</p>
<p>    <strong>发放</strong>：哈哈，现在中央政府就拿着全国的需求单。哇，全国有1000亿的需求，而巴比只基金只有500亿。也不好意思叫巴比找中国的富豪劝捐。于是只有把每个省的比例缩小一半了，并且最后做了一些调整，有的省份潜力比较好，自己能解决一些问题，就少发放一些，有些地方经济差，就多发了一些，有的省级居然乱报说需要10000亿，可能是计算错误，最后只发了100，有的地方虽然贫穷，但是很有志气，不想要美国人的施舍，一分钱都没有报。</p>
<p>    这样，做一些根据实际情况调整之后，中央政府就按照调整的数额将钱发放到各个省级政府，省级政府拿到钱感到不对劲，感觉跟自己上报金额不一致，但有不好意思反应情况，毕竟上面是老大，想给多少就多少，反应也没有用；但是有些政府也发现比自己期望的要多，有良知的官员还是决定平分到每个县级，但是也有一些官员主张直接贪污算了。</p>
<p>    总之也按中央政府的做法重新调整各个县的发放金额，然后发放到各个县级，最后到村子，村委会在发到每个人手中。</p>
<p>    这个巴比问题就这样解决了。</p>
<p>    值得注意的是：</p>
<p>    1，各级政府在发放调查通知的时候也可以给县级政府一个指标，我能给你的最大金额，或者最小金额，或者固定金额，或者无限制，你尽管反应实际情况，这样下级政府可能就会在上报前做一个实际调整。但下级也要做好心理准备，这个数字不一定是真的，只是个参考数字。</p>
<p>    2，每个人或政府在发放之后拿到的钱不一样，可能比需要的要多，这个时候政府可以选择贪污或者公平发放，而来百姓会去买二锅头庆祝一下；有时候比需要的少，这个时候就只有拮据点花了。</p>
<p><span style="color:#ff6600"><em>    读完这个故事，我们想一想，把一定数量的钱一层一层的发到每个人手里，是不是类似于Silverlight把插件大小的空间一块一块地分给每个空间啊？</em></span></p>
<p><span style="color:#ff6600"><em> </em></span></p>
<p><span style="color:#ff00ff"><strong><span style="font-size:18pt">布局原理</span></strong></span></p>
<p><strong><span style="color:#ff00ff;font-size:18pt">   </span></strong>没错，Silverlight也是按类似的原理来实现布局：</p>
<p>    首先，所有元素的最顶层必须是一个容器（通常如Grid，Canvas，StackPanel等），然后在容器中摆放元素，容器中也可能包含容器。这里的容器就像行政长官一样，他们负责分配元素的空间。同样，首先顶层的容器一个一个的问自己的子元素：你想要多大的空间？如果子元素也是容器，它又继续向下递归，最后又顶层开始向上汇报。这就是所谓的测量。</p>
<p>    测量完之后就是排列，这个时候每个容器知道自己每个子元素想要的空间大小，<span style="color:#ff0000"><strong>就按自己的实际情况进行分配</strong></span>。一致递归到最底层。</p>
<p>    注意上述红色字体部分，通过前面的故事，我们知道，资金的发放完全由行政长官控制，不管下面想要多少，都是他说了算，他甚至可以一分钱都不给，或者给你超多你的预期的数目。</p>
<p>    这里的容器也一样，容器拥有完全的分配权，不过这里容器不仅仅是分配空间，还决定元素的位置，因为空间总是跟位置相关的。也就是说，容器说想给你多大空间你就只有有那么大的空间可使用，容器想让你摆在什么位置，你就得乖乖呆着什么位置。</p>
<p>    只不过，这里的容器是遵守规则的，它遵守开发者指定的规则：</p>
<p>    <span style="color:#ff0000"><strong>Grid</strong></span>的规则是：我把我这个空间分成一格一格的格子，看起来有些像Table，在我里面的元素我完全按照附加属性Grid.Row,Grid.Column，Grid.RowSpan,Grid.ColumnSpan来决定其大小和位置。</p>
<p>    <strong><span style="color:#ff0000">Canvas</span></strong>的规则是：我读取附加属性Canvas.Left,Canvas.Right,Canvas.Top,Canvas.Bottom，并以此来决定元素的位置，我通常不限制元素开用空间</p>
<p>    <span style="color:#ff0000"><strong>StackPanel</strong></span>的规则是：根据附加属性，我要么让元素横着排列，要么竖着排列。</p>
<p> </p>
<p>    聪明的你是不是立刻想到，我可不可以定义自己的规则呢？哈哈，当然可以！</p>
<p>    比如，你可以让Panel里面的元素随机分布，并可让它们随机旋转一定角度，这不就是现在某些很酷的相册吗；你可以让元素排成一个圆形，这不就是Blend里面的例子吗；你可以让元素根据某个Path元素排列，这不就是PathListBox吗？如下图：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010101123201830.jpg" alt=""> <img src="http://pic002.cnblogs.com/images/2010/140818/2010101123215577.jpg" alt="" width="490" height="323"></p>
<p>    所以，你现在是不是觉得布局不是那么简单并且很好玩呢？</p>
<p>    下面我们就来看怎么实现这么酷的东西！</p>
<p style="padding-left:30px"> </p>
<p><strong><span style="color:#ff0000">基础框架-FrameworkElement</span></strong></p>
<p style="padding-left:30px"><span><span style="color:#0000ff"><em>为Silverlight布局中涉及的对象提供公共API的框架</em></span>。FrameworkElement还定义在Silverlight中与数据绑定，对象树和对象生存期功能区域相关的API。</span></p>
<p style="padding-left:30px"><span>继承层次结构：</span></p>
<div style="padding-left:30px"><span style="color:#000000">System<span><span>.</span></span>Object</span><br><span style="color:#000000">  System.Windows<span><span>.</span></span>DependencyObject</span><br><span style="color:#000000">    System.Windows<span><span>.</span></span>UIElement</span><br><span style="color:#000000">    <strong>  <span>System.Windows<span><span>.</span></span>FrameworkElement</span></strong></span><br><span style="color:#000000">        System.Windows.Controls<span><span>.</span></span>Border</span><br><span style="color:#000000">        System.Windows.Controls<span><span>.</span></span>Control</span><br><span style="color:#000000">        System.Windows.Controls<span><span>.</span></span>Panel</span><br><span style="color:#000000">        System.Windows.Shapes<span><span>.</span></span>Shape</span></div>
<p>     </p>
<p>      类定义：</p>
<p style="padding-left:30px"><span>namespace System.Windows<br>{<br>    public abstract class <strong>FrameworkElement </strong>: UIElement</span></p>
<p style="padding-left:30px"><span>    {</span></p>
<p style="padding-left:30px"><span>        protected virtual Size <span style="color:#ff00ff">ArrangeOverride</span>(Size finalSize);<br>        protected virtual Size <span style="color:#ff00ff">MeasureOverride</span>(Size availableSize);</span></p>
<p style="padding-left:30px"><span>        ......</span></p>
<p style="padding-left:30px"><span>     }</span></p>
<p style="padding-left:30px"><span>}</span></p>
<p> </p>
<p>    我们看到FrameworkElement定义了一个布局的框架：其中MeasureOverride就是测量过程，ArrangeOverride就是排列过程。我们一般的控件都继承自它，所以就可能建立起了这样一个递归系统。</p>
<p>    Silverlight要求，要定位可视元素，必须将它们放置于Panel或者其他容器对象中。因为Panel定义了确定如何在屏幕上绘制Panel元素的Children集合成员的布局行为。</p>
<p>    自定义Panel很简单，从Panel派生并重写其MeasureOverride和ArrangeOverride方法即可。在两个方法中，都可以获取Children属性（下面列举SDK例子）。</p>
<p><strong><span style="color:#ff0000">测量：</span></strong></p>
<p style="padding-left:30px"><span>protected</span> <span>override</span> Size MeasureOverride(Size availableSize)<br>{<br>    <span>int</span> i =0;<br>    <span>foreach</span> (FrameworkElement child <span>in</span> Children)<br>    {<br>        <span>if</span> (i &lt; 9) child.Measure(<span>new</span> Size(100, 100));<br>        <span>else </span>child.Measure(<span>new</span> Size(0, 0));<br>        i++;<br>    }<br>    <span>return</span> <span>new</span> Size(300,300);<br>}</p>
<p>    在MeasureOverride中，必须调用每个子元素的Measure方法，传递该容器可以分配的空间，确定分配多少空间给子级，然后像上级返回整个容器需要的空间大小。该方法会触发子元素执行内部的MeasureOverride方法，如此递归。</p>
<p>    其实我们想到，测量过程可以是不必要的，排列可以完全按照自己的逻辑进行，它可以不管子元素到底需要多大，但是这样肯定是不行的，除非容器额外了解很多子元素的情况，否则可能排列出的效果很差。那么测量其实是为排列提供一个重要的参考信息：</p>
<p>    DesiredSize，DesiredSize可以理解为：行政长官给在发放通知的时候给下级一个指标，但是下级会有一个实际需求，而DesiredSize相当于指标和需求之间的最小值。具体的说，DesiredSize在Measure发放之后计算的，布局系统基于传递给Measure的availableSize和元素的固有大小去定子元素的DesiredSize，一般讲DesiredSize设置为两种的最小值。这个值可以在排列的时候作为一个参考依据。</p>
<p><span style="color:#ff0000"><strong>排列：</strong></span></p>
<p>    在排列处理过程中，必须确定每个子级的布局槽的位置并设置面板的最终大小。当你计算好位置之后，调用每个元素的Arrange方法就可以了，其参数是一个Rect对象，这个对象可以表示一个元素的空间大小和位置。如下所示：</p>
<p>    image.Arrange(new Rect(10, 10, 150, 150));</p>
<p>    该句将元素image放在x为10，y为10的位置，所占宽度和高度都为150的大小空间。</p>
<p>    定义一个Panel就这里简单，这里是定义Panel规则的地方，你可以设计不同的规则，根据不同的附加属性或依赖属性进行布局。Grid，Canvas，StackPanel等都是经过这样定义的。</p>
<p>    所以，你可以在这里设计很复杂的计算，使元素按不同的方式布局排列。</p>
<p> </p>
<p><span style="color:#00ff00;font-size:18pt"><strong>神秘绝招-让Panel的子元素都具有某种行为</strong></span></p>
<p>    如果只是能摆放元素的位置和限制大小，这样未免没有多大的用途，所有元素都只是一个摆设。</p>
<p>    但是，想到我们前面花了大量篇幅分析得Behavior，试想，我们能不能给容器设置某种行为，让它把这种行为附加到每一个元素，从而让容器还能控制元素的行为呢？哈哈，你太聪明了，没错，这是完全可以的！</p>
<p>    由于讲过Behavior的原理，以及布局的原理，这里我就不分析代码了，附上源文件供参考，代码写得很粗糙，仅作演示用，请勿用在项目中，这里还是讲一下大概思路：</p>
<p>    首先定义了一个派生自Panel的类PhotoWallPanel，在ArrangeOverride中将每个元素随机选择了一个位置，并随机旋转了一个角度；</p>
<p>    然后定义一个Behavior，它作用于PhotoWallPanel，遍历其中的每一个元素，给每个元素添加一个鼠标经过和离开的效果，然后附加到PhotoWallPanel上，值得注意的是：</p>
<p>        &lt;i:Interaction.Behaviors&gt;<br>            &lt;local:ShowPictureBehavior/&gt;<br>        &lt;/i:Interaction.Behaviors&gt;<br>    &lt;/local:PhotoWallPanel&gt;</p>
<p>    在XAML中，附加Behavior的代码应该位于Panel的结尾之前，或者在其他字元素之后，这样才能保证将行为添加到元素上，因为XAML是顺序解析的，在前面的元素首先实例化其对象，如果放在前面，这个时候Panel的子元素还没有初始化。</p>
<p> </p>
<p>总结</p>
<p>    写了这么多，有的人觉得烦了，确实也是这么点东西写那么多废话，但是自定义Panel以及Behavior这些都是重要的基本概念，如果你想深入Silverlight开发，或者很在意UI，这些应该是必须掌握的。希望本篇能对学习Silverlight的朋友有点启示。</p>
<p>    值得重视的是，我们看到Silverlight中附加属性的重要性，基本上容器控件都有定义附加属性，而Behavior也是利用附加属性，很值得好好研究，并在项目中充分利用。</p>
<p>Blend截图：</p>
<p><span style="color:#00ff00"><strong><span style="font-size:18pt"><img src="http://pic002.cnblogs.com/images/2010/140818/2010101215482713.jpg" alt=""></span></strong></span></p>
<p><span style="color:#00ff00"><strong><span style="font-size:18pt"> </span></strong></span></p>
<p><span style="color:#00ff00"><strong><span style="font-size:18pt">实际效果</span></strong></span></p>
<p><span style="color:#00ff00"><strong><span style="font-size:18pt"><img src="http://pic002.cnblogs.com/images/2010/140818/2010101215493992.jpg" alt=""></span></strong></span></p>
<p> </p>
<p><span style="color:#ff6600;font-size:24pt"><strong>补充：</strong></span></p>
<p>    经@super110的提示，这里可能忽略了一点东西，而我又仔细分析了一下，可能一般人很容易在这里理解起来比较纳闷。注意，以下描述基于实验，而非基于原理：</p>
<p>    我们再仔细分析这两个方法：</p>
<p>    protected override Size <span style="color:#ff6600">MeasureOverride</span>(Size availableSize)<br>    {<br>        foreach (var item in this.Children)<br>        {<br>            item.Measure(availableSize);<br>        }<br>         return “<strong><span style="color:#3366ff">此返回值将作为该容器的DesiredSize</span></strong>”;<br>     }</p>
<p>    protected override Size <span style="color:#ff6600">ArrangeOverride</span>(Size finalSize)<br>    {<br>        foreach (var item in this.Children)<br>        {<br>            FrameworkElement my = item as FrameworkElement;<br>            my.Arrange(new Rect(0, 0, my.Width, my.Height));<br>        }</p>
<p>        return “<strong><span style="color:#3366ff">此返回值将作为此容器的ActualWidth和ActualHeight</span></strong>”;<br>    }</p>
<p> </p>
<p>    在Measure每个元素的时候，这个方法执行完就会产生DesiredSize值，其实它也就是MeasureOverride（）方法的返回值。因为这其实是一个递归系统。</p>
<p>    <strong><span style="color:#ff0000">DesiredSize的取值方法</span></strong>：</p>
<p>    1，对于非容器类的控件如Image，布局系统会取其所设置的Width和Height与availablesize相比较的最小值，打个比如，这个有点像进入公司的时候老板问你期望的待遇，当然他心中有一个数字，如果你的数字大于他的数字，以他的为准；如果你的数字小于他的数字，以你的为准，我暂且称其为“老板不吃亏原则”</p>
<p>    2，对于容器控件，由于是我们写代码控制MeasureOverride方法的返回值，所以这个返回值就是该容器的DesiredSize值。换句话说，如果这个容器处于另外一个容器当中，当父容器调用子容器的Measure方法之后，这个子容器返回的值就是它的DesiredSize值。</p>
<p> </p>
<p>    <strong><span style="color:#ff0000">ActualWidth和ActualHeight的计算</span></strong>：</p>
<p>    1，对于非容器控件，往往是由容器给定的值决定，即容器在调用的Arrange方法的时候，给根据自身规则情况来分配值，这个值就是非容器控件的ActualWidth和ActualHeight</p>
<p>    2，对于容器控件，其值由ArrangeOverride方法的返回值来确定，原理同DesiredSize一样</p>
<p> </p>
<p>    DesiredSize，ActualWidth和ActualHeight这几个值都是不能在程序中设置的，我们看到它要么由布局系统计算，要么由容器来计算，我们在程序中一般只读取这几个值。</p>
<p> </p>
<p>源代码下载：<a href="http://files.cnblogs.com/hielvis/PhotoWall.rar">http://files.cnblogs.com/hielvis/PhotoWall.rar</a></p>
<p> </p>
<p>    打一个广告，笔者现在辞职全职创业，手头有一个计划是和Silverlight以及Windows Azure有关的，我的QQ群（6183299）会大力分享这些方面的技术，包括ASP.NET，Silverlight，Windows Azure，WCF，Expression Blend，WCF RIA Service等等新技术，因为这些都是我们会用到的技术，希望在交流学习中能找到志同道合者一起创业。欢迎加入...</p><img src="http://www.cnblogs.com/hielvis/aggbug/1847025.html?type=1" width="1" height="1" alt=""><p>评论: 22　<a href="http://www.cnblogs.com/hielvis/archive/2010/10/12/1847025.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/hielvis/archive/2010/10/12/1847025.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/77126/">国内互联网的两点思考</a><span style="color:gray">(2010-10-13 10:52)</span><br>· <a href="http://news.cnblogs.com/n/77125/">最新全美最佳职业排行榜 软件架构师居首位</a><span style="color:gray">(2010-10-13 10:51)</span><br>· <a href="http://news.cnblogs.com/n/77124/">用JustSpotted看看你心目中的偶像和名人都在哪里溜达</a><span style="color:gray">(2010-10-13 10:27)</span><br>· <a href="http://news.cnblogs.com/n/77123/">深度解析 Windows Phone 7与苹果iPhone 4谁主沉浮?</a><span style="color:gray">(2010-10-13 10:22)</span><br>· <a href="http://news.cnblogs.com/n/77122/">苹果发布iOS4.2 Beta3及最新版iTunes</a><span style="color:gray">(2010-10-13 10:18)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/cmt/archive/2010/10/12/1848613.html">程序员节，10月24日！</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>