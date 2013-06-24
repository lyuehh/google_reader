---
layout: post
title:  "[分享]NoSQL数据库MongoDB之Windows安装"
date:   2010-10-24 15:23:30
author: 
categories: program
---

## [分享]NoSQL数据库MongoDB之Windows安装
### by 
### at 2010-10-24 15:23:30
### original <http://www.javaeye.com/topic/792606>

<p>就像和被人们当初炒得沸沸扬扬的SOA、OSGi等技术一样，NoSQL也成了当今的宠儿，虽然SOA、OSGi还在继续，但丝毫没有影响NoSQL的热度。</p>
<p> </p>
<p>我比较喜欢新鲜的技术和知识，几个月前Twitter向NoSQL转型时，我也试着学习下NoSQL，可惜选择的是Cassandra，虽然我非常喜欢Appache，但是Cassandra的资料实在太少，在运行了下Cassandra自带的几个单元测试、看了看其源代码之后就把它丢到一边了，主要是因为没有应用驱动，不想去费时的找学习资料学习。</p>
<p> </p>
<p>今天在CSDN中又看到了NoSQL，不过介绍的是MongoDB，话说其资料很丰富，社区也很活跃。于是我Google了一下MongoDB，不搜不知道，OMG，从安装到入门，资料真的是相当的多啊，呵呵，于是手又痒痒了，又想试一试。</p>
<p> </p>
<p>下面主要是我在Windows上（Win7）安装、运行、安装Windows服务的笔记，以作备忘。</p>
<p> </p>
<p><strong>1、下载</strong></p>
<p> </p>
<p>下载地址：<a href="http://www.mongodb.org/downloads">http://www.mongodb.org/downloads</a></p>
<p> </p>
<p>从其下载页面就可以看出MongoDB还真的是不赖，支持各个操作系统，其驱动也有好多，其中就有我喜欢的Java和Python驱动，看样子我真的应该学习下了。</p>
<p> </p>
<p>我下载的是Windows 32 1.7.1版本的【<a href="http://fastdl.mongodb.org/win32/mongodb-win32-i386-1.7.1.zip">http://fastdl.mongodb.org/win32/mongodb-win32-i386-1.7.1.zip</a>】（正在开发中，不稳定，不过学习可以了），它的说明中有个限制，数据最大只能2GB，不过对于学习来说，已经足够了。</p>
<p> </p>
<p><strong>2、安装</strong></p>
<p> </p>
<p>安装非常简单，解压就行了，我解压后，放在D:/MongoDB目录下。</p>
<p> </p>
<p>为了命令行的方便，我把D:/MongoDB/bin加到系统环境变量的path中了。</p>
<p> </p>
<p><strong>3、运行</strong></p>
<p> </p>
<p>D:\&gt;mongod --dbpath D:/MongoDB/data<br>Sun Oct 24 15:07:05 MongoDB starting : pid=2472 port=27017 dbpath=D:/MongoDB/data 32-bit</p>
<p>** NOTE: This is a development version (1.7.1) of MongoDB.<br>**       Not recommended for production.</p>
<p>** NOTE: when using MongoDB 32 bit, you are limited to about 2 gigabytes of data</p>
<p>**       see <a href="http://blog.mongodb.org/post/137788967/32-bit-limitations">http://blog.mongodb.org/post/137788967/32-bit-limitations</a></p>
<p>Sun Oct 24 15:07:05 db version v1.7.1, pdfile version 4.5<br>Sun Oct 24 15:07:05 git version: cee2d0d6816a704126c283401db24c949d5f52a3<br>Sun Oct 24 15:07:05 sys info: windows (5, 1, 2600, 2, 'Service Pack 3') BOOST_LIB_VERSION=1_35<br>Sun Oct 24 15:07:05 [initandlisten] waiting for connections on port 27017<br>Sun Oct 24 15:07:05 [websvr] web admin interface listening on port 28017</p>
<p> </p>
<p>由于是开发版，上面就有个警告，没关系，忽略它。</p>
<p> </p>
<p>最后两行说明的数据库端口和Web端口，分别是27017和28017，在浏览器中打开<a href="http://localhost:28017">http://localhost:28017</a>，可以看到其相关的一些信息。</p>
<p> </p>
<p><strong>4、安装Windows服务</strong></p>
<p> </p>
<p>每次运行mongod --dbpath D:/MongoDB/data命令行来启动MongoDB实在是不方便，就像我免安装的MySQL一样，我想把它作为Windows服务，这样就方便多了。</p>
<p> </p>
<p>D:\MongoDB\bin&gt;mongod --logpath D:\MongoDB\logs\MongoDB.log --logappend --dbpath D:\MongoDB\data --directoryperdb --serviceName MongoDB --install<br>all output going to: D:\MongoDB\logs\MongoDB.log<br>Creating service MongoDB.<br>Service creation successful.<br>Service can be started from the command line via 'net start "MongoDB"'.</p>
<p> </p>
<p><span style="color:#ff0000">注意：</span>这条命令要到MongoDB的bin目录下运行，刚开始的时候，我就直接在D:\下运行，结果服务的可执行目录为【"D:\mongod" --logpath  "D:\MongoDB\logs\MongoDB.log"  --logappend  --dbpath  "D:\MongoDB\data"  --directoryperdb  --service 】，肯定是不对的。</p>
<p> </p>
<p>该命令行指定了日志文件：D:\MongoDB\logs\MongoDB.log，日志是以追加的方式输出的；</p>
<p> </p>
<p>数据文件目录：D:\MongoDB\data，并且参数--directoryperdb说明每个DB都会新建一个目录；</p>
<p> </p>
<p>Windows服务的名称：MongoDB；</p>
<p> </p>
<p>以上的三个参数都是可以根据自己的情况而定的，呵呵。</p>
<p> </p>
<p>最后是安装参数：--install，与之相对的是--remove</p>
<p> </p>
<p><span style="color:#ff0000">启动MongoDB：</span>net start MongoDB</p>
<p><br><span style="color:#ff0000">停止MongoDB：</span>net stop MongoDB</p>
<p> </p>
<p>是不是很方便了！</p>
<p> </p>
<p>以上是我的安装MongoDB的初体验，希望能与各位朋友一起学习MongoDB！</p>
<p> </p>
<p>===========================================<br>如有批评、指教、疑惑，请：<a href="mailto:obullxl@163.com">obullxl@163.com</a><br>祝大家使用JAVA愉快！</p>
          
          <br><br>
          作者: <a href="http://obullxl.javaeye.com">obullxl</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/792606" style="color:red">已有 <strong>1</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li><li><a href="http://www.iteye.com/clicks/439"><span style="color:blue;font-weight:bold">限时报名参加Oracle技术大会</span></a></li></ul>
<br><br><br>