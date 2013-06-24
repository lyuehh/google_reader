---
layout: post
title:  "HTML+CSS 模仿Windows 7 桌面效果"
date:   2010-06-17 09:27:00
author: Gnie
categories: program
---

## HTML+CSS 模仿Windows 7 桌面效果
### by Gnie
### at 2010-06-17 09:27:00
### original <http://www.cnblogs.com/gnielee/archive/2010/06/17/html-css-windows7-desktop.html>

<p><a href="http://www.cnblogs.com/gnielee/"><img src="http://pic.cnblogs.com/face/u40474.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/gnielee/">Gnie</a> 发表于 2010-06-17 09:27 <a href="http://www.cnblogs.com/gnielee/archive/2010/06/17/html-css-windows7-desktop.html">原文链接</a> 阅读: 3270 评论: 80</p><p>     前一阵在园子里看到一篇文章《<a href="http://kb.cnblogs.com/page/65694/">使用css3仿造window7的开始菜单</a>》，文中仅使用CSS3 实现了Windows 7 开始菜单的动态效果，很久以来一直被WPF/Silverlight 山上的风景所吸引，未成想其他的山也同样引人入胜。于是心血来潮也尝试着做了一个Windows 7 桌面效果，先来看几张截图吧。</p>  <p>桌面程序鼠标Hover 效果：</p>  <p><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="Desktop" border="0" alt="Desktop" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/Desktop_3.png" width="634" height="474"></p>  <p>任务栏程序鼠标Hover 效果：</p>  <p><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="Taskbar" border="0" alt="Taskbar" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/Taskbar_1.png" width="634" height="474"> </p>  <p>开始菜单效果：</p>  <p><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="StartMenu" border="0" alt="StartMenu" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/StartMenu_1.png" width="634" height="456"> </p>  <p></p>  <p></p>  <p></p>  <h1>桌面程序图标</h1>  <p>桌面背景及程序图标的结构如下：</p>  <pre><span style="color:blue">&lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;win&quot;&gt;
    &lt;</span><span style="color:maroon">ul </span><span style="color:red">id</span><span style="color:blue">=&quot;app&quot;&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;<br>            &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/computer.png&quot;&gt;&lt;</span><span style="color:maroon">br </span><span style="color:blue">/&gt;
            </span>Computer<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>        &lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;<br>            &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/recycle.png&quot;&gt;&lt;</span><span style="color:maroon">br </span><span style="color:blue">/&gt;
            </span>Recycle Bin<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>        &lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;<br>            &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/network.png&quot;&gt;&lt;</span><span style="color:maroon">br </span><span style="color:blue">/&gt;
            </span>Network<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>        &lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
    &lt;/</span><span style="color:maroon">ul</span><span style="color:blue">&gt;
&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;
</span></pre>
<a href="http://11011.net/software/vspaste"></a>

<p>在桌面&lt;div&gt;中加入背景图片：</p>

<pre><span style="color:maroon">#win
</span>{
    <span style="color:red">background-image</span>: <span style="color:blue">url(images/win7bg.jpg)</span>;
    <span style="color:red">background-position</span>: <span style="color:blue">center</span>;
    <span style="color:red">width</span>: <span style="color:blue">880px</span>;
    <span style="color:red">height</span>: <span style="color:blue">550px</span>;
    <span style="color:red">border</span>: <span style="color:blue">#ffffff 1px solid</span>;
}</pre>

<p>     为桌面应用图标添加鼠标Hover 动态效果，text-shadow 用来设置应用程序文字阴影效果，-webkit-border-radius 可设置边框圆角：</p>

<pre><span style="color:maroon">#app
</span>{
    <span style="color:red">float</span>: <span style="color:blue">left</span>;
    <span style="color:red">text-align</span>: <span style="color:blue">center</span>;
    <span style="color:red">margin</span>: <span style="color:blue">-15px 0 0 -30px</span>;
    <span style="color:red">list-style</span>: <span style="color:blue">none</span>;
}

<span style="color:maroon">#app a
</span>{
    <span style="color:red">text-decoration</span>: <span style="color:blue">none</span>;
    <span style="color:red">border</span>: <span style="color:blue">solid 1px transparent</span>;
    <span style="color:red">display</span>: <span style="color:blue">block</span>;
    <span style="color:red">padding</span>: <span style="color:blue">3px</span>;
    <span style="color:red">margin</span>: <span style="color:blue">20px 0 0 0</span>;
    <span style="color:red">color</span>: <span style="color:blue">#ffffff</span>;
    <span style="color:red">text-shadow</span>: <span style="color:blue">0.2em 0.1em 0.3em #000000</span>;
}

<span style="color:maroon">#app a:hover
</span>{
    <span style="color:red">border</span>: <span style="color:blue">solid 1px #bbd6ec</span>;
    <span style="color:red">-webkit-border-radius</span>: <span style="color:blue">3px</span>;
    <span style="color:red">box-shadow</span>: <span style="color:blue">inset 0 0 1px #fff</span>;
    <span style="color:red">-webkit-box-shadow</span>: <span style="color:blue">inset 0 0 1px #fff</span>;
    <span style="color:red">background-color</span>: <span style="color:blue">#5caae8</span>;
}</pre>
<a href="http://11011.net/software/vspaste"></a>

<p><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/Computer1_1.png" width="96" height="78">    <img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/Computer2.png" width="98" height="78"></p>

<h1>任务栏程序图标</h1>

<p>下面是任务栏结构的HTML代码：</p>

<pre><span style="color:blue">&lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;taskbar&quot;&gt;
    &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot; </span><span style="color:red">id</span><span style="color:blue">=&quot;start&quot;&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;
    &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot; </span><span style="color:red">style</span><span style="color:blue">=&quot;</span><span style="color:red">left</span><span style="color:blue">: 60px&quot;&gt;
        &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/ie.png&quot; /&gt;
    &lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>    &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot; </span><span style="color:red">style</span><span style="color:blue">=&quot;</span><span style="color:red">left</span><span style="color:blue">: 120px&quot;&gt;
        &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/library.png&quot; /&gt;
    &lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>    &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot; </span><span style="color:red">style</span><span style="color:blue">=&quot;</span><span style="color:red">left</span><span style="color:blue">: 180px&quot;&gt;
        &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/mp.png&quot; /&gt;
    &lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>    &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot; </span><span style="color:red">style</span><span style="color:blue">=&quot;</span><span style="color:red">left</span><span style="color:blue">: 240px&quot;&gt;
        &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/live.png&quot; /&gt;
    &lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;<br>    &lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot; </span><span style="color:red">style</span><span style="color:blue">=&quot;</span><span style="color:red">left</span><span style="color:blue">: 300px&quot;&gt;
        &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/outlook.png&quot; /&gt;
    &lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;
    &lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;desktop&quot;&gt;&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;
&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;
</span></pre>

<p>     首先来看看开始菜单图标如何设置，通过Hover 操作变换start.bmp 显示位置，达到图标发亮效果。</p>

<pre><span style="color:maroon">#taskbar #start
</span>{
    <span style="color:red">position</span>: <span style="color:blue">absolute</span>;
    <span style="color:red">text-align</span>: <span style="color:blue">center</span>;
    <span style="color:red">width</span>: <span style="color:blue">57px</span>;
    <span style="color:red">height</span>: <span style="color:blue">46px</span>;
    <span style="color:red">background</span>: <span style="color:blue">url(images/start.bmp) 0 -6px no-repeat</span>;
}

<span style="color:maroon">#taskbar #start:hover
</span>{
    <span style="color:red">text-decoration</span>: <span style="color:blue">none</span>;
    <span style="color:red">background-position</span>: <span style="color:blue">0 -114px</span>;
}</pre>
<img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="start" border="0" alt="start" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/start.png" width="58" height="166">      <img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/1_1.png" width="60" height="45">      <img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/2_1.png" width="62" height="46"> <a href="http://11011.net/software/vspaste"></a>

<p></p>

<p></p>

<p>     任务栏背景通过taskbarbg.png 实现，其他图标Hover 效果通过改变taskbarhover.png 图片位置实现图标下方高亮效果。</p>

<pre><span style="color:maroon">#taskbar
</span>{
    <span style="color:red">height</span>: <span style="color:blue">42px</span>;
    <span style="color:red">width</span>: <span style="color:blue">880px</span>;
    <span style="color:red">margin</span>: <span style="color:blue">-42px 0 0 1px</span>;
    <span style="color:red">background</span>: <span style="color:blue">url(images/taskbarbg.png) no-repeat</span>;
}

<span style="color:maroon">#taskbar img
</span>{
    <span style="color:red">margin</span>: <span style="color:blue">5px 0 0 0</span>;
    <span style="color:red">width</span>: <span style="color:blue">30px</span>;
    <span style="color:red">height</span>: <span style="color:blue">29px</span>;
}

<span style="color:maroon">#taskbar a
</span>{
    <span style="color:red">position</span>: <span style="color:blue">absolute</span>;
    <span style="color:red">text-align</span>: <span style="color:blue">center</span>;
    <span style="color:red">width</span>: <span style="color:blue">57px</span>;
    <span style="color:red">height</span>: <span style="color:blue">46px</span>;
    <span style="color:red">background</span>: <span style="color:blue">url(images/taskbarhover.png) 0 -46px no-repeat</span>;
}

<span style="color:maroon">#taskbar a:hover
</span>{
    <span style="color:red">background-position</span>: <span style="color:blue">0 -3px</span>;
}</pre>
<a href="http://11011.net/software/vspaste"></a><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/3.png" width="178" height="47"> 

<p></p>

<p></p>

<h1>开始菜单</h1>

<p>     对于开始菜单的设置可以参考之前提到的那篇文章，本篇在其基础上添加了菜单分割线及透明效果。</p>

<pre><span style="color:blue">&lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;menuwin&quot;&gt;
    &lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;startmenu&quot;&gt;&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;
    &lt;</span><span style="color:maroon">ul </span><span style="color:red">id</span><span style="color:blue">=&quot;programs&quot;&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/ie.png&quot; /&gt;</span>Internet Explorer<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/mc.png&quot; /&gt;</span>Microsoft Media Center<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;<br>            &lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;leftspliter&quot;&gt;&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/word.png&quot; /&gt;</span>Microsoft Word 2010<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/excel.png&quot; /&gt;</span>Microsoft Excel 2010<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/powerpoint.png&quot; /&gt;</span>Microsoft PowerPoint 2010<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/access.png&quot; /&gt;</span>Microsoft Access 2010<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/update.png&quot; /&gt;</span>Windows Update<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;
            &lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;leftspliter&quot;&gt;&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/narrow.png&quot; /&gt;</span>All Programs<span style="color:blue">&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;
            &lt;</span><span style="color:maroon">img </span><span style="color:red">id</span><span style="color:blue">=&quot;search&quot; </span><span style="color:red">src</span><span style="color:blue">=&quot;images/search.png&quot; /&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
    &lt;/</span><span style="color:maroon">ul</span><span style="color:blue">&gt;
    &lt;</span><span style="color:maroon">ul </span><span style="color:red">id</span><span style="color:blue">=&quot;links&quot;&gt;
        &lt;</span><span style="color:maroon">li </span><span style="color:red">class</span><span style="color:blue">=&quot;icon&quot;&gt;&lt;</span><span style="color:maroon">img </span><span style="color:red">src</span><span style="color:blue">=&quot;images/user.png&quot; /&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Documents<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Pictures<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Music<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;rightspliter&quot;&gt;&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Games<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Computer<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">div </span><span style="color:red">id</span><span style="color:blue">=&quot;rightspliter&quot;&gt;&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Control Panel<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Devices and Printers<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
        &lt;</span><span style="color:maroon">li</span><span style="color:blue">&gt;&lt;</span><span style="color:maroon">a </span><span style="color:red">href</span><span style="color:blue">=&quot;#&quot;&gt;&lt;</span><span style="color:maroon">span</span><span style="color:blue">&gt;</span>Default Programs<span style="color:blue">&lt;/</span><span style="color:maroon">span</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">a</span><span style="color:blue">&gt;&lt;/</span><span style="color:maroon">li</span><span style="color:blue">&gt;
    &lt;/</span><span style="color:maroon">ul</span><span style="color:blue">&gt;
&lt;/</span><span style="color:maroon">div</span><span style="color:blue">&gt;
</span></pre>

<p>开始菜单通过opacity 设置其透明度：</p>

<pre><span style="color:maroon">#startmenu
</span>{
    <span style="color:red">border</span>: <span style="color:blue">solid 1px #102a3e</span>;
    <span style="color:red">overflow</span>: <span style="color:blue">visible</span>;
    <span style="color:red">display</span>: <span style="color:blue">block</span>;
    <span style="color:red">float</span>: <span style="color:blue">left</span>;
    <span style="color:red">height</span>: <span style="color:blue">404px</span>;
    <span style="color:red">width</span>: <span style="color:blue">400px</span>;
    <span style="color:red">opacity</span>: <span style="color:blue">0.9</span>;
    <span style="color:red">-webkit-border-radius</span>: <span style="color:blue">5px</span>;
    <span style="color:red">position</span>: <span style="color:blue">absolute</span>;
    <span style="color:red">box-shadow</span>: <span style="color:blue">inset 0 0 1px #ffffff</span>;
    <span style="color:red">-webkit-box-shadow</span>: <span style="color:blue">inset 0 0 1px #ffffff</span>;
    <span style="color:red">background-color</span>: <span style="color:blue">#619bb9</span>;
    <span style="color:red">background</span>: <span style="color:blue">-webkit-gradient(linear, center top, center bottom, from(#327aa4),color-stop(45%, #2e4b5a), to(#5cb0dc))</span>;
}</pre>

<p><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/5.png" width="407" height="441"> </p>

<p><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="" border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/gnielee/WindowsLiveWriter/HTML4CSS3Windows7_11EB7/6.png" width="407" height="441"> </p>

<p>通过jQuery（common.js） 完成开始菜单打开/关闭效果</p>

<pre>$(document).ready(<span style="color:blue">function </span>() {
    $(<span style="color:maroon">&quot;#start&quot;</span>).click(<span style="color:blue">function </span>() {
        $(<span style="color:maroon">&quot;#menuwin&quot;</span>).css(<span style="color:maroon">&quot;display&quot;</span>, <span style="color:maroon">&quot;block&quot;</span>);
    });
    $(<span style="color:maroon">&quot;#win&quot;</span>).click(<span style="color:blue">function </span>() {
        $(<span style="color:maroon">&quot;#menuwin&quot;</span>).css(<span style="color:maroon">&quot;display&quot;</span>, <span style="color:maroon">&quot;none&quot;</span>);
    });
    $(<span style="color:maroon">&quot;#desktop&quot;</span>).click(<span style="color:blue">function </span>() {
        $(<span style="color:maroon">&quot;#menuwin&quot;</span>).css(<span style="color:maroon">&quot;display&quot;</span>, <span style="color:maroon">&quot;none&quot;</span>);
    });
});</pre>
<a href="http://11011.net/software/vspaste"></a>

<h1>源代码下载</h1>

<p>请使用Chrome 浏览</p>
<img src="http://www.cnblogs.com/gnielee/aggbug/1759329.html?type=1" width="1" height="1" alt=""><p>评论: 80　<a href="http://www.cnblogs.com/gnielee/archive/2010/06/17/html-css-windows7-desktop.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/gnielee/archive/2010/06/17/html-css-windows7-desktop.html#commentform">发表评论</a></p><p><a href="http://a4.yeshj.com/rd/35721/">众里寻你千百度，百度期待您的加盟</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/66468/">人人网：70%人在世界杯期间产生消费行为</a><span style="color:gray">(2010-06-17 20:03)</span><br>· <a href="http://news.cnblogs.com/n/66467/">苹果iPad应用程序2个月时间突破1万个</a><span style="color:gray">(2010-06-17 17:48)</span><br>· <a href="http://news.cnblogs.com/n/66466/">Memeo Connect 2.0：让谷歌网盘成为现实</a><span style="color:gray">(2010-06-17 17:47)</span><br>· <a href="http://news.cnblogs.com/n/66465/">Linux操作系统需要微软的十大帮助</a><span style="color:gray">(2010-06-17 17:46)</span><br>· <a href="http://news.cnblogs.com/n/66464/">淘宝联盟称封杀影响不大 淘宝客未大规模逃离</a><span style="color:gray">(2010-06-17 17:44)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/miloyip/archive/2010/06/14/Kinematics_ParticleSystem.html">用JavaScript玩转游戏物理：运动学模拟与粒子系统</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>