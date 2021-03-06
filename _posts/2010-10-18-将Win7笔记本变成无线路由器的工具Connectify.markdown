---
layout: post
title:  "将Win7笔记本变成无线路由器的工具Connectify"
date:   2010-10-18 14:47:22
author: 
categories: program
---

## 将Win7笔记本变成无线路由器的工具Connectify
### by 
### at 2010-10-18 14:47:22
### original <http://news.cnblogs.com/n/77713/>

<p>　　Connectify是一款非常实用的免费软件，它可以把安装了Windows7的笔记本电脑模拟成一台无线路由器、无线接入点(无线AP，Access Point)。</p>
<p>　　使用方法：只需要把软件安装在你的Win7本本上，你朋友、同学的笔记本就可以通过连上你的笔记本来无线上网了;如果你的手机支持Wi-Fi，也可以通过笔记本电脑无线上网，无需流量费。</p>
<p>　　本文介绍Connectify的基本配置和功能，并分享使用 Connectify的经验。</p>
<p>　　(注1:D版Win7也没问题; 注2:不支持部分老本本的无线网卡)</p>
<p>        <strong>Connectify</strong>：<a title="Connectify官方主页" href="http://www.connectify.me/">官方主页</a> | <a title="下载Connectify" href="http://download.cnet.com/Connectify/3000-18508_4-75024171.html">下载</a>(仅1M）</p>
<p>　　Connectify-把Win7笔记本变无线路由器</p>
<p>　　1.什么情况下需要并选择 Connectify ?</p>
<p>　　简言之，Connectify并不是每个人的必需软件。</p>
<p>　　它只适合类似这样的场景：在一间只有一个网络端口、没有无线路由器的房间里，有好几台笔记本电脑都想上网，其中至少有一台安装了Windows7，而且这台笔记本是06年以后买的。</p>
<p>　　2.安装使用Connectify需要什么条件 ?</p>
<p>　　A 一台安装了windows7操作系统的笔记本电脑。</p>
<p>　　B 此笔记本电脑的无线网卡在下面的兼容列表中。</p>
<p>　　(查看方法：我的电脑属性——设备管理器——网络适配器。)</p>
<p>　　笔者经验：在网络适配器中看不到具体的型号，所以不如不看，直接安装，如果不兼容，会又提醒的。只要你的电脑年龄在4岁以下，一般没问题。</p>
<p>　　兼容的无线网卡：</p>
<p>　　Atheros AR5xxx/AR9xxx cards, driver version 8.0.0.238</p>
<p>　　Broadcom 4310-series (in many Dell laptops)</p>
<p>　　Broadcom 4321AG/4322AG/43224AG WLAN Adapter, driver version 5.60.18.8 (here)</p>
<p>　　D-link AirPlus G DWL-G510 Wireless PCI Adapter, driver version 3.0.1.0</p>
<p>　　D-Link DWA-140 RangeBooster N USB Adapter, driver version 3.0.3.0</p>
<p>　　Dell 1510 Wireless N adapter, Broadcom version,driver 5.60.18.8,(here)</p>
<p>　　Intel 5100/5300, driver version 13.0.0.107</p>
<p>　　Linksys Dual-Band Wireless-N USB Network Adapter(WUSB600N)， driver version 3.0.10.0</p>
<p>　　Netgear 108 Mbit WG311T</p>
<p>　　Ralink RT2870 (in many 802.11n USB dongles)</p>
<p>　　Realtek RTL8187B (Win7 driver ver.1178)</p>
<p>　　Realtek RTL8187SE (with the drivers that came with Windows 7)</p>
<p>　　Realtek RTL8192u with 1370(Beta)</p>
<p>　　Sitecom Wireless USB Adapter 54g WL-608,with Ralink RT2870 drivers,version 3.0.9.0</p>
<p>　　不兼容的无线网卡：</p>
<p>　　Belkin F5D7050UK</p>
<p>　　Belkin Wireless G MIMO devices (as of version 3.1.2.0)</p>
<p>　　D-Link AirPlus G DWL-G122</p>
<p>　　Gigabyte GA-WPKG 802.11g</p>
<p>　　Intel 3945/4965,2200BG (most Intel cards, unfortunately)</p>
<p>　　Mac Book Builtin Broadcom devices</p>
<p>　　Realtek RTL8187 (like in older 802.11bg USB dongles)</p>
<p>　　Zydas ZD1211 (also in 802.11bg USB dongles)</p>
<p>　　3. Connectify下载安装教程</p>
<p>　　1 到官方网站下载后，获得文件 ConnectifyInstaller.exe 仅1.08M大小。</p>
<p>　　2 双击安装即可，缺点是没有路径选项，直接安装到C盘program files。</p>
<p>　　3 安装完成后，驻留系统托盘，形如下图托盘下图标左一。(设置之前，别忘了打开你本本上无线网络)</p>
<p>　　4. Connectify的基本配置教程</p>
<p style="text-align:center"><img src="http://pic003.cnblogs.com/2010/66372/201010/2010101814474085.png" alt=""></p>
<p>Connectify-把Win7笔记本变无线路由器</p>
<p>　　对照上图，这样Settings</p>
<p>　　Wi-Fi Name:</p>
<p>　　说明：给你的无线网络起个名字来标识它，默认是Connectify-me。</p>
<p>　　要求：最多32个字符，不支持中文。</p>
<p>　　Password</p>
<p>　　说明：设置10-26个字符的密码</p>
<p>　　要求：仅支持字母a-f和数字0-9.</p>
<p>　　Internet</p>
<p>　　说明：选择你电脑现有的网络连接.</p>
<p>　　要求：一般为本地连接或宽带连接</p>
<p>　　WiFi</p>
<p>　　说明：选择你电脑无线适配器的名字</p>
<p>　　要求：一般为无线网络连接或无线网络连接2等等</p>
<p>　　Mode</p>
<p>　　说明：选择无线接入模式</p>
<p>　　要求：请选择Ad-Hoc,WEP(其他模式，请自行研究)</p>
<p>　　设置好后，点击最下边的Start Hotsopt.</p>
<p>　　启动成功后，系统托盘小图标上的红色叉号会消失。</p>
<p>　　5. Connectify的使用</p>
<p>　　现在，其他笔记本电脑可以搜索无线信号。</p>
<p>　　搜索到你刚才命名的标识默认，连接，输入你设置的密码即可.</p>
<p>　　一般是在一个屋子里，信号会满满的.</p>
<p>　　享受无线网络吧.</p>
<p>　　6.补充说明</p>
<p>　　1 杀软可能会有报警，放过，加入白名单.</p>
<p>　　2 使用时有三个进程:Connectfy.exe、Connectfyd.exe、ConnectfyService.exe;</p>
<p>　　3 退出软件后，后两个进程依然运行(但是不耗多少资源)，可以手动关闭。</p><p><br>　　相关新闻：<br>　　· <a href="http://news.cnblogs.com/n/72724/">庆祝Windows 7周年 微软重新销售家庭包</a><span style="color:gray">(2010-09-01)</span><br>　　· <a href="http://news.cnblogs.com/n/66569/">Windows 7 SP1新增功能官方视频演示</a><span style="color:gray">(2010-06-19)</span><br>　　· <a href="http://news.cnblogs.com/n/72372/">英特尔业绩下降 将直接影响Windows 7销售</a><span style="color:gray">(2010-08-30)</span><br>　　· <a href="http://news.cnblogs.com/n/66381/">微软向开发人员发邀请函 测试Win7 SP1</a><span style="color:gray">(2010-06-16)</span><br>　　· <a href="http://news.cnblogs.com/n/70070/">福布斯：Windows 7平板电脑难成iPad杀手</a><span style="color:gray">(2010-08-05)</span><br></p><p>　　本文链接：<a href="http://news.cnblogs.com/n/77713/">http://news.cnblogs.com/n/77713/</a></p><p>　　<a href="http://job.cnblogs.com">程序员找工作，就在博客园</a></p><p>　　<a href="http://a4.yeshj.com/rd/34138/">每天10分钟，轻松学外语</a></p><img src="http://news.cnblogs.com/news/rssclick.aspx?id=77713" width="1" height="1" alt="">