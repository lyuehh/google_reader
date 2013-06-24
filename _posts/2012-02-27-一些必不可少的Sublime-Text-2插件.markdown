---
layout: post
title:  "一些必不可少的Sublime Text 2插件"
date:   2012-02-27 18:36:26
author: 神飞
categories: program
---

## 一些必不可少的Sublime Text 2插件
### by 神飞
### at 2012-02-27 18:36:26
### original <http://www.qianduan.net/essential-to-sublime-the-text-2-plugins.html>

<div>
中文原文：<a href="http://www.qianduan.net/essential-to-sublime-the-text-2-plugins.html" title="一些必不可少的Sublime Text 2插件">一些必不可少的sublime text 2插件</a><br>
整理自：<a href="http://net.tutsplus.com/tutorials/tools-and-tips/essential-sublime-text-2-plugins-and-extensions/">Essential Sublime Text 2 Plugins and Extensions</a><br>
请尊重版权，转载请注明来源，多谢！
<hr>
</div>
<p>Sublime Text 2是一个轻量、简洁、高效、跨平台的编辑器，方便的配色以及兼容vim快捷键等各种优点博得了很多前端开发人员的喜爱，当然也包括我，在看到<a title="Sublime Text 2 简介" href="http://shawphy.com/2011/09/sublime-text-2.html">小飞的介绍</a>后，我就一直在用了。本文推荐一些好用的插件和扩展。</p>
<p>Sublime Text 2基本上是共享软件，免费版和收费版基本无区别，只是偶尔会弹框让你去购买，这个基本不影响使用。如果你不了解它，也可以看下<a title="Sublime Text 2 – 非常强大的跨平台编辑器" href="http://www.appinn.com/sublime-text-2/">小众软件的这篇详细介绍</a>。</p>
<h2>安装Sublime Text 2插件的方法：</h2>
<h3>1.直接安装</h3>
<p>安装Sublime text 2插件很方便，可以直接下载安装包解压缩到Packages目录（菜单-&gt;preferences-&gt;packages）。</p>
<h3>2.使用Package Control组件安装</h3>
<p>也可以安装package control组件，然后直接在线安装：</p>
<ol>
<li>按Ctrl+`调出console</li>
<li>粘贴以下代码到底部命令行并回车：<br>
<code>import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())</code>
</li>
<li>重启Sublime Text 2。</li>
<li>如果在Perferences-&gt;package settings中看到package control这一项，则安装成功。</li>
</ol>
<p>如果这种方法不能安装成功，可以<a title="手动安装 Package Control" href="http://wbond.net/sublime_packages/package_control/installation">到这里下载文件手动安装</a>。</p>
<h4>用Package Control安装插件的方法：</h4>
<ol>
<li>按下Ctrl+Shift+P调出命令面板</li>
<li>输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。</li>
</ol>
<p>不爽的是，有的网络环境可能会不允许访问陌生的网络环境从而设置一道防火墙，而Sublime Text 2貌似无法设置代理，可能就获取不到安装包列表了。<br>
好，方法介绍完了，下面是本文正题，一些有用的Sublime Text 2插件：</p>
<h3><a href="https://bitbucket.org/sublimator/sublime-2-zencoding">Zen Coding</a></h3>
<p>这个，不解释了，还不知道ZenCoding的同学强烈推荐去看一下：《<a title="Permanent Link to Zen Coding: 一种快速编写HTML/CSS代码的方法" href="http://www.qianduan.net/zen-coding-a-new-way-to-write-html-code.html" rel="bookmark">Zen Coding: 一种快速编写HTML/CSS代码的方法</a>》。</p>
<p><img title="zen" src="http://www.qianduan.net/wp-content/uploads/2012/02/zen.png" alt="" width="374" height="197"><br>
PS:Zen Coding  for Sublime  Text 2插件的开发者已经停止了在Github上共享了，现在只有通过Package Control来安装。</p>
<h3><a href="https://github.com/mrmartineau/Jquery">jQuery Package for sublime Text</a></h3>
<p>如果你离不开jQuery的话，这个必备～～</p>
<h3><a href="https://github.com/wbond/sublime_prefixr">Sublime Prefixr</a></h3>
<p>Prefixr，CSS3 私有前缀自动补全插件，显然也很有用哇</p>
<p><img style="border-style:initial;border-color:initial" title="pr" src="http://www.qianduan.net/wp-content/uploads/2012/02/pr.png" alt="" width="607" height="201"></p>
<h3><a href="https://github.com/jdc0589/JsFormat">JS Format</a></h3>
<p>一个JS代码格式化插件。</p>
<h3><a href="https://github.com/kronuz/SublimeLinter/">SublimeLinter</a></h3>
<p>一个支持lint语法的插件，可以高亮linter认为有错误的代码行，也支持高亮一些特别的注释，比如“TODO”，这样就可以被快速定位。（IntelliJ IDEA的TODO功能很赞，这个插件虽然比不上，但是也够用了吧）</p>
<p><img title="linter" src="http://www.qianduan.net/wp-content/uploads/2012/02/linter.png" alt="" width="386" height="138"></p>
<h3><a href="https://github.com/mrmartineau/Placeholders">Placeholders</a></h3>
<p>故名思意，占位用，包括一些占位文字和HTML代码片段，实用。</p>
<h3><a href="https://github.com/wbond/sublime_alignment">Sublime Alignment</a></h3>
<p>用于代码格式的自动对齐。传说最新版Sublime 已经集成。</p>
<p><img title="align" src="http://www.qianduan.net/wp-content/uploads/2012/02/align.png" alt="" width="402" height="139"></p>
<h3><a href="https://github.com/kemayo/sublime-text-2-clipboard-history">Clipboard History</a></h3>
<p>粘贴板历史记录，方便使用复制/剪切的内容。</p>
<h3><a href="https://github.com/phillipkoebbe/DetectSyntax">DetectSyntax</a></h3>
<p>这是一个代码检测插件。</p>
<h3><a href="https://github.com/weslly/Nettuts-Fetch">Nettuts Fetch</a></h3>
<p>如果你在用一些公用的或者开源的框架，比如 <a href="http://necolas.github.com/normalize.css/">Normalize.css</a>或者<a href="http://www.modernizr.com/">modernizr.js</a>，但是，过了一段时间后，可能该开源库已经更新了，而你没有发现，这个时候可能已经不太适合你的项目了，那么你就要重新折腾一遍或者继续用陈旧的文件。Nettuts Fetch可以让你设置一些需要同步的文件列表，然后保存更新。</p>
<p><img title="remote" src="http://www.qianduan.net/wp-content/uploads/2012/02/remote.jpg" alt="" width="598" height="187"></p>
<h3><a href="https://github.com/cgutierrez/JsMinifier">JsMinifier</a></h3>
<p>该插件基于Google Closure compiler，自动压缩js文件。</p>
<h3><a href="https://github.com/Kronuz/SublimeCodeIntel">Sublime CodeIntel</a></h3>
<p>代码自动提示</p>
<h3><a href="https://github.com/facelessuser/BracketHighlighter">Bracket Highlighter</a></h3>
<p>类似于代码匹配，可以匹配括号，引号等符号内的范围。</p>
<p><img title="braket" src="http://www.qianduan.net/wp-content/uploads/2012/02/braket.png" alt="" width="367" height="242"></p>
<h3><a href="https://github.com/atadams/Hex-to-HSL-Color">Hex to HSL</a></h3>
<p>自动转换颜色值，从16进制到HSL格式，快捷键 Ctrl+Shift+U</p>
<p><img title="hsl" src="http://www.qianduan.net/wp-content/uploads/2012/02/hsl.png" alt="" width="359" height="87"></p>
<h3><a href="http://www.sublimetext.com/forum/viewtopic.php?f=5&amp;p=22274">GBK to UTF8</a></h3>
<p>将文件编码从GBK转黄成UTF8，快捷键Ctrl+Shift+C</p>
<h3><a href="https://github.com/kemayo/sublime-text-2-git"> Git</a></h3>
<p><img title="git" src="http://www.qianduan.net/wp-content/uploads/2012/02/git.png" alt="" width="624" height="306"></p>
<p>该插件基本上实现了git的所有功能。</p>
<h3>总结</h3>
<p>好吧，大概就这些，如果你有常用的插件或者扩展，欢迎推荐。Sublime Text 2真是一款一见钟情的编辑器，每次和别人聊到编辑器时必荐的。。。 :)</p>