---
layout: post
title:  "memcached模块"
date:   2013-01-31 09:57:55
author: asoqa
categories: program
---

## memcached模块
### by asoqa
### at 2013-01-31 09:57:55
### original <http://article.yeeyan.org/view/207217/346811>

<p>译者  <a href="http://user.yeeyan.org/u/207217">asoqa</a></p><p>
	<strong>memcached</strong>
</p>
<p>
	<br>
</p>
<p>
	memcached模块可以通过memecached协议使用elasticsearch API
</p>
<p>
	<br>
</p>
<p>
	由transport-memcached插件安装。另一个安装办法就是下载memcached插件放在plugins目录下。
</p>
<p>
	<br>
</p>
<p>
	memcached协议支持二进制和文本两种协议，并自动检测使用哪种协议。
</p>
<p>
	<br>
</p>
<p>
	<strong>Rest方式使用Memcached协议</strong>
</p>
<p>
	Memcached命令映射到REST接口上，由同样的elasticsearch的REST层处理这些命令。这里给出一个支持memcached命令的清单：
</p>
<p>
	<br>
</p>
<p>
	<strong>GET</strong>
</p>
<p>...<img src="http://www1.feedsky.com/t1/712823171/yeeyan-tech/feedsky/s.gif?r=http://article.yeeyan.org/view/207217/346811" border="0" height="0" width="0"></p>