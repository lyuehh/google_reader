---
layout: post
title:  "把Windows 8安装到U盘 创建Windows to Go启动盘"
date:   2012-03-02 07:04:34
author: 
categories: program
---

## 把Windows 8安装到U盘 创建Windows to Go启动盘
### by 
### at 2012-03-02 07:04:34
### original <http://www.cnbeta.com/articles/175152.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/480.htm"><img src="http://img.cnbeta.com/topics/win7.gif" alt="Windows 8" name="sign" align="right"></a>
        <p><b>¸ÐÐ»<a rel="nofollow" href="http://www.naiqus.com">32GBµÄUÅÌÒªÕÇ¼ÛÁË</a>µÄÍ¶µÝ</b><br>
Windows To GoÊÇÀ´×ÔWindows 8µÄÈ«ÐÂ°²×°·½Ê½£¬Í¨¹ýËüÎÒÃÇ¿ÉÒÔ´´½¨Ò»¸ö´ÓUÅÌÆô¶¯µÄÍêÕûµÄWindows 8ÏµÍ³»·¾³¡£<br>
ÄãÐèÒª×¼±¸£º</p>
		<p><p>ÄãÐèÒª×¼±¸£º</p>
<ul><li>ÈÝÁ¿ÖÁÉÙÎª32GBµÄUÅÌÒ»Ã¶£¨»òÕßÓ²ÅÌ£©</li>
<li>°²×°ºÃWindows 8µÄPC</li>
<li>Windows 8 DVD ISOÎÄ¼þ</li>
<li>Imagex.exe £¨°²×° Windows 7 Automated Installation KitÖ®ºó£¬¿ÉÒÔ´ÓC:\Program 
Files\Windows AIK\Tools\amd64 »òÕß C:\Program Files\Windows 
AIK\Tools\x86ÖÐÕÒµ½¸ÃÎÄ¼þ£©</li>
</ul>
<p>½¨Á¢Windows To Go USB Çý¶¯Éè±¸£º</p>
<ol><ol><li>°²×°Windows To Go ÐèÒªÒ»¸öµ¥¶ÀµÄ·ÖÇø£¬ÎÒÃÇÊ¹ÓÃdiskparkÀ´½øÐÐ·ÖÇø²Ù×÷¡£Ê¹ÓÃ¹ÜÀíÔ±Éí·ÝÔËÐÐcmd.exe</li>
<li>È·ÈÏÄãµÄUÅÌÒÑ¾­²åÔÚµçÄÔÉÏ²¢ÇÒ±»³É¹¦Ê¶±ð</li>
<li>ÔÚÃüÁîÌáÊ¾ÐÐÖÐÊäÈë ¡°list disk¡± ²¢»Ø³µ£¬²é¿´ÁÐ±íÖÐÄãµÄUSBÉè±¸</li>
<p></p>
<li>ÊäÈë¡°select disk #¡±È»ºó»Ø³µ£¨#ÊÇÄãµÄUÅÌµÄÐòºÅ£©Ñ¡ÔñUÅÌÎªµ±Ç°Éè±¸</li>
<li>ÇåÀí·ÖÇø£¬ÊäÈë¡° clean ¡±£¬ »Ø³µ</li>
<li>ÊäÈë¡°create partition primary¡±´´½¨ÐÂ·ÖÇø</li>
<li>¸ñÊ½»¯·ÖÇø¡° format fs=ntfs quick ¡±</li>
<li>½«·ÖÇøÉèÖÃÎª»î¶¯·ÖÇø£¬ÊäÈë¡° active ¡±</li>
<li>ÊäÈë¡°exit¡±ÍË³ödiskpart</li>
</ol>
</ol>
<p><img style="font-size:xx-small" title="windows to go 1" src="http://img.cnbeta.com/newsimg/120302/07043501440580995.png" alt="" height="266" width="500"></p>
<ol><li>ÏÖÔÚ´ò¿ªWindows 8µÄISOÎÄ¼þ£¬¸´ÖÆ \source\install.wim µ½ imagex.exe µÄÏàÍ¬ÎÄ¼þ¼Ð¡£</li>
<li>ÔÚÃüÁîÌáÊ¾·ûÖÐ½øÈëimagex.exeµÄÎÄ¼þ¼Ð£¬ÊäÈë¡° imagex.exe /apply install.wim 1 x:\ ¡±£¨Ìæ»»xÎªuÅÌÔÚÏµÍ³ÖÐµÄÅÌ·û£©</li>
<li>°²×°ºÃÁËÖ®ºó£¬ÊäÈë¡° bcdboot.exe  x:\windows  /s  x:  /f  ALL ¡±£¨Ìæ»»xÎªuÅÌÔÚÏµÍ³ÖÐµÄÅÌ·û£©</li>
</ol>
<div>Èç¹ûÒÔÉÏ²½Öè³É¹¦Íê³É£¬×£ºØÄã£¬ÄãÓµÓÐÒ»Ã¶Windows 8 To GoµÄUÅÌÁË¡£</div>
<div>
<p><a rel="nofollow" href="http://tweaks.com/windows/52279/how-to-create-a-windows-to-go-usb-drive/">À´Ô´</a></p>
</div></p></div>