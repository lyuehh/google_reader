---
layout: post
title:  "【web开发】一起来写段JS drag代码"
date:   2010-12-09 15:32:00
author: 周尚武
categories: program
---

## 【web开发】一起来写段JS drag代码
### by 周尚武
### at 2010-12-09 15:32:00
### original <http://www.cnblogs.com/275095923/archive/2010/12/09/1901352.html>

<p><p>记得几年前刚接触前端的时候，觉得能在网页上拖移元素是一件很爽的事，能写一段这样的代码是件很了不起的事情，于是乎google,baidu蛮多代码来学习，大致明白了思路，总结如下：</p>
<p>1、为要被拖移三个事件,onmousedown,onmousemove,onmouseup</p>
<p>2、在onmousemove事件中来处理被拖移元素的位置的变化，其实说白了元素要移动的距离就是鼠标两次移动之间的距离。</p>
<p>3、其中还包括setCapture,releaseCapture，目的就是为了被搬移的元素始终能拥有焦点。</p>
<p>以前大致就是以前的认识，可以参见<a href="http://heshengyu84a.blog.163.com/blog/static/34640681200711268630756/">http://heshengyu84a.blog.163.com/blog/static/34640681200711268630756/</a>这个实现，后来随着工作要求的提高，做的工作都是要跨浏览器的，所以就重新构思并参考一些开源代码做了实现。</p>
<p>现在大致思路可分析为以下几步，我一一为您展现。</p>
<p>1、 我们是为了拖移而搬移吗？当然不是，如google地图，当每次搬移后他的目的是为了计算当前空间坐标来加载地理信息。所以我这里设计了几个常见事件。以下是事件列表</p>
<p>onDragStart ：元素发生拖移时，如果注册该事件，触发时会接收到两个参数x,y分别为被搬移元素发生拖移时的坐标</p>
<p>onDrag ：   元素拖移过程中，如果注册该事件，触发时会接收到两个参数nx,ny拖移过程中坐标发生的偏移量 </p>
<p>onDragEnd ：元素发生结束时，如果注册该事件，触发时会接收到两个参数x,y分别为被搬移元素的当前坐标</p>
<p>2、 onmousedown事件应该绑定给谁?以前我都是绑定给被拖移元素的，后来发现很不灵活，现在设计为可绑定给任意不相关的元素同时实现该元素的拖动。</p>
<p>3、 如何来实现元素搬移过程中一直拥有焦点？因为要跨浏览器，所以就不再用setCapture之类的方法，这里换种思维，为什么元素元素搬移过程中没有了焦点呢，主要是我们把onmousemove事件注册到了被拖移的元素上，而这并不是必需的，所以当元素触发onmousedown事件后，我把onmousemove、onmouseup事件直接注册到document上，这样鼠标移到哪都会有焦点。</p>
<p>说完了这么多，直接看代码结构吧！</p>
<p> </p>
<div>
<pre>Drag = {
    obj: null,
    init: function (options) {
        options.handler.onmousedown = this.start;
        options.handler.root = options.root || options.handler;
		var root = options.handler.root;
        root.onDragStart = options.dragStart || new Function();
        root.onDrag = options.onDrag || new Function();
        root.onDragEnd = options.onDragEnd || new Function();
    },
    start: function (e) {//此时的this是handler
        var obj = Drag.obj = this;
        obj.style.cursor = 'move';
        e = e || Drag.fixEvent(window.event);
        ex = e.pageX;
        ey = e.pageY;
        obj.lastMouseX = ex;
        obj.lastMouseY = ey;
        var x = obj.root.offsetLeft;
        var y = obj.root..offsetTop;
		obj.root.style.left = x + "px";
obj.root.style.top = y + "px";
        document.onmouseup = Drag.end;
        document.onmousemove = Drag.drag;
        obj.root.onDragStart(x, y);
    },
    drag: function (e) {
        e = e || Drag.fixEvent(window.event);
        ex = e.pageX;
        ey = e.pageY;
        var root = Drag.obj.root;
        var x = root.style.left ? parseInt(root.style.left) : 0;
        var y = root.style.top ? parseInt(root.style.top) : 0;
        var nx = ex - Drag.obj.lastMouseX + x;
        var ny = ey - Drag.obj.lastMouseY + y;
        root.style.left = nx + "px";
        root.style.top = ny + "px";
        Drag.obj.root.onDrag(nx, ny);
        Drag.obj.lastMouseX = ex;
        Drag.obj.lastMouseY = ey;

    },
    end: function (e) {
        var x = Drag.obj.root.style.left ? parseInt(Drag.obj.root.style.left) : 0;
        var y = Drag.obj.root.style.top ? parseInt(Drag.obj.root.style.top) : 0;
        Drag.obj.root.onDragEnd(x, y);
        document.onmousemove = null;
        document.onmouseup = null;
        Drag.obj = null;
},
fixEvent: function (e) {
   e.pageX = e.clientX + document.documentElement.scrollLeft;
   e.pageY = e.clientY + document.documentElement.scrollTop;
   return e;
}
}

</pre>
</div>
<p> </p>
<p>上面init主要处理一些初如化工作，如记录onDragStart、onDrag、onDragEnd三个事件, handler为处理拖动事件的那个元素，root为被拖动的元素。</p>
<p>当在handler上点击后触发Drag.start方法，Drag.start主要为记录住鼠标相对整个页面的位置;给document注册onmouseup、onmousemove事件，及触发onDragStart事件。</p>
<p>Drag.drag方法也很简单，主要作用就是更新被搬移元素位置，同时记录住鼠标相对整个页面的位置。</p>
<p>Drag.end方法就更简单了，就是做一些收尾工作。</p>
<p> </p>
<p>最后给大家附段使用的代码吧,祝大家学习愉快！</p>
<p> </p>
<div>
<pre>Drag.init({
                handler: document.getElementById(&quot;handler&quot;),
				root:document.getElementById(&quot;root&quot;);
       });

&lt;div id=&quot;root&quot;&gt;
	&lt;h2 id=&quot;handler&quot;&gt;&lt;/h2&gt;
&lt;/div&gt;

</pre>
</div><img src="http://www.cnblogs.com/275095923/aggbug/1901352.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/275095923/">周尚武</a> 发表于 2010-12-09 15:32 <a href="http://www.cnblogs.com/275095923/archive/2010/12/09/1901352.html">原文链接</a></p><p>评论: 5　<a href="http://www.cnblogs.com/275095923/archive/2010/12/09/1901352.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/275095923/archive/2010/12/09/1901352.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/84129/">Facebook和Twitter关闭维基解密支持者账户</a><span style="color:gray">(2010-12-10 07:39)</span><br>· <a href="http://news.cnblogs.com/n/84128/">微软高管证实鲍尔默曾试图收购Facebook</a><span style="color:gray">(2010-12-10 07:39)</span><br>· <a href="http://news.cnblogs.com/n/84127/">商务社交网站XING收购在线活动平台Amiando</a><span style="color:gray">(2010-12-10 07:37)</span><br>· <a href="http://news.cnblogs.com/n/84126/">YouTube首次取消部分用户上传视频长度限制</a><span style="color:gray">(2010-12-10 07:36)</span><br>· <a href="http://news.cnblogs.com/n/84125/">Groupon创始人专访：上周增加300万用户</a><span style="color:gray">(2010-12-10 07:34)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/DSharp/archive/2010/12/09/1901208.html">Chrome OS，对程序员和Windows意味着什么？</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>