---
layout: post
title:  "李秉骏：在Phonegap下实现oAuth认证"
date:   2012-07-18 12:21:48
author: 伯乐
categories: program
---

## 李秉骏：在Phonegap下实现oAuth认证
### by 伯乐
### at 2012-07-18 12:21:48
### original <http://blog.jobbole.com/23875/?utm_source=rss&utm_medium=rss&utm_campaign=%25e6%259d%258e%25e7%25a7%2589%25e9%25aa%258f%25ef%25bc%259a%25e5%259c%25a8phonegap%25e4%25b8%258b%25e5%25ae%259e%25e7%258e%25b0oauth%25e8%25ae%25a4%25e8%25af%2581>

<p>前段时间做过两次关于Phonegap的现场交流会议分享。基本上把Phonegap的一些特性和大家交流了一下，大家对于Phonegap的 兴趣也是非常多的。但是因为Phonegap相对于原生应用来说，只有一个View，这个View就是一个Web的容器，这使得Phonegap就存在很 多限制。其中一部分的限制我们已经通过HTML5的API以及Phonegap为我们搭建的桥去实现了，另外一部分我们就得通过Phonegap plugins来实现，而实际上我个人认为Phonegap最强大的地方也在于有那么大的一个群体在为他提供各种各样的Plugins，以便去应对实际项 目中遇到的问题。</p>
<p>我记得在和大家交流的时候大家经常会问Phonegap如何做本地的缓存啊（WebSQL），如何贴近原生应用（这个涉及到架构，界面渲染问题，这 里我也不好深入讲，毕竟不是本文要讨论的内容），还有一个很头疼的问题就是如果要做一个开放平台的应用，如何实现oAuth认证。此前我也遇到过类似的一 些情况，当我再次遇到这个棘手的问题的时候，我相信一定也有Phonegap的粉丝遇到类似的情况的。于是我就总结下来何大家探讨一下如何解决这个问题 吧。</p>
<p>首先目标：解决oAuth认证。</p>
<p>制定计划：1. 知道oAuth原理；2.了解Phonegap在处理这个问题时候的运行机制；3. 制定计划实现代码。</p>
<p>下面我们就来一步一步地分析，看如何解决这个情况吧。（因为我在项目中遇到的是腾讯微博开放平台的oAuth认证，那么下面我就用腾讯oAuth认证为例吧）</p>
<p>关于oAuth认证，相信做过开放平台应用的朋友都已经非常熟悉了，如果你还没有做过或者对于oAuth认证流程非常不了解，那么我建议你先了解一 下原理，在这里我不希望花太多的篇幅去介绍这个东西，因为在很多地方都可以找到，下面我推荐两个地址方便大家去阅读，一定要阅读，这会对你理解下面的文章 有莫大的帮助的。</p>
<p>腾讯微博开放平台：<a href="http://wiki.open.t.qq.com/index.php/OAuth2.0%E9%89%B4%E6%9D%83">http://wiki.open.t.qq.com/index.php/OAuth2.0%E9%89%B4%E6%9D%83</a></p>
<p>新浪微博开放平台：<a href="http://open.weibo.com/wiki/Oauth2">http://open.weibo.com/wiki/Oauth2</a></p>
<p>当然在这里上面需要阐述说明的是oAuth认证机制是一个通用的手法，但是因为每个开放平台有自己的政策，因此可能在其中稍有改变，而且最终获得的权限也会各不相同。而最近新浪微博实在太多坑爹的事情了，实在忍受不了，于是我转战到腾讯的平台了。</p>
<p>好的，如果你看完了oAuth认证的流程，就直接到这里来。众所周知，在oAuth认证的流程中，有一个授权页面，而这个授权页面是通过开放平台提供的，具体的样式见下图：</p>
<p><img src="http://cashwordpress-wordpress.stor.sinaapp.com/uploads/2012/07/oauth-page.png" alt=""></p>
<p>这个页面用于输入开放平台的帐户以及密码，通过授权获取响应的openid以及openkey，最终换取access-token（待会我会结合腾讯微博oAuth认证流程的特点，以及代码和大家分析的）。</p>
<p>这个页面是弹出的，如果在Phonegap里面做的话会很奇怪：1,因为属于弹窗，在Phonegap中本身就是一个WebView如果你还弹的话 就会飞到了Safari的<span><a href="http://blog.jobbole.com/12749/" title="浏览器">浏览器</a></span>中，这就跳出应用本身，跳出去认证还有戏吗？2,通过iFrame，首先体验非常不好，其次iFrame本身又属于跨域的 问题，这就不好解决了（为什么体验不好呢，主要是因为授权页面的样式是不固定的，类似腾讯微博开放平台，就比新浪的授权页面做得差，根本不和手机匹配的， 而且有些做得好的，认证页宽度就是320px，就占了你整个应用的版面，体验很不好）那么Phonegap中该如何实现呢？</p>
<p>带着问题，我们就希望在Phonegap中再度引入一个WebView。刚刚提到Phonegap的强大还在于很多人以及官方的团队，为其提供了一 套很好的插件机制，以解决各种各样的需要。在Phonegap中有一个插件叫做ChildBrowser，顾名思义就是：子浏览器。（其实我在上两次的 Phonegap专题技术分享中以及提及到让大家用这个东西去解决，不过当时分享时间有限只能够草率带过，抱歉）子浏览器的作用其实就是让你在 Phonegap应用内部调起一个浏览器的View，让你进行pdf，图片，视频，甚至网页阅读的工具。（实际上你看我上面的截图，就是用 ChildBrowser来实现的）这就好了，这就可以让你调起浏览器而且不跳出应用本身了，可以很好地解决oAuth认证的问题。 ChildBrowser下载地址：</p>
<p><a href="https://github.com/phonegap/phonegap-plugins/tree/master/iOS/ChildBrowser">https://github.com/phonegap/phonegap-plugins/tree/master/iOS/ChildBrowser</a></p>
<p>在地址上面，你应该已经看到ChildBrowser的安装方法以及使用方法了，非常简单，真正的即插即用。如果你觉得英文太难，这我就帮不了你 了，你就自行Google翻译一下吧。相信你很快就可以做出一个ChildBrowser的Demo的。在这个地址上面，其实你返回上一层目录，其实你也 已经看到各式各样的Phonegap Plugins，通过这些东西，你还可以调用起手机内部更多有趣的资源的！这个就要靠你自己去发掘啦！（其他平台的应用也有相应的插件的Android开 发者不要骂果粉哦！）</p>
<p><a href="https://github.com/phonegap/phonegap-plugins/tree/master/iOS">https://github.com/phonegap/phonegap-plugins/tree/master/iOS</a></p>
<p>好了慢慢地我们就要涉及到代码部分了。首先我们看看调用ChildBrowser的Javascript代码：</p>
<pre>        cb = window.plugins.childBrowser;
/*
        if(cb != null) {
        cb.onLocationChange = function(loc){ root.locChanged(loc); };//地址发生改变时候执行的函数
        cb.onClose = function(){root.onCloseBrowser(); };//通过js关闭ChildBrowser的办法
        cb.onOpenExternal = function(){root.onOpenExternal(); };
*/
        cb.showWebPage(&quot;http://google.com&quot;);</pre>
<p>其中cb就是初始化的ChildBrowser，而showWebPage就是调起这个页面的方法。可见代码中要打开的网址就是 Google.com啦，这个地球人都能够看得懂了。于是我们就可以马上想到我们要用ChildBrowser打开的网址是我们在网上指定的应用授权站点 了。而我是部署在SAE上面的，所以下面的例子也用PHP来说吧，期待语言也是相同的道理，转义就可以了。在说代码之前，我们先来说说具体通讯的流程，以 及我们接下来要达到的目标。</p>
<p style="text-align:left"><img src="http://cashwordpress-wordpress.stor.sinaapp.com/uploads/2012/07/%E6%89%8B%E6%9C%BAApp.png" alt="" width="498" height="235"><br>
在这里，我们的手机端是通过访问SAE服务器，由SAE服务器管理数据并与腾讯微博开放平台通讯的，这里手机端并没有直接和腾讯微博开放平台通讯（我这样 处理的原因是1,方便在服务器端管理帐户，这样的话可以观察自己的应用的帐户状况；2,服务器端实现推送机制，方便管理token以及做api；3,服务 器端还可以和其他开放平台帐户绑定）。因此，我们的整套认证方案会在服务器端完成。</p>
<p>而根据腾讯微博开放平台，我们首先会在开放平台上面注册自己的应用，注册的流程以及办法我不说了，注册的地址是：<a href="http://dev.open.t.qq.com/development/">http://dev.open.t.qq.com/development/</a>，注册你的应用后，你相应能够获得的东西是：</p>
<pre>应用名称：mobile_test_api
应用类型：客户端应用
App Key：88888888
App Secret：ainidenideiienfeomeomroemrome</pre>
<p>在这里我的App key以及App Secret是假的（你懂的，你应该有你自己的），下面我们就使用腾讯提供给我们的PHP SDK，下载地址：<a href="http://wiki.open.t.qq.com/index.php/SDK%E4%B8%8B%E8%BD%BD">http://wiki.open.t.qq.com/index.php/SDK%E4%B8%8B%E8%BD%BD</a>。有了SDK后我们就可以把SDK放到自己的环境上面，然后配置服务器端的代码了。下图是我简单配置的服务端的代码，lib下存放的就是腾讯微博的sdk。当然实际生产环境和这个有不同。这里仅仅作为演示使用：</p>
<p style="text-align:center"><a href="http://cashwordpress-wordpress.stor.sinaapp.com/uploads/2012/07/tencent-sdk.png" rel="lightbox[23875]" title="tencent-sdk"><img title="tencent-sdk" src="http://cashwordpress-wordpress.stor.sinaapp.com/uploads/2012/07/tencent-sdk.png" alt="" width="479" height="396"></a></p>
<p>下面就根据腾讯微博认证的流程，逐一讲解一下这些文件以及内部的代码吧。</p>
<p>index.php</p>
<pre>&lt;?php
require_once &#39;app_config.php&#39;;

$url=&quot;https://open.t.qq.com/cgi-bin/oauth2/authorize?client_id=&quot;.$client_id.&quot;&amp;APP_KEY=&quot;.$app_key.&quot;&amp;wap=2&amp;response_type=code&amp;redirect_uri=http://yoururl.com/get_auth.php&quot;;//指定URL地址

header(&#39;Location:&#39;.$url);</pre>
<p>这里引入的app_config.php</p>
<pre>&lt;?php

$client_id = &#39;8888888888&#39;;

$app_key = &#39;anienineiienrieireowq2839289&#39;;</pre>
<p>因为根据腾讯微博开放平台，我们第一步要获取的是Code，如下所述，我们要做的就是做好配置，获取这个Code</p>
<pre>第一步：请求code

请求方法：
GET

请求地址：

https://open.t.qq.com/cgi-bin/oauth2/authorize?client_id=APP_KEY&amp;response_type=code&amp;redirect_uri=http://www.myurl.com/example

返回结果：
如果授权成功，授权服务器会将用户的浏览器重定向到redirect_uri，并带上code，openid和openkey等参数，重定向的url如下：

http://www.myurl.com/example?code=CODE&amp;openid=OPENID&amp;openkey=OPENKEY</pre>
<p>具体需要请求附带的参数，必须要按照oAuth2.0鉴权的页面提示的进行。（<a href="http://wiki.open.t.qq.com/index.php/OAuth2.0%E9%89%B4%E6%9D%83">http://wiki.open.t.qq.com/index.php/OAuth2.0%E9%89%B4%E6%9D%83</a>）</p>
<p>然后我们再来看看我们所配置的文件：</p>
<p>get_auth.php</p>
<pre>&lt;?php

require_once &#39;app_config.php&#39;;

$code = $_REQUEST[&#39;code&#39;];

$openid = $_REQUEST[&#39;openid&#39;];

$openkey = $_REQUEST[&#39;openkey&#39;];

$url = &quot;https://open.t.qq.com/cgi-bin/oauth2/access_token?client_id=&quot;.$client_id.&quot;&amp;client_secret=&quot;.$app_key.&quot;&amp;grant_type=authorization_code&amp;code=&quot;.$code.&quot;&amp;redirect_uri=http://yoururl.com/get_auth.php&quot;;

$message = file_get_contents($url);

/* success to print the access token message */ 

$access = explode(&quot;=&quot;,$message);

print_r(&quot;&lt;br /&gt;&quot;);

$access_message = explode(&quot;&amp;&quot;,$access[1]);

$access_token = $access_message[0];

$user_name = $access[4];

print_r($access_token .&quot;   &quot; . $user_name);</pre>
<p>其实到以上为止，我们的配置文件已经弄好了。在这个配置文件中，实际上我们要做的就是腾讯微博开放平台中提及的第二步：</p>
<pre>第二步：请求accesstoken

请求地址：

https://open.t.qq.com/cgi-bin/oauth2/access_token?client_id=APP_KEY&amp;client_secret=APP_SECRET&amp;redirect_uri=http://www.myurl.com/example&amp;grant_type=authorization_code&amp;code=CODE

返回结果：
返回字符串：
access_token=ACCESS_TOKEN&amp;expires_in=60&amp;refresh_token=REFRESH_TOKEN</pre>
<p>如果你现在已经配置好你的服务端口，已经配置好手机端的ChildBrowser，你就已经可以在手机上面看看整个认证的流程了。现在的工作已经完 成了大部分了，不过细心的朋友可能会发现，对啊，认证是完成了，手机上还是没有获得授权啊，因为授权后的信息还不能够通过手机去获取。不要 急，ChildBrowser有趣的地方还没有完呢。在手机端上面我们完成了oAuth认证，总有一些参数返回，不管accesstoken是否存在手机 端，你总得有个帐户机制和服务端通讯。我的服务端在SAE上面，我就建立一个唯一<em>id</em>给手机，于是我就建立了一个帐户机制，存在服务端上，服务端上存储的东西是：</p>
<pre>CREATE TABLE IF NOT EXISTS `auth_user` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `muser` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `access_token` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `openid` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `openkey` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM  DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci AUTO_INCREMENT=1 ;</pre>
<p>其实这个表也还没有完善，我必须还要纪录这个用户是否在线（如果有推送机制）。此后手机端和服务端通讯就通过上面的id以及token等的信息进行 通讯，再通过服务端想腾讯微博进行api的通讯，获取我们想要的信息。那么认证后我们通过什么途径拿到认证后返回的信息呢？大家看看上面JS控制 ChildBrowser的代码。会发现里面有一个方法：</p>
<pre>cb.onLocationChange = function (loc){ console.warn(loc); };</pre>
<p>如果你在xCode上面跑这段代码的话，你会发现loc打出来的是每次ChildBrowser里面浏览的网页改变的地址。这个时候我们就可以顺水 推舟，根据这里提供的办法，用url的方式把地址返回到Phonegap负责逻辑处理的JS代码中，同时将相关需要通讯的信息也返回。返回后还可以通过回 调的方式执行关闭ChildBrowser的代码：</p>
<pre>cb.close();</pre>
<p>当然，你还可以执行更多异步请求的代码。至于说还可以通过怎么样的方式进行通讯其实还有很多办法，我这里仅仅是提供一下思路引导以及方法。具体的 话，还要实践出真理论，做到非常安全的通讯还值得我们继续深入探讨。那么我要介绍的大概就到这里为止。因为实际项目中我们还有push notification的机制，所以此后我应该还会联同<a href="http://weibo.com/jeffjie">@Jeff_Kit</a> <a href="http://www.jeffkit.info/">http://www.jeffkit.info/</a> 实现一下Phonegap的推送方案，并整理出sdk，成文后开放出来方便大家交流。</p>
<p>对于本文如果有什么疑问或者建议都可以直接向我反馈，我的新浪微博是：<a href="http://weibo.com/lbj96347">@CashLee李秉骏</a> ，我还经常分享一些代码片段在<span><a href="http://blog.jobbole.com/6492/" title="GitHub如何运作：时间并不决定一切">GitHub</a></span>上面（开源的精力不多，所以开源项目较少，希望日后增加吧。）我的<a href="http://github.com/lbj96347">Github账号</a>， 欢迎您和我随时进行交流，也希望Phonegap的中国开发者社区会变得越来越精彩。</p>
<p>注意：ChildBrowser控件在实际环境中因为安全问题可能需要修改，通讯过程中参数也建议加密。:-)</p>
<p> </p>
<p>本文由李秉骏（<a href="http://weibo.com/lbj96347">@CashLee李秉骏</a>）投稿于<a title="伯乐" href="http://www.jobbole.com/">伯乐</a>在线，也欢迎其他朋友<a href="http://blog.jobbole.com/tougao/">投稿</a>。提示：投稿时记得留下微博账号哦 <img src="http://blog.jobbole.com/wp-includes/images/smilies/icon_smile.gif" alt=":-)"></p>
<p><span style="color:#ff0000">【如需转载，请标注并保留原文链接和作者等信息，谢谢合作！】</span></p>
<p> </p>
<p> </p>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/17368/" title="PhoneGap开发不可或缺的五件装备">PhoneGap开发不可或缺的五件装备</a></li><li><a href="http://blog.jobbole.com/22883/" title="百行 HTML5 代码实现四种双人对弈游戏">百行 HTML5 代码实现四种双人对弈游戏</a></li><li><a href="http://blog.jobbole.com/22231/" title="HTML5 基础知识 – 第 1 部分">HTML5 基础知识 – 第 1 部分</a></li><li><a href="http://blog.jobbole.com/19598/" title="Adobe、标准和HTML5">Adobe、标准和HTML5</a></li><li><a href="http://blog.jobbole.com/19321/" title="HTML5+CSS3+jQuery制作视频播放器完全指南">HTML5+CSS3+jQuery制作视频播放器完全指南</a></li><li><a href="http://blog.jobbole.com/18012/" title="说说JSON和JSONP">说说JSON和JSONP</a></li><li><a href="http://blog.jobbole.com/13612/" title="开发者须知 HTML5 的15个新特性">开发者须知 HTML5 的15个新特性</a></li><li><a href="http://blog.jobbole.com/13522/" title="Google Web应用开发指南第一章：什么是Web应用？">Google Web应用开发指南第一章：什么是Web应用？</a></li><li><a href="http://blog.jobbole.com/13278/" title="10个出色的HTML5画布示例">10个出色的HTML5画布示例</a></li><li><a href="http://blog.jobbole.com/12588/" title="信息图：HTML5在游戏领域不敌Flash">信息图：HTML5在游戏领域不敌Flash</a></li></ul>