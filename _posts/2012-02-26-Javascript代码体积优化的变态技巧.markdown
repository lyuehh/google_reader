---
layout: post
title:  "Javascript代码体积优化的变态技巧"
date:   2012-02-26 18:54:09
author: Huan Du
categories: program
---

## Javascript代码体积优化的变态技巧
### by Huan Du
### at 2012-02-26 18:54:09
### original <http://www.udpwork.com/item/6903.html>

<p>最近看到了js1k网站上的<a href="http://js1k.com/2012-love/">比赛</a>，突然很有兴趣参加一把，于是写了一个自己的作品<a href="http://js1k.com/2012-love/demo/1179">Save Messed Up Love</a>放了上去。</p>
<p>所有提交的作品必须用<strong>小于1024B的javascript实现</strong>
，我花了一些精力用一些变态技巧优化用工具精简过的代码体积，成功将代码从原来的1271B减到988B，空出的代码让我有空间增加一些额外的功能。在这里给大家分享一下这些变态技巧，注意，<strong>千万不要在实际项目中使用</strong>
。</p>
<p>首先介绍一些不变态的技巧，算是入门级的内容，其要点是<strong>时间换空间</strong>
，这是与jslint的原则背道而驰的，把下面每个点都反过来看可能就是正确的编程方法了……</p>
<p><strong>尽量使用with</strong>
</p>
<p>javascript的with关键字是神器，详见下面的代码：</p>
<pre>
// var a is the canvas 2d context
a.save();
a.fillStyle = &quot;red&quot;;
// ... many many canvas methods/attributes calls
a.restore();

//
// can be optimized to following code.
//
with (a) {
    save();
    fillStyle = &quot;red&quot;;
    // ...
    restore();
}
</pre><p><strong>将所有用到的变量放到开头声明</strong>
</p>
<p>不要分函数声明变量，所有变量都应该放到一起声明，这样才最有利于代码压缩工具减少代码体积。还有一点要注意，千万不要声明全局变量，否则代码压缩工具会以为这些是需要在window对象中导出的变量，从而放弃压缩它们。</p>
<pre>
// the &quot;with&quot; here is quite important to create a local closure for vars.
with (a) {
    var min, complete, posX, // and more
        draw = function() {
            // no var in this function
        },
        main = function() {
            // no var either
        };

    main();
}
</pre><p><strong>将经常出现的常量变成变量</strong>
</p>
<p>在我的程序里因为不断会出现字体，所以我将字体单独作为全局变量声明就可以省去不少bytes。程序中还经常会出现100这个数字，我也作为常量声明，省去超过10个byte。</p>
<pre>
F = &quot;2px Arial&quot;;
with (a) {
    // ... other rendering code
    font = 1 + F; // 12px Arial
}
</pre><p><strong>将常用的函数变成变量</strong>
</p>
<p>程序中使用了Math.cos好几次，将它变成变量以后就能够节省代码量。可惜的是canvas的各种方法都不能用这种技巧，所以这个技巧效果一般。</p>
<pre>
with (a) {
    var cos = Math.cos;
    // use cos instead of Math.cos, then code compiler can optimize it
    y = 13 * (-13 * cos(t) + 5 * cos(2 * t) + 2 * cos(3 * t) + cos(4 * t) + 13);
}
</pre><p>优化完这些之后，接着就可以去压缩了，压缩推荐使用<a href="http://closure-compiler.appspot.com/home">Google Closure Compiler Online</a>，这已经是当前压缩比例最变态的工具了，一定要选择“Advanced”压缩，效果最好。</p>
<p>压缩过后体积很不如我所意，但又不想砍功能，于是只好面对生成的一堆接近于乱码的东西直接开刀。最后所列的这几条就是变态优化的精髓了，每个可能只能省掉几个到十几个byte，但积少成多。</p>
<p><strong>修复Closure Compiler的压缩bug</strong>
</p>
<p>恐怕很少有人真的去读过压缩过后的js代码吧，仔细一读发现居然有若干明显问题。</p>
<ol><li>会自作主张的把浮点数前面补0，比如.3会“优化”成0.3，毫无道理的浪费字节数</li>
<li>会自作聪明的将从来没变过的变量直接代入计算，比如我前面所说的F = &quot;2px Arial&quot;，我故意抽取出这个不变的部分来优化体积，但经过编译之后竟给我消除了这个变量，直接把常量在代码中展开。解决方法是将这个声明放到全局，让工具以为这是window下的全局变量从而放弃优化。</li>
<li>没有尽可能的去掉分号，比如在每个}之前的分号是没必要的，工具并没有主动去掉</li>
</ol>
<p>解决完上述问题后就可以进行下面的更变态的优化。</p>
<p><strong>去掉var</strong>
</p>
<p>var声明其实是完全没有必要的，工具帮忙搞定了变量名的问题之后，就可以将var语句给删除了，一下就可以节省十几个byte。</p>
<pre>
with(a){var b,d,e,f,g,h,i,j,k,l,m,n=[/*data*/]}

// no need to declare every var.
// delete &quot;var b,d,e,f,g,h,i,j,k,l,m,&quot; directly.
// can be optimized to following
with(a){n=[/*data*/]}
</pre><p><strong>将表达式变成函数参数</strong>
</p>
<p>也许很少人会想到可以利用javascript对函数参数无约束这个特性优化代码大小吧，看下面的例子。</p>
<pre>
with (a) {
    lineWidth = 3;
    main();
}

//
// given the main() always takes no param,
// it can be optimized to following code
//
with (a) {
    main(lineWidth = 3);
}
</pre><p>效果就是优化掉了一个分号。</p>
<p>对于压缩过的代码其实有更好的压缩效果，可以看下面的例子。</p>
<pre>
b&amp;&amp;(c=z,u())

// can be optimized to following to save 3 bytes
b&amp;&amp;u(c=z)
</pre><p>这样就优化了3个byte！</p>
<p>这种优化的空间很大，因为除了自己写的函数，还有不少api都是不接受任何参数的（比如canvas 2d context的save()、fill()等），都可以用来做这个优化。还有接受参数的函数，只要确定后续多余的参数肯定不会使用，也可以用这个变态技巧。</p>
<p><strong>尽量复用常量</strong>
</p>
<p>常量一旦大量的复用，就可以节省代码。主要有两种复用，一种是让尽可能多的算式或参数出现同样的常数，比如100；另一种是尽可能让一个量有两种值，比如作为bool的量其实不关心具体数值如何，用零和非零表达即可，只要恰好有个值能表达这个意思就是用这个值。</p>
<p>因为第一种情况比较好理解，下面只对第二种情况举个例子。</p>
<pre>
T=&quot;Win!&quot;,z(1)

// can be optimized to following
z(T=&quot;Win!&quot;)
</pre><p>关于复用常量其实还有另外一个非常小的技巧，即有相同常量的时候尽量用连等。我的代码里恰好有一个数组声明，里面有0~15的常量，于是我就直接在数组里声明。</p>
<pre>
n=[12,2,3,7,C=o=0,6,10,5,4,15,9,14,8,B=13,1,11]
</pre><p>有了以上变态方法之后，代码体积明显的减小了，最终达到了变态的体积要求。最后的最后在提醒大家一次，<strong>不要在项目中使用这些技巧</strong>
，只会害人害己哦。</p>
<img src="http://www1.feedsky.com/t1/609890843/realdodo/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/realdodo/~8631699/609890843/6015014/1/item.html">
			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/6903.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/6903.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/6903.html#reviews">评论: 0</a>
				</p>
			</div>