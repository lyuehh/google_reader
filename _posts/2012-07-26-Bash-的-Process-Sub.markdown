---
layout: post
title:  "Bash 的 Process Sub"
date:   2012-07-26 00:18:26
author: gnawux
categories: program
---

## Bash 的 Process Sub
### by gnawux
### at 2012-07-26 00:18:26
### original <http://www.udpwork.com/item/7790.html>

<p>Shell这东西，如果你真会用的话，你可能永远不敢说你会用。</p>
<p>前两天看了Process Sub，一种结合了管道和命令替换（command sub）的使用方式，非常有趣。</p>
<p>嗯，这得先说说命令替换，就是反引号或者 “$()”，非常好用，但是，你能把它当成文件用么，有点时候，你要把命令输出送给其他命令处理，而这个命令只是接受文件参数，无法用管道，或者，你要两个命令的输出，没法用管道，怎么办呢，process_sub 来啦，看这个：</p>
<pre>diff -u &lt;(grep &quot;^5076&quot; strace.log |sed -ne &#39;s/.*futex(\(0x[0-9a-f]\+\),.*/\1/p&#39; |sort|uniq) &lt;(grep &quot;^5061&quot; strace.log |sed -ne &#39;s/.*futex(\(0x[0-9a-f]\+\),.*/\1/p&#39; |sort|uniq)</pre><p>嗯，不解释了，自己理解哈。</p>
<p>Process sub 的另一种场合，下面这两个命令几乎是等价的，试图用个循环来做 wc 的工作：</p>
<pre>while read x; do ((y++)); done &lt; &lt;(netstat -nr)

netstat -nr | while read x; do ((y++)); done</pre><p>不留心很难看出差别，但是，如果这两条命令之前，先把 y 置空，那么，第一个命令之后，y是有值的，第二个则不会。这是因为，管道命令可以看作是</p>
<pre>cmd1 | (cmd2)</pre><p>也就是说，后面的命令是在子 shell 里运行的，因此，无法改动父 shell 的内部状态，而 process sub 方式运行的命令则没有这个问题。</p>
<p>嗯，则个很好玩，前两天刚看到的，参考了 TLDP 的 Advanced Bash-Scripting Guide 。</p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7790.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7790.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7790.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>