---
layout: post
title:  "[Silverlight动画]转向行为 - 到达行为"
date:   2010-06-24 19:18:00
author: 王喆(nasa)
categories: program
---

## [Silverlight动画]转向行为 - 到达行为
### by 王喆(nasa)
### at 2010-06-24 19:18:00
### original <http://www.cnblogs.com/nasa/archive/2010/06/24/sl-steering-behaviors-7-Arrive.html>

<p><a href="http://www.cnblogs.com/nasa/"><img src="http://pic.cnblogs.com/face/u20522.gif" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/nasa/">王喆(nasa)</a> 发表于 2010-06-24 19:18 <a href="http://www.cnblogs.com/nasa/archive/2010/06/24/sl-steering-behaviors-7-Arrive.html">原文链接</a> 阅读: 497 评论: 1</p><p><span style="font-family:Verdana;line-height:normal;font-size:13px">
<div style="margin-top:0px;margin-bottom:0px">到达行为在很多场合都可以被当作是寻找行为。实际上，它们之间的算法和处理方式都一样。唯一不同的是，在到达模式中，一辆机车在到达目标的某一距离时，会变成一种精确模式慢慢地靠近目标点。</div>
<div style="margin-top:0px;margin-bottom:0px">为了了解到达行为的必要性，可以先运行一下SeekTest类，然后移动鼠标到某处让机车过来“抓住”它。会看到机车快速的越过了鼠标，接着它发现过头了，又返回来，还是过头了....于是会一直循环下去。这是因为机车始终保持着最大速度迈向目标，哪怕离目标只有几像素。</div>
<div style="margin-top:0px;margin-bottom:0px">到达行为通过减速接近目标，解决了这个问题：</div>
</span></p>
<div style="margin-top:0px;margin-bottom:0px">
<pre>        public void arrive(Vector2D target) {
            Vector2D desiredVelocity = target.subtract(_postion);
            desiredVelocity.normalize();

            double dist = _postion.dist(target);
            if (dist&gt;_arrivalThreshold)
            {
                desiredVelocity = desiredVelocity.multiply(_maxSpeed);
            }
            else
            {
                desiredVelocity = desiredVelocity.multiply(_maxSpeed * dist / _arrivalThreshold);
            }
            Vector2D force = desiredVelocity.subtract(_velocity);
            _steeringForce = _steeringForce.add(force);
        }
</pre>
</div>
<p>
<div style="margin-top:0px;margin-bottom:0px">程序一开始和寻找行为一样。但是在期望速度乘以最大速率时，做了距离检测。如果距离大于某个值，那一切照旧。程序往后走，接着的事情也和寻找一样。</div>
<div style="margin-top:0px;margin-bottom:0px">关键是，距离小于某个值时所做的事情。本来乘以_maxSpeed现改为乘以_maxSpeed * dist / _arriveThreshold。如果距离仅仅小于某个值一点点，那么dist / _arriveThreshold会非常接近1.0，可能是0.99。因此，期望速度的大小也会非常接近于（略小于）最大速率。如果距离接近0，那么得到 的比率也会非常非常小，期望速度改变也会很小。最终速度会趋向于0（假设只有一个行为作用于该机车）。</div>
<p><img height="350" width="345" src="http://docs.google.com/File?id=df5kjk97_1150fs2n55gp_b"> </p>
</p>
<p>
<div style="margin-top:0px;margin-bottom:0px">当然，转向机车类需要这么一个“某个值”属性，所以我们把它加上去：</div>
<div style="margin-top:0px;margin-bottom:0px">private double _arrivalThreshold = 100;</div>
<div style="margin-top:0px;margin-bottom:0px">public double arriveThreshold {</div>
<div style="margin-top:0px;margin-bottom:0px">    get { return _arrivalThreshold; }</div>
<div style="margin-top:0px;margin-bottom:0px">    set { _arrivalThreshold = value; }</div>
<div style="margin-top:0px;margin-bottom:0px">}</div>
看看这些是如何运用在测试类中的:</p>
<div style="margin-top:0px;margin-bottom:0px">
<pre>&lt;UserControl
	xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
	xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
	xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;
	xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
	xmlns:local=&quot;clr-namespace:Steer&quot; xmlns:ed=&quot;http://schemas.microsoft.com/expression/2010/drawing&quot;
	mc:Ignorable=&quot;d&quot;
	x:Class=&quot;Steer.ArriveTest&quot;
	d:DesignWidth=&quot;640&quot; d:DesignHeight=&quot;480&quot;&gt;

	&lt;Grid x:Name=&quot;LayoutRoot&quot; Background=&quot;White&quot;&gt;
		&lt;local:SteeredVehicle x:Name=&quot;myStar&quot; HorizontalAlignment=&quot;Left&quot; Height=&quot;40&quot; VerticalAlignment=&quot;Top&quot; Width=&quot;40&quot;&gt;
			&lt;ed:RegularPolygon Fill=&quot;Blue&quot; Height=&quot;40&quot; InnerRadius=&quot;1&quot; PointCount=&quot;3&quot; Stretch=&quot;Fill&quot; Stroke=&quot;Black&quot; UseLayoutRounding=&quot;False&quot; Width=&quot;40&quot; RenderTransformOrigin=&quot;0.5,0.5&quot; StrokeThickness=&quot;0&quot;&gt;
				&lt;ed:RegularPolygon.RenderTransform&gt;
					&lt;CompositeTransform Rotation=&quot;90&quot;/&gt;
				&lt;/ed:RegularPolygon.RenderTransform&gt;
			&lt;/ed:RegularPolygon&gt;
		&lt;/local:SteeredVehicle&gt;
	&lt;/Grid&gt;
&lt;/UserControl&gt;
</pre>
<pre>	public partial class ArriveTest : UserControl
    {
        double mouseX = 0;
        double mouseY = 0;
		public ArriveTest()
		{
			// Required to initialize variables
            InitializeComponent();

            Loaded += new RoutedEventHandler(SeekTest_Loaded);
        }

        void SeekTest_Loaded(object sender, RoutedEventArgs e)
        {
            MouseMove += new MouseEventHandler(SeekTest_MouseMove);
            CompositionTarget.Rendering += new EventHandler(CompositionTarget_Rendering);
        }

        void CompositionTarget_Rendering(object sender, EventArgs e)
        {
            myStar.arrive(new Vector2D(mouseX, mouseY));
            myStar.update();
        }

        void SeekTest_MouseMove(object sender, MouseEventArgs e)
        {
            mouseX = e.GetPosition(null).X;
            mouseY = e.GetPosition(null).Y;
        }
	}
</pre>
</div>
<div style="margin-top:0px;margin-bottom:0px">









</div>
<p>
<div style="margin-top:0px;margin-bottom:0px">和测试寻找行为唯一的不同就是在Rendering中把函数名seek换成了arrive。运行一下试试把鼠标移动到某处，机车先是以寻找模式发现 目标，然后慢慢的停在鼠标所在位置。再次移动鼠标又会回到寻找模式。通过调整arriveThreshold属性，看看机车接近目标时的变化吧。</div>
如果愿意可以再试着玩玩增加多辆机车，或者现在就进入下一个行为：追捕。 </p><img src="http://www.cnblogs.com/nasa/aggbug/1764632.html?type=1" width="1" height="1" alt=""><p>评论: 1　<a href="http://www.cnblogs.com/nasa/archive/2010/06/24/sl-steering-behaviors-7-Arrive.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/nasa/archive/2010/06/24/sl-steering-behaviors-7-Arrive.html#commentform">发表评论</a></p><p><a href="http://a4.yeshj.com/rd/35451/">软件研发团队管理年会(上海，7.10-7.11)</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/66953/">我完全支持云计算，但不要让它成为一个噱头</a><span style="color:gray">(2010-06-25 08:12)</span><br>· <a href="http://news.cnblogs.com/n/66952/">Android 2.2 for Intel 夏天公布</a><span style="color:gray">(2010-06-25 08:10)</span><br>· <a href="http://news.cnblogs.com/n/66951/">盛大“出海”</a><span style="color:gray">(2010-06-25 07:46)</span><br>· <a href="http://news.cnblogs.com/n/66950/">视频：三星 Windows Phone 7 原型机上手玩</a><span style="color:gray">(2010-06-25 07:44)</span><br>· <a href="http://news.cnblogs.com/n/66949/">iBooks 扼杀字体，苹果正破坏标准</a><span style="color:gray">(2010-06-25 07:42)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/firelong/archive/2010/06/24/1764597.html">C#会重蹈覆辙吗？反射及元数据的性能问题</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>