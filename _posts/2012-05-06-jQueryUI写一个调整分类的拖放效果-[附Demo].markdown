---
layout: post
title:  "jQueryUI写一个调整分类的拖放效果 [附Demo]"
date:   2012-05-06 01:51:37
author: Xiaoxia
categories: program
---

## jQueryUI写一个调整分类的拖放效果 [附Demo]
### by Xiaoxia
### at 2012-05-06 01:51:37
### original <http://www.udpwork.com/item/7249.html>

<p>最近，想用jQuery做一个网页的树目录结构，并且可以使用鼠标拖动调整选项的位置。我在网上找了一下插件，基本上看了好几款比较著名的，都觉得代码太复杂了或者界面太丑了等各种不符合我的要求。所以还是自己动手丰衣足食，还是坚持简单就是美的代码风格。</p>
<p><a href="http://xiaoxia.org/upfiles/2012/05/rr18.png"><img src="http://xiaoxia.org/upfiles/2012/05/rr18.png"></a></p>
<p></p>
<p><strong>Demo演示</strong>
</p>
<p>试试在iframe里嵌入一个测试页面，你可以使用鼠标拖动项目，调整分类：</p>
<p>
<br>
</p>
<p>当然，这并不是我要的最终效果，只能说它已经实现了一个我想要的拖放效果。我要求的在这个基础上，还要增加自动排序，位置变更后恢复之前元素bind的jQuery事件等。</p>
<p><strong>代码</strong>
</p>
<p>代码如下，如要复制，请先查看纯文本版本！</p>
<pre>
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Drag &amp; Drop Test&lt;/title&gt;
&lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot;&gt;
&lt;script type=&quot;text/javascript&quot;
src=&quot;https://readself.com/static/js/jquery.min.js?v=52337&quot;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot;
src=&quot;https://readself.com/static/js/jquery-ui.min.js?v=ab482&quot;&gt;&lt;/script&gt;
&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot;
href=&quot;https://readself.com/static/css/smoothness/jquery-ui.css?v=af3ef&quot; /&gt;
&lt;style type=&quot;text/css&quot;&gt;
li {cursor: pointer}
.menu_hover {background-color: #d0d0d0;}
#menu p{margin: 5px 0 5px 0;}
&lt;/style&gt;
&lt;body&gt;
&lt;ul id=&quot;menu&quot;&gt;
    &lt;li class=&quot;folder&quot;&gt;
        &lt;p&gt;Fruits&lt;/p&gt;
        &lt;ul&gt;
            &lt;li&gt;Apple&lt;/li&gt;
            &lt;li&gt;Pear&lt;/li&gt;
            &lt;li&gt;Banana&lt;/li&gt;
        &lt;/ul&gt;
    &lt;/li&gt;
    &lt;li class=&quot;folder&quot;&gt;&lt;p&gt;Vegetables&lt;/p&gt;
        &lt;ul&gt;
            &lt;li&gt;Tomato&lt;/li&gt;
            &lt;li&gt;Potato&lt;/li&gt;
            &lt;li&gt;Cucumber&lt;/li&gt;
        &lt;/ul&gt;
    &lt;/li&gt;
    &lt;li class=&quot;folder&quot;&gt;&lt;p&gt;Meet&lt;/p&gt;
        &lt;ul&gt;
            &lt;li&gt;Beaf&lt;/li&gt;
            &lt;li&gt;Pork&lt;/li&gt;
            &lt;li&gt;Chicken&lt;/li&gt;
        &lt;/ul&gt;
    &lt;/li&gt;
&lt;/ul&gt;

&lt;script&gt;
$(&#39;#menu li&#39;).disableSelection();
$(&#39;li&#39;, $(&#39;#menu ul&#39;)).draggable({revert: &#39;invalid&#39;, helper: &#39;clone&#39;});
$(&#39;#menu .folder&#39;).droppable({
    hoverClass: &quot;menu_hover&quot;,
    drop: function(event, ui){
        if(ui.draggable.parents(&#39;.folder&#39;).get(0) == $(this).get(0))
            return ;
        $(&#39;ul&#39;, this).append(ui.draggable.clone());
        ui.draggable.remove();
        $(&#39;li&#39;, this).draggable({remove: &#39;invalid&#39;, helper: &#39;clone&#39;});
    }
});
$(&#39;#menu .folder p&#39;).click(function(){
    $(this).next().toggle();
});
&lt;/script&gt;
</pre><p>在编写网页上我还是菜鸟，欢迎网页大牛指点迷津！！！</p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7249.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7249.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7249.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>