---
layout: post
title:  "Jailbreakme 3 PDF 越狱漏洞分析"
date:   2011-07-06 13:57:12
author: 
categories: program
---

## Jailbreakme 3 PDF 越狱漏洞分析
### by 
### at 2011-07-06 13:57:12
### original <http://www.cnbeta.com/articles/147954.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/464.htm"><img src="http://img.cnbeta.com/topics/2011-3-5%2016-42-48.gif" alt="Apple iPad" name="sign" align="right"></a>
        <p><b>感谢<a rel="nofollow" href="http://www.appvv.com">维维网</a>的投递</b><br>
Jailbreakme 3 PDF 越狱使用Apple iOS操作系统中处理Postscript Type(又称Adobe Type 1)字体的一个缓冲区溢出漏洞，漏洞存在于t1_decoder_parse_charstrings()
函数，该函数用于解码Type 1字体文件中编码过的CharStrings字段。越狱漏洞中使用的字体文件中包含了一个351字节的特殊CharStrings的字段，当该函数处理这个特殊的
字段后，导致保存在堆栈中的一个用于处理字形(glyph)的回调解析函数被覆盖，从而影响了程序的执行流程，可以用于执行任意的代码。</p>
		<p>由于iOS系统的特殊性，程序的代码签名，ARM处理器的Nerver execute比特位设置，地址随机化保护等等，增加了越狱的难度。由于这个漏洞的特殊性，可以在字体程序中判断出
dyld_shared_cache中的地址，执行ROP的Shellcode。<br>
<br>
PDF漏洞利用成功后会调用iOKit接口中的内核漏洞提升root权限，关闭iOS中的代码签名，并且替换内核中的sandbox。<br>
<br>
浏览器提权随后解压可执行文件locutus到/tmp目录下并且执行, 该文件执行后会连接comex的越狱服务器a.qoid.us(现已关闭）下载install.dylib,完美越狱的deb包saffron-jailbreak以及Cydia的安装文件。<br>
<br>
由于comex的越狱服务器在PDF越狱程序泄露后不久就关闭了，所以很多朋友都出现了safari浏览器闪退并报403错误，并且未越狱成功的情况。<br>
<a rel="nofollow" href="http://www.appvv.com"><b>维维网</b></a>在第一时间从推特上获取到泄露的PDF越狱文件后，第一时间着手分析PDF越狱程序中的原理，并且通过修改内置的下载服务器地址，搭建国内的越狱服务器镜像，而无需按照老外的教程繁琐的架设单机越狱环境。<br>
<br>
经过测试发现针对iPad 1、iPhone 4 GSM、iPad2 CDMA的越狱均可以成功。<br>
<br>
<b>目前支持下列版本:</b> <b><br>
1.IPad2 CDMA 4.3.3(8J2) </b> <br>
2.IPad2 WIFI 4.3(8F191)<br>
3.IPad1 WIFI/3G 4.3.3(8J3)<br>
4.IPhone 4.3.3(8J2)<br>
<br>
<b>越狱方法:</b>使用iPad打开Safari浏览器,在浏览器中访问 jb3.appvv.com ,然后点击一键越狱即可。<br>
<br>
从7月4日晚上小范围公开越狱地址后, 截止到发稿时间,已经为用户越狱了1175台设备。其中ipad2 wifi 4.3.0 573台,ipad2 cdma 4.3.3 235台,打酱油的(iPhone4,iPad1) 367台。该数据还在持续增长中。<br>
<br>
<img style="width:580px;height:435px" src="http://img.cnbeta.com/newsimg/110706/13571201885084254.png"><br></p></div>