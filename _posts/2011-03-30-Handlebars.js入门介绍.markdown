---
layout: post
title:  "Handlebars.js入门介绍"
date:   2011-03-30 16:23:52
author: yuanyi
categories: program
---

## Handlebars.js入门介绍
### by yuanyi
### at 2011-03-30 16:23:52
### original <http://heikezhi.com/2011/03/30/handlebars-js%e5%85%a5%e9%97%a8%e4%bb%8b%e7%bb%8d/>

<p><a href="http://handlebars.strobeapp.com/"><img src="http://heikezhi.com/wp-content/uploads/2011/03/handlebars.png" alt="" title="handlebars" width="705" height="205"></a>现如今各种网络应用比以往任何时候都更倾向于使用JavaScript来创建动态的用户界面，并且这一趋势至少在短期不会改变。DOM操作对于简单的JS应用来说不成问题，但如果你对视图的每次更新都需要对我文档中非常大量的块进行操作呢？这时JS模版就派上用场了。</p>
<p>但能够让人感到眼前一亮的JS模版库实在不多，刚开始，我使用的是一个叫<a href="https://github.com/janl/mustache.js">mustache.js</a>的模版库，它是非常棒的Mustache模版语言的JS实现，后来我就转向使用John Resig的<a href="http://ejohn.org/blog/javascript-micro-templating/">JavaScript Micro-Templating</a>。jQuery有一个官方的模版插件，还有Underscore.js，37signals甚至也有他们自己的JS模版语言eco，尽管它是针对CoffeeScript的。而我个人最喜欢的JS模版语言则是Handlebars.js。</p>
<p>必须说明我的选择是带有倾向性的——因为Handlebars.js是我和Yehuda Katz一块编写的。我们创造Handlebars的初衷是因为我们都喜欢Mustache的“Logic-less templating（无逻辑模版）”的思路，并且我们都经受过时不时跳出来调用全局Helper的折磨，以及没法在模版的调用栈上访问变量的痛苦。同时，我们也都希望模版能够在加载前被预编译，而不是到了客户端再去编译，我们希望我们能够尽最大可能写一个最快的模版语言。尽管我们离最快还有差距，但是Handlebars.js已经非常快并且实现了我们设定的大部分目标。</p>
<p><strong>安装和使用</strong></p>
<p>Handlebars的安装非常简单，你只需要<a href="https://github.com/wycats/handlebars.js/downloads">从Github下载最新版本</a>，我们离发布1.0版本还有点距离，但是handlebars.js已经被许多项目广泛使用了，handlebars是一个纯JS库，因此你可以像使用其他JS脚本一样用script标签来包含handlebars.js：</p>
<pre name="code">

</pre>
<p>一开始，你可能想在文件中直接内联模版代码来体验handlebars，很简单，你可以用一个自定义类型的script标签将模版直接包含在文档中：</p>
<pre name="code">

</pre>
<p>接下来你可以使用下面的代码来编译，处理以及显示模版。</p>
<pre name="code">
  var source   = $("#some-template").html();
  var template = Handlebars.compile(source);
  var data ={ users:[
      {username:"alan", firstName:"Alan", lastName:"Johnson", email:"alan@test.com"},
      {username:"allison", firstName:"Allison", lastName:"House", email:"allison@test.com"},
      {username:"ryan", firstName:"Ryan", lastName:"Carson", email:"ryan@test.com"}
    ]};
  $("#content-placeholder").html(template(data));
</pre>
<p>在上面的代码里我使用了jQuery，但是Handlebars并不依赖jQuery，因此，你也可以使用其他任何一个你喜欢的框架。还有一点就是handlebars会将模版编译成一个JS函数，这样使用起来就方便多了。</p>
<p><strong>基本表达式</strong></p>
<p>handles模版中最简单的动态元件就是表达式（expression），每个表达式像{{表达式}}这样由两个大括号包裹，当模版中出现一个表达式，handlebars会根据上下文来自动对表达式进行匹配，如果匹配项是个变量，则会输出变量的值，如果匹配项是个函数，则函数会被调用。如果没找到匹配项，则没有输出。表达式也支持点操作符，因此你可以使用{{user.firstName}}这样的形式来调用嵌套的值或者方法，handlebars会根据当前上下文输出user变量的firstName属性的值。</p>
<p>另外，handlebars默认会对结果进行escape，如果你希望将结果原样输出，可以使用{{{表达式}}}来告诉handlebars不要对结果进行escape。</p>
<p><strong>Blocks</strong>     </p>
<p>有时候当你需要对某条表达式进行更深入的操作时，Blocks就派上用场了，在Handlebars中，你可以在表达式后面跟随一个#号来表示Blocks，然后通过{{/表达式}}来结束Blocks。</p>
<p>如果当前的表达式是一个数组，则Handlebars会自动展开数组，并将Blocks的上下文设为数组中的项目，让我们来看看下面的例子：</p>
<pre name="code">
var data ={ people:[
    {name:"Alan"},
    {name:"Allison"},
    {name:"Ryan"}
  ], group:"Bloggers"};
</pre>
<pre name="code">

</pre>
<p>因为Blocks改变了表达式的上下文，handlebars提供了”../表达式“方式供Blocks中的表达式访问上一层的上下文，所以在上面的例子中，我们可以使用../group很方便的输出group的值：</p>
<pre name="code">

</pre>
<p>如果表达式不是一个数组，则Blocks的执行上下文不会变化，这样你可以少打很多字来输出对象的属性。</p>
<pre name="code">
var data ={ person:{
    firstName:"Alan",
    lastName:"Johnson",
    email:"alan@test.com",
    phone:"123-456-7890"
  }};
</pre>
<pre name="code">

</pre>
<p><strong>然后呢？</strong></p>
<p>当然，还有一大堆东西没有讲到，所以我会在下周继续开个新贴，讲讲Handlebars.js的高级技巧。我们会涉及Partials，Blocks helper，Global helper，以及如何预编译你的模版，而不是让客户端去编译它们。</p>
<p><a href="http://thinkvitamin.com/code/getting-started-with-handlebars-js/">原文链接</a>，作者：<a href="http://thinkvitamin.com/author/alanjohnson/">Alan Johnson</a>，Twitter：<a href="http://twitter.com/thinkvitamin">@thinkvitamin</a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">无觅猜您也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Javascript版Heroku: Akshell" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F22%2Fakshell-a-heroku-clone-for-javascript%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F30%2Fhandlebars-js%25E5%2585%25A5%25E9%2597%25A8%25E4%25BB%258B%25E7%25BB%258D%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/03/23/4089256.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Javascript版Heroku: Akshell</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="JavaScript版查找树介绍及性能分析" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F26%2Fjavascript-trie-performance-analysis%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F30%2Fhandlebars-js%25E5%2585%25A5%25E9%2597%25A8%25E4%25BB%258B%25E7%25BB%258D%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/03/27/4332087.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">JavaScript版查找树介绍及性能分析</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="所以，你要试试SICP" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F27%2Fso-you-want-to-do-the-sicp%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F30%2Fhandlebars-js%25E5%2585%25A5%25E9%2597%25A8%25E4%25BB%258B%25E7%25BB%258D%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/03/28/4419890.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">所以，你要试试SICP</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="将数学变成音乐: 那么Pi听起来会如何呢？" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F13%2Fwhat-pi-sounds-like%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F30%2Fhandlebars-js%25E5%2585%25A5%25E9%2597%25A8%25E4%25BB%258B%25E7%25BB%258D%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">将数学变成音乐: 那么Pi听起来会如何呢？</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="4" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/494773524/heikezhi/feedsky/s.gif?r=http://heikezhi.com/2011/03/30/handlebars-js%e5%85%a5%e9%97%a8%e4%bb%8b%e7%bb%8d/" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/heikezhi/494773524/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/heikezhi/494773524/art01.gif"></a></p>