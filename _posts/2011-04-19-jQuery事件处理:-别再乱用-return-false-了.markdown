---
layout: post
title:  "jQuery事件处理: 别再乱用“return false”了"
date:   2011-04-19 01:51:30
author: yuanyi
categories: program
---

## jQuery事件处理: 别再乱用“return false”了
### by yuanyi
### at 2011-04-19 01:51:30
### original <http://heikezhi.com/2011/04/18/jquery-events-stop-misusing-return-false/>

<p>可能在你刚开始学习关于jQuery事件处理时，看到的第一个例子就是关于如何阻止浏览器执行默认行为，比如下面这段演示click事件的代码：</p>
<pre name="code">
$("a.toggle").click(function () {
    $("#mydiv").toggle();
    return false; // Prevent browser from visiting `#`
});
</pre>
<p>这个函数使用toggle来显示或者隐藏#mydiv，然后阻止浏览器继续访问href中指定的链接。</p>
<p>像上面这样的例子会让用户养成使用“return false”来阻止浏览器执行默认行为的坏习惯，在这篇文章里，我将会讨论关于阻止浏览器执行默认行为的两个非常重要的主题：</p>
<ul>
<li>选择正确的方法： return false还是preventDefault，stopPropagation或者stopImmediatePropagation</li>
<li>选择合适的位置，开始，结束，还是中间某个地方：你应该在事件回调的哪个部分取消浏览器执行默认行为？</li>
</ul>
<p>注意：当我在这篇文章中提到event bubbling（事件冒泡），我想表达的是大部分事件都是先在初始DOM上触发，然后再通过DOM树往上，在每一级父元素上触发，事件不会在兄弟节点或是子节点上冒泡（当事件向下冒泡时，我们叫它事件捕捉（event capturing))，你可以在这里了解更多关于<a href="http://www.nczonline.net/blog/2009/06/30/event-delegation-in-javascript/">事件起泡和捕捉</a>的介绍。</p>
<p><strong>选择正确的方法</strong></p>
<p>“return false”之所以被误用的如此厉害，是因为它看起来像是完成了我们交给它的工作，浏览器不会再将我们重定向到href中的链接，表单也不会被继续提交，但这么做到底有什么不对呢？</p>
<p>”return false“到底做了什么？</p>
<p>当你每次调用”return false“的时候，它实际上做了3件事情：</p>
<ul>
<li>event.preventDefault();</li>
<li>event.stopPropagation();</li>
<li>停止回调函数执行并立即返回。</li>
</ul>
<p>“等等”，你叫了起来！我只是想让浏览器停止继续执行默认行为而已，我不需要它去做另外2件事。</p>
<p>这3件事中用来阻止浏览器继续执行默认行为的只有preventDefault，除非你想要停止事件冒泡，否则使用return false会为你的代码埋下很大的隐患，让我们通过一个真实的例子来看看这样的误用会造成什么后果：</p>
<p>这是我们用来演示的HTML：</p>
<pre name="code">
<div>
<h2><a href="http://heikezhi.com/path/to/page">My Page</a></h2>
<div>
    Teaser text...
  </div>
</div>
<div>
<h2><a href="http://heikezhi.com/path/to/other_page">My Other Page</a></h2>
<div>
    Teaser text...
  </div>
</div>
</pre>
<p>现在假设我们想要在用户点击文章标题时，将文章动态载入到div.contentd中：</p>
<pre name="code">
jQuery(document).ready(function ($) {
  $("div.post h2 a").click(function () {
    var a    = $(this),
    href = a.attr('href'), // Let jQuery normalize `href`,
    content  = a.parent().next();
    content.load(href + " #content");
    return false; // "cancel" the default behavior of following the link
  });
});
</pre>
<p>这段代码可以正常工作（至少目前是），但如果我们顺着这个思路继续，如果我想要在用户点击了一个div.post元素（或者任何一个它的子元素）时，给它加上一个active类，我就需要给div.post增加了一个click回调：</p>
<pre name="code">
// Inside Document Ready:
var posts = $("div.post");
  posts.click(function () {
  // Remove active from all div.post
  posts.removeClass("active");
  // Add it back to this one
  $(this).addClass("active");
});
</pre>
<p>现在，如果我们点击一个帖子的标题，这段代码会工作吗？答案是不会，因为我们在标题的click回调里使用了return false而不是我们应该使用的，”return false“等于event.preventDefault();加event.stopPropagation();，所以事件冒泡就被终止了，click事件不会被冒泡到div.post上，我们为它添加的事件回调当然也就不会被调用了。</p>
<p>如果我们把它和live或者delegate事件混在一起使用时，情况就更糟了。</p>
<pre name="code">
$("a").click(function () {
  // do something
  return false;
});

$("a").live("click", function () {
  // THIS WON'T FIRE
});
</pre>
<p>那么我们真正需要的是什么呢？</p>
<p><strong>preventDefault()</strong></p>
<p>大多数情况下，当你使用return false时，你其实真正需要的是e.preventDefault()。要使用e.preventDefault，你需要确保你传递了event参数到你的回掉函数中（在这个例子里，就是那个e）：</p>
<pre name="code">
$("a").click(function (e) {
  // e == our event data
  e.preventDefault();
});
</pre>
<p>它会替我们完成所有工作，但不会阻止父节点继续处理事件，要记住，你放在代码中的限制越少，你的代码就越灵活，也就越易于维护。</p>
<p><strong>stopPropagation()</strong></p>
<p>但有些情况下，你有可能需要停止事件冒泡，让我们看看下面的例子：</p>
<pre name="code">
<div>
    Normal text and then a <a href="http://heikezhi.com/path">link</a> and then more text.
</div>
</pre>
<p>现在，让我们假设如果你点了div上除了a链接之外的地方，我们希望能发生点什么事情（比如改变下背景什么的），但是不能影响用户点击a链接的行为（从可用性的角度，这个例子不怎么好，你可能不希望用户点击别的地方时发生任何事情）。</p>
<pre name="code">
$("div.post").click(function () {
  // Do the first thing;
});

$("div.post a").click(function (e) {
  // Don't cancel the browser's default action
  // and don't bubble this event!
  e.stopPropagation();
});
</pre>
<p>在这种情况下，如果我们使用return false，div的click事件不会被触发，但是用户也不会到达他们点的那个链接。</p>
<p><strong>stopImmediatePropagation()</strong></p>
<p>这个方法会停止一个事件继续执行，即使当前的对象上还绑定了其它处理函数，所有绑定在一个对象上的事件会按绑定顺序执行，看看下面的例子：</p>
<pre name="code">
$("div a").click(function () {
  // Do something
});

$("div a").click(function (e) {
  // Do something else
  e.stopImmediatePropagation();
});

$("div a").click(function () {
  // THIS NEVER FIRES
});

$("div").click(function () {
  // THIS NEVER FIRES
});
</pre>
<p>你可能会觉得这个例子看起来很别扭，没错，尽管如此，但有时这的确会发生，如果你的代码非常复杂，那么不同的widgets和plugin就有可能在同一个对象上添加事件，如果遇到这种情况，那你就很有必要理解和使用stopImmediatePropagation。</p>
<p><strong>return false</strong></p>
<p>只有当你同时需要preventDefault和stopPropagation，并且你的代码可以接受直到你的回调执行完成才停止执行浏览器的默认行为，那你就可以使用”return false“。但我强烈建议你别在写给其它jQuery开发者的演示代码中使用这个方法，因为这会造成更多误用，只有在你确信非用不可的情况下再去使用”return false“。</p>
<p><strong>选择适当的位置</strong></p>
<p>如果你使用了”return false“，它只会在你的回调函数执行结束才去取消浏览器的默认行为，但是使用e.preventDefault，我们有更多的选择，它可以随时停止浏览器执行默认动作，而不管你将它放在函数的哪个部分。</p>
<p>1. 开发阶段，你应该总是将它放在第一行。你最不想做的事情可能就是你正在调试将一个form改成ajax提交的时候，它却已经被按照老方法提交了。</p>
<p>2.产品阶段，如果你采用了渐进增强（<strong>progressive enhancement）</strong>，那就把它放到回调的结束位置，或者是逻辑终点，如果在一个普通页面采用渐进增强，那你就需要在服务器端考虑如果浏览器不支持JS时（或者被禁用时），对链接的click事件和表单的提交事件的处理。这里的好处是，我们不考虑关闭JS的情况，只考虑支持js时的强狂，如果你的回调代码出错抛出了异常，让我们看看下面的代码：</p>
<pre name="code">
var data = {};
$("a").click(function (e) {
  e.preventDefault(); // cancel default behavior
  // Throws an error because `my` is undefined
  $("body").append(data.my.link);
  // The original link doesn't work AND the "cool"
  // JavaScript has broken. The user is left with NOTHING!
});
</pre>
<p>现在，让我们看看同样的事件，把preventDefault调用放在底部的效果：</p>
<pre name="code">&gt;
var data = {};
$(&quot;a&quot;).click(function (e) {
  // Throws an error because `my` is undefined
  $(&quot;body&quot;).append(data.my.link);

  // This line is never reached, and your website
  // falls back to using the `href` instead of this
  // &quot;cool&quot; broken JavaScript!
  e.preventDefault(); // cancel default behavior
});
</pre>
<p>这对表单提交也同样有效，你可以更好的应对出错的情况，别指望你的代码一直正常工作，在发生错误时有正确的应对总胜过假设代码不会出错。</p>
<p>3. 在产品阶段，如果功能这设计JS，那就还应该放在第一行。</p>
<p>记住，不必非得是函数的第一行，但是越早越好，这里的原则是：如果函数的功能是通过JS实现的（不涉及服务端交互），那就没必要考虑兼容，在这种情况下，添加在第一行可以防止URL中出现#字符，但显然，你还是应该尽可能多的增加些错误处理代码，以防止用户在出错时变得不知所措。</p>
<p><strong>结论</strong></p>
<p>我希望这篇文章传达的信息足够你在需要阻止浏览器执行默认行为时做出正确的选择。记住，只有当你真的明白你在做什么时，才使用”return false“，并确保你是在函数的正确位置调用了相应的代码。最后，尽可能保持代码的灵活性，尽量不要再用“return false”了！</p>
<p>本文翻译自“<a href="http://fuelyourcoding.com/jquery-events-stop-misusing-return-false/">jQuery Events: Stop (Mis)Using Return False</a>”，作者：<a href="http://fuelyourcoding.com/author/dougnein/">Douglas Neiner</a>。</p>
<p>想和我们一道传播黑客精神？<a href="http://heikezhi.com/join">快来加入吧！</a></p>
<table cellspacing="0" cellpadding="2" border="0" width="100%" style="clear:both">
    
    <tr>
        <td><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">无觅猜您也喜欢：</font></b></td>
    </tr>
    
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F21%2F10-things-i-learned-from-the-jquery-source%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F18%2Fjquery-events-stop-misusing-return-false%2F">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:12px!important">我从jQuery源码中学到的10堂课</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F03%2Fa-simple-exercise-to-train-your-photographic-eye%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F18%2Fjquery-events-stop-misusing-return-false%2F">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:12px!important">一个训练你摄影技巧的方法</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F17%2F%25E5%2588%259B%25E4%25B8%259A%25E5%2585%25AC%25E5%258F%25B8%25E5%258A%259E%25E5%2585%25AC%25E5%25AE%25A4%25E6%2597%25B6%25E9%2597%25B4%25EF%25BC%259F%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F18%2Fjquery-events-stop-misusing-return-false%2F">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:12px!important">创业公司办公室时间？</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F09%2F13-things-you-must-do-every-week-as-a-startup-ceo%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F18%2Fjquery-events-stop-misusing-return-false%2F">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:12px!important">创业公司CEO每周必做的13件事</font>
                    </a>
                </td>
            </tr>
    
    <tr>
        <td align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/502699840/heikezhi/feedsky/s.gif?r=http://heikezhi.com/2011/04/18/jquery-events-stop-misusing-return-false/" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/heikezhi/502699840/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/heikezhi/502699840/art01.gif"></a></p>