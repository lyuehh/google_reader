---
layout: post
title:  "Hybrid开发Javascript与Objective-C双向交互的技巧"
date:   2012-10-22 18:10:13
author: 涂雅
categories: program
---

## Hybrid开发Javascript与Objective-C双向交互的技巧
### by 涂雅
### at 2012-10-22 18:10:13
### original <http://iove.net/archives/3093.html>

<p>Hybrid开发中不可回避的问题，就是让Objective-C与Javascript进行交互，既然是交互，就是双向的。Objective-C可以使用注入的方式实现在Objective-C中调用UIWebView中的Javascript。</p>
<p>但如果你要在Javascript调用Objective-C的代码，可就没那么容易了，你需要通过url的方式绕过去。也就是说，你在Javascript发起一个http请求，然后把数据通过URL传输。在Objective-C中拦截这个URL的请求，曲线救国的方式达到调用Objective-C。<br>
上面简单介绍一下背景，这些网上都有，我想说说网上比较少找到的技巧吧。</p>
<p>如何从Javascript传输入大量的数据到Objective-C中？</p>
<p>一般少量的数据，我们可以通过url?param=value这种方式传输。但这种方式是有缺陷的，一是只能传输少量数据，因为url的长度是有限制的，二是有些数据会有奇怪的问题，比如说特殊字符。所以这里我们要用到异步技术，先从Javascript生成一个唯一的Key，然后发起一个http请求，在URL后面带上这个唯一的Key。Objective-C通过这个Key调用Javascript请求数据，Javascript根据Key获取数据。有点绕吧，伪代码如下。</p>
<p>在Javascript的环境代码</p>
<p>var _events = {};</p>
<p>callObjc(fn, data){<br>
var key = guid;               //生成唯一的key<br>
//将返回数据的写入_events<br>
_events[key] = data;</p>
<p>var url = url + “key=key&amp;fn=fn”;<br>
httpRequest(url);<br>
};</p>
<p>//用于objc调用<br>
getData(key){<br>
var data = _events[key];<br>
delete_events[key];<br>
return data;<br>
};</p>
<p>在Objective-C中的伪代码</p>
<p>//拉载url请求<br>
request(http){<br>
//1.取得url中的functionName和key<br>
//2.调用javascript的getData函数，并且带上Key<br>
//3.取出数据，调用Objective-C中的代码<br>
}</p>
<p>大致代码是这样，我想已经能够清楚地说明问题了，这样我们就解决了大数据从Javascript传输到Objective的问题。如果这个数据是JSON数据，你稍为加工一样，就可以完全无压力地调用Objective-C的方法了。既然能传输数据，要做其它事情当然是同理了，我就不再写得详细了，我想读者应该思考一下的。</p>
<p>讲另一个问题，从Javascript通过URL的方式调用Objective-C有时候会出现请求不响应的情况。这种问题在并发高的时候特别明显，比如说你同时发起两个http请求，Objective的拦截处理函数只能拦截到一个。所以，你不能并行处理，你需要串行处理http请求。</p>
<p>这有点像协议握手的感觉，双方握手代表一个交互。Javascript先请请求存入Queue中，然后按FIFO机制处理请求，Objective-C成功收到请求后，再通知Javascript已经获取这个请求，Javascript开始执行Queue中的下一个http请求。注意一点，只要一收到请求，就立即通知Javascript，不要等处理完了再通知。当然还有另一种方法就是用延时，你可以使用计时器让每个请求间隔50毫秒，这个间隔是安全的。但这显示不能队列处理好。</p>
<p>说另一个相关话题，Hybrid开发不要使用Web Database，因为iOS升级后，可能会改变Sqlite数据的存储位置，并且删除原数据。这事已经发生过一次了，估计以后还会发生。所以如果你有数据要存Sqlite，一定要用Obj-C存储文件的方式存储。通过我上面讲的异步机制，代码量也不是很大，估计100行以内的代码可以完全Sqlite存储到Documents文件夹中去。</p>
<p>再讲一个相关话题吧，从Javascript通过URL请求要注意大小写，建议全部小写，因为iOS4和后面的版本不一样。忘记是4还是后面的版本对大小写敏感，大概是在4和以前的版本，会将url全部转换为小写。抓狂吧，没办法，注意下吧。</p>
<p>说明：大家凑合看吧。我现在写博客完全不注意格式了，也不给出完整代码了，抱歉，要用心整一篇文章太费事了，特别是技术类文章。阅读本文要求你要有点底子才行，伸手党就跳过吧。</p>