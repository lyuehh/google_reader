---
layout: post
title:  "Ext Designer 1.0.2 破解程序"
date:   2010-10-17 13:19:21
author: 
categories: program
---

## Ext Designer 1.0.2 破解程序
### by 
### at 2010-10-17 13:19:21
### original <http://www.javaeye.com/topic/786752>

<p>现项目中需要使用ext js ，顺便下了个Ext Designer，但只有14天的该用，过期后可以调整系统日期可以继续使用，但较麻烦，在网上找到了补丁程序，但是其他软件写的，为了方便Java朋友，现将补丁程序改成了Java补丁程序(<span style="color:#ff0000">请注意，请在官方下载1.0.2的版本</span>)：</p>
<p> </p>
<pre name="code">import java.io.IOException;
import java.io.RandomAccessFile;

public class Patch {
	public static void main(String[] args) throws IOException {
		RandomAccessFile rf = new RandomAccessFile(
				"C:/Documents and Settings/xxxx/Local Settings/Application Data/Ext JS, Inc/Ext Designer/libion-1.0.2.0.dll",
				"rw");
		RandomAccessFile rf2 = new RandomAccessFile(
				"C:/Program Files/ExtDesigner/libion-1.0.2.0.dll", "rw");
		rf.seek(31209);
		rf2.seek(31209);
		byte[] by = "\u00B3\u0001".getBytes("ISO8859-1");
		rf.write(by);
		rf2.write(by);
		rf.close();
		rf2.close();
	}
}</pre>
<p> 请注意程序中的XXX，请将它改为你windowx的当前使用账号，另外我是装在了C:/Program Files/ExtDesigner下。</p>
<p> </p>
<p>下面是过期了，但使用补丁后还是可以使用：</p>
<p><br><img src="http://dl.javaeye.com/upload/attachment/332353/7602710b-1c34-368c-8242-fb45da9ea5fa.jpg" alt=""></p>
<p><strong>300多天够使了！</strong></p>
<p><br><img src="http://dl.javaeye.com/upload/attachment/332355/996aa6e3-ed04-3f61-8fcf-8c1da67d4c4b.jpg" alt=""><br> </p>
<p><br><img src="http://dl.javaeye.com/upload/attachment/332357/c4129891-1c49-36e6-8199-32bbe41e5747.jpg" alt=""><br> </p>
<p><br> </p>
          
          <br><br>
          作者: <a href="http://jiangzhengjun.javaeye.com">junJZ_2008</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/786752" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li><li><a href="http://www.iteye.com/clicks/439"><span style="color:blue;font-weight:bold">限时报名参加Oracle技术大会</span></a></li></ul>
<br><br><br>