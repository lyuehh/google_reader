---
layout: post
title:  "转移 VirtualBox ubuntu.vdi 后的网络连接问题"
date:   2013-04-14 12:01:43
author: leegorous
categories: program
---

## 转移 VirtualBox ubuntu.vdi 后的网络连接问题
### by leegorous
### at 2013-04-14 12:01:43
### original <http://feedproxy.google.com/~r/leegorous/gowing/~3/TE6bFjE1YGE/>

<p>由于磁盘空间的问题，需要迁移 vdi 文件去新的位置。关闭，转移，新建 Ubuntu 虚拟机直接引用转移后的 vdi，其它都保留原有的设置，一切都很好。</p>
<p>唯独启动后，ifconfig 一看就发现问题了，只有本地网络了。</p>
<p>一开始还是以为是 VBox 网络连接的问题，反复试了几次，问题依旧，而其它虚拟机同样的配置却可以正常运作。经过一翻 google，最终从 <a href="http://stackoverflow.com/questions/13335950/moving-a-virtualbox-vdi-linux-machine-to-a-new-host-computer" title="Moving a VirtualBox .vdi Linux machine to a new host computer">这里</a> 得到启示。   </p>
<pre>
&gt; sudo mv /etc/udev/rules.d/70-persistent-net.rules /etc/udev/rules.d/70-persistent-net.rules.bak
</pre>
<p>删掉应该也是可以的，重启之后问题就解决了。</p>
<img src="http://feeds.feedburner.com/~r/leegorous/gowing/~4/TE6bFjE1YGE" height="1" width="1">