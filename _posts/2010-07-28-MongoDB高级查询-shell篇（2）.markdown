---
layout: post
title:  "MongoDB高级查询-shell篇（2）"
date:   2010-07-28 23:34:34
author: 
categories: program
---

## MongoDB高级查询-shell篇（2）
### by 
### at 2010-07-28 23:34:34
### original <http://www.javaeye.com/topic/723851>

<p><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><br></span></span></span></p>
<p><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><br></span></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;background:white"><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff">接上篇~~~~~</span></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;background:white"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">sort()    </span></span></span></strong><strong><span style="color:#000080"><span style="font-size:medium">排序</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><span style="font-size:medium"><strong><br></strong></span></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="color:#000080"><span style="font-size:medium">这个非常实用。即</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">sql</span></span></span><span style="color:#000080"><span style="font-size:medium">语言中的</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">OrderBy</span></span></span><span style="color:#000080"><span style="font-size:medium">。</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">db.myCollection.find().sort(
{ ts : -1 } )</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">也可以多个字段排序</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">db.myCollection.find().sort(
{ ts : -1 ,ds : 1 } )</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">这里的</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">1</span></span></span><span style="font-size:medium"><span style="color:#000080">代表升序，</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">-1</span></span></span><span style="font-size:medium"><span style="color:#000080">代表降序。</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><span style="font-size:medium"><span style="color:#000080"><br></span></span></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">经过</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">ME</span></span></span><span style="font-size:medium"><span style="color:#000080">的实验，小于</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">0</span></span></span><span style="font-size:medium"><span style="color:#000080">的数字就是降序，</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">0</span></span></span><span style="font-size:medium"><span style="color:#000080">以上（包括</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">0</span></span></span><span style="font-size:medium"><span style="color:#000080">）就是升序。</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">limit()   skip()</span></span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"><strong><span style="color:#000080"><span style="font-size:medium">这两个</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">ME</span></span></span><span style="color:#000080"><span style="font-size:medium">想连起来讲，他们就是你实现数据库分页的好帮手</span></span></strong><strong><span style="color:#000080"><span style="font-size:medium">。</span></span></strong></p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">limit()</span></span></span></strong><strong><span style="color:#000080"><span style="font-size:medium">控制返回结果数量，如果参数是</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">0</span></span></span><span style="color:#000080"><span style="font-size:medium">，则当作没有约束，</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">limit()</span></span></span><span style="color:#000080"><span style="font-size:medium">将不起作用。</span></span></strong></p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">skip()</span></span></span></strong><strong><span style="color:#000080"><span style="font-size:medium">控制返回结果跳过多少数量，如果参数是</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">0</span></span></span><span style="color:#000080"><span style="font-size:medium">，则当作没有约束，</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">skip()</span></span></span><span style="color:#000080"><span style="font-size:medium">将不起作用，或者说跳过了</span></span><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">0</span></span></span><span style="color:#000080"><span style="font-size:medium">条。</span></span></strong></p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="color:#000080"><span style="font-size:medium">例如：</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US"> db.test.find().skip(5).limit(5)</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">结果就是取第</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">6</span></span></span><span style="font-size:medium"><span style="color:#000080">条到第</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">10</span></span></span><span style="font-size:medium"><span style="color:#000080">条数据。</span></span></strong></p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium"> </span></span></span></p>
<p style="text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium">snapshot()   </span></span></span></strong><strong><span style="color:#000080"><span style="font-size:medium">（没有尝试）</span></span></strong><span style="color:#000080"><span style="font-size:medium"></span></span></p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"><span style="color:#000080"><span style="font-size:medium"> </span></span></span></p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080"> </span></span></span></p>
<p style="text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">count()   </span></span></span></strong><strong><span style="font-size:medium"><span style="color:#000080">条数</span></span></strong></p>
<p style="text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">返回结果集的条数。</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">db.test.count()</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">在加入</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">skip()</span></span></span><span style="font-size:medium"><span style="color:#000080">和</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">limit()</span></span></span><span style="font-size:medium"><span style="color:#000080">这两个操作时，要获得实际返回的结果数，需要一个参数</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">true</span></span></span><span style="font-size:medium"><span style="color:#000080">，否则返回的是符合查询条件的结果总数。</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"><strong><span style="font-size:medium"><span style="color:#000080">例子如下：</span></span></strong></p>
<p style="margin-top:12.0pt;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">&gt;
db.test.find().skip(5).limit(5).count()</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">9</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">&gt;
db.test.find().skip(5).limit(5).count(true)</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US">4</span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"> </p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">group()   (</span></span></span></strong><strong><span style="font-size:medium"><span style="color:#000080">没有尝试</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#000080">)</span></span></span></strong></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US"><span style="font-size:medium">########### </span></span><span style="font-size:medium"><span style="color:#99cc00">华丽丽的分割线</span></span><span lang="EN-US"><span style="font-size:medium"> #############</span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm;text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"> </span></p>
<p style="text-align:left;background:white" align="left"><span style="font-size:medium">关于在控制台中的查询差不多就这么多了，可以说这些查询都是普通</span><span lang="EN-US"><span style="font-size:medium">Sql</span></span><span style="font-size:medium">语句中进行单表查询的一些操作，貌似没有看到涉及多表查询，即不同的</span><span lang="EN-US"><span style="font-size:medium">collection</span></span><span style="font-size:medium">间的关联查询。</span></p>
<p style="text-align:left;background:white" align="left"><span style="font-size:medium">所以在设计表结构的时候，常用的一些基于关系数据库的表的设计方式已经不再适用。</span></p>
<p style="text-align:left;background:white" align="left"><span style="font-size:medium">但是不得不说，单表查询（应该说是</span><span lang="EN-US"><span style="font-size:medium">collection</span></span><span style="font-size:medium">查询）的功能还是灰常灰常强大的。</span></p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"><span style="font-size:medium"> </span></span><span style="font-size:medium"></span></p>
<p style="text-align:left;background:white" align="left"><span lang="EN-US"><span style="font-size:medium"> </span></span><span style="font-size:medium"></span></p>
<p style="text-align:left;background:white" align="left"><span style="font-size:medium">下次讲解下使用</span><span lang="EN-US"><span style="font-size:medium">JAVA</span></span><span style="font-size:medium">语言的</span><span lang="EN-US"><span style="font-size:medium">Driver</span></span><span style="font-size:medium">怎么实现上面的查询的。</span></p>
<p style="margin:0cm;background:white"><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><span style="font-size:medium"><br></span></span></span></span></p>
<p style="margin:0cm;background:white"><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><span style="font-size:medium"><br></span></span></span></span></p>
<p style="margin:0cm;background:white"><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><span style="font-size:medium"><br></span></span></span></span></p>
<p style="margin:0cm;background:white"><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><span style="font-size:medium">PS：oh my lady gaga！这个排版太有问题了。ME下次得好好搞搞……</span></span></span></span></p>
<p style="margin:0cm;background:white"><span style="background-color:#ffffff"><span style="background-color:#c0c0c0"><span style="background-color:#ffffff"><br></span></span></span></p>
<p style="margin:0cm;background:white"> </p>
          
          <br><br>
          作者: <a href="http://pangwu86.javaeye.com">pangwu86</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/723851" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>