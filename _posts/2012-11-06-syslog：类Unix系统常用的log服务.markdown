---
layout: post
title:  "syslog：类Unix系统常用的log服务"
date:   2012-11-06 09:40:21
author: 
categories: program
---

## syslog：类Unix系统常用的log服务
### by 
### at 2012-11-06 09:40:21
### original <http://blog.jobbole.com/29633/?utm_source=rss&utm_medium=rss&utm_campaign=syslog%25ef%25bc%259a%25e7%25b1%25bbunix%25e7%25b3%25bb%25e7%25bb%259f%25e5%25b8%25b8%25e7%2594%25a8%25e7%259a%2584log%25e6%259c%258d%25e5%258a%25a1>

<p><strong>注：</strong><span style="color:#888888"><strong>本文</strong>来自文章作者<span style="text-decoration:underline"><a href="http://weibo.com/u/2276577452" rel="nofollow"><span style="color:#888888;text-decoration:underline">@Jeremy黄国华</span></a></span> 的投稿。<span><a href="http://www.jobbole.com" title="伯乐">伯乐</a></span>在线也欢迎其他朋友<span style="text-decoration:underline"><a href="http://blog.jobbole.com/tougao"><span style="color:#888888"><strong>投稿</strong></span></a></span>，如果您有新浪微博，请投稿时记得留下哦~</span></p>
<p>当前的一个项目需要音视频的技术，而公司刚好没有类似的产品。幸好开源社区如此的强大，稍微谷歌一下，就找到了强大的Kamailio整合Asstersik的解决方案。</p>
<p>接下来的事情非常顺利，安装ubuntu操作系统、下载源码、编译、安装软件，一步步安装官方文档进行中。并且过程中没有提示任何错误，这让我们很兴奋。</p>
<p>最后我们使用imsandroid(一个Android系统的Sip客户端)进行测试，结果却显示连接失败。这个客户端软件严格遵守”不要打印任何让用户挫败的错误”原则，这是非常正确的，但是对开发人员来说，这并不是个好消息。</p>
<p>我们只能从服务器端开始查错，首先查看服务端的kamailio是否正常启动，执行命令 pa –ef | grep kamailio。结果显示我们sip服务没有启动。接下来我们需要的则是更多的关于软件执行的信息，包括启动信息、错误信息、以及任何软件开发人员想告诉我们的信息。</p>
<p>可是，问题来了。</p>
<p>如果你经常在windows下开发，并且没有太多的Linux开发经验，你会试图在软件的安装目录下查找包含log关键字的文字，结果是not found。</p>
<p><strong>查错需要的信息</strong></p>
<p>●系统打印的信息：比如远端链接失败、账密验证失败等信息，或者是软件依赖的底层服务启动失败的信息以及本地服务启动的执行信息。<br>
●网络服务的错误信息：远端服务不存在、链接超时、或者需要代理等等信息。<br>
●事件记录簿：这里可以很快找到关于服务运行的某个事件信息，失败以及失败原因等等。</p>
<p><strong>谁可以帮助你</strong></p>
<p>linux提供一个syslogd服务可以提供你log文件，帮助你获得以上的信息，syslogd记录的同时还会帮助你分类这些信息。比如在/var/log目录下，你可以找到以上的信息：</p>
<pre>/var/log/cron：
 记录系统排程启动的信息。
 /var/log/dmesg：
 记录系统开机时产生的各种信息。
 /var/log/lastlog：
 记录用户登录系统的信息。
 /var/log/maillog 或 /var/log/mail/*：
 记录邮件相关的信息，主要有sendmain与devecot产生的信息，前者是SMTP协议的提供者，而后者是POP3协议的提供者。
 /var/log/messages：
 记录系统发生的错误信息，对于差错非常重要。
 /var/log/secure：
 记录软件验证的信息。包含ssh、telnet以及login程序等程序的验证信息，基本上，涉及验证的软体都会在这个档案中记录。
 /var/log/wtmp, /var/log/faillog：
 这两个档案记录正确登入系统的账号与错误登入的账号信息。
 /var/log/httpd/*, /var/log/news/*, /var/log/samba/*：
 不同的服务可能使用自己的log档记录产生的各项信息，这需要软件在配置档中进行相应的配置。</pre>
<p><strong>你可能不知道，你的系统已经在使用syslogd服务了</strong></p>
<p>这是文章的目的之一，syslog是unix like系统记录log的规范，非常多的系统都默认支持它。redhat、ubuntu centos等等。你可以在你系统中执行ps –ef | grep syslogd来确认。</p>
<p>正因为它是如此广泛地被支持，所以很可能你的软件就刚好是使用它来记录log的。</p>
<p><strong>软件是否使用syslog，还是自定义log呢?</strong></p>
<p>为了确认软件使用何种方式记录log，可以查看官方文档提到的该软件的log配置，比如Asterisk的logger.conf配置文件，其中就有一段</p>
<pre>;debug =&gt; debug
 ;security =&gt; security
 console =&gt; notice,warning,error
 ;console =&gt; notice,warning,error,debug
 messages =&gt; notice,warning,error
 ;full =&gt; notice,warning,error,debug,verbose,dtmf,fax
 ;syslog keyword : This special keyword logs to syslog facility ;
 ;syslog.local0 =&gt; notice,warning,error ;</pre>
<p>上面的syslog facility告诉你，软件支持syslog(这里facility意为服务类别)。即使它不说，如果你熟知syslog的facility，也可以猜到它是否支持。syslog有以下级别的服务类别:</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">服务类别</td>
<td valign="top">说明</td>
</tr>
<tr>
<td valign="top">auth (authpriv)</td>
<td valign="top">记录与验证有关的信息</td>
</tr>
<tr>
<td valign="top">cron</td>
<td valign="top">记录系统排程的信息</td>
</tr>
<tr>
<td valign="top">daemon</td>
<td valign="top">记录各个daemon产生的信息</td>
</tr>
<tr>
<td valign="top">kern</td>
<td valign="top">记录核心产生的信息</td>
</tr>
<tr>
<td valign="top">lpr</td>
<td valign="top">记录列印相关的信息</td>
</tr>
<tr>
<td valign="top">mail</td>
<td valign="top">记录邮件的有关的信息</td>
</tr>
<tr>
<td valign="top">news</td>
<td valign="top">记录新闻有关的信息</td>
</tr>
<tr>
<td valign="top">syslog</td>
<td valign="top">记录syslog本省产生的信息</td>
</tr>
<tr>
<td valign="top">user, uucp, local0 ~ local7</td>
<td valign="top">记录各种传统的Unix系统本身有关的信息</td>
</tr>
</tbody>
</table>
<p> </p>
<p>调用每个服务产生的对应log文件，它是在/etc/syslog.conf中配置的(ubuntu是/etc/rsyslog.d/50-default.conf)中配置的，这里不详细介绍。</p>
<p>还应该注意的是，每个服务还有相应的log级别，这和java 的log4j工具很类似。</p>
<p> </p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">等级</td>
<td valign="top">等级名称</td>
<td valign="top">说明</td>
</tr>
<tr>
<td valign="top">1</td>
<td valign="top">info</td>
<td valign="top">一般的说明信息，一般提示用户级别的操作信息</td>
</tr>
<tr>
<td valign="top">2</td>
<td valign="top">notice</td>
<td valign="top">比info还需要被注意的信息</td>
</tr>
<tr>
<td valign="top">3</td>
<td valign="top">warning<br>
(warn)</td>
<td valign="top">警示信息，可能出现了问题，但是不影响软件的正常运行</td>
</tr>
<tr>
<td valign="top">4</td>
<td valign="top">err<br>
(error)</td>
<td valign="top">一些重大的错误，可能导致软件不能正常使用</td>
</tr>
<tr>
<td valign="top">5</td>
<td valign="top">crit</td>
<td valign="top">严重的信息</td>
</tr>
<tr>
<td valign="top">6</td>
<td valign="top">alert</td>
<td valign="top">比crit更为严重的信息</td>
</tr>
<tr>
<td valign="top">7</td>
<td valign="top">emerg<br>
(panic)</td>
<td valign="top">罪严重的警告，意指系统可能当机。通常只有硬件出现错误，导致核心无法正常工作才会出现。</td>
</tr>
</tbody>
</table>
<p> </p>
<p> </p>
<p><strong>教你看懂syslog文件</strong></p>
<p>每条信息均会记录以下的重要信息:</p>
<p>●事件发生的日期和时间。</p>
<p>●启动事件的主机名称。</p>
<p>●启动事件的服务名称(比如httpd、samba等)或函数名称(oam_unix）。</p>
<p>●信息的实际内容</p>
<p>如果你想要更多的信息，或者比较少的、特别关注的信息，你可修改syslog的配置档来实现，这个在以后的章节会介绍。例子为系统/var/log/secure的截取：</p>
<pre>[root@www ~]# cat /var/log/secure
 1 Mar 14 15:38:00 www atd[18701]: pam_unix(atd:session): session opened for user root by (uid=0)
 2 Mar 14 15:38:00 www atd[18701]: pam_unix(atd:session): session closed for user root
 3 Mar 16 16:01:51 www su: pam_unix(su-l:auth): authentication failure; logn ame=vbird uid=500 euid=0 tty=pts/1 ruser=vbird rhost= user=root
 4 Mar 16 16:01:55 www su: pam_unix(su-l:session): session opened for user root by vbird(uid=500)
 5 Mar 16 16:02:22 www su: pam_unix(su-l:session): session closed for user root
 |--日期/时间------|-H-|--------服务或函数---------|--訊息說明------&gt;</pre>
<p><strong>结论</strong></p>
<p>syslog的应用是如此的广泛，许多的软件采用它来记录log，所以我们很有必要理解并掌握它。不仅要认识syslog提供的服务，以及如何去发现、甚至配置服务对应的log文档路径，还要知道如何在软件中定义服务的log级别。</p>
<p> </p>
<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/29546/">Ubuntu桌面生存指南(1)：选择 Linux</a></li><li><a href="http://blog.jobbole.com/28784/">提问的智慧</a></li><li><a href="http://blog.jobbole.com/26251/">阮一峰：读懂 diff</a></li><li><a href="http://blog.jobbole.com/25792/">王垠：Unix的缺陷</a></li><li><a href="http://blog.jobbole.com/25748/">写给在集市中迷失的一代</a></li></ul>