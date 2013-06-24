---
layout: post
title:  "[转]linux的ulimit命令"
date:   2013-02-04 15:59:46
author: Skyman
categories: program
---

## [转]linux的ulimit命令
### by Skyman
### at 2013-02-04 15:59:46
### original <http://www.udpwork.com/item/9214.html>

<p>原文地址：http://www.ibm.com/developerworks/cn/linux/l-cn-ulimit/</p>
<p></p>
<table border="0" cellspacing="0" cellpadding="0"><tbody><tr><td><strong>选项 [options]</strong>
</td>
<td><strong>含义</strong>
</td>
<td><strong>例子</strong>
</td>
</tr>
<tr><td>-H</td>
<td>设置硬资源限制，一旦设置不能增加。</td>
<td>ulimit – Hs 64；限制硬资源，线程栈大小为 64K。</td>
</tr>
<tr><td>-S</td>
<td>设置软资源限制，设置后可以增加，但是不能超过硬资源设置。</td>
<td>ulimit – Sn 32；限制软资源，32 个文件描述符。</td>
</tr>
<tr><td>-a</td>
<td>显示当前所有的 limit 信息。</td>
<td>ulimit – a；显示当前所有的 limit 信息。</td>
</tr>
<tr><td>-c</td>
<td>最大的 core 文件的大小， 以 blocks 为单位。</td>
<td>ulimit – c unlimited； 对生成的 core 文件的大小不进行限制。</td>
</tr>
<tr><td>-d</td>
<td>进程最大的数据段的大小，以 Kbytes 为单位。</td>
<td>ulimit -d unlimited；对进程的数据段大小不进行限制。</td>
</tr>
<tr><td>-f</td>
<td>进程可以创建文件的最大值，以 blocks 为单位。</td>
<td>ulimit – f 2048；限制进程可以创建的最大文件大小为 2048 blocks。</td>
</tr>
<tr><td>-l</td>
<td>最大可加锁内存大小，以 Kbytes 为单位。</td>
<td>ulimit – l 32；限制最大可加锁内存大小为 32 Kbytes。</td>
</tr>
<tr><td>-m</td>
<td>最大内存大小，以 Kbytes 为单位。</td>
<td>ulimit – m unlimited；对最大内存不进行限制。</td>
</tr>
<tr><td>-n</td>
<td>可以打开最大文件描述符的数量。</td>
<td>ulimit – n 128；限制最大可以使用 128 个文件描述符。</td>
</tr>
<tr><td>-p</td>
<td>管道缓冲区的大小，以 Kbytes 为单位。</td>
<td>ulimit – p 512；限制管道缓冲区的大小为 512 Kbytes。</td>
</tr>
<tr><td>-s</td>
<td>线程栈大小，以 Kbytes 为单位。</td>
<td>ulimit – s 512；限制线程栈的大小为 512 Kbytes。</td>
</tr>
<tr><td>-t</td>
<td>最大的 CPU 占用时间，以秒为单位。</td>
<td>ulimit – t unlimited；对最大的 CPU 占用时间不进行限制。</td>
</tr>
<tr><td>-u</td>
<td>用户最大可用的进程数。</td>
<td>ulimit – u 64；限制用户最多可以使用 64 个进程。</td>
</tr>
<tr><td>-v</td>
<td>进程最大可用的虚拟内存，以 Kbytes 为单位。</td>
<td>ulimit – v 200000；限制最大可用的虚拟内存为 200000 Kbytes。</td>
</tr>
</tbody>
</table>

<br>
<div>作者：skyman_2001 发表于2013-2-4 15:59:46<a href="http://blog.csdn.net/skyman_2001/article/details/8569330">原文链接</a></div>
<div>阅读：30 评论：0<a href="http://blog.csdn.net/skyman_2001/article/details/8569330#comments">查看评论</a></div>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/9214.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/9214.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">udpwork.com</a> 聚合
					|
					<a href="http://www.udpwork.com/item/9214.html#reviews">评论: 0</a>
					|
					<a href="http://www.jikenow.com/">要! 要! 即刻! Now!</a>
				</p>
			</div>