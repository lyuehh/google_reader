---
layout: post
title:  "Traceur: 体验下一代Javascript语言"
date:   2011-05-07 01:26:09
author: yuanyi
categories: program
---

## Traceur: 体验下一代Javascript语言
### by yuanyi
### at 2011-05-07 01:26:09
### original <http://heikezhi.com/2011/05/06/traceur-compiler-next-to-javascript-of-today/>

<p><a href="http://code.google.com/p/traceur-compiler/">Traceur项目</a>旨在让你预先体验还处于草案阶段的下一代<a href="http://code.google.com/p/traceur-compiler/wiki/LanguageFeatures">Javascript语言新特性</a>，并根据实际运行的代码来提供<a href="http://groups.google.com/group/traceur-compiler-discuss">反馈</a>，帮助标准制定。</p>
<p>要体验Traceur非常简单，只需要在你的HTML文件头部包含两个js文件即可：</p>
<pre name="code">
&lt;html&gt;
  &lt;head&gt;
    ...
    &lt;script src=&quot;http://traceur-compiler.googlecode.com/svn/branches/v0.10/src/traceur.js&quot;        type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;http://traceur-compiler.googlecode.com/svn/branches/v0.10/src/bootstrap.js&quot;        type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
  &lt;/head&gt;
&lt;/html&gt;
</pre>
<p>traceur.js是编译器，bootstrap.js则负责将所有script标签中的脚本编译为当前浏览器可执行的Javascript，如果不想这么麻烦，你也可以直接通过这个<a href="http://code.google.com/p/traceur-compiler/wiki/GettingStarted">在线模拟器</a>来体验。</p>
<p>目前Traceur支持以下新的语言特性，每个都十分令人期待：</p>
<ul>
<li>类继承</li>
<li>Traits，traits特性让你可以将一组方法打包起来，然后混入一个或多个类中，类似Ruby语言中的mixin功能</li>
<li>Modules，Modules主要用来解决模块依赖和部署问题，让你可以有选择的从外部模块导入指定的部分。</li>
<li>迭代器(Iterators)</li>
<pre name="code">
for (let element : [1, 2, 3]) {
  console.log(element);
}
</pre>
<li>生成器(Generators)，生成器使用yield语法让创建迭代器变得更简单，而无需再去实现__iterator__</li>
<li>延时函数(Deferred Functions)，延时函数让你可以不用通过回调函数来执行异步的非阻塞代码，而是使用await函数来刮起当前函数的执行并返回一个包含当前函数执行状态的对象：</li>
<pre name="code">
function deferredAnimate(element) {
    for (var i = 0; i &lt; 100; ++i) {
        element.style.left = i;
        await deferredTimeout(20);
    }
};
deferredAnimate(document.getElementById(&#39;box&#39;));
</pre>
<li>块范围绑定(Block Scoped Bindings)</li>
<pre name="code">
{
  const tmp = a;
  a = b;
  b = tmp;
}
alert(tmp); // error: 'tmp' is not defined.
</pre>
<li>自动解包赋值(Destructuring Assignment)</li>
<pre name="code">
var [a, [b], c, d] = ['hello', [', ', 'junk'], ['world']];
alert(a + b + c); // hello, world
</pre>
<li>函数默认参数(Default Parameters)</li>
<pre name="code">
function slice(list, indexA = 0, indexB = list.length) {
  // ...
}
</pre>
<li>剩余参数（Rest Parameters）</li>
<pre name="code">
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
  });
}
</pre>
<li>展开操作符(Spread Operator)，作为其余参数的辅助，展开操作符负责将剩余参数进行展开：</li>
<pre name="code">
function push(array, ...items) {
  array.push(...items);
}
function add(x, y) {
  return x + y;
}
var numbers = [4, 38];
add(...numbers);  // 42
</pre>
</ul>
<p>更多关于新特性及实现细节的介绍请看<a href="http://code.google.com/p/traceur-compiler/wiki/LanguageFeatures">这里</a>。</p>
<p>想和我们一道传播黑客精神？<a href="http://heikezhi.com/join">快来加入吧！</a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">无觅猜您也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Handlebars.js入门介绍" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F30%2Fhandlebars-js%25E5%2585%25A5%25E9%2597%25A8%25E4%25BB%258B%25E7%25BB%258D%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F05%2F06%2Ftraceur-compiler-next-to-javascript-of-today%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/03/30/4588419.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Handlebars.js入门介绍</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="超简单的MapReduce Javascript实现" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F05%2F17%2Fsimple-mapreduce-with-javascript%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F05%2F06%2Ftraceur-compiler-next-to-javascript-of-today%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">超简单的MapReduce Javascript实现</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="你不懂Javascript" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F04%2F26%2Fyou-dont-know-javascript%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F05%2F06%2Ftraceur-compiler-next-to-javascript-of-today%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">你不懂Javascript</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Javascript版Heroku: Akshell" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F22%2Fakshell-a-heroku-clone-for-javascript%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F05%2F06%2Ftraceur-compiler-next-to-javascript-of-today%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/03/23/4089256.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Javascript版Heroku: Akshell</font>
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
</table><img src="http://www1.feedsky.com/t1/512596489/heikezhi/feedsky/s.gif?r=http://heikezhi.com/2011/05/06/traceur-compiler-next-to-javascript-of-today/" border="0" height="0" width="0">