---
layout: post
title:  "ps | grep app 命令不显示grep app本身进程的几种方式"
date:   2010-12-02 11:27:38
author: 
categories: program
---

## ps | grep app 命令不显示grep app本身进程的几种方式
### by 
### at 2010-12-02 11:27:38
### original <http://www.javaeye.com/topic/831297>

<p>使用ps命令查询进程，常常我们不想打印出"ps | grep app"这个当前进程，比如如下：</p>
<p> </p>
<p> </p>
<pre name="code">[root@localhost root]# ps -ef | grep java
root     20070 20049  0 Nov19 ?        00:24:33 /usr/java/jdk1.6.0_07/bin/java 
-Dprogram.name=run.sh -server -Xms512m -Xmx1024m -Xss128k -XX:+UseParallelGC 
-XX:+UseParallelOldGC -XX:PermSize=256m -XX:MaxPermSize=512m 
-Dsun.rmi.dgc.client.gcInterval=3600000 -Dsun.rmi.dgc.server.gcInterval=3600000 
-Djava.net.preferIPv4St
root      5523  5203  0 10:49 pts/0    00:00:00 grep java</pre>
 
<p> 显示java进程的同时还会把“grep java”这个进程打印出来“<span style="white-space:pre">root      5523  5203  0 10:49 pts/0    00:00:00 grep java</span>”，为了不打印此行，有以下几种方式，大家看着哪个好使吧：</p>
<p>1.ps aux | grep java | grep -v "grep"</p>
<p>2.ps aux|grep \[j]ava 或ps aux|grep [j]ava</p>
<p> </p>
<p>如果需要打印出进程号，可以在后面再用awk打印，如下：</p>
<p>ps aux|grep [j]ava | awk '{print $2}'</p>
<p>ps -ef|grep java|awk '{if($0~/run.sh/) print $2}'</p>
<p>ps aux|grep [j]ava | awk '{if($0~/run.sh/) print $2}'</p>
<p>ps aux | grep java | grep -v "grep" | awk '{print $2}'</p>
<p>等等写法</p>
<p> </p>
<p>如果要kill当前的进程，可以写成，如下杀掉java进程：</p>
<p>ps aux|grep [j]ava | awk '{print $2}' | xargs kill -9</p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
          
          <br><br>
          作者: <a href="http://yuanliyin.javaeye.com">yuanliyin</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/831297" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>