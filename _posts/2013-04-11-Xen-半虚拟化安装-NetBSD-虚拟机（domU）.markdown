---
layout: post
title:  "Xen 半虚拟化安装 NetBSD 虚拟机（domU）"
date:   2013-04-11 22:08:48
author: vpsee
categories: program
---

## Xen 半虚拟化安装 NetBSD 虚拟机（domU）
### by vpsee
### at 2013-04-11 22:08:48
### original <http://feedproxy.google.com/~r/vpsee/~3/DwUQxSB0NDg/>

<p>很久以前我们开始提供 NetBSD 的 VPS，期间陆续有朋友写信询问怎么制作 NetBSD/FreeBSD 的半虚拟化（PV）Xen 镜像，今天以安装最新的 NetBSD 6.0.1 版本为例演示一下，随便说一下制作 FreeBSD PV 虚拟机方法是类似的。Xen 全虚拟化方式（HVM）安装 NetBSD 虚拟机就不说了，和安装其他操作系统 Linux, Windows 差不多，毕竟全虚拟化不用考虑内核，安装要简单得多。考虑到性能，半虚拟化依然是 Xen 的强项，所以我们的 VPS 都是采用半虚拟化的，不过随着硬件的发展和 CPU 厂商的大力支持，半虚拟化的优势正渐渐消失。</p>
<p>这里演示的是安装 NetBSD domU，如果想了解安装 NetBSD dom0 的话看这里：<a href="http://www.vpsee.com/2010/11/install-xen-on-netbsd/">在 NetBSD 上安装和配置 Xen</a>.</p>
<h1>准备</h1>
<p>新建一个目录，然后下载一些东西，netbsd-INSTALL_XEN3_DOMU.gz 是安装 NetBSD 时候用的内核，netbsd-XEN3_DOMU.gz 是运行 NetBSD 用的内核，不要弄混了；NetBSD-6.0.1-amd64.iso 是我们需要的安装光盘。</p>
<pre>
$ mkdir netbsd
$ cd netbsd

$ wget http://ftp.netbsd.org/pub/NetBSD/NetBSD-6.0.1/iso/NetBSD-6.0.1-amd64.iso

$ wget http://ftp.netbsd.org/pub/NetBSD/NetBSD-6.0.1/amd64/binary/kernel/netbsd-INSTALL_XEN3_DOMU.gz
$ wget http://ftp.netbsd.org/pub/NetBSD/NetBSD-6.0.1/amd64/binary/kernel/netbsd-XEN3_DOMU.gz
</pre>
<h1>安装</h1>
<p>首先制作一个大小为 5GB 的硬盘：</p>
<pre>
$ dd if=/dev/zero of=netbsd.disk bs=1024k count=1 seek=5000
</pre>
<p>然后新建一个 Xen domU 配置文件，并输入以下内容，：</p>
<pre>
$ vi netbsd
name = "netbsd"
memory = "256"
kernel = "/home/vpsee/netbsd/netbsd-INSTALL_XEN3_DOMU.gz"
disk = [ 'file:/home/vpsee/netbsd/netbsd.disk,xvda,w', 'file:/home/vpsee/netbsd/NetBSD-6.0.1-amd64.iso,ioemu:hdc:cdrom,r' ]
vif = [ 'bridge=xenbr0' ]
vcpus = "1"
</pre>
<p>使用 root 帐号启动这个名为 netbsd 的虚拟机开始 NetBSD 安装过程，这一步需要注意的是在选择安装介质的时候选择从 CD-ROM 安装并使用设备名 xbd0a (device [xbd1a]: xbd0a)：</p>
<pre>
$ su

# xm create netbsd -c
</pre>
<p>NetBSD 安装界面如下，顺着屏幕提示安装即可，没有难度：</p>
<p><img style="clear:both;float:left;width:565px;display:block" src="http://www.vpsee.com/wp-content/uploads/2013/04/install-netbsd6-domu.png" alt="google apps directory sync"></p>
<p>
<p>安装完毕后关闭这个虚拟机，修改 netbsd 的 Xen 配置文件，然后用新的配置文件启动虚拟机：</p>
<pre>
# xm destroy netbsd

# vi netbsd
name = "netbsd"
memory = "256"
kernel = "/home/vpsee/netbsd/netbsd-XEN3_DOMU.gz"
disk = [ 'file:/home/vpsee/netbsd/netbsd.disk,xvda,w' ]
vif = [ 'bridge=xenbr0' ]
vcpus = "1"

# xm create netbsd
Using config file "./netbsd".
Started domain netbsd
</pre>
<p>虚拟机启动后用 xm list 确认一下，然后就可以从控制台登陆 NetBSD 虚拟机了：</p>
<pre>
# xm list
Name                                      ID Mem(MiB) VCPUs State   Time(s)
Domain-0                                   0     1024     4 r----- 279689.0
netbsd                                   242      256     1 -b----      0.6

# xm console netbsd
</pre>
<p>剩下的配置 NetBSD 环境、IP 地址等这里就不用赘述了～</p>
<img src="http://feeds.feedburner.com/~r/vpsee/~4/DwUQxSB0NDg" height="1" width="1"></p>