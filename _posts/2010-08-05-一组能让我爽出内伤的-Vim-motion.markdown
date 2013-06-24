---
layout: post
title:  "一组能让我爽出内伤的 Vim motion"
date:   2010-08-05 11:17:38
author: asins
categories: program
---

## 一组能让我爽出内伤的 Vim motion
### by asins
### at 2010-08-05 11:17:38
### original <http://nootn.com/blog/Tool/37/>

<p>今天有人在群里说VIM的motionn特性，后去搜索了下，顿时让我爽得想哭啊，这就不是我一直所寻找的嘛！</p>

<p>对于天天写HTML的我来说命令<code>vit</code>、<code>vat</code>、<code>vi"</code>真的太有用了，尤其<code>vi"</code>编辑属性就别太爽，以前一直是移动到属性区域按<code>i</code>后再疯狂按删除键。</p>

<p>用vim这么长时间，现看到下边几句，对vim的认识提高一个台阶。呵呵~~</p>

<p>以下是选择区域内的内容：</p>

<pre><code>ci[ 删除一对 [] 中的所有字符并进入插入模式
ci( 删除一对 () 中的所有字符并进入插入模式
ci&lt; 删除一对 &lt;&gt; 中的所有字符并进入插入模式
ci{ 删除一对 {} 中的所有字符并进入插入模式
cit 删除一对 HTML/XML 的标签内部的所有字符并进入插入模式
ci” ci’ ci` 删除一对引号字符 (” 或 ‘ 或 `) 中所有字符并进入插入模式
</code></pre>

<p><strong>注：如果想一起删除区域边界可把<code>ciX</code>中的<code>i</code>换成<code>a</code>；如果只是想选择这些区域可将<code>ciX</code>中的<code>c</code>改为<code>v</code></strong></p>