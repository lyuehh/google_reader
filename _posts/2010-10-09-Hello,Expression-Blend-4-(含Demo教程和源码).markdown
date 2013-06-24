---
layout: post
title:  "Hello,Expression Blend 4 (含Demo教程和源码)"
date:   2010-10-09 01:23:00
author: hievis
categories: program
---

## Hello,Expression Blend 4 (含Demo教程和源码)
### by hievis
### at 2010-10-09 01:23:00
### original <http://www.cnblogs.com/hielvis/archive/2010/10/09/1846046.html>

<p><a href="http://www.cnblogs.com/hielvis/"><img src="http://pic.cnblogs.com/face/u140818.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/hielvis/">hievis</a> 发表于 2010-10-09 01:23 <a href="http://www.cnblogs.com/hielvis/archive/2010/10/09/1846046.html">原文链接</a> 阅读: 1354 评论: 6</p><p><span style="color:#ff00ff"><strong><span style="font-size:14pt">前言</span></strong></span></p>
<p>   这段时间的开发不会用到Blend，到年底才会大量用到，本来打算到时候在写Blend相关的笔记，不过看到一些朋友还比较感兴趣，所以这里提前整理了一下。</p>
<p>   首先，我希望你记住下面几点：</p>
<ol>
<li>Blend并不完全是为Designer设计的，玩得最好的一定是Programmer</li>
<li>必须了解Behavior，Blend很多特性基于Behavior</li>
<strong><span style="color:#ff6600">﻿</span></strong></ol>
<p><span style="color:#0000ff"><strong><span style="font-size:14pt">Expression Blend的技术发展历史</span></strong></span></p>
<p><span style="color:#ff0000;font-size:14pt"><strong>2007</strong></span></p>
<p>    Blend的第一个版本就可以进行基本的动画设计。它通过提供一个Object and Timeine面板来进行动画的设计，这和Flash有些类似。在程序中，一段动画就是一个Timeline对象。</p>
<p>    <span style="color:#ff6600"><em>那么TimeLine对象是怎样实现动画效果的呢？</em></span></p>
<p>    我们首先看一下抽象类TimeLine的定义：</p>
<p>namespace System.Windows.Media.Animation<br>{<br>    public <span style="color:#3366ff">abstract </span>class <strong><span style="color:#ff0000">Timeline </span></strong>: DependencyObject<br>    {<br>        public bool AutoReverse { get; set; }<br>        public TimeSpan? BeginTime { get; set; }<br>        public Duration Duration { get; set; }<br>        public FillBehavior FillBehavior { get; set; }<br>        public RepeatBehavior RepeatBehavior { get; set; }<br>        public double SpeedRatio { get; set; }</p>
<p>        public event EventHandler Completed;<br>    }<br>}</p>
<p>    我们查看MS文档的描述：<span style="color:#ff6600"><em>时间线表示时间段。它提供的属性可以让您指定该时间段的长度（Duration），开始时间（BeginTime），重复次数（RepeatBehavior），该时间段内时间进度的快慢（SpeedRatio）等等。从TimeLine派生的类可以提供动画功能（例如DoubleAnimation，ColorAnimation等）。</em></span></p>
<p><strong>    一些时间线类： </strong></p>
<div><a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System<span><span>.</span><span>.</span><span>::</span><span>.</span><span>.</span></span>Object</a><br>  <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows<span><span>.</span><span>.</span><span>::</span><span>.</span><span>.</span></span>DependencyObject</a><br>    <span>System.Windows.Media.Animation<span><span>.</span></span>Timeline</span><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>ColorAnimation</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>ColorAnimationUsingKeyFrames</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>DoubleAnimation</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>DoubleAnimationUsingKeyFrames</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>ObjectAnimationUsingKeyFrames</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>PointAnimation</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span>PointAnimationUsingKeyFrames</a><br>      <a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">System.Windows.Media.Animation<span><span>.</span></span><strong><span style="color:#ff0000">Storyboard</span></strong></a></div>
<div><strong> </strong></div>
<div>    现在我们仿佛有了一点能实现动画的基础了，然而光靠TimeLine还是不行的，看具体定义我们知道DoubleAnimation定义了一个Double属性的开始值和结束值，而ColorAnimation定义了一个Color属性的其实颜色和最终颜色，可它们怎样和一个控件关联，以及怎样播放呢？TimeLine并没有定义这些。</div>
<div>    这就要了解神奇的StoryBoard：</div>
<div>namespace System.Windows.Media.Animation<br>{<br>    public sealed class <strong><span style="color:#ff0000">Storyboard </span></strong>: Timeline<br>    {<br>        public static readonly DependencyProperty <span style="color:#ff9900">TargetNameProperty</span>;<br>        public static readonly DependencyProperty <span style="color:#ff9900">TargetPropertyProperty;</span></div>
<p>        public <span style="color:#ff0000">TimelineCollection </span><strong><span style="color:#00ff00">Children </span></strong>{ get; }</p>
<p>        public void <span style="color:#00ff00">Begin</span>();<br>        public void <span style="color:#00ff00">Pause</span>();<br>        public void <span style="color:#00ff00">Resume</span>();</p>
<p>        public void <span style="color:#00ff00">Stop</span>();<br>    }<br>}</p>
<p>    这里看虽然StoryBoard同样继承与TimeLine，但却比其他兄弟类多了一些属性：Begin，Pause，Resume，Stop，还有一个TimeLine的集合属性，以及两个附加属性TargetName和TargetProperty。</p>
<p>    原来，StoryBoard才是真正和UI线程打交道的类，它的原理大概就是这样：一个StoryBoard对象包含一些TimeLine元素，每个TimeLine对象如DoubleAnimation定义动画，并用StoryBoard的两个附加属性来描述作用于哪个元素的哪个属性，当调用StoryBoard对象的Begin()方法时，UI线程会根据TimeLine的描述，找到对应元素的属性，时间线内不断修改其属性值，这样就实现了动画。</p>
<p>    值得注意的是，StoryBoard本身也是继承自TimeLine。有人不明白，StoryBoard主要用于实现动画而不是定义动画，为什么它也要继承自TimeLine呢？其实这是一种很好的设计模式（后面会写一篇架构和设计模式的文章，标题暂定《谁是架构师？》），从某种程度上我们可以把StoryBoard视为一些特定动画的集合，UI线程将这一些动画一起执行，然而我们可以把这个动画集合也视为一个特定的动画，所以StoryBoard也继承自TimeLine，这样你可以把一个StoryBoard对象作为另一个StoryBoard对象的子元素。</p>
<p>    Blend的Objects and TimeLine面板实现的就是上述功能。在面板上点击新建就会在XAML中新增一个StoryBoard对象，选择不同的时间之后没改变一个元素的某个属性，都会增加对应属性类型的TimeLine对象，这就实现了用Blend对动画的编辑。</p>
<p>    然而一个控件通常有许多状态，在不同的状态下会有不同的动画组，在第一版本中Blend几乎是无能为力的，所幸的是微软的Silverlight以及Blend的技术发展非常快！</p>
<p> </p>
<p><strong><span style="color:#ff9900;font-size:14pt">2008</span></strong></p>
<p>    如前面描述，一个控件有非常复杂的状态，如果全靠这样一个一个修改属性值，软件开发将会变得非常痛苦而低效。在Blend SP1中，Blend Team革命性的提出了VSM（Visual State Manager）试图状态管理器，一个控件可以有很多状态，一个状态到另一个状态有很多属性值需要发生改变，这样就需要启动一个StoryBoard过渡不同的状态。而VSM则管理不同的状态，所以他们有以下关系：</p>
<p> <img src="http://pic002.cnblogs.com/images/2010/140818/2010100822171981.jpg" alt=""></p>
<p>    给自己的控件添加状态非常简单，在Blend中有一个State面板：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010100822282386.jpg" alt=""></p>
<p>    状态分为状态组VisualStateGroup以及状态VisualState，一个VisualStateGroup包含多个VisualState，其理解的很好的一个例子是看Button控件：</p>
<p>    <span style="color:#ff9900">CommonStates<span style="color:#000000">(VisualStateGroup)</span></span></p>
<p>        Normal</p>
<p>        MouseOver</p>
<p>        Pressed</p>
<p>        Disabled</p>
<p>    <span style="color:#ff9900">FocusStates<span style="color:#000000">(VisualStateGroup)</span></span></p>
<p>        Unfocused</p>
<p>        Fouces</p>
<p>    状态统一由VisualStateManager管理，VisualManager类有一个附加属性VisualStateManager.VisualStateGroups，我们看Button默认模板的定义：</p>
<p>    &lt;ControlTemplate TargetType=&quot;Button&quot;&gt;<br>      &lt;Grid&gt;<br>      &lt;VisualStateManager.VisualStateGroups&gt;<br>      &lt;VisualStateGroup x:Name=&quot;<span style="color:#ff6600">CommonStates</span>&quot;&gt;</p>
<p>          &lt;VisualStateGroup.<span style="color:#0000ff">Transitions</span>&gt;<br>              &lt;VisualTransition To=&quot;MouseOver&quot;  GeneratedDuration=&quot;0:0:0.5&quot;/&gt;<br>         &lt;/VisualStateGroup.Transitions&gt;<br>         &lt;VisualState x:Name=&quot;<span style="color:#00ff00">Normal</span>&quot;/&gt;<br>         &lt;VisualState x:Name=&quot;<span style="color:#00ff00">MouseOver</span>&quot;&gt;<br>            &lt;Storyboard&gt;<br>               &lt;DoubleAnimation Duration=&quot;0&quot; To=&quot;1&quot; </p>
<p>                                          Storyboard.TargetProperty=&quot;Opacity&quot; </p>
<p>                                          Storyboard.TargetName=&quot;BackgroundAnimation&quot;/&gt;<br>               &lt;ColorAnimation Duration=&quot;0&quot; To=&quot;#F2FFFFFF&quot; </p>
<p>                                        Storyboard.TargetProperty=&quot;(Rectangle.Fill).(GradientBrush.GradientStops)[1].(GradientStop.Color)&quot;          </p>
<p>                                        Storyboard.TargetName=&quot;BackgroundGradient&quot;/&gt;<br>               &lt;ColorAnimation Duration=&quot;0&quot; To=&quot;#CCFFFFFF&quot; </p>
<p>                                        Storyboard.TargetProperty=&quot;(Rectangle.Fill).(GradientBrush.GradientStops)[2].(GradientStop.Color)&quot; </p>
<p>                                        Storyboard.TargetName=&quot;BackgroundGradient&quot;/&gt;<br>               &lt;ColorAnimation Duration=&quot;0&quot; To=&quot;#7FFFFFFF&quot; </p>
<p>                                        Storyboard.TargetProperty=&quot;(Rectangle.Fill).(GradientBrush.GradientStops)[3].(GradientStop.Color)&quot; </p>
<p>                                        Storyboard.TargetName=&quot;BackgroundGradient&quot;/&gt;<br>            &lt;/Storyboard&gt;<br>          &lt;/VisualState&gt;</p>
<p>     .......</p>
<p style="text-align:left">    其中VisualStateGroup还包含一个Transitions属性，Transitions是VisualTransition的集合，VisualTransition表示控件从一个状态过渡到另一个状态时发生的可视行为，其包含一个StoryBoard对象，当控件在VisualStateGroup中定义的各种状态之间过渡时将应用Transitions中的VisualTransition对象。</p>
<p style="text-align:left">    通过上面的分析我们不难分析出<strong><span style="color:#ff0000">VSM的整个逻辑</span></strong>：</p>
<p style="text-align:left">    附加属性VisualStateManager.VisualStateGroups可以被放置于任何控件；可以通过VisualStateGroup来设置控件的状态组，每个状态组之间的过渡可视行为通过设置VisualStateGroup.Transitions来实现，Transitions实际上就是一些StoryBoard的集合，可以在定义各个子控件的属性改变；每个VisualStateGroup包含一些VisualState，VisualState包含StoryBoard属性，当过渡到这个VisualState时会触发这段动画。</p>
<p style="text-align:left">    状态之间的过渡控制我们可以通过VisualStateManager.GotoState()方法来控制。s</p>
<p style="text-align:left">    所以，VSM能让你实现很复杂的动画处理，可以说有了VSM之后，Blend才开始变得强大。</p>
<p style="text-align:left"> </p>
<p style="text-align:left"><span style="color:#ff0000;font-size:14pt">2009</span></p>
<p style="text-align:left">    Blend的强大，得益于两个东西：第一是VSM，第二是Behavior。</p>
<p style="text-align:left">    2009年，Blend进行了4个方面的增强：</p>
<p style="text-align:left">      EasingFunction</p>
<p style="text-align:left">      GoToStateBehavior</p>
<p style="text-align:left">      FluidMoveBehavior</p>
<p style="text-align:left">      FluidLayout</p>
<p style="text-align:left">    EasingFunction（缓动函数）是个很重要的增强。以前的StoryBoard在运行时，只能根据属性的初始值和结束值进行线性改变，这样的动画有时候不够生动，比如我们可能希望在前半段时间内改变快一点，或者在某个时间段回退一下，这些都可以通过EasingFunction来实现。它本质上根据一个二次方程决定属性值的渐变，这使得我们的动画生动得多。</p>
<p style="text-align:left">    每一个TimeLine的实现类几乎都定义了EasingFunction属性，微软也提供了一些基本的EasingFunction，如：</p>
<ul>
<li>
<p><span style="color:#0000ff">BackEase</span>：在某一动画开始沿指示的路径进行动画处理前稍稍收回该动画的移动。</p>
</li>
<li>
<p><span style="color:#0000ff">BounceEase</span>：创建弹回效果。</p>
</li>
<li>
<p><span style="color:#0000ff">CircleEase</span>：创建使用循环函数加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">CubicEase</span>：创建使用公式 <em>f</em>(<em>t</em>) = <em>t</em><sup>3</sup> 加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">ElasticEase</span>：创建表示弹簧在停止前来回振荡的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">ExponentialEase</span>：创建使用指数公式加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">PowerEase</span>：创建使用公式 <em>f</em>(<em>t</em>) = <em>t</em><sup>p</sup>（其中，p 等于 <span><a href="http://mce_host/hielvis/admin/mk:@MSITStoreD%5CBook%5CCHS_Silverlight_4.chm">Power</a></span> 属性）加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">QuadraticEase</span>：创建使用公式 <em>f</em>(<em>t</em>) = <em>t</em><sup>2</sup> 加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">QuarticEase</span>：创建使用公式 <em>f</em>(<em>t</em>) = <em>t</em><sup>4</sup> 加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">QuinticEase</span>：创建使用公式 <em>f</em>(<em>t</em>) = <em>t</em><sup>5</sup> 加速和/或减速的动画。</p>
</li>
<li>
<p><span style="color:#0000ff">SineEase</span>：创建使用正弦公式加速和/或减速的动画。</p>
</li>
</ul>
<p>    我们看一个例子：</p>
<p>     &lt;DoubleAnimation From=&quot;30&quot; To=&quot;200&quot; Duration=&quot;00:00:3&quot; <br>                                Storyboard.TargetName=&quot;myRectangle&quot; <br>                                Storyboard.TargetProperty=&quot;Height&quot;&gt;<br>                &lt;DoubleAnimation.<span style="background-color:#3399ff;color:#ffffff">EasingFunction</span>&gt;<br>                    &lt;BounceEase Bounces=&quot;2&quot; EasingMode=&quot;EaseOut&quot; Bounciness=&quot;2&quot; /&gt;<br>                &lt;/DoubleAnimation.<span style="background-color:#3399ff;color:#ffffff">EasingFunction</span>&gt;<br>     &lt;/DoubleAnimation&gt;<br>    在Blend中设置EasingFunction的图示：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010100900010078.jpg" alt=""></p>
<p> </p>
<p>    <span style="color:#ff0000"><strong>GotoStateBehavior</strong></span>是一个Behavior，它用于控制我们前面VSM描述的状态的转换，在Asset面板的Behaviors中，即可以看到GotoStateBehavior控件，可以将其选择拖动到任何一个控件，即可利用Blend实现状态转换的逻辑。在Properties面板进行设置，设置触发事件，要转换的状态等：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010100900060477.jpg" alt=""></p>
<p>    如我们拖动到Button控件上，会产生如下代码：</p>
<p>    &lt;Button x:Name=&quot;button&quot; Content=&quot;Button&quot;&gt;<br>         &lt;i:Interaction.Triggers&gt;<br>            &lt;i:EventTrigger EventName=&quot;Click&quot;&gt;<br>               &lt;ei:GoToStateAction StateName=&quot;VisualState&quot;/&gt;<br>           &lt;/i:EventTrigger&gt;<br>        &lt;/i:Interaction.Triggers&gt;<br>    &lt;/Button&gt;   </p>
<p>    GoToStateAction本身是继承自IAttachedObject，这和我们前面讲的Behavior的原理是一样，这也是Blend利用Behavior的一个例子。</p>
<p> </p>
<p>    <strong><span style="color:#ff0000">FluidMoveBehavior</span></strong>：</p>
<p>    StoryBoard能够做到属性值改变过程的过渡。然而还有一种场景，那就是Silverlight中控件可以随着界面大小的调整而重新布局，这是通过控件的MeasureOverride和ArrangeOverride方法来实现。一般情况下，到界面重新布局时，控件瞬间被安排到新的位置，然而有时候我们希望看到这个重新排列的过程。</p>
<p>    我们可以自己写逻辑实现这一功能，但是非常复杂。Behavior的用途是什么，就是重用，所以Blend小组开发了这个Behavior。</p>
<p>    FluidMoveBehavior也位于Asset面板中的Behaviors部分，拖动FluidMoveBehavior到一个WPF WarpPanel控件，配置的属性即可：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010100900263478.jpg" alt=""><br>    可以设置AppliesTo属性为Children，这一当Grid的尺寸大小发生改变时，可以看到起内部元素重新排列的过程，你还可以设置缓动函数来设置这段动画的变化效果。如下图：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010100900312885.jpg" alt=""></p>
<p> </p>
<p>    最后要介绍的是FluidLayout，它通常用于在两种状态之间过渡的时候，控件属性的更改没有应用动画效果，而又希望平滑过渡的效果。例如，当控件从一种状态变为另一种状态时，Grid.Row有1变为2，并且Grid.Rowspan由1变为2，这一只要开启FluidLayout就能看到变换过程。你可以在States面板中设置FluidLayout：</p>
<p>    <img src="http://pic002.cnblogs.com/images/2010/140818/2010100901004366.jpg" alt=""></p>
<p>    很简单，只要点击&quot;Turn on FluidLayout&quot;就能开启状态之间的过渡效果，当然同样可以设置缓动函数EasingFunction。</p>
<p> </p>
<p><span style="font-size:14pt"><strong><span style="color:#ff0000">2010</span></strong></span></p>
<p>    .......</p>
<p> </p>
<p><strong><span style="color:#00ff00;font-size:18pt">暂停</span></strong></p>
<p>    2010 Blend 4发布，这自然包含一些新特性，由于时间及篇幅关系，我打算到下一篇续写Blend 4的一些功能。</p>
<p>    Blend 4基本上没有新增知识点，主要都是围绕Behavior提供的一些很酷的功能。</p>
<p>    所以，对于Blend，只要掌握了VSM和Behavior，我想其他的都不是问题了。</p>
<p>    预告后面的内容是，一个Blend Team据说花了4年时间研究的功能<span style="font-family:Calibri,sans-serif;color:#0000ff;font-size:10pt"><strong>FluidMoveTagSetBehavior，</strong><span style="color:#000000">这是一个很有意思的特性。相信你和我一样会非常感兴趣，还有PathListBox，它能实现更酷的效果，当然还有其他东东，到时候再揭晓了。</span></span></p>
<p><span style="font-family:Calibri,sans-serif;color:#0000ff;font-size:10pt"><span style="color:#000000">     </span></span></p>
<p>    Blend并不难，而且我们看到它并不是为Designer设计的，它的许多概念都需要Programer的思维。所以其实我们是很容易去学习和利用的。多摸几次就熟悉了。 </p>
<p>    这里主要是讲理论的，后面可能会再写一篇操作的，看时间了，相信有了理论之后，再看一面操作，就可以了。</p>
<p>    这里打一个广告，笔者现在辞职全职创业，手头有一个计划是和Silverlight以及Windows Azure有关的，我的QQ群（6183299）会大力分享这些方面的技术，包括ASP.NET，Silverlight，Windows Azure，WCF，Expression Blend，WCF RIA Service等等新技术，因为这些都是我们会用到的技术，希望在交流学习中能找到志同道合者一起创业。欢迎加入...</p>
<p> </p>
<p><strong><span style="color:#ff0000;font-size:14pt">源代码下载：</span></strong></p>
<p>     这篇是在Blend 4发布的时候写的一篇教程的基础上改写的</p>
<p>     原教程下载：<a href="http://files.cnblogs.com/hielvis/Behavior2.rar">http://files.cnblogs.com/hielvis/Behavior2.rar</a></p>
<p>     源代码：     <a href="http://files.cnblogs.com/hielvis/SLBlendDemo.rar">http://files.cnblogs.com/hielvis/SLBlendDemo.rar</a></p>
<p>     你需要安装VS2010，Silverlight 4 Toolkit</p><img src="http://www.cnblogs.com/hielvis/aggbug/1846046.html?type=1" width="1" height="1" alt=""><p>评论: 6　<a href="http://www.cnblogs.com/hielvis/archive/2010/10/09/1846046.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/hielvis/archive/2010/10/09/1846046.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76756/">沃尔玛下周对外出售苹果iPad</a><span style="color:gray">(2010-10-09 22:30)</span><br>· <a href="http://news.cnblogs.com/n/76754/">互联计算：英特尔变革10倍速时代</a><span style="color:gray">(2010-10-09 22:06)</span><br>· <a href="http://news.cnblogs.com/n/76753/">联想弄潮：不仅仅满足做PC市场强者</a><span style="color:gray">(2010-10-09 22:01)</span><br>· <a href="http://news.cnblogs.com/n/76752/">英特尔研究院：准直于企业发展战略</a><span style="color:gray">(2010-10-09 21:47)</span><br>· <a href="http://news.cnblogs.com/n/76751/">被指重塑在线旅游市场 谷歌收购ITA遇双重阻力</a><span style="color:gray">(2010-10-09 21:46)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/76477/">远离.NET</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>