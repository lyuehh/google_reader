---
layout: post
title:  "一步一步制作jquery插件Tabs（ajax只请求一次效果，78行完成）"
date:   2010-07-06 10:43:00
author: 黑曜石
categories: program
---

## 一步一步制作jquery插件Tabs（ajax只请求一次效果，78行完成）
### by 黑曜石
### at 2010-07-06 10:43:00
### original <http://www.cnblogs.com/bestfc/archive/2010/07/06/1771935.html>

<p>作者: <a href="http://www.cnblogs.com/bestfc/">黑曜石</a> 发表于 2010-07-06 10:43 <a href="http://www.cnblogs.com/bestfc/archive/2010/07/06/1771935.html">原文链接</a> 阅读: 1946 评论: 6</p><p>      tabs是现在网页应用最广的一种效果，jquery插件和非jquery插件也有不少，有一些朋友问我怎么用jquery.ui.tabs的ajax怎么只请求服务器一次</p>
<p>原来我想其实很简单，看看官方的API就了解，不过我在回复这些朋友之前，用firebug查看了官方的ui.tabs发现，声明了ajax缓存，每点一个tabs时，仍然会有服务器请求</p>
<p>这应该是服务器缓存，而不是实际上我们要求的只ajax一次，不再请求服务器了</p>
<p>接下来我找了一下其它的tabs插件，基本上没有符合要求的，不是太庞大就是太简单，太过庞大的话不如用ui.tabs，文档和代码规范上都是可靠的</p>
<p>因此，自制一个简洁的tabs插件还是有必要的</p>
<p>在设计之前，先整理好思路，实现tabs，自动轮换，ajax等主要功能，然后是dom的排列形式，这里采用传统的</p>
<p>&lt;div id=&quot;tabs&quot;&gt;</p>
<p>　　&lt;ul&gt;</p>
<p>　　　　&lt;li&gt;&lt;a href=&quot;#tabs1&quot;&gt;tabs1&lt;/a&gt;&lt;/li&gt;</p>
<p>　　　　&lt;li&gt;&lt;a href=&quot;#tabs2&quot; rel=&quot;ajax.htm&quot;&gt;tabs2&lt;/a&gt;&lt;/li&gt;</p>
<p>　　&lt;/ul&gt;</p>
<p>　　&lt;div id=&quot;tabs1&quot;&gt;Hello World!&lt;/div&gt;</p>
<p>　　&lt;div id=&quot;tabs2&quot;&gt;&lt;/div&gt;</p>
<p>&lt;/div&gt;</p>
<p>一个li对应一个div的方式，当ajax时，添加一个a的rel属性，并将内容写入对应的div中，再去掉rel属性，这样就只请求服务器一次，接下来都是div已经写入的内容了</p>
<p>我这里没有使用cookie，可以结合jquery.cookie插件，这样即使用户关闭网页下次再打开，也不用请求服务器了</p>
<p>一，首先写个jquery插件的闭包，园子里这两天有个朋友写了javascript的闭包概念，挺好的，有兴趣的朋友去看看</p>
<pre>(function ($) {
//code here
})(jQuery);</pre>
<p>二，插件命名，这里命名为aTabs，这样绑定的时候可以用$(...).aTabs()，本人英文名Allen，所以用a字头命名了~</p>
<pre>$.fn.aTabs = function (options) {
//api
//main function
}</pre>
<p>三，把想好的功能写成API，供外部修改</p>
<pre>$.fn.aTabs.defaults = {
            firstOn: 0,
            className: 'selected',
            eventName: 'all',           //click,mouserover,all
            loadName: '加载中...',     //ajax等待字符串
        fadeIn: 'normal',
            autoFade: false,
            autoFadeTime: 3
        };
var opts = $.extend({}, $.fn.aTabs.defaults, options); //这里可以将外部输入的代替掉默认的值，$.extend作用详见 <a href="http://api.jquery.com/jQuery.extend/">http://api.jquery.com/jQuery.extend/</a>，看不懂英文的直接看其中的例子就行</pre>
<p>四，编写主体功能，说明在代码中看注释</p>
<pre>return this.each(function () { //这里为每个绑定dom插件
            var target = $(this);
            var div = target.children().not(&quot;ul,span&quot;);  //所有的tabs显示体div
            var tabs = target.find(&#39;ul:eq(0) li&#39;);   //所有的tabs头部索引
            function Tabs() {
                if ($(this).hasClass(opts.className)) {
                    return false;
                }
                tabsShow(div, $(this));
                return false;
            }

            function tabsShow(div, li, index) {
                div.stop(true, true).hide();
                //自动轮换用
                if (typeof (index) == &quot;number&quot;) {
                    if (li.find(&quot;a&quot;).attr(&quot;rel&quot;)) ajax(div, li);
                    $(div[index]).stop(true,true).fadeIn(opts.fadeIn);
                    tabs.stop(true, true).removeClass(opts.className);
                    $(tabs[index]).stop(true, true).addClass(opts.className);
                }
                //非自动轮换
                else {
                    var tabBody = div.filter(li.find(&quot;a&quot;).attr(&quot;href&quot;));
                    if (li.find(&quot;a&quot;).attr(&quot;rel&quot;)) ajax(div, li);
                    tabBody.stop(true,true).fadeIn(opts.fadeIn);
                    tabs.stop(true, true).removeClass(opts.className);
                    li.stop(true, true).addClass(opts.className);
                }
            }

            function ajax(div, li) {　　//这里是关键ajax，通过操作rel的方式实现只请求服务器一次
                var href = li.find(&quot;a&quot;).attr(&quot;href&quot;);
                var rel = li.find(&quot;a&quot;).attr(&quot;rel&quot;);     //ajax请求url
                var i = div.filter(href);                 //当前div
                if (rel) {                                      //如果ajax请求url不为空，只ajax一次
                    i.html(opts.loadName);
                    $.ajax({
                        url: rel,
                        cache: false,
                        success: function (html) {
                            i.html(html);
                        },
                        error: function () {
                            i.html(&#39;加载错误，请重试！&#39;);
                        }
                    });
                    li.find(&quot;a&quot;).removeAttr(&quot;rel&quot;);  //只ajax一次
                }
            }
            if (opts.autoFade) {
                var index = opts.firstOn + 1;
                setInterval(function () {
                    if (index &gt;= div.length) {
                        index = 0;
                    }
                    tabsShow(div, $(this), index++);
                }, opts.autoFadeTime * 1000);
            }

            tabs.bind(opts.eventName == &#39;all&#39; ? &#39;click mouseover&#39; : opts.eventName, Tabs)   //绑定事件
                .filter(&#39;:first&#39;).trigger(opts.eventName == &#39;all&#39; ? &#39;click&#39; : opts.eventName);          //自动触发事件
        });
</pre>
<p>最后，将以上整合，tabs插件就诞生了，下面是全部源码：</p>
<pre>/*
* 作者：黑曜石
*/
(function ($) {
    $.fn.aTabs = function (options) {
        $.fn.aTabs.defaults = {
            firstOn: 0,
            className: &#39;selected&#39;,
            eventName: &#39;all&#39;,           //click,mouserover,all
            loadName: &#39;加载中...&#39;,     //ajax等待字符串
            fadeIn: &#39;normal&#39;,
            autoFade: false,
            autoFadeTime: 3
        };
        var opts = $.extend({}, $.fn.aTabs.defaults, options);

        return this.each(function () {
            var target = $(this);
            var div = target.children().not(&quot;ul,span&quot;);  //所有的tabs显示体div
            var tabs = target.find(&#39;ul:eq(0) li&#39;);   //所有的tabs头部索引
            function Tabs() {
                if ($(this).hasClass(opts.className)) {
                    return false;
                }
                tabsShow(div, $(this));
                return false;
            }

            function tabsShow(div, li, index) {
                div.stop(true, true).hide();
                //自动轮换用
                if (typeof (index) == &quot;number&quot;) {
                    if (li.find(&quot;a&quot;).attr(&quot;rel&quot;)) ajax(div, li);
                    $(div[index]).stop(true,true).fadeIn(opts.fadeIn);
                    tabs.stop(true, true).removeClass(opts.className);
                    $(tabs[index]).stop(true, true).addClass(opts.className);
                }
                //非自动轮换
                else {
                    var tabBody = div.filter(li.find(&quot;a&quot;).attr(&quot;href&quot;));
                    if (li.find(&quot;a&quot;).attr(&quot;rel&quot;)) ajax(div, li);
                    tabBody.stop(true,true).fadeIn(opts.fadeIn);
                    tabs.stop(true, true).removeClass(opts.className);
                    li.stop(true, true).addClass(opts.className);
                }
            }

            function ajax(div, li) {
                var href = li.find(&quot;a&quot;).attr(&quot;href&quot;);
                var rel = li.find(&quot;a&quot;).attr(&quot;rel&quot;);     //ajax请求url
                var i = div.filter(href);                 //当前div
                if (rel) {                                      //如果ajax请求url不为空，只ajax一次
                    i.html(opts.loadName);
                    $.ajax({
                        url: rel,
                        cache: false,
                        success: function (html) {
                            i.html(html);
                        },
                        error: function () {
                            i.html(&#39;加载错误，请重试！&#39;);
                        }
                    });
                    li.find(&quot;a&quot;).removeAttr(&quot;rel&quot;);  //只ajax一次
                }
            }
            if (opts.autoFade) {
                var index = opts.firstOn + 1;
                setInterval(function () {
                    if (index &gt;= div.length) {
                        index = 0;
                    }
                    tabsShow(div, $(this), index++);
                }, opts.autoFadeTime * 1000);
            }

            tabs.bind(opts.eventName == &#39;all&#39; ? &#39;click mouseover&#39; : opts.eventName, Tabs)   //绑定事件
                .filter(&#39;:first&#39;).trigger(opts.eventName == &#39;all&#39; ? &#39;click&#39; : opts.eventName);          //自动触发事件
        });
    };
})(jQuery);
</pre>
<p> </p><img src="http://www.cnblogs.com/bestfc/aggbug/1771935.html?type=1" width="1" height="1" alt=""><p>评论: 6　<a href="http://www.cnblogs.com/bestfc/archive/2010/07/06/1771935.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/bestfc/archive/2010/07/06/1771935.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/enterprise/2453/">百度期待您的加盟</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/67697/">Meego平台必知的常识 你知道多少？</a><span style="color:gray">(2010-07-06 22:16)</span><br>· <a href="http://news.cnblogs.com/n/67696/">Twitter或将推新服务拓展营收模式</a><span style="color:gray">(2010-07-06 22:11)</span><br>· <a href="http://news.cnblogs.com/n/67695/">腾讯公告称回购100万股涉资1.3亿港元</a><span style="color:gray">(2010-07-06 22:10)</span><br>· <a href="http://news.cnblogs.com/n/67694/">苹果谷歌Hulu抢夺电视广告市场 局面尚不明朗</a><span style="color:gray">(2010-07-06 21:47)</span><br>· <a href="http://news.cnblogs.com/n/67693/">乔布斯的iPhone 4 再一次改变一切？</a><span style="color:gray">(2010-07-06 21:04)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/topic/53/">C#大论战：在讨论中学习</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>