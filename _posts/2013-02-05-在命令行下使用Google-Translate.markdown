---
layout: post
title:  "在命令行下使用Google Translate"
date:   2013-02-05 16:00:00
author: 
categories: program
---

## 在命令行下使用Google Translate
### by 
### at 2013-02-05 16:00:00
### original <http://www.soimort.org/posts/138>

<p>所以说我还是发篇中文博介绍一下这个东西好了。貌似对其他人还蛮有用的样子。</p>

<p><strong>Google Translate CLI:</strong> <a href="http://www.soimort.org/google-translate-cli/">http://www.soimort.org/google-translate-cli/</a></p>

<hr>

<p>学习语言的时候，<a href="http://translate.google.com/">Google Translate</a>是一大神器，可惜要免费的话，只能在浏览器里用。API什么的是要收费的，<a href="https://developers.google.com/translate/v2/pricing">每百万字20刀</a>，当然还需要开发者自己注册API key，而且还有流量限额，真心麻烦……</p>

<p>个人用户可能只是想在终端下查查单词短语翻译几个句子（尤其是对于我这样的重度命令行用户来说，实在是不想每次都开个浏览器标签去查啊）。如果Google Translate提供了一套命令行接口（或者至少是开放的不需要认证key的API）就会方便很多，显然，这件事情永远都不可能发生。</p>

<p>还有，在文本编辑器里面翻译<del><a href="https://www.google.com/search?q=%E6%96%87%E6%98%A5">#文春</a>（误）</del>文章的时候，每次切换到浏览器里去查效率太低了（何况安心干活的时候可能根本就不想去开浏览器，巨耗资源）。如果能在vim或者emacs里一键查看翻译的话，岂不妙哉？</p>

<p>简而言之，想法就是在命令行下直接一个HTTP Request调用Google Translate来做翻译，不用浏览器，也不需要去用花钱的API key。</p>

<p>以上是背景铺垫。</p>

<p>以前的命令行工具差不多都是基于Google Translate的官方API，鉴于不要钱的API v1已经关闭了，目测它们已经失效（你总不能让用户去自己注册一个API key然后付了钱才能用你的工具吧……）</p>

<p>写了一个简单的hack，直接裸调与浏览器访问Google Translate时相同的HTTP Request（不用Google提供的API，当然也不需要key），解析出翻译的结果。用不超过100行AWK脚本实现，只要系统上有<a href="http://www.gnu.org/software/gawk/">GNU Awk</a>就能运行。不需要其他任何依赖库。</p>

<p>什么？为什么要拿AWK来写？解决这种程度的小问题，Perl、Python、Ruby之类的也太重量级了吧<img src="http://static.tieba.baidu.com/tb/editor/images/jd/sn_0010.gif" alt=""></p>

<p>源码在这里： <a href="https://github.com/soimort/google-translate-cli">https://github.com/soimort/google-translate-cli</a></p>

<h2>安装</h2>
<div><pre><code>$ git clone git://github.com/soimort/google-translate-cli.git
$ cd google-translate-cli/
$ make install
</code></pre></div>
<p>（不想安装的话，直接调用<code>./translate.awk</code>也可以）</p>

<p>查看帮助：</p>
<div><pre><code>$ translate
</code></pre></div>
<h2>示例</h2>

<p>翻译短语或句子：（默认任意语言转英文）</p>
<div><pre><code>$ translate ヘヴィー・ローテーション
Heavy Rotation
</code></pre></div>
<p>翻译多行句子：</p>
<div><pre><code>$ translate &quot;
&gt; Das ist doch kein Geheimnis
&gt; Nicht für dich und nicht für mich
&gt; Das Leben ist oft hart
&gt; Doch wir kämpfen - verlieren und gewinnen&quot;

It&#39;s no secret
Not for you and not for me
Life is often hard
But we fight - lose and win
</code></pre></div>
<p>翻译不知道什么鸟语到中文：<br>
（参数格式：<code>{源语言=目标语言}</code>，在源语言参数省略的情况下，让系统自动识别）</p>
<div><pre><code>$ translate {=zh} &#39;UEFI Secure Boot is fun!&#39;
UEFI安全启动是很好玩！
</code></pre></div>
<p>翻译中文到各国鸟语：<br>
（参数格式：<code>{源语言=目标语言1+目标语言2+...+目标语言n}</code>）</p>
<div><pre><code>$ translate {zh=en+fr+de+sv+es} 挽尊
Pull the statue
tirez la statue
Ziehen Sie die Statue
dra statyn
Tire de la estatua
</code></pre></div>
<p>查看汉字的正确发音<del>（误）</del>：<br>
（在语言代码前加上<code>@</code>即可）</p>
<div><pre><code>$ translate {=@zh} 夜露死苦
Yèlù sǐ kǔ

$ translate {ja=@ja} 夜露死苦
Yoroshiku
</code></pre></div>
<p>翻译一个文本文件：（到标准输出）</p>
<div><pre><code>$ translate {=zh} /usr/share/licenses/common/CCPL/cc-readme.txt
有6种不同的Creative Commons许可，所有这些都包括在内
在这licenses目录：

CC-BY- 3.0.txt - 署名
CC-BY- NC- 3.0.txt - 署名 - 非商业性使用
CC-BY- NC- ND- 3.0.txt - 署名 - 非商业性使用 - NoDerivs
CC-BY- NC- SA- 3.0.txt - 署名 - 非商业性使用 - 相同方式共享
CC-BY- ND- 3.0.txt - 署名 - 禁止演绎“
CC-BY- SA- 3.0.txt - 署名 - 相同方式共享

如果一个软件包使用这些许可证之一，它应该被引用如下：
许可（ CCPL ：BY- NC-SA “ ）
</code></pre></div>
<h2>Vim一键翻译</h2>

<p>在<code>.vimrc</code>里加上一行（假定需要日文转罗马字发音+英文翻译）：</p>
<div><pre><code>set keywordprg=trs\ {ja=@ja+en}
</code></pre></div>
<p>然后就可以在Vim中用<code>Shift-K</code>召唤Google翻译娘，即时查看光标下对应单词的翻译了。是不是很方便？<br>
（基本上，这东西的便捷程度完全取决于你的网速……）</p>

<p><img src="http://i.imgur.com/OK2UYyn.gif" alt=""></p>

<h2>参考链接</h2>

<p>更多示例请参见：<a href="http://www.soimort.org/google-translate-cli">http://www.soimort.org/google-translate-cli</a></p>

<p>Google Translate所使用的语言代码（地球人都知道的：<code>en</code>，<code>fr</code>，<code>de</code>，<code>ru</code>，<code>zh</code>/<code>zh-CN</code>，<code>zh-TW</code>，<code>ja</code>）<br>
详细列表可以参考这里：<a href="https://developers.google.com/translate/v2/using_rest#language-params">https://developers.google.com/translate/v2/using_rest#language-params</a></p>

<p><a href="https://github.com/soimort/google-translate-cli"><img style="border:0" src="https://s3.amazonaws.com/github/ribbons/forkme_right_orange_ff7600.png" alt="Fork me on GitHub"></a></p>