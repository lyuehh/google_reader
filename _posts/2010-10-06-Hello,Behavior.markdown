---
layout: post
title:  "Hello,Behavior"
date:   2010-10-06 15:26:00
author: hievis
categories: program
---

## Hello,Behavior
### by hievis
### at 2010-10-06 15:26:00
### original <http://www.cnblogs.com/hielvis/archive/2010/10/06/1806813.html>

<p><a href="http://www.cnblogs.com/hielvis/"><img src="http://pic.cnblogs.com/face/u140818.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/hielvis/">hievis</a> 发表于 2010-10-06 15:26 <a href="http://www.cnblogs.com/hielvis/archive/2010/10/06/1806813.html">原文链接</a> 阅读: 717 评论: 7</p><p><span style="color:#ff0000"><strong>引言         </strong></span></p>
<p><span style="color:#000000">     在看PDC-09大会的视频时，其中一篇讲利用Blend来扩展Silverlight元素的行 为，当时感觉很酷：在Blend中，将MouseDragElementBehavior拖到任意一个元素上，这个元 素就可以被随意拖动。</span></p>
<p><span style="color:#000000">     因为之前在Silverlight SDK中好像没有看到相关的介绍，事实如此， Microsoft.Expression.Interactions和System.Windows.Interactivity程序集在Blend工具中才有。但是从来没有去看过Blend的帮助文档，因为我以为那不过是告诉你怎样使用工具，事实上 我错了，Microsoft Expression Blend 软件开发工具包(SDK) 没有包含怎样使用Blend的信息， 而只有介绍以上提到的两个程序集。</span></p>
<p><span style="color:#000000">     无知的人第一件会做的事就是上Google，我也是这样的人。所以赶紧Google一下，原来 Behavior是一种Silverlight元素行为的重用方式，并且每一篇介绍Behavior的作者，无不流露出对Behavior的赞赏，甚至对有些人来讲，那是Silverlight最令人激动的特性，因为那使他们可能很方便并且简单的为自己的界面添加只有少数具有美术创意的人才能做出的元素效果。         </span></p>
<p><span style="color:#000000">     于是我决定整理一下Behavior相关的知识。</span></p>
<p><span style="color:#000000">     然而Blend SDK一看却感到很吃惊：这不是微软史上最简单的SDK么！如果排除类库介绍 这个SDK只有3个页面，而里面每个例子的代码加起来还不到50行。更惊讶的是微软认为这么简单的东西我却怎么看也不会做，因为我有一种先天缺陷：在不明白为什么的情况下再简单的事情我都不会做</span></p>
<p>     Behavior真这么简单么？事实上不能这么说，从使用的角度，实现一个简单的Behavior不需要多少代码，需要做的事情也不多，然而在这背后的思路呢。比如ASP.NET，一般人看几个小时说就能Hello,World，但是真正做起来却会遇到很多问题。尤其现在开发工具越来越强大，许多东西不需要我过多理解就能实现，但是我相信，只有真正理解一些东西，我们的技术才会真正提高。</p>
<p>     本文从自己的学习角度更深入点的分析这种特性。网上介绍的一般只是教怎么做，很多差不多就是SDK里面的简单的例子。Behavior特性的背后其实是有比较好的思想的。这些思想有助于我们更好的理解这种特性。<br>     本文不打算写太多例子，因为例子实在很简单，可以参考Blend SDK。</p>
<p><span style="color:#000000"> </span> </p>
<p><span style="color:#00ff00;font-size:18pt"><strong>Behavior 的设计模式是一种行为模式</strong></span></p>
<p><span style="color:#000000"><span style="color:#ff0000"><strong>我们应该怎样认识设计模式？</strong></span>         </span></p>
<p><span style="color:#000000">     设计模式到底是一种什么样的东西，每个开发人员有自己的理解。有的人认为那是具有很高技术经验的人才应该掌握的东西；有的人也认为我们一般的开发中不会用到什么设计模式……. 这些观点好像想说明这样一件事情：设计模式是一种可有可无的东西，你有那个兴致就可以去看 下，看了记不住也没有关系。</span></p>
<p><span style="color:#000000">     当然每个人有自己的个人经验，也许不懂设计模式不会影响你做某些事情，但是明白一种设计模式却能帮助你更好的做某件事情。其实在我们的开发中，很多技术都从设计模式中抽象了出来，使我们不用去实现这些高度抽象的技术思维。从语言级别，比如C#中的迭代器模式，有一次我面试没有答上来的委托对应的Observer模式；到框架级别，比如MVC，MVP以及MVVM模 式。Ruby On Rails从2004年出现到现在，一直受到不少开发者的追捧，其实ROR的开发者只不过是用Ruby来实现了MVC的模式，这个框架使其他开发者可以直接利用MVC的模式进行开发，而不必关心怎样自己去实现这个模式。但是与对照教程做练习相比，理解MVC的思路绝对有助于真正的掌握Ruby On Rails技术。到我们的WPF也是一样，XAML不仅仅是一个可以编写界面的方式，它包含的是一种MVP的设计模式，当我们明白这种设计模式的思路时，我相信我们会觉得 XAML就是那么一回事，不然，跟着技术细节跑，你总会有许多的为什么并且不知道为什么。比如WPF有事件模式，为什么又要来个命令模式（Command），其实命令模式也是23种经典模式之一，在WPF中得到支持，看看命令模式再来看WPF中的Command，相信会有不同的感觉。<br>     所以，在我们的学习中，可以将对技术的理解和设计模式一起思考，那样将有助于我们更深刻的理解技术。我们本可以用更底层的技术来实现很多东西，正是因为一些高级语言和开发工具将一些复杂的设计模式融入到各种设计框架和语言中，才使得我们的开发更加简单有效。既然 是基于这些抽象的模式之上，那么理解这些不同的模式将有助于我们真正掌握技术的内涵。</span></p>
<p><span style="color:#000000"> </span></p>
<p><span style="color:#000000"><strong><span style="color:#ff0000">重用行为 </span></strong>       </span></p>
<p><span style="color:#000000">     </span><span style="color:#000000">语言发展到今天，很重要的一个进步就是重用。无论是从语言级别的类，还是COM里的dll，这些技术为技术的发展做出了很大的贡献。     </span></p>
<p><span style="color:#000000">     Behavior也是可重用的一种特性。Expression的网站上有许许多多的Behavior可供下载。Blend本身也提供了几个，如下图：</span></p>
<p><span style="color:#000000"><span style="color:#ff0000"><strong>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615241577.jpg" alt=""></strong></span></span> </p>
<p><span style="color:#000000">     我们只要将一个Behavior拖到一个元素上，就能使这个元素具有某种行为，而实现这种行为的程序集和你的项目是独立的，你创建的Behavior也可以被其他人使用。我们后面将会讨论这是怎样 实现的。<br>     在行为设计模式中。往往有一下几个参与者： </span></p>
<p><span style="color:#000000">        •Element ：要被附加行为的对象元素（这里也可以是一种逻辑功能的抽象，比如排序）</span></p>
<p><span style="color:#000000">        •IBehavior：行为接口 </span></p>
<p><span style="color:#000000">        •Behavior：不同的行为实现<br>     通常，Element所在的程序集和Behavior所在的程序集是不同的，Behavior有多个实现版本，可以将不同的版本附加到Element，就会使Element具有不同的行为。比如这里Element是 一种排序的功能，那么你可以定义一个Behavior是归并排序，而另一个Behavior是快速排序；所 不同的是行为模式中通常有一种选择器来决定选取那个Behavior，而WPF采用一种附加的技术将 Behavior附加到一个Element，并且很独特的是可以附加多个Behavior，后面我们将看到， Behavior中通常用于给元素附加事件，而事件的Add方法类似于多播委托，即使都添加相同的事 件仍然不会影响其他Behavior，事件和委托的一个重要区别就是事件没有赋值方法（另一个是事 件确保只有事件所在的类才能触发一个事件通知），这避免不小心取消其他订阅。所以在 Silverlight中，一个元素可以添加多个Behavior。</span></p>
<p><span style="color:#000000">     不管怎样，我们看到的是这样一种思路：你可以为某个元素添加某种独特的行为，而这种行 为不是直接通过为元素添加事件来实现，因为那样在其他地方将达不到重用而不得不拷代码或者 重写。在汉语中很多相同字开头的词往往是近义词，但我认为重写和重用是反义词。而是通过一种附加的技术，将其他程序集的Behavior附加到一个元素，这个意义非常重大，使得我们可无限 扩展并且重用行为。这样你可以享受别人的成果。那么Silverlight怎样实现这种附加呢？下一节将会讨论。<br></span></p>
<p><span style="color:#00ff00"><strong><span style="font-size:18pt"> 怎样将一种行为附加到一个元素呢？</span></strong></span></p>
<p><strong><span style="color:#ff00ff">附加，非常可怕的一种技术</span></strong>       </p>
<p>      当你在自定义一个Behavior的时候，你将决定千千万万以后会引用你的Behavior的元素的行为，你可以让他放大，你可以让缩小，旋转，任何你想得出来的创意。他人的添加你的程序集 并将你自定义的附加到一个元素时，这个元素就具有了你定义的行为，想想要是世界上有一种附加技术将一种杀人的行为附加到你身上真是非常可怕。<br><strong><span style="color:#ff00ff">背后的英雄 ： IAttachedObject </span></strong></p>
<p>     Interface这种技术隐含的意义真是无法估量。.Net Framework中就有大量的接口，以下是 IAttachedObject 定义：        </p>
<p>     An interface for an object that can be attached to another object.    </p>
<p>     public interfaceIAttachedObject   </p>
<p>    {</p>
<p>         DependencyObject AssociatedObject { get; }     </p>
<p>         void Attach(DependencyObject dependencyObject);   </p>
<p>         void Detach();</p>
<p>    }<br>     实现这个接口的类的实例可以被附加到另一个对象，如：TriggerBase，TriggerAction， Behavior等。在程序中我们可以调用void Attach(DependencyObject dependencyObject)方 法将继承该接口的对象附加到另一对象，这里是dependencyObject对象。         </p>
<p>     通常我们在自定义行为的时候不是直接继承自IAttachedObject接口，而是继承自从 IAttachedObject继承的一些类，这些类都有可重载的OnAttached(),OnDetached()方法，比如 Behavior类，TriggerBase类，TriggerAction类等。通常在Attach()方法中会触发 OnAttached()方法，在Detach（）方法中会触发OnDetached（）方法。         </p>
<p>     所以Blend SDK中自定义Action，Behavior和Trigger的例子都非常简单，你只需要重写 Ontached（）方法和OnDetached（）方法即可。你可以为AssociatedObject 添加各种事件从 而改变其行为。       </p>
<p>    <span style="color:#ff9900"> <strong>以上分析的就是Silverlight中Behavior的机制。所以，本质上，Behavior仍然是利用事件机制来实现的。但是我们通过引入一种不同的设计模式而使得相同的功能更具有扩展性。这进一步说明设计模式的重要性。</strong></span> 后面我们进一步来分析实现方法。</p>
<p>
<p><strong><span style="font-size:11.04pt"><span style="font-family:寰蒋闆呴粦,sans-serif;color:#000000"><span style="font-family:寰蒋闆呴粦,sans-serif;color:#000000"><span style="font-family:寰蒋闆呴粦,sans-serif;color:#000000"><span style="font-family:寰蒋闆呴粦,sans-serif;color:#000000"></span></span></span></span></span></strong></p>
</p>
<p><strong><span style="color:#ff0000">回顾附加属性：</span></strong>         </p>
<p>     这里有必要回顾以下附加属性的概念，这将有助于理解Behavior的附加方式。附加属性其 实是一种普通的依赖项属性。并且它由WPF属性系统管理，不同的是附加属性被应用到一个非定 义到该属性的类，附加属性通过DependencyProperty.RegisterAttached（）方法来定义。并 \且该属性不需要.NET属性包装器，因为附加属性可以被应用与任何附加的对象。看一下Grid的定义：</p>
<p>     public class Grid: Panel   </p>
<p>    {</p>
<p>        public static readonly DependencyProperty <span style="color:#00ffff">ColumnProperty</span>;      </p>
<p>        public static readonly DependencyProperty <span style="color:#00ffff">ColumnSpanProperty</span>;</p>
<p>        public static readonly DependencyProperty <span style="color:#00ffff">RowProperty</span>;</p>
<p>        public static readonly DependencyProperty <span style="color:#00ffff">RowSpanProperty</span>;</p>
<p>        public static readonly DependencyProperty <span style="color:#00ffff">ShowGridLinesProperty</span>;</p>
<p>        public Grid();</p>
<p>        public ColumnDefinitionCollection ColumnDefinitions { get; }</p>
<p>        public RowDefinitionCollection RowDefinitions { get; }</p>
<p>        public bool ShowGridLines { get; set; }</p>
<p>        protected override sealed Size ArrangeOverride(Size arrangeSize);</p>
<p>        public static int GetColumn(FrameworkElement element);</p>
<p>        public static int GetColumnSpan(FrameworkElement element);</p>
<p>        public static intGetRow(FrameworkElement element); </p>
<p>        public static int GetRowSpan(FrameworkElement element);</p>
<p>        protected override sealed Size MeasureOverride(Size constraint);</p>
<p>        public static void SetColumn(FrameworkElement element, int value);</p>
<p>        public static void SetColumnSpan(FrameworkElement element, int value);</p>
<p>        public static void SetRow(FrameworkElement element, int value);</p>
<p>        public static void SetRowSpan(FrameworkElement element, int value);</p>
<p>    }<br>     可以看到上面标记处的Grid的几个附加属性，它们没有.NET属性包装器，而通过 GetPropertyName（）和SetPropertyName（）静态方法来取值和赋值。        </p>
<p>     在被附加属性的元素内部看不到属性的值，所以也根本不知道它的存在---但是在自己定义 的代码中可以根据相应的值进行操作。比如设置了Grid.Row属性后，Grid在计算和排列内部元素 时知道每个元素的这个属性的值，可以根据此值安排此元素在Grid中的位置，Canvac的 Top,Left，Bottom,Right附加属性也是如此，只不过Canvas的附加属性更精确，所以内部计算 更少，因此Canvas的执行效率通常更高。         </p>
<p>     你甚至可以在不包含Grid的地方使用Grid.Row等附加属性，但是这没有丝毫意义，除非你 想在程序中使用这个值。所以你也可以自定义这种附加属性，比如你想在每个元素上保存一个状 态值，你在程序中需要用到这个状态值。我们接下来将看到Silverlight正是通过这种附加属性的 特性实现了行为的附加</p>
<p> </p>
<p><strong><span style="color:#00ffff">Silverlight 通过Interaction的附加属性来实现这种附加</span></strong>          </p>
<p>     实现IAttachedObject的TriggerBase，TriggerAction以及Behavior通过Interaction的附 加属性Triggers和Behaviors来实现这种附加。 </p>
<p>     namespace System.Windows.Interactivity </p>
<p>    {</p>
<p>        public static class <strong><span style="color:#ff0000">Interaction</span></strong></p>
<p>       {</p>
<p>             public static readonly DependencyProperty BehaviorsProperty;</p>
<p>             public static readonly DependencyProperty TriggersProperty;<br>             public static BehaviorCollection GetBehaviors(DependencyObject obj);</p>
<p>             public static TriggerCollection GetTriggers(DependencyObject <span style="color:#ff0000"><strong>obj</strong></span>);</p>
<p>       }</p>
<p>    }</p>
<p>     通过Reflectot工具我们可以看到BehaviorsProperty和TriggersProperty的声明：   </p>
<p>     public static readonly DependencyProperty BehaviorsProperty = </p>
<p>              DependencyProperty.RegisterAttached(&quot;Behaviors&quot;, </p>
<p>                                                                      typeof(BehaviorCollection), </p>
<p>                                                                      typeof(Interaction), </p>
<p>                                                                      new PropertyMetadata( new PropertyChangedCallback(Interaction.OnBehaviorsChanged) )) ;</p>
<p>     TriggersProperty的声明方式类似，不再贴代码。我们看到其数据类型为BehaviorCollection，并且注册为附加属性，在属性发生改变时会触发 Interaction.OnBehaviorsChanged方法： </p>
<p>     private static void OnBehaviorsChanged(DependencyObjectobj, DependencyPropertyChangedEventArgs args) </p>
<p>    {</p>
<p>         BehaviorCollection oldValue = (BehaviorCollection) args.OldValue;</p>
<p>         BehaviorCollection newValue = (BehaviorCollection) args.NewValue;</p>
<p>         if (oldValue != newValue)</p>
<p>        { </p>
<p>             if ((oldValue != null) &amp;&amp; (oldValue.AssociatedObject != null)) </p>
<p>            {</p>
<p>                 oldValue.Detach(); </p>
<p>             }</p>
<p>            if ((newValue != null) &amp;&amp; (obj != null))  </p>
<p>            {</p>
<p>                 if (newValue.AssociatedObject != null)</p>
<p>                {</p>
<p>                      throw new InvalidOperationException(ExceptionStringTable.CannotHostBehaviorCollectionMultiple TimesExceptionMessage);</p>
<p>                } </p>
<p>               <strong><span style="color:#ff0000"> newValue.Attach(obj);</span></strong></p>
<p>             }</p>
<p>        } </p>
<p>     }</p>
<p>     分析此方法，此方法最主要的功劳就是调用BehaviorCollection.Attach()方法。而 BehaviorCollection集合会调用里面每一个子项的Attach（）方法。前面我们讲过在调用Behavior类的Attach（）方法的时候，该方法会调用OnAttached（）方法，所以在OnAttached方法里面，我们就可以为元素添加各种事件代码。而因为事件的Add操作使得其中 一个Behavior添加的相同的事件处理程序不会覆盖另一个Behavior添加的事件处理程序，从而我 们可以为一个元素添加很多的Behavior。         </p>
<p>     这里值得注意的是，上面标出的红色背景的参数obj，在依赖项属性中 PropertyChangedCallback函数中的第一个参数是定义这个属性的类本身的实例对象。而对于以 来属性，这个参数是指被附加了附加属性的那个对象。这里不明白的话就看不懂上面的代码。</p>
<p> </p>
<p><strong><span style="color:#3366ff">Behaviors VS Triggers </span></strong>       </p>
<p>     下面来看下所谓简单的实例代码：</p>
<p>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615500884.jpg" alt=""></p>
<p>     看得出这都是Blend提供的程序集才有的命名空间。在实例中我们为Rectangle和Canvas 元素添加了Interaction的两个附加属性，这两个附加属性是集合类型的，里面添加了一些子项， 比如MouseDragElementBehavior，EventTrigger。          </p>
<p>     Trigger和Behavior是类似的，而且用法也如此，Triggers的子项也都是继承自 IAttachedObject接口的类的实例，所不同的是Triggers子项包含Actions属性，这个属性是一个 Action集合，每个Action也都是继承自IAttachedObject接口。都有Attach（）方法和 DeTach（）方法，并且相应的会触发OnAttached（）和OnDetached（）方法。而Action继承 自Action的子类还包含InVoke（）方法。         </p>
<p>     所以，对于Interaction.Triggers中的每个子项，会制定特定的事件，当该事件触发会会自动调用Actions属性中每个子项的InVoke（）方法；而对于Interaction.Behaviors中的每个子 项，通常具体对什么事件做处理封装在每个Behavior中，这种方式更灵活。         </p>
<p>     对照下面的代码相信很容易理解两种的区别：<br>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615542376.jpg" alt=""></p>
<p>     上面的类ListBoxItemSendToTop是Blend示例中的ColorSwatchSL中定义的一个Action。 该行为将ListBoxItem中的每个元素添加一种行为：当鼠标经过每个Item时将该元素放大，并且 浮到其他元素的上面。可以参考Blend的实例效果。我们可以参考下面的基础层次结构，事实上 TriggerAction还继承自IAttachedObject接口。</p>
<p>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615555280.jpg" alt=""></p>
<p>     这几行代码将元素添加附加属性Interaction.Triggers，并添加一个子项，指明当Loaded事 件发生时触发ListBoxItemSendToTop的Invoke（）方法。不过奇怪的是Blend的实例并没有按 正确的思路使用，下面是截图：</p>
<p>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615564788.jpg" alt=""></p>
<p>     从上面的代码可以看出，对元素行为的附加发生在OnAttached（）方法中，而此方法不是 被EventTrigger制定的Loaded事件触发，而是发生在程序初始化时TriggerCollection.Attach()触发 EventTrigger.Attach(),一次触发ListBoxItemSendToTop.Attach()而附加行为的。当这种触发Loaded事 件的时候什么都没有做。虽然这些写程序没有问题，但是和Triggers的思路却是不一致的。以上例 子我们可以看出Triggers的执行过程。       </p>
<p>     为了比较Triggers和Behaviors，我将上面的Triggers实现改为Behaviors实现，代码如下：</p>
<p>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615575760.jpg" alt=""></p>
<p>     我将SendToTopBehavior声明为一种Behavior，继承自Behavior&lt;T&gt;,Behavior&lt;T&gt;的基 础层次结构和Action类似，同样它也继承自IAttachedObject接口：</p>
<p>     <img src="http://pic002.cnblogs.com/images/2010/140818/2010100615584928.jpg" alt=""></p>
<p>     通过改写为Behavior，这才是真正通过OnAttached（）方法来添加行为，这和前面分析的过程一致。      </p>
<p>     通过以上比较应该能明白Triggers和Behaviors的区别。Behaviors提供了更灵活的方式，行为响应什么方式将完全有行为自己决定。而Action只提供行为，行为受什么事件触发则由元素定义中 自己决定。<br><span style="color:#ff0000"><strong> </strong></span></p>
<p><span style="color:#ff0000"><strong>结论：</strong></span>        </p>
<p>     Behavior简单吗？现在看来它的思路不算复杂。然而从上面的分析过程来看也不算简单。最主 要的问题，里面涉及到的东西比较多，比如要理解附加属性。还得对XAML有一定理解，对于 XAML ，它里面的每一个标记都是对象或者对象的属性。那么既然是对象，我们就可以利用对象初 始化的时候去干一些事情，比如为元素添加事件。这样我们就能理解上面发生的一切，并且会明 白为什么我们感觉XAML又在做一些我们在后台代码才能做的事情，认为XAML就是一堆死标记， 这就不能真正理解 XAML 背后的设计模式和意义。       </p>
<p>     另外，我也从设计模式的角度思考这个问题。这样更有助于理解。        </p>
<p>     Blend SDK上两分钟就能看完的例子，我花两天才勉强理解。我在想微软是不是太高估开发者？还是我太笨了？</p>
<p> </p>
<p><span style="font-size:18pt"><strong><span style="color:#ff0000">System.Windows.EventTrigger</span> VS</strong></span></p>
<p><strong><span style="color:#00ff00;font-size:18pt">System.Windows.Interactivity.EventTrigger</span></strong><br>     正当准备收笔的时候，才想起Silverlight本身也有触发器的啊。那里的用法好像可不一样。        </p>
<p>     这可吃惊不小，因为很多天没看Silverlight了，加上一直没做过项目，概念理解也不是很深。于是赶紧Go To Definition。才发现两种是有区别的，这里做一下比较：<br><strong><span style="color:#00ffff">1 ，命名空间区别：</span></strong>          </p>
<p>     Silverlight本身的Triggers位于System.Windows命名空间,而Blend提供的Triggers位于 System.Windows.Interactivity命名空间，并且随Blend一起附带，与Silverlight SDK是独立的。<br><strong><span style="color:#800080">2 ，定义区别：</span></strong> </p>
<p>     namespace System.Windows.Interactivity </p>
<p>    {</p>
<p>       public static class Interaction    </p>
<p>      {</p>
<p>          public static readonly DependencyProperty BehaviorsProperty;</p>
<p>          public static readonly DependencyProperty <strong><span style="color:#ff0000">TriggersProperty</span></strong>;</p>
<p>          public static BehaviorCollection GetBehaviors(DependencyObject obj); </p>
<p>          public static TriggerCollection GetTriggers(DependencyObject obj);</p>
<p>      }</p>
<p>    }<br>     namespace System.Windows </p>
<p>    {</p>
<p>       public abstract class FrameworkElement : UIElement</p>
<p>       {</p>
<p>            …...        </p>
<p>            public TriggerCollectionTriggers{ get; }   </p>
<p>            …..<br>       }</p>
<p>     }</p>
<p>     从定义上看，两种是有区别的，这决定了使用方式会有区别。       </p>
<p>      Silverlight本身的Triggers是一个普通的.NET属性，而Blend提供的Triggers是一个附加属性。而我 们转到TriggerCollection的定义，前者是一个普通的.NET集合类，继承自 DependencyObject,IList&lt;T&gt;, ICollection&lt;T&gt;, IEnumerable&lt;T&gt;, IList, ICollection, IEnumerable ，而后者 继承自 DependencyObjectCollection&lt;T&gt; ， IAttachedObject ，具有 Attach(),DeTatch(),OnAttached()，OnDeTached()方法。<br><strong><span style="color:#ff0000">3 ，使用方式区别 </span></strong>        </p>
<p>     前者的子项是StoryBoard，通过响应事件来激发动画，而后者正如前面分析，子项是继承自 IAttachedObject接口的Action对象，通过响应事件来激发Action的Invoke()方法。<br>     以上简单的分析可以看出两者虽然名称相同，但要功能是不一样的，Silverlight的Trigger是用 来触发动画，动画是在一段时间内持续的改变元素属性。而Blend的Trigger更多用来响应鼠标和键 盘这种用户行为，当然你也可以在OnAttached里面来启动一个动画，这也是可以的。        </p>
<p>     总之，Blend提供的Trigger更灵活，功能更丰富，可以这么说，你能做出一切你想得到的用户交互行为。正如WPF的外观可以无限定制。虽然SDK是那么的简单！</p><img src="http://www.cnblogs.com/hielvis/aggbug/1806813.html?type=1" width="1" height="1" alt=""><p>评论: 7　<a href="http://www.cnblogs.com/hielvis/archive/2010/10/06/1806813.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/hielvis/archive/2010/10/06/1806813.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76506/">NuPack, ASP.NET MVC 3 Beta, WebMatrix Beta 2发布资源</a><span style="color:gray">(2010-10-07 16:04)</span><br>· <a href="http://news.cnblogs.com/n/76505/">Google新一代实时搜索系统的核心机制</a><span style="color:gray">(2010-10-07 16:01)</span><br>· <a href="http://news.cnblogs.com/n/76504/">W3C认为HTML5目前暂无法大规模应用</a><span style="color:gray">(2010-10-07 15:52)</span><br>· <a href="http://news.cnblogs.com/n/76503/">巧克力过后，这次是乐高Windows Phone 7手机了</a><span style="color:gray">(2010-10-07 15:51)</span><br>· <a href="http://news.cnblogs.com/n/76502/">报告称过去一年全球互联网流量增长62%</a><span style="color:gray">(2010-10-07 15:00)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/76302/">2010年10月编程语言排行榜：Java的混乱之治</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>