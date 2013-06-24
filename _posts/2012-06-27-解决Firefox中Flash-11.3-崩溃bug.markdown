---
layout: post
title:  "解决Firefox中Flash 11.3 崩溃bug"
date:   2012-06-27 19:45:45
author: asins
categories: program
---

## 解决Firefox中Flash 11.3 崩溃bug
### by asins
### at 2012-06-27 19:45:45
### original <http://nootn.com/blog/Program/51/>

<p>一名视频网站的前端人员，firefox无法使用那绝对是场灾难，这两天我就经历了这么一场，原因为flash自动升级到11.3版本出现与Firefox无法兼容导致浏览器崩溃。</p>

<p>flash 11.3这次升级特地针对Firefox提供了沙箱功能，但附加了这个bug给我们</p>

<p>解决Firefox中Flash 11.3 崩溃bug就是关掉这沙箱功能就OK了，方法如下：</p>

<p>Ctrl+r打开目录</p>

<pre><code>%WINDIR%\System32\Macromed\Flash（32位操作系统）
%WINDIR%\SysWOW64\Macromed\Flash（64位操作系统）
</code></pre>

<p>找到 mms.cfg 文件，用记事本打开，新增一行：</p>

<pre><code>ProtectedMode=0
</code></pre>

<p>保存退出，OK了。</p>

<p>方法来自论坛的帖子 http://www.firefox.net.cn/forum/viewtopic.php?f=5&amp;t=44101</p>