---
layout: post
title:  "Sessionid被窃取带来的风险"
date:   2010-06-26 22:46:29
author: 
categories: program
---

## Sessionid被窃取带来的风险
### by 
### at 2010-06-26 22:46:29
### original <http://www.javaeye.com/topic/700057>

今天用Http抓包工具抓了一下发出的请求包，在cookies里看到了sessionid。
<br>请求包如下：
<br>
<br>GET /w3/global/j/global.js HTTP/1.1
<br>Accept: */*
<br>Referer: http://www.jiayuan.com/login/index.php?pre_url=/usercp
<br>Accept-Language: zh-cn
<br>Accept-Encoding: gzip, deflate
<br>User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; EmbeddedWB 14.52 from: http://www.bsalsa.com/ EmbeddedWB 14.52)
<br>Host: images.jiayuan.com
<br>Connection: Keep-Alive
<br>Cookie: <span style="color:red">SESSION_HASH=aa200d999de428f8e84ad56f4fc0afb9ac88fb78; </span>stadate1=25727411; myloc=53%7C5301; myage=24; mysex=m; myuid=25727411; myincome=30; last_login_time=1277561249; new_msg=0; pop_1268278480=1277575662747; pop_time=1277561290653
<br>
<br>
<br>按照session机制，服务器收到客户端发送的sessionid就会去找对应的session！
<br>
<br>
<br>而木马抓一个这样的请求包，应该难度不大的，这样岂不是风险挺大的吗。不知session机制有没有其他控制的手段。
<br>
<br>-----------------------------------
<br>
<br>接下来，我测试走ssl的情况，抓包如下：
<br>GET /personbank/main_center.jsp?dse_sessionId=IFAPIXAGCEDLJAEJFHAQGPGTJRFGEBCJFLALBIBZ&amp;netType=0 HTTP/1.1
<br>Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/x-silverlight, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, application/QVOD, application/x-shockwave-flash, */*
<br>Referer: https://pbank.95559.com.cn/personbank/jump.jsp?dse_sessionId=IFAPIXAGCEDLJAEJFHAQGPGTJRFGEBCJFLALBIBZ&amp;netType=0
<br>Accept-Language: zh-cn
<br>Accept-Encoding: gzip, deflate
<br>User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; CIBA; 360SE)
<br>Host: pbank.95559.com.cn
<br>Connection: Keep-Alive
<br>Cookie: BocommRegVerState=1; BocommLastLogon=0; <span style="color:red">JSESSIONID=0000v3Ar_VMgAxauqlrDo-mJdVz:-1;</span> userLanguage=zh_CN
<br>
<br>
<br>这点更困惑了，ssl情况下依然能抓到sessionid，ssl又是用什么机制防止session被篡的？
<br>
          
          <br><br>
          作者: <a href="http://zhoupjam.javaeye.com">zhoupjam</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/700057" style="color:red">已有 <strong>28</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>