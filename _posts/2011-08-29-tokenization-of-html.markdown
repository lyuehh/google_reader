---
layout: post
title:  "tokenization of html"
date:   2011-08-29 22:38:50
author: 
categories: program
---

## tokenization of html
### by 
### at 2011-08-29 22:38:50
### original <http://yiminghe.iteye.com/blog/1160895>

<h1 style="text-align:center">html 符号解析问题</h1>
<p> </p>
<h2>场景：</h2>
<p> </p>
<p>在页面上输出包含已有数据的 textarea ，一般的做法即是，将所有的数据从数据库取出后都 <a href="http://docs.kissyui.com/docs/html/api/seed/lang/escapeHTML.html">escapeHtml </a>
一下：</p>
<p> </p>
<pre name="code">&lt;textarea&gt;&amp;lt;script&amp;gt;if(a&amp;amp;&amp;amp;1)alert(1);&amp;lt;script&amp;gt;&lt;/textarea&gt;</pre>
<p> </p>
<p>页面瞬时增大了很多，特别是对于富文本情况（包含了很多 &lt; &gt; &amp;） 等，但这又是必须做的，否则会被恶意结束 textarea 标签而造成 script 注入问题，但还是存在可以进一步减少体积的余地。</p>
<p> </p>
<p> </p>
<h2>
规范：
</h2>
<p> </p>
<p>查看 html 对 textarea 标签及其内容的<a href="http://www.w3.org/TR/html5/tree-construction.html">解析规则</a>
:实际上textarea 的内容解析规则是按照 <a href="http://www.w3.org/TR/html5/tokenization.html#rcdata-state">RCDATA </a>
类型（一系列状态），简单描述如下</p>
<p> </p>
<p>1. 遇到 &amp; ，尽力得到<a href="http://yiminghe.iteye.com/blog/788929">实体字符</a>
代表的值</p>
<p>2. 遇到 &lt; , 如果下一个字符为 / 则结束当前标签</p>
<p>3. 否则作为 textarea 内容</p>
<p> </p>
<p>恶意结束标签主要发生在 2 ，那么我们只要打破 2 ，保证 &lt; 和 / 不相连即可，在服务器端渲染页面事先只做一点处理，再把处理后的内容放在 textarea 中：</p>
<p> </p>
<pre name="code">&quot;&lt;a&gt;x&lt;/&gt;&quot;.replace(/&lt;\//gi,&quot;&amp;lt;/&quot;)</pre>
<p> </p>
<p>而如果允许用户输入 &amp;gt; 等代表实体字符的字符串，则还要进行 &amp; 替换：</p>
<p> </p>
<pre name="code">&quot;&lt;a&gt;x&lt;/&gt;&quot;.replace(/&amp;/gi,&quot;&amp;amp;&quot;)</pre>
<p> </p>
<p>最终页面体积也能减小不少.</p>
<p> </p>
<h2>
其他类型：
</h2>
<p> </p>
<p> tokenization 部分还存在其他特殊类型的解析规则，比如常见的 <a href="http://javascript.about.com/library/blxhtml.htm">script </a>
，在 xhtml 时代，推荐的写法是:</p>
<p> </p>
<pre name="code">&lt;script type=&quot;text/javascript&quot;&gt;
/* &lt;![CDATA[ */
// TODO
/* ]]&gt; */
&lt;/script&gt; </pre>
<p> </p>
<p>这在 html 中也是支持的， <a href="http://www.w3.org/TR/html5/tokenization.html#cdata-section-state">cdata 的解析规则</a>
 比较简单：在遇到 ]]&gt; 之前都算作 script 的内容，并不解析实体字符.</p>
<p> </p>
<p>而 html 则是完全可以不用 cdata 这个规则的 ，script 解析本身就是一种单独规则了：</p>
<p> </p>
<p>1. 类似 rcdata (因此代码中不能出现类似 &quot;&lt;/script&gt;&quot;，必须改做 &quot;&lt;\/script&gt;&quot;)，但是不解析实体字符</p>
<p>2. 还识别了 &lt;!-- ，似乎是为了兼容不支持js的远古浏览器的注释：</p>
<p> </p>
<p> </p>
<pre name="code">&lt;script type=&quot;text/javascript&quot;&gt;
&lt;!-- // hide from really old browsers that noone uses anymore
// TODO
// --&gt;
&lt;/script&gt; </pre>
<p> </p>
<p>这在现在看来则是完全不必要了。</p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
              
              <br><br>
              <span style="color:red">
                <a href="http://yiminghe.iteye.com/blog/1160895#comments" style="color:red">已有 <strong>2</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://yiminghe.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>