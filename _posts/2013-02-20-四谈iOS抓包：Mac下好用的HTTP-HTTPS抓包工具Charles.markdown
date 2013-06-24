---
layout: post
title:  "四谈iOS抓包：Mac下好用的HTTP/HTTPS抓包工具Charles"
date:   2013-02-20 15:21:17
author: ytzong
categories: program
---

## 四谈iOS抓包：Mac下好用的HTTP/HTTPS抓包工具Charles
### by ytzong
### at 2013-02-20 15:21:17
### original <http://www.99css.com/archives/1272>

<p>在Mac下做开发，用Fiddler抓包由于离不开Windows比较痛苦，还好有Charles，到官网<a href="http://www.charlesproxy.com/">http://www.charlesproxy.com/</a>可下载到最新版本（若不支持rMBP可拖到<a href="http://retinizer.mikelpr.com/">Retinizer</a>中把文字变清晰）</p>
<h3>HTTP抓包</h3>
<ol>
<li>打开Charles程序</li>
<li>查看Mac电脑的IP地址，如192.168.1.7</li>
<li>打开iOS设置，进入当前wifi连接，设置HTTP代理Group，将服务器填为上一步中获得的IP，即192.168.1.7，端口填8888</li>
<li>iOS设备打开你要抓包的app进行网络操作</li>
<li>Charles弹出确认框，点击Allow按钮即可</li>
</ol>
<h3>HTTPS抓包</h3>
<ol>
<li>下载Charles证书<a href="http://www.charlesproxy.com/ssl.zip">http://www.charlesproxy.com/ssl.zip</a>，解压后导入到iOS设备中（将crt文件作为邮件附件发给自己，再在iOS设备中点击附件即可安装；也可上传至dropbox之类的网盘，通过safari下载安装）</li>
<li>在Charles的工具栏上点击设置按钮，选择Proxy Settings…</li>
<li>切换到SSL选项卡，选中Enable SSL Proxying，别急，选完先别关掉，还有下一步</li>
<li>这一步跟Fiddler不同，Fiddler安装证书后就可以抓HTTPS网址的包了，Charles则麻烦一些，需要在上一步的SSL选项卡的Locations表单填写要抓包的域名和端口，点击Add按钮，在弹出的表单中Host填写域名，比如填api.instagram.com，Port填443</li>
</ol>
<p>接下来就跟HTTP抓包一样了</p>