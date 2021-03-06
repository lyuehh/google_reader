---
layout: post
title:  "overflow:hidden真的失效了吗"
date:   2012-12-30 17:41:29
author: sapphire
categories: program
---

## overflow:hidden真的失效了吗
### by sapphire
### at 2012-12-30 17:41:29
### original <http://www.aliued.cn/2012/12/30/overflowhidden%e7%9c%9f%e7%9a%84%e5%a4%b1%e6%95%88%e4%ba%86%e5%90%97.html>

<p>项目中常常有同学遇到这样的问题，现象是给元素设置了overflow:hidden，但超出容器的部分并没有被隐藏，难道是设置的hidden失效了吗？<br>
其实看似不合理的现象背后都会有其合理的解释。</p>
<p>我们知道，overflow属性值有这几种：<br>
visible：声明内容不会被剪裁。比如内容可能被渲染到容器外面。<br>
hidden：声明内容将被剪裁，并且也甭想使用滚动条来查看剪裁掉的内容。<br>
scroll：声明内容将被剪裁，但有可能出现滚动条来查看被剪裁掉的内容。滚动条出现的位置在inner border adge和outer padding adge之间。<br>
auto：声明决策将依赖于客户端，优先使用scroll。</p>
<p><a title="w3c overflow" href="http://www.w3.org/TR/CSS21/visufx.html#overflow">W3C标准</a>中指明：<br>
通常一个盒子的内容是被限制在盒子边界之内的。但有时也会产生溢出，即部分或全部内容跑到盒子边界之外。溢出将在满足下列条件之一时出现：<br>
1. 一个不换行的行元素宽度超出了容器盒子宽度。<br>
2. 一个宽度固定的块元素放在了比它窄的容器盒子内。<br>
3. 一个元素的高度超出了容器盒子的高度。<br>
4. 一个子孙元素，由负边距值引起的部分内容在盒子外部。<br>
5. text-indent属性引起的行内元素在盒子的左右边界外。<br>
6. 一个绝对定位的子孙元素，部分内容在盒子外。但超出的部分不是总会被剪裁。子孙元素的内容就不会被子孙元素和其包含块之间的祖先元素的overflow的设置所剪裁。</p>
<p>当溢出发生时，overflow属性约定了容器盒子是否剪裁掉超出其内边界的部分，并且决定是否出现滚动条来访问被剪裁掉的内容。它会影响到元素所有内容的剪裁，但有个例外情况，即上面第6条所提到的：元素的子孙元素的包含块（Containing blocks）是整个视窗（viewport）或是该元素的祖先元素，内容将不会被剪裁。<a title="containing-block" href="http://www.w3.org/TR/CSS21/visuren.html#containing-block">包含块</a>是什么呢？简单的说，就是可以决定一个元素位置和大小的块。通常一个元素的包含块由离它最近的块级祖先元素的内容边界决定。但当元素被设置成绝对定位时，包含块由最近的position不是static的祖先元素决定。</p>
<p><span></span><br>
看起来有点绕，让我们来听个简单的故事吧。<br>
html片段:</p>
<div style="padding:10px;color:#ff7300;border:1px #666 dotted;background-color:#eee;font-family:&#39;Courier New&#39;,monospace">
&lt;div class=”ocean”&gt;<br>
    &lt;div class=”land”&gt;<br>
        &lt;p class=”joke”&gt;<br>
                Mrs. Smith couldn’t get her husband to exercise. <br>
                She asked Mrs. Jones what she should do. Jones replied, <br>
                ”Tape the remote control between his toes.”<br>
        &lt;/p&gt;<br>
    &lt;/div&gt;<br>
&lt;/div&gt;
</div>
<p>style:</p>
<div style="color:#ff7300;border:1px #666 dotted;background-color:#eee;font-family:&#39;Courier New&#39;,monospace;padding:10px">
div.ocean{<br>
    position:relative;<br>
    background-color:blue;<br>
    width:120px;<br>
    height:120px;<br>
    }<br>
div.land{<br>
    width:100px;<br>
    height:100px;<br>
    background-color:red;<br>
    overflow:hidden;<br>
    }<br>
p.joke{<br>
    width:150px;<br>
    height:110px;<br>
    margin-top:30px;<br>
    margin-left:30px;<br>
    background-color:yellow;<br>
    }
</div>
<p>
上面的代码讲述的是这样一个故事：蓝色的海洋里有块红色的大地，红色大地内有个黄色的段子。由于段子样式的设置，它的部分内容超出了红色大地。为避免黄色段子污染到蓝色海洋，红色大地警惕的为自己设置了overflow:hidden;这样超出大地的黄色部分就被剪掉了，我们看到的将是这样一派和谐景象，如图1：<br>
<a href="http://www.aliued.cn/wp-content/uploads/2012/12/1.png"><img src="http://www.aliued.cn/wp-content/uploads/2012/12/1.png" alt="图1：和谐的星球"></a><br>
图1：和谐的星球<br>
<br>
如果事物都是这样有理有序，天下可不就太平了。没多久，黄色段子觉得凭自己的显赫身份不该受红色大地的控制，于是绞尽脑汁将自己变改成了绝对定位，一下子就摆脱了大地的束缚，如图2：</p>
<div style="color:#ff7300;border:1px #666 dotted;background-color:#eee;font-family:&#39;Courier New&#39;,monospace">
p.joke{<br>
    position:absolute;<br>
    width:150px;<br>
    height:110px;<br>
    top:30px;<br>
    left:30px;<br>
    background-color:yellow;<br>
    }
</div>
<p><a href="http://www.aliued.cn/wp-content/uploads/2012/12/2.png"><img src="http://www.aliued.cn/wp-content/uploads/2012/12/2.png" alt="图2：猖獗的段子"></a><br>
图2：猖獗的段子<br>
<br>
为什么会这样呢？这便是创造了上面提到过的第6个条件。当黄色段子变成position:absolute时，它的包含块已由原来的红色大地的内容边界升级到了离它最近的position不是static的蓝色海洋了。而海洋此刻对此还一无所知呢，自身没有设置overflow:hidden属性，导致黄色段子本该被裁剪的部分全部可见，不仅污染到海洋，还影响到整个星球，情况万分火急啊。即使这时海洋设置上overflow:hidden，也只能将超出蓝色海洋的黄色部分剪裁，就像图3，海洋此时是手足无措啊。<br>
<a href="http://www.aliued.cn/wp-content/uploads/2012/12/3.png"><img src="http://www.aliued.cn/wp-content/uploads/2012/12/3.png" alt="图3：无辜的海洋"></a><br>
图3：无辜的海洋<br>
<br>
俗语说的好，魔高一尺道高一丈，解铃还须系铃人。红色大地怎就甘心段子跑出去呢。怎么说大地终归是段子的祖先元素，怎么能甘心由着段子胡作非为呢。于是，大地历尽千辛，寻得秘籍，在自己的样式中添加position:relative属性，将段子的包含块又改成了大地来决定。这下段子就乖乖的被关起来了。星球看起来又回到了最初的状态。</p>
<div style="color:#ff7300;border:1px #666 dotted;background-color:#eee;font-family:&#39;Courier New&#39;,monospace">
</div>
<div style="color:#ff7300;border:1px #666 dotted;background-color:#eee;font-family:&#39;Courier New&#39;,monospace;padding:10px">
div.ocean{<br>
    position:relative;<br>
    background-color:blue;<br>
    width:120px;<br>
    height:120px;<br>
    }<br>
div.land{<br>
    position:relative;<br>
    width:100px;<br>
    height:100px;<br>
    background-color:red;<br>
    overflow:hidden;<br>
    }<br>
p.joke{<br>
    position:absolute;<br>
    width:150px;<br>
    height:110px;<br>
    top:30px;<br>
    left:30px;<br>
    background-color:yellow;<br>
    }
</div>
<p>所以说，hidden并没有失效，而是有可能我们遇到的情况恰好满足了第6个条件，使得元素的包含块发生了变化。上面的故事中，也提到了在遇到‘hidden’失效的情况时，可以根据需要来改变元素的包含块来达到正义的目的。</p>
<p>最后，假期愉快。</p>