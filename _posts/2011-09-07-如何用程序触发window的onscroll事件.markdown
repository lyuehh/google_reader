---
layout: post
title:  "如何用程序触发window的onscroll事件"
date:   2011-09-07 18:29:01
author: admin
categories: program
---

## 如何用程序触发window的onscroll事件
### by admin
### at 2011-09-07 18:29:01
### original <http://item.feedsky.com/~feedsky/helloJavaScript/~8514355/606431457/6618683/1/item.html>

<p>很多延迟加载的组件监听的都是window.onscroll事件.所以如果你自己模拟滚动条的话,你需要在滚动到时候能用js触发这个事件.</p>
<p>如何做呢,直接上代码,不要问为什么,就是这样:</p>
<pre>if (document.createEvent)
            {
                var evObj = document.createEvent("HTMLEvents")
                evObj.initEvent( "scroll", true, false )
                window.dispatchEvent(evObj)
            }else{
                document.body.click();
            }
</pre>
<p>ie里面有人说用scrollTo或者scrollBy可以触发….不错,不过如果你的鼠标左键被按下或者滚轮在滚动,这种方法就失效了….为什么我也不知道.<br>
后来发现只需要点击一下body…就能触发onscroll….注意是window的onscroll…其他元素的可能存在不同.</p>
<div style="float:left">
<div>
	<a></a>
	<a></a>
	<a></a>
	<a></a>
	<a></a>
	<a href="http://www.jiathis.com/share/"></a>
	<a></a>
</div>

</div><img src="http://www1.feedsky.com/t1/606431457/helloJavaScript/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/helloJavaScript/~8514355/606431457/6618683/1/item.html" border="0" height="0" width="0">