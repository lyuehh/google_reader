---
layout: post
title:  "利用reDuh打通HTTP隧道"
date:   2011-02-23 20:20:31
author: 大眼夹
categories: program
---

## 利用reDuh打通HTTP隧道
### by 大眼夹
### at 2011-02-23 20:20:31
### original <http://item.feedsky.com/~feedsky/clippit/~7808845/516545930/1226479/1/item.html>

<div><img width="580" height="338" src="http://dayanjia.com/wp-content/uploads/2011/02/hacker-580x338.png" alt="hacker" title="hacker"></div><p>在某些缺乏安全性的Windows服务器上，总会运行着一个Tomcat，上面跑着某些国内外包公司做的缺乏安全性的JSP程序。这些一环套一环的缺乏安全性就造就了一台可怜地服务器，暴露在Internet的某个小角落。但是有时，这些缺乏安全性的服务器会被一面强有力的防火墙保护着，这位忠实的卫士竭尽所能安慰着那千疮百孔的服务器。但是外来的捍卫终究抵不过自身的体弱多病……</p>
<p>说得这么矫情，其实只是假设一个环境。我们现在有一台跑Tomcat的Windows服务器，我们已经能够自由地上传一些文件，执行一些命令行——想必一般的Webshell都能做到这些。这时候我们想获取服务器的终极操控权——远程桌面，却发现3389端口却被防火墙挡住了，这该怎么办呢？</p>
<p><span></span></p>
<h2>准备工作</h2>
<p>当然我们先看一下3389端口是否是开放的，命令行下执行<code>netstat -an</code>，如果能看到类似“TCP    0.0.0.0:3389 LISTENING”这样的字样就说明很有希望了。然后我们再新建一个用户吧。依次执行<code>net user username password /add</code>和<code>net localgroup administrators username /add</code>。这时候我们满心欢喜地想在机器上远程桌面连接服务器，却发现失败了。排除了一些IP排除策略的可能性后（在注册表中可以检查相关设置），多半就是防火墙的问题了。</p>
<p><a rel="attachment wp-att-1213" href="http://dayanjia.com/2011/02/use-reduh-create-a-http-tunnel.html/mstsc-error"><img title="mstsc error" src="http://dayanjia.com/wp-content/uploads/2011/02/mstsc-error.png" alt="" width="406" height="272"></a></p>
<h2>使用reDuh建立隧道</h2>
<p><a title="reDuh" href="http://www.sensepost.com/labs/tools/pentest/reduh">reDuh</a>是由国外一个名叫Glenn Wilkinson的安全人员编写的一个通过HTTP协议建立隧道传输各种其他数据的工具。运行于服务器的JSP脚本接受HTTP请求，在本地转发给相应的端口，并接受本地端口的数据再通过HTTP发送给远程客户端。<strong>这样本来应该走其他端口的数据变摇身一变，披上了HTTP协议的报文头，换走HTTP的端口了。</strong>所有HTTP通道中的数据都是经过Base64编码的（Base64可以将二进制数据转换成ASCII字符序列，并且可以解码还原）。下面这张图详细地说明了reDuh的工作流程（图片汉化自<a title="Copy of the Slides" href="http://www.sensepost.com/cms/resources/labs/conferences/eye_of_the_needle/SensePost_Eye_of_a_Needle.pdf">SensePost's BlackHat USA 2008 talk on tunnelling data in and out of networks</a>）：</p>
<p><a rel="attachment wp-att-1214" href="http://dayanjia.com/2011/02/use-reduh-create-a-http-tunnel.html/reduh-tunnel"><img title="reDuh tunnel" src="http://dayanjia.com/wp-content/uploads/2011/02/reDuh-tunnel-580x404.png" alt="" width="580" height="404"></a></p>
<p>将JSP文件上传至服务器后，我们在自己机器上运行客户端建立连接：<code>java -jar reDuhClient.jar http://internal.bigcompany.com/uploads/reDuh.jsp</code>。产生输出：</p>
<pre>[Info]Querying remote web page for usable remote service port
[Info]Remote RPC port chosen as 42000
[Info]Attempting to start reDuh from internal.bigcompany.com/uploads/reDuh.jsp.  Using service port 42000. Please wait...
[Info]reDuhClient service listener started on local port 1010</pre>
<p>这时候我们登录进本机的1010端口，可以使用telnet或者netcat。登录后我们会看到<code>Welcome to the reDuh command line</code>的提示。输入<code>[createTunnel]1234:127.0.0.1:3389</code>便可以将远程服务器的3389端口和本地的1234端口绑定起来。</p>
<p>这时候我们便可以打开远程桌面连接，连接127.0.0.1:1234，然后就能看到熟悉的界面了。java的控制台会不断输出运行信息。因为绕了很多弯的缘故，所以远程桌面的速度并不快，但是毕竟连接上了，不是吗？</p>
<p><a rel="attachment wp-att-1215" href="http://dayanjia.com/2011/02/use-reduh-create-a-http-tunnel.html/mstsc"><img title="mstsc" src="http://dayanjia.com/wp-content/uploads/2011/02/mstsc-580x448.png" alt="" width="580" height="448"></a></p>
<p>当需要断开连接的时候，我们只需要输入<code>[killReDuh]</code>即可。</p>
<h2>隧道是个好东西</h2>
<p>如果大家知道利用国外服务器的SSH隧道可以用来访问某些被“防火墙”挡住的国外网站的话，一定会觉得这似乎很熟悉。没错，只不过本文是使用的HTTP的80端口，而那是利用的SSH的22端口，采用的协议不同而已。</p>
<p><strong>隧道就是这么个好东西，能够绕过防火墙的限制，在原本不属于自己的道路上跑运输——这就是“隧道”的本意吧？</strong></p>


<h3>没有相关日志</h3><img src="http://www1.feedsky.com/t1/516545930/clippit/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/clippit/~7808845/516545930/1226479/1/item.html" border="0" height="0" width="0">