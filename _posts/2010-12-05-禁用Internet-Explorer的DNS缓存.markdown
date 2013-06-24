---
layout: post
title:  "禁用Internet Explorer的DNS缓存"
date:   2010-12-05 14:13:38
author: 枯藤昏鸦
categories: program
---

## 禁用Internet Explorer的DNS缓存
### by 枯藤昏鸦
### at 2010-12-05 14:13:38
### original <http://ioio.name/disable-ie-dns-cache.html>

<p>在开发期间要频繁改动hosts文件，而IE会将其缓存不能立即获得更改后的DNS，常常需要重启浏览器才能生效，给开发带来了非常不便。为了解决该问题，需要禁止浏览器缓存DNS，将下面内容加入注册表：</p>
<pre lang="reg">
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]
"DnsCacheEnabled"=dword:00000000
"DnsCacheTimeout"=dword:00000000
"ServerInfoTimeOut"=dword:00000000
</pre>
<p>加入以后IE浏览器将不再缓存DNS，我们对hosts便会立即生效，无需再重启浏览器。<br>
如果需要恢复DNS缓存，移除我们之前加入的注册表内容即可。</p>
<pre lang="reg">
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings]
"DnsCacheEnabled"=-
"DnsCacheTimeout"=-
"ServerInfoTimeOut"=-
</pre>
<p>-EOF-</p>
<hr><h2>Related posts:</h2><ul><li><a href="http://ioio.name/cache-everything.html" rel="bookmark" title="Permanent Link: 万物皆缓存">万物皆缓存</a></li><li><a href="http://ioio.name/iis-cache.html" rel="bookmark" title="Permanent Link: IIS 缓存问题">IIS 缓存问题</a></li><li><a href="http://ioio.name/ubuntu-fonts-install.html" rel="bookmark" title="Permanent Link: Ubuntu 下字体的安装">Ubuntu 下字体的安装</a></li><li><a href="http://ioio.name/how-to-transfer-a-domain-from-google-apps-to-other-registrant.html" rel="bookmark" title="Permanent Link: How to Transfer a domain from Google Apps to other registrant">How to Transfer a domain from Google Apps to other registrant</a></li></ul><hr><small>Copyright © 2005~2009 | <a href="http://ioio.name/disable-ie-dns-cache.html" title="Permalink">Permalink</a> | <a href="http://ioio.name/disable-ie-dns-cache.html#comments">0 Comments</a> | <a href="http://closetou.com" title="Close To U">Close To U</a> <br>
<a href="http://feeds.feedburner.com/miss">订阅</a> <a href="https://twitter.com/tearnon">Twitter</a> <a href="http://ioio.name/godaddy">域名优惠码</a> <a href="http://ioio.name/mt">Media Temple空间</a>
<a href="http://shortener.ioio.name/">Google URL Shortener</a> 
<a href="http://750m.com/">Hot Topics New!</a>
<a href="http://affiliate.godaddy.com/redirect/3FA34877354E42FE78E82878FE3CAE7C45E1074DB489803F8705676166D4FFE0" title="Domain Sale! $6.89 .com at GoDaddy">Domain Sale! $6.89 .com at GoDaddy</a>
</small> )