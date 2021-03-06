---
layout: post
title:  "jQuery Timeline"
date:   2012-04-03 11:17:34
author: Airen
categories: program
---

## jQuery Timeline
### by Airen
### at 2012-04-03 11:17:34
### original <http://www.w3cplus.com/jquery/jquery-timeline>

<div><div><div><p><a href="http://timeline.verite.co/">Timeline</a>是<a href="https://github.com/VeriteCo">VeriteCo</a>写的一个<a href="http://jquery.com">jQuery</a>插件。它将一连串的事件集合到一起，按时间轴的顺序展示这些发生的事件。你可以将各种媒体、图片、视频、地图等相关信息插入到timeline。结合设计可以完美的展示你的工作效率或者更强的信息。比如说你一年，或者多年下来人生中的大事等。</p>
<p><a href="https://github.com/VeriteCo">VeriteCo</a>的<a href="http://timeline.verite.co/">Timeline</a>与<a href="http://www.facebook.com">Facebook</a>的<a href="http://www.w3cplus.com/jquery/facebook-timeline-design-using-jquery">timeline</a>都有类似功能之处。前面在<a href="http://www.w3cplus.com/jquery/facebook-timeline-design-using-jquery">jQuery制作Facebook Timeline</a>一文中学习了<a href="http://www.facebook.com">Facebook</a>的<a href="http://www.w3cplus.com/jquery/facebook-timeline-design-using-jquery">timeline</a>使用。今天主要想让大家和我一起跟着<a href="http://timeline.verite.co/">VeriteCo</a>和<a href="http://tutorialzine.com/2012/04/timeline-portfolio/">Martin Angelov </a>学习如何使用timeline。</p>
<p><a href="http://www.w3cplus.com/w3cplusDemo/demos/timeline/index.html"><img alt="" src="http://www.w3cplus.com/w3cplusDemo/images/timeline.jpg"></a></p>
<h4>准备工作</h4>
<p>要成功的使用<a href="http://timeline.verite.co/">Timeline</a>，首先需要做好以下的准备工作：</p>
<ol><li>jQuery版本库：可以到<a href="http://jquery.com">官网</a>下载最新版本库；</li>
<li>timeline插件：点击<a href="http://veritetimeline.appspot.com/latest/timeline-min.js">下载</a>；</li>
<li>timeline样式：点击<a href="http://veritetimeline.appspot.com/latest/timeline.css">下载</a>，当然样式大家可以在自己的项目中，使用覆盖的方式去覆盖这个样式文件；</li>
<li>json文件：这个文件大家需要根据自己的需求去下写了（我也不懂，所以我使用了一个<a href="http://veritetimeline.appspot.com/latest/examples/kitchen-sink.json">在线</a>的json文件）,当然大家还可以使用<a href="https://docs.google.com/a/digitalartwork.net/previewtemplate?id=0AppSVxABhnltdEhzQjQ4MlpOaldjTmZLclQxQWFTOUE&amp;mode=public">Google Doc Template</a>，说细的介绍点击<a href="http://timeline.verite.co/#fileformat">这里</a>。</li>
</ol><p>大家也可以从<a href="https://github.com">github</a>中<a href="https://github.com/VeriteCo/Timeline">下载</a>所需文件，当然还可以到<a href="http://timeline.verite.co/">官网</a>点击<a href="https://github.com/VeriteCo/Timeline/zipball/master">Download</a>按钮下载。文件下载到本地后进行解压缩后就可以按照下面的方法进行使用了。</p>
<h4>HTML　Markup</h4>
<p>timeline带有一个默认的主题，当然大家可以根据自己的设计需求进行主题的覆写。</p>
<p>首先，让我们看看首页的基本模板</p>
<pre>		&lt;body&gt;
			&lt;div id=&quot;timeline&quot;&gt;
				 &lt;!-- Timeline will generate additional markup here --&gt;
			&lt;/div&gt;
		&lt;/body&gt;
	</pre><p>如果你只想让整个页面是timeline效果，在body中放一个div#timeline容器就够了，这里需要特别声明一点“<strong style="color:red">使用这个timeline必需要这个id为‘timeline’，不然将会失去效果。</strong>”如果你需要其他的效果，可以在div#timeline添加别的类名，或者在其外部添加别的容器。</p>
<p>接着在&lt;head&gt;中载入你下载的timeline.css样式文件:</p>
<pre>	&lt;!-- The default timeline stylesheet --&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;assets/css/timeline.css&quot; /&gt;
	</pre><p>你也可以加载在线样式文件：</p>
<pre>		&lt;!-- The default timeline stylesheet --&gt;
		&lt;link rel=&quot;stylesheet&quot; href=&quot;http://veritetimeline.appspot.com/latest/timeline.css&quot; /&gt;
	</pre><p>不过加载在线样式文件的风险大家知道的，我就不多说了。</p>
<p>样式加载完后，其后在你的&lt;body&gt;底部加载js文件：</p>
<pre>	 &lt;!-- JavaScript includes - jQuery, turn.js and our own script.js --&gt;
   &lt;script src=&quot;http://code.jquery.com/jquery-1.7.1.min.js&quot;&gt;&lt;/script&gt;
   &lt;script src=&quot;assets/js/timeline-min.js&quot;&gt;&lt;/script&gt;
	</pre><p>这两件文件也和样式文件一样，可以加载在线的文件：</p>
<pre>	 &lt;!-- JavaScript includes - jQuery, turn.js and our own script.js --&gt;
   &lt;script src=&quot;http://code.jquery.com/jquery-1.7.1.min.js&quot;&gt;&lt;/script&gt;
   &lt;script src=&quot;http://veritetimeline.appspot.com/latest/timeline-min.js&quot;&gt;&lt;/script&gt;
	</pre><p>最后启用这个插件：</p>
<pre>		&lt;script&gt;
			$(document).ready(function() {
				var timeline = new VMM.Timeline();
				timeline.init(&quot;your_data.json&quot;);			
			});
		&lt;/script&gt;
	</pre><p>此时你将看到timline在默认主题下的一个效果了。</p>
<p><a href="http://timeline.verite.co/examples/houston/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/timenline.jpg"></a></p>
<p>大家都看到了上面的效果了，当我们启用了timeline.js后，就会搜索文档中的div#timeline容器，并按下面的模板格式将json中的数据加载到div#timeline容器中：</p>
<pre>	&lt;div class=&quot;container main&quot; id=&quot;timeline&quot;&gt;
	&lt;div class=&quot;feature slider&quot; style=&quot;overflow-y: hidden;&quot;&gt;
		&lt;div class=&quot;slider-container-mask slider-container slider-item-container&quot;&gt;

			&lt;!-- The divs below are the events of the timeline --&gt;

			&lt;div class=&quot;slider-item content&quot;&gt;
				&lt;div class=&quot;text container&quot;&gt;

					&lt;h2 class=&quot;start&quot;&gt;Johnny B Goode&lt;/h2&gt;
					&lt;p&gt;&lt;em&gt;&lt;span class=&quot;c1&quot;&gt;Designer&lt;/span&gt; &amp; &lt;span class=
					&quot;c2&quot;&gt;Developer&lt;/span&gt;&lt;/em&gt;&lt;/p&gt;

				&lt;/div&gt;

				&lt;div class=&quot;media media-wrapper media-container&quot;&gt;
					&lt;!-- Images or other media go here --&gt;
				&lt;/div&gt;
			&lt;/div&gt;

			&lt;div class=&quot;slider-item content content-container&quot;&gt;
				&lt;div class=&quot;text container&quot;&gt;

					&lt;h2 class=&quot;date&quot;&gt;March 2009&lt;/h2&gt;
					&lt;h3&gt;My first experiment in time-lapse photography&lt;/h3&gt;
					&lt;p&gt;Nature at its finest in this video.&lt;/p&gt;

				&lt;/div&gt;

				&lt;div class=&quot;media media-wrapper media-container&quot;&gt;
					&lt;!-- Images or other media go here --&gt;
				&lt;/div&gt;
			&lt;/div&gt;

			&lt;!-- More items go here --&gt;
		&lt;/div&gt;

        &lt;!-- Next arrow --&gt;
		&lt;div class=&quot;nav-next nav-container&quot;&gt;
			&lt;div class=&quot;icon&quot;&gt;&lt;/div&gt;
			&lt;div class=&quot;date&quot;&gt;March 2010&lt;/div&gt;
			&lt;div class=&quot;title&quot;&gt;Logo Design for a pet shop&lt;/div&gt;
		&lt;/div&gt;

        &lt;!-- Previous arrow --&gt;
		&lt;div class=&quot;nav-previous nav-container&quot;&gt;
			&lt;div class=&quot;icon&quot;&gt;&lt;/div&gt;
			&lt;div class=&quot;date&quot;&gt;July 2009&lt;/div&gt;
			&lt;div class=&quot;title&quot;&gt;Another time-lapse experiment&lt;/div&gt;
		&lt;/div&gt;
	&lt;/div&gt;

	&lt;div class=&quot;navigation&quot;&gt;

			&lt;!-- The navigation items go here (the tooltips in the bottom)
                one for each of the events --&gt;

			&lt;div class=&quot;time&quot;&gt;
				&lt;!-- The timeline numbers go here --&gt;
			&lt;/div&gt;
		&lt;/div&gt;

		&lt;div class=&quot;timenav-background&quot;&gt;
			&lt;div class=&quot;timenav-line&quot; style=&quot;left: 633px;&quot;&gt;&lt;/div&gt;

			&lt;div class=&quot;timenav-interval-background top-highlight&quot;&gt;&lt;/div&gt;
		&lt;/div&gt;

		&lt;div class=&quot;toolbar&quot; style=&quot;top: 27px;&quot;&gt;
			&lt;div class=&quot;back-home icon&quot; title=&quot;Return to Title&quot;&gt;&lt;/div&gt;
			&lt;div class=&quot;zoom-in icon&quot; title=&quot;Expand Timeline&quot;&gt;&lt;/div&gt;
			&lt;div class=&quot;zoom-out icon&quot; data-original-title=&quot;Contract Timeline&quot;&gt;&lt;/div&gt;
		&lt;/div&gt;
	&lt;/div&gt;
&lt;/div&gt;
	</pre><h4>CSS Code</h4>
<p>上面展示的是timeline的一个默认主题样式，下面我们一起来看看<a href="http://tutorialzine.com/2012/04/timeline-portfolio/">Martin Angelov</a>是如何覆写主题样式。</p>
<p>首先需要创建一个新的主题样式文件“style.css”并且加载到项目中，下面我们具体来看是怎样一步一步覆写主题：</p>
<p><strong>1、修改timeline的背景</strong></p>
<pre>		#timeline{
			background:none;
		}

		/* The individual events in the slider */
		.slider .slider-container-mask .slider-container{
			background:none;
		}

		/* Setting a custom background image */
		#timeline div.navigation{
				background: url('../img/timeline_bg.jpg') repeat;
				border-top:none;
		}
	</pre><p><strong>2、修改时间轴navigation样式</strong></p>
<pre>		#timeline div.navigation:before{
			position:absolute;
			content:'';
			height:40px;
			width:100%;
			left:0;
			top:-40px;
			background: url('../img/timeline_bg.jpg') repeat;
		}

		#timeline div.navigation:after{
			position:absolute;
			content:'';
			height:10px;
			width:100%;
			left:0;
			top:-40px;
			background:repeat-x;

			background-image: linear-gradient(bottom, #434446 0%, #363839 100%);
			background-image: -o-linear-gradient(bottom, #434446 0%, #363839 100%);
			background-image: -moz-linear-gradient(bottom, #434446 0%, #363839 100%);
			background-image: -webkit-linear-gradient(bottom, #434446 0%, #363839 100%);
			background-image: -ms-linear-gradient(bottom, #434446 0%, #363839 100%);
		}
	</pre><p><strong>3、给tooltips样式</strong></p>
<pre>		#timeline div.timenav-background{
			background-color:rgba(0,0,0,0.4) !important;
		}

		#timeline .navigation .timenav-background .timenav-interval-background{
			background:none;
		}

		#timeline .top-highlight{
			background-color:transparent !important;
		}
	</pre><p><strong>4、覆写toolbar工具栏样式</strong></p>
<pre>		#timeline .toolbar{
			border:none !important;
			background-color: #202222 !important;
		}

		#timeline .toolbar div{
			border:none !important;
		}
	</pre><p><strong>5、底部数据轴的样式</strong></p>
<pre>		#timeline .navigation .timenav .time .time-interval-minor .minor{
			margin-left:-1px;
		}
		#timeline .navigation .timenav .time .time-interval div{
			color: #CCCCCC;
		}
	</pre><p><strong>6、向前向后按钮样式</strong></p>
<pre>		.slider .nav-previous .icon {
			background: url("timeline.png") no-repeat scroll 0 -293px transparent;
		}

		.slider .nav-previous,.slider .nav-next{
			font-family:'Segoe UI',sans-serif;
		}

		.slider .nav-next .icon {
			background: url("timeline.png") no-repeat scroll 72px -221px transparent;
			width: 70px !important;
		}

		.slider .nav-next:hover .icon{
			position:relative;
			right:-5px;
		}

		.slider .nav-previous:hover, .slider .nav-next:hover {
				color: #666;
				cursor: pointer;
		}

		#timeline .thumbnail {
			border: medium none;
		}
	</pre><p><strong>7、loading样式</strong></p>
<pre>		#timeline .feedback {
			background-color: #222222;
			box-shadow: 0 0 30px rgba(0, 0, 0, 0.2) inset;
			border:none;
		}

		#timeline .feedback div{
			color: #AAAAAA;
			font-size: 14px !important;
			font-weight: normal;
		}
	</pre><p><strong>8、slider的样式</strong></p>
<pre>		#timeline .slider-item h2,
		#timeline .slider-item h3{
			font-family:'Antic Slab','Segoe UI',sans-serif;
		}

		#timeline .slider-item h2{
			color:#fff;
		}

		#timeline .slider-item p{
			font-family:'Segoe UI',sans-serif;
		}

		#timeline .slider-item img,
		#timeline .slider-item iframe{
			border:none;
		}
	</pre><p><strong>9、页面样式</strong></p>
<pre>		/* Customizing the first slide - the cover */
		#timeline .slider-item:nth-child(1) h2{
			font:normal 70px/1 'Antic Slab','Segoe UI',sans-serif;
			background:rgba(0,0,0,0.3);
			white-space: nowrap;
			padding:10px 5px 5px 20px;
			position:relative;
			right:-60px;
			z-index:10;
		}
		#timeline .slider-item:nth-child(1) p i{
			font:normal normal 40px 'Dancing Script','Segoe UI',sans-serif;
			background:rgba(0,0,0,0.3);
			white-space: nowrap;
			padding:5px 20px;
			position:relative;
			right:-60px;
			z-index:10;
		}
		#timeline .slider-item:nth-child(1) p .c1{
			color:#1bdff0;
		}
		#timeline .slider-item:nth-child(1) p .c2{
			color:#c92fe6;
		}
		#timeline .slider-item:nth-child(1) .media-container {
			left: -30px;
			position: relative;
			z-index: 1;
		}
		#timeline .slider-item:nth-child(1) .credit{
			text-align: center;
		}
	</pre><p>到这主题就覆写完了，大家可以看看覆写后的样效果：</p>
<p><a href="http://www.w3cplus.com/w3cplusDemo/demos/timeline/index.html"><img alt="" src="http://www.w3cplus.com/w3cplusDemo/images/timeline.jpg"></a></p>
<p>那么有关于timeline的使用就说到这里了。最后在结束此篇文章时，非常感谢<a href="https://github.com/VeriteCo">VeriteCo</a>提供的<a href="http://timeline.verite.co/">Timeline</a>插件，特别感谢<a href="http://tutorialzine.com/2012/04/timeline-portfolio/">Martin Angelov </a>的覆写模板教程。</p>
<p>如果你对这个感兴趣可以到<a href="http://timeline.verite.co/">Timeline</a>的官网查看详细的doc文档，也可以点击<a href="http://tutorialzine.com/2012/04/timeline-portfolio/">这里</a>查看具体使用的案例，或者直接<a href="http://www.w3cplus.com/w3cplusDemo/demos/demoZip/timeline.zip">下载</a>对应的<a href="http://www.w3cplus.com/w3cplusDemo/demos/timeline/index.html">demo</a>学习。</p>
<p>特别声明：上面的代码来自于<a href="http://timeline.verite.co/">VeritoCo</a>和<a href="http://tutorialzine.com/2012/04/timeline-portfolio/">Martin Angelov</a>。</p>
<p>如需转载烦请注明出处：<strong><a href="http://www.w3cplus.com">W3CPLUS</a></strong></p>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blogclassify/5.html">js/jquery</a></li></ul></div><div><div><div><div>
      <div>0</div>
                  <a href="http://www.w3cplus.com/vote/node/267/1/vote/alternate/_i1hF3YRQ_V_q6X3wTZj_6bZjV9y1Y0q8aIHKdRT-zM/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><img src="http://www1.feedsky.com/t1/654894291/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/jquery/jquery-timeline" border="0" height="0" width="0">