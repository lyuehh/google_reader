---
layout: post
title:  "Oracle客户端toad汉字乱码解决方法"
date:   2011-04-11 12:14:02
author: 
categories: program
---

## Oracle客户端toad汉字乱码解决方法
### by 
### at 2011-04-11 12:14:02
### original <http://www.iteye.com/topic/997202>

应一直受服务端编码和客户端编码要一致误导，结果害了我半个小时时间，来解决这个小问题，现在把解决记录一下，以备忘记：
<br>
<br>出现中文乱码的主要原因是字符集不同。在Oracle中，我们关心三个地方的字符集：
<br>
<br>先来查看一下数据库字符集：
<br>在Oracle中可能使用Select userenv('language') from dual;或者：Select name, value$ from props$;查看。
<br>
<br>l Oracle服务器内部的字符集
<br>修改方法：
<br>connect sys/chang_on_install
<br>update props$　set value$='TRADITIONAL CHINESE_TAIWAN.AL32UTF8'where name='NLS_CHARACTERSET';
<br>commit;
<br>
<br>2 NLS_LANG变量里保存的字符集
<br>
<br>这个是Oracle设置的一个变量。在Windows中，这个变量保存在注册表中：
<br>HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\HOME0 保存着NLS_LANG变量。
<br>
<br>在Unix/Linux中，则需要自己进行设置，在.profile里面加上 NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
<br>export NLS_LANG
<br>
<br>3 客户端应用的字符集
<br>
<br>下面是我用到的字符集
<br>american_america.AL32UTF8
<br>TRADITIONAL CHINESE_TAIWAN.ZHT16MSWIN950
<br>TRADITIONAL CHINESE_TAIWAN.AL32UTF8
<br>AMERICAN_AMERICA.WE8ISO8859P1
<br>AMERICAN_AMERICA.ZHS16GBK
<br>
<br>特别注意：如果服务端编码为AL32UTF8，那么客户端就应该安装自己环境来设置字符编码。
<br>比如我有一个服务器端，其中两个客户端一个为简体，一个为繁体：
<br>服务器端字符集设置：american_america.AL32UTF8
<br>简体客户端字符集设置：AMERICAN_AMERICA.ZHS16GBK
<br>繁体客户端字符集设置：TRADITIONAL CHINESE_TAIWAN.ZHT16MSWIN950
<br>
<br>这样toad和sqlplus不论在繁体还是简体都可以正常显示汉字了。
          
          <br><br>
          作者: <a href="http://seekweel.iteye.com">seekweel</a> 
          <br>
          声明: 本文系ItEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.iteye.com/topic/997202" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">ItEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>