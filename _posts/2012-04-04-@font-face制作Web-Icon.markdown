---
layout: post
title:  "@font-face制作Web Icon"
date:   2012-04-04 16:13:29
author: Airen
categories: program
---

## @font-face制作Web Icon
### by Airen
### at 2012-04-04 16:13:29
### original <http://www.w3cplus.com/css3/web-icon-with-font-face>

<div><div><div><p><a href="http://www.w3cplus.com/content/css3-font-face">@font-face</a>是<a href="http://www.w3cplus.com/css3">CSS3</a>中有关于字体设置的属性，通过@font-face可以将本地字体设置为Web页面字体，并能兼容所有浏览器，使用这个属性就不必担心用户本地不具备这样的字体。因为我们把字体都上传到服务器上，不过这样一来很多人担心影响性能问题。鱼和熊掌不能兼得嘛，我们就不在为这个问题说太多的话了，不过我今天要与大家分享的主题和这个@font-face还是有很在关系的，使用他配合一定的字体来制作Web页面中的Icon图标。初一看有点不实际，以前的Icon都是依靠图片来完成，怎么可能用字体就能实现呢？如果你以前有用过HTML的实体符，我想就不会那么惊奇了。</p>
<p>如果你以前关注过本站，我想这个你并不会陌生，因为我在<a href="http://www.w3cplus.com/css3/pure-css3-create-3d-pager">css3制作3D分页导航</a>一文中就有用到@font-face来制作icon的效果：</p>
<p><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/css3-pager-2.png"></p>
<p>今天我想和大家进一步的探讨这方面的应用，同时为大家准备了多种字体制作Icon的效果，下面我们一起来看看吧。</p>
<h4>制作思路</h4>
<p>思路其实很清晰，我们就是需要得到font icon的字体，然后通过@font-face将这个字体运用到一定的字符上，从而让他渲染出来是ICON的效果。难就难在无法得到这样的字体，今天我给大家搜集了三种免费的字体，以供大家学习。接下来我们一起来学习这三种字体如何转换成Web上的Icon图标。</p>
<h4>第一种：Guifx字体</h4>
<p>要使用Guifx字体制作Icon，你首先要得到相应的<a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon/Guifx/guifx_v2_transports-webfont.ttf">字体</a>，然后通过<a href="http://www.fontsquirrel.com/fontface/generator">fontsquirrel</a>转换各浏览器所需的字体，详细的操作过程大家可以参考<a href="http://www.w3cplus.com/content/css3-font-face">@font-face</a>。</p>
<p>字体得到了就好办了，现在只需要使用一定的编码来代表相应的字符，打个比方来说：</p>
<pre>		&lt;div class=&quot;icon&quot;&gt;A&lt;/div&gt;
	</pre><p>那么上面的“A”字符代表的就是一个Icon，只不过我们需要把刚才得到字体运用上去：</p>
<pre>		@font-face {
    font-family: 'GuifxIcon';
    src: url('Guifx/guifx_v2_transports-webfont.eot');
    src: url('Guifx/guifx_v2_transports-webfont.eot?#iefix') format('embedded-opentype'),
    url('Guifx/guifx_v2_transports-webfont.woff') format('woff'),
    url('Guifx/guifx_v2_transports-webfont.ttf') format('truetype'),
    url('Guifx/guifx_v2_transports-webfont.svg#Guifxv2TransportsRegular') format('svg');
    font-weight: normal;
    font-style: normal;
    }
    .icon {
    font-family: 'GuifxIcon';
    }
	</pre><p>这样运用过后，"A"在web中就渲染成下图的icon效果：</p>
<p><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/webfonticon-1.jpg"></p>
<p>上面拿了一个简单的Icon来做实例，其他的运用也是这样的，下面我将对应的字符和Icon对照表放上来让大家使用时好参考，省得测试的时间：</p>
<p><a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon/index.html"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/webfonticon-2.jpg"></a></p>
<p>有关于详细的代码大家可以查看<a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon.html">这里</a>，当然你可以点击<a href="http://www.w3cplus.com/w3cplusDemo/demos/demoZip/webFontIcon.zip">下载</a>相关字体和代码。</p>
<h4>第二种：websymbols字体</h4>
<p>这种字体其实运用和前面介绍的方法是一样的，唯一区别是使用的字体不一样，从而字母渲染出来的Icon也就不一样了。下面我们同样来看“A”渲染出来的效果</p>
<pre>		&lt;div class=&quot;icon&quot;&gt;A&lt;/div&gt;
	</pre><p>样式的使用：</p>
<pre>		@font-face{ 
			font-family: 'WebSymbolsRegular';
			src: url('websymbols/fonts/websymbols-regular-webfont.eot');
			src: url('websymbols/fonts/websymbols-regular-webfont.eot?#iefix') format('embedded-opentype'),
					 url('websymbols/fonts/websymbols-regular-webfont.woff') format('woff'),
					 url('websymbols/fonts/websymbols-regular-webfont.ttf') format('truetype'),
					 url('websymbols/fonts/websymbols-regular-webfont.svg#WebSymbolsRegular') format('svg');
		}
		.icon {
    font-family: 'WebSymbolsRegular';
    }
	</pre><p>对应出来的Icon就是下图的样子了：</p>
<p><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/webfonticon-3.jpg"></p>
<p>我想大家肯定也在头痛字体的来源吧，不急，点<a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon//websymbols/fonts/websymbols-regular-webfont.ttf">这里</a>下载吧。同样给大家附上相应的字符照图，以供大家参考：</p>
<p><a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon/demo2.html"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/webfonticon-4.jpg"></a></p>
<p>有关于详细的代码大家可以查看<a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon/demo2.html">这里</a>，当然你可以点击<a href="http://www.w3cplus.com/w3cplusDemo/demos/demoZip/webFontIcon.zip">下载</a>相关字体和代码。</p>
<h4>第三种：Font Awesome</h4>
<p><a href="http://fortawesome.github.com/Font-Awesome/">Font Awesome</a>是一个强大的字体制作Icon的案例，作者在<a href="http://twitter.github.com/bootstrap/base-css.html#icons">Bootstrap Icon</a>的基础上将Icon图片换成了字体来制作。初看真的让我汗颜呀，太强大了。下面我们一起来看看如何使用？</p>
<p><strong>HTML Markup</strong></p>
<pre>		 &lt;div class=&quot;icon-glass&quot;&gt;&lt;/div&gt;
	</pre><p>相对来说这个字体制作ICON复杂一点，他是在<a href="http://twitter.github.com/bootstrap/base-css.html#icons">Bootstrap Icon</a>基础上扩展的，只不过区别是<a href="http://twitter.github.com/bootstrap/base-css.html#icons">Bootstrap Icon</a>采用的是IMG，而他采用的是@font-face:</p>
<pre>		@font-face {
  font-family: 'FontAwesome';
  src: url('font/fontawesome-webfont.eot');
  src: url('font/fontawesome-webfont.eot?#iefix') format('embedded-opentype'), url('../font/fontawesome-webfont.woff') format('woff'), url('../font/fontawesome-webfont.ttf') format('truetype'), url('../font/fontawesome-webfont.svgz#FontAwesomeRegular') format('svg'), url('../font/fontawesome-webfont.svg#FontAwesomeRegular') format('svg');
  font-weight: normal;
  font-style: normal;
}
/* sprites.less reset */
[class^="icon-"], 
[class*=" icon-"] {
  display: inline;
  width: auto;
  height: auto;
  line-height: inherit;
  vertical-align: baseline;
  background-image: none;
  background-position: 0% 0%;
  background-repeat: repeat;
}
[class^="icon-"]:before, 
[class*=" icon-"]:before {
  font-family: FontAwesome;
  font-weight: normal;
  font-style: normal;
  display: inline-block;
  text-decoration: inherit;
}
/*  Font Awesome uses the Unicode Private Use Area (PUA) to ensure screen
    readers do not read off random characters that represent icons */
.icon-glass:before              { content: "\f000"; }
.icon-music:before              { content: "\f001"; }
.icon-search:before             { content: "\f002"; }
.icon-envelope:before           { content: "\f003"; }
.icon-heart:before              { content: "\f004"; }
.icon-star:before               { content: "\f005"; }
.icon-star-empty:before         { content: "\f006"; }
.icon-user:before               { content: "\f007"; }
.icon-film:before               { content: "\f008"; }
.icon-th-large:before           { content: "\f009"; }
.icon-th:before                 { content: "\f00a"; }
.icon-th-list:before            { content: "\f00b"; }
.icon-ok:before                 { content: "\f00c"; }
.icon-remove:before             { content: "\f00d"; }
.icon-zoom-in:before            { content: "\f00e"; }

.icon-zoom-out:before           { content: "\f010"; }
.icon-off:before                { content: "\f011"; }
.icon-signal:before             { content: "\f012"; }
.icon-cog:before                { content: "\f013"; }
.icon-trash:before              { content: "\f014"; }
.icon-home:before               { content: "\f015"; }
.icon-file:before               { content: "\f016"; }
.icon-time:before               { content: "\f017"; }
.icon-road:before               { content: "\f018"; }
.icon-download-alt:before       { content: "\f019"; }
.icon-download:before           { content: "\f01a"; }
.icon-upload:before             { content: "\f01b"; }
.icon-inbox:before              { content: "\f01c"; }
.icon-play-circle:before        { content: "\f01d"; }
.icon-repeat:before             { content: "\f01e"; }

/* \f020 is not a valid unicode character. all shifted one down */
.icon-refresh:before            { content: "\f021"; }
.icon-list-alt:before           { content: "\f022"; }
.icon-lock:before               { content: "\f023"; }
.icon-flag:before               { content: "\f024"; }
.icon-headphones:before         { content: "\f025"; }
.icon-volume-off:before         { content: "\f026"; }
.icon-volume-down:before        { content: "\f027"; }
.icon-volume-up:before          { content: "\f028"; }
.icon-qrcode:before             { content: "\f029"; }
.icon-barcode:before            { content: "\f02a"; }
.icon-tag:before                { content: "\f02b"; }
.icon-tags:before               { content: "\f02c"; }
.icon-book:before               { content: "\f02d"; }
.icon-bookmark:before           { content: "\f02e"; }
.icon-print:before              { content: "\f02f"; }

.icon-camera:before             { content: "\f030"; }
.icon-font:before               { content: "\f031"; }
.icon-bold:before               { content: "\f032"; }
.icon-italic:before             { content: "\f033"; }
.icon-text-height:before        { content: "\f034"; }
.icon-text-width:before         { content: "\f035"; }
.icon-align-left:before         { content: "\f036"; }
.icon-align-center:before       { content: "\f037"; }
.icon-align-right:before        { content: "\f038"; }
.icon-align-justify:before      { content: "\f039"; }
.icon-list:before               { content: "\f03a"; }
.icon-indent-left:before        { content: "\f03b"; }
.icon-indent-right:before       { content: "\f03c"; }
.icon-facetime-video:before     { content: "\f03d"; }
.icon-picture:before            { content: "\f03e"; }

.icon-pencil:before             { content: "\f040"; }
.icon-map-marker:before         { content: "\f041"; }
.icon-adjust:before             { content: "\f042"; }
.icon-tint:before               { content: "\f043"; }
.icon-edit:before               { content: "\f044"; }
.icon-share:before              { content: "\f045"; }
.icon-check:before              { content: "\f046"; }
.icon-move:before               { content: "\f047"; }
.icon-step-backward:before      { content: "\f048"; }
.icon-fast-backward:before      { content: "\f049"; }
.icon-backward:before           { content: "\f04a"; }
.icon-play:before               { content: "\f04b"; }
.icon-pause:before              { content: "\f04c"; }
.icon-stop:before               { content: "\f04d"; }
.icon-forward:before            { content: "\f04e"; }

.icon-fast-forward:before       { content: "\f050"; }
.icon-step-forward:before       { content: "\f051"; }
.icon-eject:before              { content: "\f052"; }
.icon-chevron-left:before       { content: "\f053"; }
.icon-chevron-right:before      { content: "\f054"; }
.icon-plus-sign:before          { content: "\f055"; }
.icon-minus-sign:before         { content: "\f056"; }
.icon-remove-sign:before        { content: "\f057"; }
.icon-ok-sign:before            { content: "\f058"; }
.icon-question-sign:before      { content: "\f059"; }
.icon-info-sign:before          { content: "\f05a"; }
.icon-screenshot:before         { content: "\f05b"; }
.icon-remove-circle:before      { content: "\f05c"; }
.icon-ok-circle:before          { content: "\f05d"; }
.icon-ban-circle:before         { content: "\f05e"; }

.icon-arrow-left:before         { content: "\f060"; }
.icon-arrow-right:before        { content: "\f061"; }
.icon-arrow-up:before           { content: "\f062"; }
.icon-arrow-down:before         { content: "\f063"; }
.icon-share-alt:before          { content: "\f064"; }
.icon-resize-full:before        { content: "\f065"; }
.icon-resize-small:before       { content: "\f066"; }
.icon-plus:before               { content: "\f067"; }
.icon-minus:before              { content: "\f068"; }
.icon-asterisk:before           { content: "\f069"; }
.icon-exclamation-sign:before   { content: "\f06a"; }
.icon-gift:before               { content: "\f06b"; }
.icon-leaf:before               { content: "\f06c"; }
.icon-fire:before               { content: "\f06d"; }
.icon-eye-open:before           { content: "\f06e"; }

.icon-eye-close:before          { content: "\f070"; }
.icon-warning-sign:before       { content: "\f071"; }
.icon-plane:before              { content: "\f072"; }
.icon-calendar:before           { content: "\f073"; }
.icon-random:before             { content: "\f074"; }
.icon-comment:before            { content: "\f075"; }
.icon-magnet:before             { content: "\f076"; }
.icon-chevron-up:before         { content: "\f077"; }
.icon-chevron-down:before       { content: "\f078"; }
.icon-retweet:before            { content: "\f079"; }
.icon-shopping-cart:before      { content: "\f07a"; }
.icon-folder-close:before       { content: "\f07b"; }
.icon-folder-open:before        { content: "\f07c"; }
.icon-resize-vertical:before    { content: "\f07d"; }
.icon-resize-horizontal:before  { content: "\f07e"; }

.icon-bar-chart:before          { content: "\f080"; }
.icon-twitter-sign:before       { content: "\f081"; }
.icon-facebook-sign:before      { content: "\f082"; }
.icon-camera-retro:before       { content: "\f083"; }
.icon-key:before                { content: "\f084"; }
.icon-cogs:before               { content: "\f085"; }
.icon-comments:before           { content: "\f086"; }
.icon-thumbs-up:before          { content: "\f087"; }
.icon-thumbs-down:before        { content: "\f088"; }
.icon-star-half:before          { content: "\f089"; }
.icon-heart-empty:before        { content: "\f08a"; }
.icon-signout:before            { content: "\f08b"; }
.icon-linkedin-sign:before      { content: "\f08c"; }
.icon-pushpin:before            { content: "\f08d"; }
.icon-external-link:before      { content: "\f08e"; }

.icon-signin:before             { content: "\f090"; }
.icon-trophy:before             { content: "\f091"; }
.icon-github-sign:before        { content: "\f092"; }
.icon-upload-alt:before         { content: "\f093"; }
.icon-lemon:before              { content: "\f094"; }
	</pre><p>有关详细的使用大家可以点击<a href="http://fortawesome.github.com/Font-Awesome/">官网</a>。下图是部分类名对应的icon图标</p>
<p><a href="http://www.w3cplus.com/w3cplusDemo/demos/webFontIcon/demo3.html"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/webfonticon-5.jpg"></a></p>
<p>这个比较有名气的使用方法，也是一个开源的宝贝，大家可以通过多种路径下载到相应的源码，然后按其api运用到你的项目中。如果你对这个感兴趣可以点击<a href="http://www.w3cplus.com/w3cplusDemo/demos/demoZip/webFontIcon.zip">这里</a>下载所需文件和源码。或者到<a href="https://github.com/FortAwesome/Font-Awesome">Github</a>上查和下载所有源码。</p>
<p>上面给大家介绍了三种字体，配合@font-face制作web icon方法，希望对大家以后的运用上有所帮助。</p>
<p>如需转载烦请注明出处：<strong><a href="http://www.w3cplus.com">W3CPLUS</a></strong></p>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blogclassify/11.html">css3</a></li></ul></div><div><div><div><div>
      <div>0</div>
                  <a href="http://www.w3cplus.com/vote/node/270/1/vote/alternate/InXXBkQcXijL5mgEbGS6kdbIVxrhGUPE9Ua6fHxkeCc/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><img src="http://www1.feedsky.com/t1/654894288/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/css3/web-icon-with-font-face" border="0" height="0" width="0">