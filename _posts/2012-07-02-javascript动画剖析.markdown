---
layout: post
title:  "javascript动画剖析"
date:   2012-07-02 14:49:49
author: admin
categories: program
---

## javascript动画剖析
### by admin
### at 2012-07-02 14:49:49
### original <http://stylechen.com/javascript-animate.html>

<p>最近将去年写过的<a href="http://stylechen.com/easyanim.html">easyAnim</a>进行了重构和优化以整合到我的javascript框架中，回过头来发现以前写的代码确实还有很多可以改进的地方，这也证明自己还是有点进步的。趁有点时间，将javascript动画运行的机制和实现的思路整理了一下，算是做个小总结，也希望对有兴趣的人有点帮助。篇幅稍长，看之前请自备瓜子啤酒。</p>
<p>当然这里说的javascript动画是指利用javascript来计算DOM元素的CSS属性值来实现的动画，HTML5和CSS3的发展让WEB中的动画有了更多的可能，但这些看起来比较高级的东西还需要浏览器给力点才行。</p>
<h3>解析CSS属性值</h3>
<p>理论上，只要CSS的属性值包含了数值，就可以进行动画，事实上，包含了数值的CSS属性值有很多很多，拿最常见的属性来说：</p>
<pre>
div{ left:100px; }
</pre>
<p>上面的位置值就包含了数值“100”，要想实现该元素从100px的位置到600px的位置，只要不断的修改该元素的位置值，达到一定的速度就实现了动画，据国外的统计，每25毫秒切换一次画面，就可以实现“动”的效果。</p>
<p>CSS属性值都是字符串，为了方便进行计算，需先将字符串转换成数值，并且很多属性值都包含了单位，所以最好解析成下面这种格式：</p>
<pre>
&#39;100px&#39; ==&gt; { val : 100, unit : &#39;px&#39; }
</pre>
<p>100px是元素开始动画的位置，600px是元素结束动画时的位置，同样的方法解析结束的位置值：</p>
<pre>
&#39;600px&#39; ==&gt; { val : 600, unit : &#39;px&#39; }
</pre>
<h3>动画的运行原理</h3>
<p>结束值和开始值之差500px就是动画的总距离，通常动画的时间都是固定的，假定动画时间为1000毫秒，那么速度=总距离/总时间为0.5px/毫秒。</p>
<p>总距离S = 总时间T * 速度V  ==&gt;  V = S/T</p>
<p>当前距离s = S/T * 已耗时t  ==&gt;  s = S * (t/T)</p>
<p>即：当前距离 = 总距离 * (已耗时/总时间)</p>
<p>有了上面的公式就可以计算出在固定了总距离和总时间的动画中，某个时间点的元素应该出现的精确位置了。仅仅有这些还不够，元素的一系列连贯性的变化才能形成动画，这里就有必要引入帧这个比较通用的概念。</p>
<p>从动画开始运行到动画结束，总距离和总时间都是固定的，根据上面提到的每25毫秒切换一次画面，那么可以计算出每秒的传输帧数为40帧/秒，也就是40fps。这个动画的总时间为1000毫秒，恰好是运行40帧完成动画。</p>
<p>那么javascript如何表示一帧呢？javascript跟时间有关系的肯定是定时器setTimeout和setInterval，动画需要重复运行，所以这里使用setInterval，每25毫秒执行一次就是一帧，用代码表示就是：</p>
<pre>
window.setInterval( tick, 25 );
</pre>
<p>每秒钟帧数 (fps) 愈多，所显示的动作就会愈流畅。通常，要避免动作不流畅的最低fps是30。事实上，在浏览器中，30fps还是会显得不太流畅，各大javascript框架都有默认的fps数，一般为50-80之间，各框架的fps和interval的设置在<a href="http://forum.jquery.com/topic/increasing-animation-frame-interval-and-exposing-it">这里</a>有简短的介绍。jQuery的默认的interval是13毫秒，fps在75左右，在各框架中算是很高的了。是不是fps越高越好？浏览器能使用的系统资源是有限的，fps越高也就越耗性能，也有可能出现丢帧的现象，在肉眼觉得可以接受的动画流畅的范围内，应适当的设置interval，所以我的动画框架的interval设置成了16毫秒，fps在60左右。</p>
<h3>running</h3>
<p>有了上面这些信息，就可以开始动画了，在定时器开始运行的那一刹那，要获取动画开始的时间，然后每一帧动画都要获取一次当前时间。动画开始时的属性值为sv，结束时的属性值是tv，结束时的单位是tu，当前时间减去开始的时间即为已耗时，已耗时/总时间的时间比为t，那么最终的运算公式为：</p>
<pre>
( sv + (tv - sv) * t ).toFixed(7) + tu;
</pre>
<p>这里的toFixed的参数也是有讲究的，CSS属性值大多数都允许小数点的数值，所以精度越高，动画的效果看起来就会显得越流畅，toFixed(0)和toFixed(7)的效果会有明显的差异。</p>
<p>当已耗时大于或等于总时间，就该结束动画了，结束动画就是清除定时器，因为动画的运行的结果不太可能刚好等于结束属性值，所以结束动画时应该将CSS属性值还原，既是最后的关键帧。</p>
<h3>tween</h3>
<p>tween翻译成中文是缓动，要想动画有特殊的效果，比如加速或者减速效果，就需要引入tween函数。下面是我从司徒正美的博客节选出来的一些内容，原文在<a href="http://www.cnblogs.com/rubylouvre/archive/2009/09/17/1567607.html">这里</a>。</p>
<p>javascript的tween函数的由于计算方法不同，基本可以分成flash流派和prototype流派，动画的效果都差不多。flash流派的缓动函数通常都有4个参数，分别是：</p>
<ul>
<li>t: timestamp，指缓动效果开始执行到当前帧开始执行时经过的时间段，单位ms</li>
<li>b: beginning position，起始位置</li>
<li>c: change，要移动的距离，就是终点位置减去起始位置</li>
<li>d: duration ，缓和效果持续的时间</li>
</ul>
<p>flash流派的匀速运动的函数是这样的：</p>
<pre>
var linear = function( t,b,c,d ){ 
	return c*t/d + b; 
};
</pre>
<p>prototype流派的计算公式更简单，封装得更好，只有一个参数–时间比，就是我上面说到的已耗时/总时间，匀速运动的函数是这样的：</p>
<pre>
var linear = function( t ){
    return t;
};
</pre>
<p>显然只需传一个参数的计算方法更简洁，我的动画框架就采用了这类tween函数，早期的jQuery以及现在的YUI3采用的是flash流派，现在的1.7版也改成了prototype流派。有了tween，那么上面的运算公式可以改成这样：</p>
<pre>
( sv + (tv - sv) * linear(t) ).toFixed(7) + tu;
</pre>
<p>tween函数有很多，有兴趣的同志可以自己一一尝试，这里就不多说了。</p>
<h3>队列</h3>
<p>很多复杂的动画效果都是很多单个的动画按顺序执行组合而成的，所以队列的控制尤为重要。动画的队列设计只要弄明白了就比较简单，这里简单的说说设计思路。将单个的动画都添加到一个队列数组中，始终取0索引的队列函数来执行，执行的时候给该队列数组添加一个’running’标志在数组的0索引位置，无该标志就是第一次运行该队列，有该标志就表示该队列正在运行，这样可以保证在队列运行的状态也可以添加新的队列成员，下面贴一段伪代码，这里用到了缓存系统：</p>
<pre>
// 将数据添加到队列中
queue : function( elem, data ){
    // 取出该元素的队列缓存
	var animQueue = this.data( elem, 'animQueue', [] );
    // 添加队列
	if( data ){
		animQueue.push( data );
	}
   // 根据running标志来判断是否执行队列
	if( animQueue[0] !== 'runing' ){
		this.dequeue( elem );
	}		
},

// 取出队列中的数据并执行
dequeue : function( elem ){
	var animQueue = this.data( elem, 'animQueue', [] ),
		fn = animQueue.shift(),
		delay;		
		
	if( fn === 'running' ){
		fn = animQueue.shift();
	}

	if( fn ){
		animQueue.unshift( 'running' );
        // 递归调用依次执行队列函数
		fn.call( elem, function(){
			easyAnim.dequeue( elem );
		});
	}
	
	// 无队列时清除相关的缓存
	if( !animQueue.length ){
		this.removeData( elem, 'animQueue' );
	}
}
</pre>
<h3>结束语</h3>
<p>明白了上面的东西，很多类型的动画都只是套用公式就行了。任何包含了数值的CSS属性值都可以先解析，然后进行动画，比如颜色值的动画可以先<a href="http://stylechen.com/parsecolor.html">将16进制的颜色值转换成rgb格式</a>再进行动画，还有其他的一些CSS属性值如：backgroundPosition、textShadow等只要分别计算各个值照样可以实现动画。</p>
<p>源码阅读：<a href="https://github.com/chenmnkken/easyjs/blob/master/core/src/anim.js">https://github.com/chenmnkken/easyjs/blob/master/core/src/anim.js</a></p>
<div>原载于：雨夜带刀&#39;s Blog<br>
本文链接：<a title="javascript动画剖析" href="http://stylechen.com/javascript-animate.html">http://stylechen.com/javascript-animate.html</a><br>
如需转载请以链接形式注明原载或原文地址。
</div>