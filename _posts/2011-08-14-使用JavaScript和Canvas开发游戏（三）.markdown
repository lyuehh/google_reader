---
layout: post
title:  "使用JavaScript和Canvas开发游戏（三）"
date:   2011-08-14 08:16:38
author: 为之漫笔
categories: program
---

## 使用JavaScript和Canvas开发游戏（三）
### by 为之漫笔
### at 2011-08-14 08:16:38
### original <http://www.cn-cuckoo.com/2011/08/14/game-development-with-javascript-and-the-canvas-element-3-2604.html>

<p>原文作者：Matthew Casperson • 编辑：Michele McDonough<br>
原文链接: <a href="http://www.brighthub.com/hubfolio/matthew-casperson/blog/archive/2009/06/29/game-development-with-javascript-and-the-canvas-element.aspx">Game Development with JavaScript and the Canvas element</a></p>
<p>1、认识一下Canvas<br>
2、在Canvas上绘图<br>
3、通过Canvas元素实现高级图像操作<br>
4、写一个游戏框架（一）<br>
5、写一个游戏框架（二）<br>
6、通过Canvas实现视差滚动<br>
7、动画<br>
8、JavaScript键盘输入<br>
9、综合运用<br>
10、定义级别<br>
11、跳跃与坠落<br>
12、添加道具<br>
13、加载资源<br>
14、添加主菜单</p>
<h1>4、写一个游戏框架（一）</h1>
<p><a href="http://www.brighthub.com/internet/web-development/articles/40512.aspx">http://www.brighthub.com/internet/web-development/articles/40512.aspx</a><br>
在知道了如何使用画布元素之后，接下来我教大家写一个框架，有了这个框架，我们就可以把它作为基础来创建游戏。在这第一部分，我们会介绍前两个文件/类。</p>
<p>编写代码之前，我们先来看一看<a href="http://webdemos.sourceforge.net/jsplatformer3/jsplatformer3.html">随后几篇文章将致力于创建的示例Demo</a>。表面上看起来，这个Demo跟第二篇文章里的那个没啥区别，但如果你看看后台（查看网页源代码）就会发现，为了更方便地创建这个最终效果，一个凝聚不少心血的基础框架已经写好了。<br>
<img src="http://www.cn-cuckoo.com/cache/img/2604_1.jpg"></p>
<p>下面我们要介绍的JavaScript代码使用面向对象的方式来编写。对于没有编写过多少JavaScript代码的人来说，恐怕第一眼看到它们会觉得有点奇怪。如果你真的不太熟悉JavaScript的面向对象编程，建议通过Mozilla Developer Network的这个教程<a href="https://developer.mozilla.org/en/Introduction_to_Object-Oriented_JavaScript">https://developer.mozilla.org/en/Introduction_to_Object-Oriented_JavaScript</a>来补补课。这篇教程里解释了我们稍后会用到的一些编程技术。</p>
<p>从设计思想上来看，这个框架可以分成两部分：与底层的2D引擎交互的类（用于操作画布、控制渲染循环、处理输入等的代码）和用来创建对象以便构成游戏的类。前者可以归为引擎类，后者可以归为应用类。由于应用类要构建于引擎类之上，所以我们需要先来创建引擎类。<br>
<span></span></p>
<h2>Main.js</h2>
<p>如果你研究了前面例子中的代码，就会发现Main.js文件中包含了不少代码。</p>
<pre>

/** 每秒多少帧
    @type Number
*/
var FPS = 30;
/** 两帧间间隔的秒数
    @type Number
*/
var SECONDS_BETWEEN_FRAMES = 1 / FPS;
/** GameObjectManager 实例的全局引用
    @type GameObjectManager
*/
var g_GameObjectManager = null;
/** 应用中用到的图像
    @type Image
*/
var g_image = new Image();
g_image.src = &quot;jsplatformer3-smiley.jpg&quot;;

// 将应用的入口设置为init函数
window.onload = init;

/**
    应用的入口
*/
function init()
{
    new GameObjectManager().startupGameObjectManager();
}
</pre>
<p>首先是定义全局变量的代码。然后，跟以前一样，当页面加载完毕后立即运行init函数。在init函数里，创建GameObjectManager类的实例。</p>
<p>这里在GameObjectManager类的实例上调用了startupGameObjectManager函数。这篇文章以及后面的几篇文章还将多次提到几个命名上具有startupClassName形式的函数。这些函数实际上充当了各自类的构造函数，这样做有两个原因。</p>
<p>首先，JavaScript不支持函数重载（至少不容易实现）。如果你想让一个类有多个构造函数，那么这就成了问题。而通过把构造工作分配给另一组函数（如startupClassName1、startupClassName2），就可以比较容易地定义构造类的不同方式了。</p>
<p>第二个原因（很大程度上也是个人的问题）是我经常会在构造函数中引用尚未定义的变量。这可能是我使用C++、Java和C#这些语言落下的毛病，在这些语言里，类变量在源代码中的位置对其在构造函数中的可见性没有影响。拿下面这个C#类为例：</p>
<pre>

class Test
{
	public void Test() {this.a = 5;}
	public int a;
}
</pre>
<p>这些代码是合乎语法的，可以正常工作。下面再看看JavaScript中一个相同的例子：</p>
<pre>

function Test()
{
	this.a = 5;
	var a;
}
</pre>
<p>这段代码的问题在于，局部变量a在我们把数值5赋给它的时候还不存在。只有运行到var a;这一行，变量a才存在。尽管这个例子有点故意编排的意味，但的确能够说明我所遇到的问题。通过把类的创建放到一个类似startupClassName这样的函数中完成，并且在构造函数中定义（但不初始化）局部变量，然后当我在这些构建函数中引用相应的局部变量时，就能够确保它们一定是存在的。</p>
<h2>GameObjectManager.js</h2>
<pre>

/**
    管理游戏中所有对象的管理器
    @class
*/
function GameObjectManager()
{
    /** 保存游戏中对象的数组
        @type Arary
    */
    this.gameObjects = new Array();
    /** 上一次帧被渲染的时间
        @type Date
    */
    this.lastFrame = new Date().getTime();
    /** x轴的全局滚动值
        @type Number
    */
    this.xScroll = 0;
    /** y轴的全局滚动值
        @type Number
    */
    this.yScroll = 0;
    /** 对ApplicationManager实例的引用
        @type ApplicationManager
    */
    this.applicationManager = null;
    /** 对画布元素的引用
        @type HTMLCanvasElement
    */
    this.canvas = null;
    /** 对画布元素2D上下文的引用
        @type CanvasRenderingContext2D
    */
    this.context2D = null;
    /** 对内存中用作后台缓冲区的画布的引用
        @type HTMLCanvasElement
    */
    this.backBuffer = null;
    /** 对后台缓冲画布的2D上下文的引用
        @type CanvasRenderingContext2D
    */
    this.backBufferContext2D = null;

    /**
        初始化这个对象
        @return A reference to the initialised object
    */
    this.startupGameObjectManager = function()
    {
        // 设置引用this对象的全局指针
        g_GameObjectManager = this;

        // 取得画布元素及其2D上下文的引用
        this.canvas = document.getElementById(&#39;canvas&#39;);
        this.context2D = this.canvas.getContext(&#39;2d&#39;);
        this.backBuffer = document.createElement(&#39;canvas&#39;);
        this.backBuffer.width = this.canvas.width;
        this.backBuffer.height = this.canvas.height;
        this.backBufferContext2D = this.backBuffer.getContext(&#39;2d&#39;);

        // 创建一个新的ApplicationManager
        this.applicationManager = new ApplicationManager().startupApplicationManager();

        // 使用setInterval来调用draw函数
        setInterval(function(){g_GameObjectManager.draw();}, SECONDS_BETWEEN_FRAMES);

        return this;
    }

    /**
        渲染循环
    */
    this.draw = function ()
    {
        // 计算从上一帧到现在的时间
        var thisFrame = new Date().getTime();
        var dt = (thisFrame - this.lastFrame)/1000;
        this.lastFrame = thisFrame;

        // 清理绘制上下文
        this.backBufferContext2D.clearRect(0, 0, this.backBuffer.width, this.backBuffer.height);
        this.context2D.clearRect(0, 0, this.canvas.width, this.canvas.height);

        // 首先更新所有游戏对象
        for (x in this.gameObjects)
        {
            if (this.gameObjects[x].update)
            {
                this.gameObjects[x].update(dt, this.backBufferContext2D, this.xScroll, this.yScroll);
            }
        }

        // 然后绘制所有游戏对象
        for (x in this.gameObjects)
        {
            if (this.gameObjects[x].draw)
            {
                this.gameObjects[x].draw(dt, this.backBufferContext2D, this.xScroll, this.yScroll);
            }
        }

        // 将后台缓冲复制到当前显示的画布
        this.context2D.drawImage(this.backBuffer, 0, 0);
    };

    /**
        向gameObjects集合中添加一个GameObject
        @param gameObject The object to add
    */
    this.addGameObject = function(gameObject)
    {
        this.gameObjects.push(gameObject);
        this.gameObjects.sort(function(a,b){return a.zOrder - b.zOrder;})
    };

    /**
        从gameObjects集合中删除一个GameObject
        @param gameObject The object to remove
    */
    this.removeGameObject = function(gameObject)
    {
        this.gameObjects.removeObject(gameObject);
    }
}
</pre>
<p>首先看一看GameObjectManager类。GameObjectManager是一个引擎类，用于管理画布的绘制操作，还负责分派GameObject类（下一篇文章里介绍）的事件。</p>
<p>GameObjectManager类的startupGameObjectManager函数的代码如下：</p>
<pre>

    /**
        初始化这个对象
        @return A reference to the initialised object
    */
    this.startupGameObjectManager = function()
    {
        // 设置引用this对象的全局指针
        g_GameObjectManager = this;

        // 取得画布元素及其2D上下文的引用
        this.canvas = document.getElementById(&#39;canvas&#39;);
        this.context2D = this.canvas.getContext(&#39;2d&#39;);
        this.backBuffer = document.createElement(&#39;canvas&#39;);
        this.backBuffer.width = this.canvas.width;
        this.backBuffer.height = this.canvas.height;
        this.backBufferContext2D = this.backBuffer.getContext(&#39;2d&#39;);

        // 创建一个新的ApplicationManager
        this.applicationManager = new ApplicationManager().startupApplicationManager();

        // 使用setInterval来调用draw函数
        setInterval(function(){g_GameObjectManager.draw();}, SECONDS_BETWEEN_FRAMES);

        return this;
    }
</pre>
<p>前面已经说过，我们会把每个类的初始化工作放在startupClassName函数中来做。因此，GameObjectManager类将由startupGameObjectManager函数进行初始化。</p>
<p>而引用这个GameObjectManager实例的全局变量g_GameObjectManager经过重新赋值，指向了这个新实例。</p>
<pre>

        // 设置引用this对象的全局指针
        g_GameObjectManager = this;
</pre>
<p>对画布元素及其绘图上下文的引用也同样保存起来：</p>
<pre>

        // 取得画布元素及其2D上下文的引用
        this.canvas = document.getElementById(&#39;canvas&#39;);
        this.context2D = this.canvas.getContext(&#39;2d&#39;);
</pre>
<p>在前面的例子中，所有绘图操作都是直接在画布元素上完成的。这种风格的渲染一般称为单缓冲渲染。在此，我们要使用一种叫做双缓冲渲染的技术：任意游戏对象的所有绘制操作，都将在一个内存中的附加画布元素（后台缓冲）上完成，完成后再通过一次操作把它复制到网页上的画布元素（前台缓冲）。</p>
<p>双缓冲技术（<a href="http://www.brighthub.com/internet/web-development/articles/11012.aspx">http://www.brighthub.com/internet/web-development/articles/11012.aspx</a>）通常用于减少画面抖动。我自己在测试的时候从没发现直接向画布元素上绘制有抖动现象，但我在网上的确听别人念叨过，使用单缓冲渲染会导致某些浏览器在渲染时发生抖动。</p>
<p>不管怎么说，双缓冲还是能够避免最终用户看到每个游戏对象在绘制过程中最后一帧的组合过程。在通过JavaScript执行某些复杂绘制操作时（例如透明度、反锯齿及可编程纹理），这种情况是完全可能发生的。</p>
<p>使用附加缓冲技术占用的内存非常少，多执行一次图像复制操作（把后台缓冲绘制到前台缓冲）导致的性能损失也可以忽略不计，可以说实现双缓冲系统没有什么缺点。</p>
<p>如果将在HTML页面中定义的画布元素作为前台缓冲，那就需要再创建一个画布来充当后台缓冲。为此，我们使用了document.createElement函数在内存里创建了一个画布元素，把它用作后台缓冲。</p>
<pre>

        this.backBuffer = document.createElement(&#39;canvas&#39;);
        this.backBuffer.width = this.canvas.width;
        this.backBuffer.height = this.canvas.height;
        this.backBufferContext2D = this.backBuffer.getContext(&#39;2d&#39;);
</pre>
<p>接下来，我们创建了ApplicationManager类的一个新实例，并调用startupApplicationManager来初始化它。这个ApplicationManager类将在下一篇文章中介绍。</p>
<pre>

        // 创建一个新的ApplicationManager
        this.applicationManager = new ApplicationManager().startupApplicationManager();
</pre>
<p>最后，使用setInterval函数重复调用draw函数，这个函数是渲染循环的核心所在。</p>
<pre>

        // 使用setInterval来调用draw函数
        setInterval(function(){g_GameObjectManager.draw();}, SECONDS_BETWEEN_FRAMES);
</pre>
<p>下面来看一看draw函数。</p>
<pre>

    /**
        渲染循环
    */
    this.draw = function ()
    {
        // 计算从上一帧到现在的时间
        var thisFrame = new Date().getTime();
        var dt = (thisFrame - this.lastFrame)/1000;
        this.lastFrame = thisFrame;

        // 清理绘制上下文
        this.backBufferContext2D.clearRect(0, 0, this.backBuffer.width, this.backBuffer.height);
        this.context2D.clearRect(0, 0, this.canvas.width, this.canvas.height);

        // 首先更新所有游戏对象
        for (x in this.gameObjects)
        {
            if (this.gameObjects[x].update)
            {
                this.gameObjects[x].update(dt, this.backBufferContext2D, this.xScroll, this.yScroll);
            }
        }

        // 然后绘制所有游戏对象
        for (x in this.gameObjects)
        {
            if (this.gameObjects[x].draw)
            {
                this.gameObjects[x].draw(dt, this.backBufferContext2D, this.xScroll, this.yScroll);
            }
        }

        // 将后台缓冲复制到当前显示的画布
        this.context2D.drawImage(this.backBuffer, 0, 0);
    };
</pre>
<p>这个draw函数就是所有渲染循环的核心。在前面的例子中，渲染循环的函数会直接修改要绘制到屏幕上的对象（笑脸）。如果只需绘制一个对象，这样做没有问题。但是，一个游戏要由几十个单独的对象组成，所以这个draw函数并没有直接在渲染循环中直接处理要绘制的对象，而是维护了一个保存着这些对象的数组，让这些对象自己来更新和绘制自己。</p>
<p>首先，计算自上一帧渲染所经过的时间。即便我们在代码里写了每秒钟调用30次draw函数，但谁也无法保证事实如此。通过计算自上一帧渲染所经过的时间，可以做到尽可能让游戏的执行与帧速率无关。</p>
<pre>

        // 计算从上一帧到现在的时间
        var thisFrame = new Date().getTime();
        var dt = (thisFrame - this.lastFrame)/1000;
        this.lastFrame = thisFrame;
</pre>
<p>接着清理绘制上下文。</p>
<pre>

        // 清理绘制上下文
        this.backBufferContext2D.clearRect(0, 0, this.backBuffer.width, this.backBuffer.height);
        this.context2D.clearRect(0, 0, this.canvas.width, this.canvas.height);
</pre>
<p>然后，就是调用游戏对象（这些对象是由GameObject类定义的，下一篇文章将介绍该类）自己的更新（update）和绘制（draw）方法。注意，这两个方法是可选的（这也是我们在调用它们之前先检查它们是否存在的原因），但差不多每一个对象都需要更新和绘制自已。</p>
<pre>

        // 首先更新所有游戏对象
        for (x in this.gameObjects)
        {
            if (this.gameObjects[x].update)
            {
                this.gameObjects[x].update(dt, this.backBufferContext2D, this.xScroll, this.yScroll);
            }
        }

        // 然后绘制所有游戏对象
        for (x in this.gameObjects)
        {
            if (this.gameObjects[x].draw)
            {
                this.gameObjects[x].draw(dt, this.backBufferContext2D, this.xScroll, this.yScroll);
            }
        }
</pre>
<p>最后，把后台缓冲复制到前台缓冲，最终用户就可以看到下一帧了。</p>
<pre>

        // 将后台缓冲复制到当前显示的画布
        this.context2D.drawImage(this.backBuffer, 0, 0);
</pre>
<p>理解了draw函数，下面再分别讲一讲addGameObject和removeGameObject函数。</p>
<pre>

    /**
        向gameObjects集合中添加一个GameObject
        @param gameObject The object to add
    */
    this.addGameObject = function(gameObject)
    {
        this.gameObjects.push(gameObject);
        this.gameObjects.sort(function(a,b){return a.zOrder - b.zOrder;})
    };

    /**
        从gameObjects集合中删除一个GameObject
        @param gameObject The object to remove
    */
    this.removeGameObject = function(gameObject)
    {
        this.gameObjects.removeObject(gameObject);
    }
</pre>
<p>利用addGameObject和removeGameObject（在Utils.js文件里通过扩展Array.prototype添加）函数，可以在GameObjectManager所维护的GameObject集合（即gameObjects变量）中添加和删除游戏对象。</p>
<p>GameObjectManager类是我们这个游戏框架中最复杂的一个类。在下一篇文章中，我们会讲解游戏框架的另外几个类：GameObject、VisualGameObject、Bounce和ApplicationManager。</p>
<p>好了，现在放松一下，<a href="http://webdemos.sourceforge.net/jsplatformer3/jsplatformer3.html">看一看Demo吧</a>。</p>
<p>为之漫笔 最后编辑于：<br>
2011/08/14 @ 23:19</p>