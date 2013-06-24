---
layout: post
title:  "artTemplate 自定义语法扩展"
date:   2012-07-15 21:39:47
author: admin
categories: program
---

## artTemplate 自定义语法扩展
### by admin
### at 2012-07-15 21:39:47
### original <http://www.planeart.cn/?p=1839>

<p>artTemplate 默认采用的是原生的 javascript 语法，类似微型模板引擎 tmpl。不同的是 artTemplate 的模板会采用沙箱机制限制全局对象的读写，这种机制很大程度上能够避免模板产生维护性的问题：若模板中若引用外部对象，随着项目复杂度增加，那时候谁都不能确定模板中的变量到底是数据还是全局对象，这种复杂的依赖关系将为会项目带来巨大的维护成本。在 artTemplage 的设计中，公用的方法与数据由 template.helper() 方法管理，既满足定义公用辅助方法的需求，也避免依赖外部对象产生维护问题。</p>
<p>众所周知 javascript 的语法非常自由，模板逻辑语句使用原生语法对于 javascript 开发人员来说几乎没有学习成本。但不可否认，模板的逻辑使用最多的也不过是条件表达式与循环表达式，而采用原生语法未免有大炮打蚊子的感觉，并且纯 javascript 语法对页面设计人员来说并不是那么容易掌握，因此设计一种简单易用的模板语法还是有必要的。</p>
<p>而 javascript 模板引擎语法无外乎三种类型：<br>
<span></span></p>
<p><strong>1、崇尚强大与自由的原生语法派</strong></p>
<pre>
&lt;h3&gt;&lt;%=title%&gt;&lt;/h3&gt;
&lt;ul&gt;
    &lt;% for (i = 0, l = list.length; i &lt; l; i ++) { %&gt;
        &lt;li&gt;name: &lt;%=list[i].user%&gt;; url: &lt;%=list[i].site%&gt;&lt;/li&gt;
    &lt;% } %&gt;
&lt;/ul&gt;
</pre>
<p><strong>2、崇尚干净利落的无逻辑派</strong></p>
<pre>
&lt;h3&gt;{{title}}&lt;/h3&gt;
&lt;ul&gt;
    {{#list}}
        &lt;li&gt;name: {{user}}; url: {{site}}&lt;/li&gt;
    {{/list}}
&lt;/ul&gt;
</pre>
<p>其中原生语法派以 tmpl 为代表，无逻辑派以 Mustache 为代表。原生语法派与无逻辑派各有优劣：tmpl 语法最为自由且强大，但弊端就是对设计人员不友好，阅读起来也比较吃力；而 Mustache 对设计人员非常友好，也非常利于模板迁移，但这种无逻辑语法弊端也十分明显，连最基本的数组索引值都没法输出。</p>
<p><strong>3、求功能与易用平衡的中庸派</strong></p>
<pre>
&lt;h3&gt;{{title}}&lt;/h3&gt;
&lt;ul&gt;
    {{#each list as val}}
        &lt;li&gt;name: {{val.user}}; url: {{val.site}}&lt;/li&gt;
    {{/each}}
&lt;/ul&gt;
</pre>
<p>中庸派较好的在功能与易用性之前取得了平衡，而且这种类型的模板语法也经过了后端模板的工业级应用考验，相对来说比较成熟。artTemplate 的自定义语法扩展也采用此类型语法，上面的模板例子用 artTemplate 的写法：</p>
<pre>
&lt;h3&gt;{title}&lt;/h3&gt;
&lt;ul&gt;
    {each list}
        &lt;li&gt;name: {$value.user}; url: {$value.site}&lt;/li&gt;
    {/each}
&lt;/ul&gt;
</pre>
<p>注：若没有特别说明，文章以下提到的 artTemplate 语法均指自定义语法。</p>
<h2>安装扩展</h2>
<p>只需要把 template-syntax.js 合并到 template.js 即完成自定义语法扩展安装。</p>
<h2>表达式</h2>
<p>&quot;{&quot; 与 &quot;}&quot; 符号包裹起来的语句则为模板的逻辑表达式。这两个界定符是可以自定义的，对应的配置属性接口是 template.openTag 与 template.closeTag。例如用 HTML 注释当作逻辑界定符：</p>
<pre>
template.openTag = &#39;&lt;!--{&#39;;
template.colseTag = &#39;}--&gt;&#39;;
</pre>
<h3>输出表达式</h3>
<p>对内容编码输出：</p>
<pre>
{content}
</pre>
<p>编码可以防止数据中含有 HTML 字符串，避免引起 XSS 攻击。</p>
<p>不编码输出：</p>
<pre>
{echo content}
</pre>
<h3>条件表达式</h3>
<pre>
{if admin}
    {content}
{/if}
</pre>
<pre>
{if user <h3> &#39;admin&#39;}
    {content}
{else if user <h3> &#39;007&#39;}
    &lt;strong&gt;hello world&lt;/strong&gt;
{/if}
</h3></h3></pre>
<h3>遍历表达式</h3>
<p>无论数组或者对象都可以用 each 进行遍历。</p>
<pre>
{each list}
    &lt;li&gt;{$index}. {$value.user}&lt;/li&gt;
{/each}
</pre>
<p>其中 list 为要遍历的数据名称, $value 与 $index 是系统变量， $value 表示数据单条内容, $index 表示索引值，这两个变量也可以自定义：</p>
<pre>
{each list as value index}
    &lt;li&gt;{index}. {value.user}&lt;/li&gt;
{/each}
</pre>
<h3>模板包含表达式</h3>
<p>用于嵌入子模板。</p>
<pre>
{include &#39;templateID&#39; data}
</pre>
<p>其中 &#39;templateID&#39; 是外部模板的 ID, data 为传递给 &#39;templateID&#39; 模板的数据。 data 参数若省略，则默认传入当前模板的数据。</p>
<pre>
{include &#39;templateID&#39;}
</pre>
<h2>辅助方法</h2>
<p>先使用 template.helper() 注册公用辅助方法，例如一个简单的 UBB 替换方法：</p>
<pre>
template.helper(&#39;$ubb2html&#39;, function (content) {
    return content
    .replace(/\[b\]([^\[]*?)\[\/b\]/igm, &#39;&lt;b&gt;$1&lt;/b&gt;&#39;)
    .replace(/\[i\]([^\[]*?)\[\/i\]/igm, &#39;&lt;i&gt;$1&lt;/i&gt;&#39;)
    .replace(/\[u\]([^\[]*?)\[\/u\]/igm, &#39;&lt;u&gt;$1&lt;/u&gt;&#39;)
    .replace(/\[url=([^\]]*)\]([^\[]*?)\[\/url\]/igm, &#39;&lt;a href=&quot;$1&quot;&gt;$2&lt;/a&gt;&#39;)
    .replace(/\[img\]([^\[]*?)\[\/img\]/igm, &#39;&lt;img src=&quot;$1&quot; /&gt;&#39;);
});
</pre>
<p>模板中使用的方式：</p>
<pre>
{$ubb2html content}
</pre>
<p>若辅助方法有多个参数使用一个空格分隔即可：</p>
<pre>
{helperName args1 args2 args3}
</pre>
<p>artTemplate 主页：<a href="http://aui.github.com/artTemplate/" title="http://aui.github.com/artTemplate/">http://aui.github.com/artTemplate/</a></p>