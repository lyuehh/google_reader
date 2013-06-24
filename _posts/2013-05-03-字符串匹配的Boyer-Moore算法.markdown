---
layout: post
title:  "字符串匹配的Boyer-Moore算法"
date:   2013-05-03 13:47:25
author: 
categories: program
---

## 字符串匹配的Boyer-Moore算法
### by 
### at 2013-05-03 13:47:25
### original <http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html>

<p>上一篇文章，我介绍了<a href="http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html">KMP算法</a>。</p><p>但是，它并不是效率最高的算法，实际采用并不多。各种文本编辑器的"查找"功能（Ctrl+F），大多采用<a href="http://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_string_search_algorithm">Boyer-Moore算法</a>。</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050301.jpg"></p>

<p>Boyer-Moore算法不仅效率高，而且构思巧妙，容易理解。1977年，德克萨斯大学的Robert S. Boyer教授和J Strother Moore教授发明了这种算法。</p>

<p>下面，我根据Moore教授自己的<a href="http://www.cs.utexas.edu/~moore/best-ideas/string-searching/fstrpos-example.html">例子</a>来解释这种算法。</p>

<p>1.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050302.png"></p>

<p>假定字符串为"HERE IS A SIMPLE EXAMPLE"，搜索词为"EXAMPLE"。</p>

<p>2.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050303.png"></p>

<p>首先，"字符串"与"搜索词"头部对齐，从尾部开始比较。</p>

<p>这是一个很聪明的想法，因为如果尾部字符不匹配，那么只要一次比较，就可以知道前7个字符（整体上）肯定不是要找的结果。</p>

<p>我们看到，"S"与"E"不匹配。这时，<strong>"S"就被称为"坏字符"（bad character），即不匹配的字符。</strong>我们还发现，"S"不包含在搜索词"EXAMPLE"之中，这意味着可以把搜索词直接移到"S"的后一位。</p>

<p>3.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050304.png"></p>

<p>依然从尾部开始比较，发现"P"与"E"不匹配，所以"P"是"坏字符"。但是，"P"包含在搜索词"EXAMPLE"之中。所以，将搜索词后移两位，两个"P"对齐。</p>

<p>4.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050305.png"></p>

<p>我们由此总结出<strong>"坏字符规则"</strong>：</p>

<blockquote>

<p>　　后移位数 = 坏字符的位置 - 搜索词中的上一次出现位置</p>

</blockquote>

<p>如果"坏字符"不包含在搜索词之中，则上一次出现位置为 -1。</p>

<p>以"P"为例，它作为"坏字符"，出现在搜索词的第6位（从0开始编号），在搜索词中的上一次出现位置为4，所以后移 6 - 4 = 2位。再以前面第二步的"S"为例，它出现在第6位，上一次出现位置是 -1（即未出现），则整个搜索词后移 6 - (-1) = 7位。</p>

<p>5.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050306.png"></p>

<p>依然从尾部开始比较，"E"与"E"匹配。</p>

<p>6.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050307.png"></p>

<p>比较前面一位，"LE"与"LE"匹配。</p>

<p>7.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050308.png"></p>

<p>比较前面一位，"PLE"与"PLE"匹配。</p>

<p>8.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050309.png"></p>

<p>比较前面一位，"MPLE"与"MPLE"匹配。<strong>我们把这种情况称为"好后缀"（good suffix），即所有尾部匹配的字符串。</strong>注意，"MPLE"、"PLE"、"LE"、"E"都是好后缀。</p>

<p>9.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050310.png"></p>

<p>比较前一位，发现"I"与"A"不匹配。所以，"I"是"坏字符"。</p>

<p>10.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050311.png"></p>

<p>根据"坏字符规则"，此时搜索词应该后移 2 - （-1）= 3 位。问题是，此时有没有更好的移法？</p>

<p>11.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050309.png"></p>

<p>我们知道，此时存在"好后缀"。所以，可以采用<strong>"好后缀规则"</strong>：</p>

<blockquote>

<p>　　后移位数 = 好后缀的位置 - 搜索词中的上一次出现位置</p>

</blockquote>

<p>举例来说，如果字符串"ABCDAB"的后一个"AB"是"好后缀"。那么它的位置是5（从0开始计算，取最后的"B"的值），在"搜索词中的上一次出现位置"是1（第一个"B"的位置），所以后移 5 - 1 = 4位，前一个"AB"移到后一个"AB"的位置。</p>

<p>再举一个例子，如果字符串"ABCDEF"的"EF"是好后缀，则"EF"的位置是5 ，上一次出现的位置是 -1（即未出现），所以后移 5 - (-1) = 6位，即整个字符串移到"F"的后一位。</p>

<p>这个规则有三个注意点：</p>

<blockquote>

<p>　　（1）"好后缀"的位置以最后一个字符为准。假定"ABCDEF"的"EF"是好后缀，则它的位置以"F"为准，即5（从0开始计算）。</p>

<p>　　（2）如果"好后缀"在搜索词中只出现一次，则它的上一次出现位置为 -1。比如，"EF"在"ABCDEF"之中只出现一次，则它的上一次出现位置为-1（即未出现）。</p>

<p>　　（3）如果"好后缀"有多个，则除了最长的那个"好后缀"，其他"好后缀"的上一次出现位置必须在头部。比如，假定"BABCDAB"的"好后缀"是"DAB"、"AB"、"B"，请问这时"好后缀"的上一次出现位置是什么？回答是，此时采用的好后缀是"B"，它的上一次出现位置是头部，即第0位。这个规则也可以这样表达：如果最长的那个"好后缀"只出现一次，则可以把搜索词改写成如下形式进行位置计算"(DA)BABCDAB"，即虚拟加入最前面的"DA"。</p>

</blockquote>

<p>回到上文的这个例子。此时，所有的"好后缀"（MPLE、PLE、LE、E）之中，只有"E"在"EXAMPLE"还出现在头部，所以后移 6 - 0 = 6位。 </p>

<p>12.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050312.png"></p>

<p>可以看到，"坏字符规则"只能移3位，"好后缀规则"可以移6位。所以，<strong>Boyer-Moore算法的基本思想是，每次后移这两个规则之中的较大值。</strong></p>

<p>更巧妙的是，这两个规则的移动位数，只与搜索词有关，与原字符串无关。因此，可以预先计算生成《坏字符规则表》和《好后缀规则表》。使用时，只要查表比较一下就可以了。</p>

<p>13.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050313.png"></p>

<p>继续从尾部开始比较，"P"与"E"不匹配，因此"P"是"坏字符"。根据"坏字符规则"，后移 6 - 4 = 2位。</p>

<p>14.</p>

<p><img src="http://image.beekka.com/blog/201305/bg2013050314.png"></p>

<p>从尾部开始逐位比较，发现全部匹配，于是搜索结束。如果还要继续查找（即找出全部匹配），则根据"好后缀规则"，后移 6 - 0 = 6位，即头部的"E"移到尾部的"E"的位置。</p>

<p>（完）<br>
</p><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;border:1px solid #d3d3d3;margin:1em;background-color:#aad2f0;border-radius:10px"><h3>文档信息</h3>
<ul>
<li>版权声明：自由转载-非商用-非衍生-保持署名 | <a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html">http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html</a></li>
<li>最后修改时间：2013年5月17日 10:02</li>
<li>付费支持：<a href="https://me.alipay.com/ruanyf"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" title="人民币" alt="人民币 - 支付宝" style="border:none;vertical-align:middle"></a> | <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=0.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="美元 - paypal" title="美元" style="border:none;vertical-align:middle"></a> </li>
</ul></div><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;margin:1em;border-radius:10px"><p><strong>[广告]</strong>　<a href="http://ushan.cn?utm_source=ruanyifeng.com" style="border:none">优衫（Ushan）是国内顶尖的定制西服店，常年为众多政商名流、影视明星、跨国高管定制衬衫与西服。以工艺精良、用料考究、版型出色、性价比高等特点广受各界好评。</a>
</p>

</div>