---
layout: post
title:  "NOSQL之mongodb简介及安装 ... 4 replies"
date:   2010-10-09 13:08:38
author: Latest from ChinaonRails
categories: program
---

## NOSQL之mongodb简介及安装 ... 4 replies
### by Latest from ChinaonRails
### at 2010-10-09 13:08:38
### original <http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/cV7Owh7tH_o/4523.html>

自打Facebook的问世，NOSQL得到了最大力度的宣传与推广，面对海量数据的快速存储及读取，关系性数据库已经显得越来越滞后，目前全世界已经有很多的知名的网站开始使用NOSQL。 <br>
<br>
NOSQL，从字面上理解，它就不是SQL，它真正的含义其实是指“非关系性数据库”，而我们日常所用到的MYSQL、SQL SERVER、ORACLE等都属于关系性数据库，二者的最明显的区别就是：关系性数据库中各个表之间可以建立关系，取数据时可以根据关系联表查询，而NOSQL则不一样，它内部的数据是以KEY-VALUE的形式进行存储的，通过KEY直接取到VALUE值。 <br>
<br>
NOSQL发展至今，出现了好几种非关系性数据库，今天就以NOSQL中目前表现最好的MONGODB为例来进行说明。 <br>
<br>
首先讲讲如何在WINDOWS下安装mongodb(下一篇将会讲如何在linux下安装mongodb) <br>
<br>
1.先去官网(http://www.mongodb.org/)下载一个与您的WINDOWS操作系统版本一致的mongodb软件包 <br>
<br>
2.解压软件包，你会看到解压后的文件夹中有个BIN目录，进去之后会看到很多.exe可执行文件，只用关注两个即可(mongod.exe,mongo.exe) <br>
<br>
3.mongod.exe //mongodb的服务器端，运行该执行文件可以设置mongodb的端口号，数据文件存储目录等 <br>
<br>
mongo.exe //mongodb的客户端，用来连接mongodb并进行相关数据的查询 <br>
<br>
4.部署，假如我压缩包解压的位置为 D:\我的文档\Downloads\mongodb-win32-i386-1.6.3 <br>
&gt;&gt; ctrl+r打开运行窗口 <br>
<br>
&gt;&gt; cmd <br>
<br>
&gt;&gt; d: <br>
<br>
&gt;&gt; cd 我的文档\Downloads\mongodb-win32-i386-1.6.3\bin <br>
<br>
&gt;&gt; mongod --dbpath c:/data/db <br>
&gt;&gt; mongo 此时即可进行客户端与mongodb的连接了，成功后将自动连接mongodb的test库，如果你想连接指定的库，那就使用命令：mongo 数据库名称，如果你将mongodb部署在服务器上，那就使用命令：mongo 服务器IP/数据库名称，此处是&quot;/&quot;，而非&quot;\&quot;，用错了就无法连接成功。 <br>
不要关闭该 DOS 窗口。 <br>
<br>
<br>
mongoDB 服务端的默认连接端口是 27017。 <br>
<br>
<br>
<br>
5，mongoDB 服务端启动后，重新打开一个 DOS 窗口，进入D:\我的文档\Downloads\mongodb-win32-i386-1.6.3\bin 目录，运行命令 mongo.exe <br>
<br>
D:\我的文档\Downloads\mongodb-win32-i386-1.6.3\bin&gt;mongo <br>
将会出现下面的信息： <br>
<br>
MongoDB shell version: 1.4.0 <br>
<br>
url: test <br>
<br>
connecting to: test <br>
<br>
type "exit" to exit <br>
<br>
type "help" for help <br>
<br>
&gt; <br>
<br>
mongo.exe 是官方自带的一个命令行管理客户端，在这里可以进行数据库管理和数据库系统的维护。 <br>
<br>
6.运行： <br>
<br>
&gt;help 是帮助命令 <br>
<br>
&gt; show dbs;显示所有数据库显示了 mongoDB 预置的几个数据库。 <br>
<br>
&gt; use test;打开数据库 <br>
<br>
&gt; show collections;显示collection <br>
<br>
&gt; db.test.save({rpg:100});向 collection test 中保存一条信息 <br>
<br>
&gt; db.test.find() ;检索所有记录 <br>
<br>
{ "_id" : ObjectId("5c558875dd6f010304531637"), "rpg" : 100} <br>
<br>
讲完部署，接下来讲讲它的性能如何，mongodb的数据写入与数据读取速度是关系性数据库的很多倍以上，具体多少倍可以去看看老赵的空间，他有进行相关的测试，MONGODB现在已经被国内很多网站用来记录网站的日志。MONGODB也属于内存数据库，它会自动将一些数据缓存到内存中，这个我想也是大家平时自己会去做的事情，毕竟占内存总比占CPU好，MONGODB支持分布式部署，这样我们可以分多台服务器来进行部署。注：MONGODB不建议部署在windows 32的机器上，当数据超过2G的时候会出问题，官方建议安装在windows 64位或linux上。（随着microsoft在全球范围内打击盗版的力度越来越大，加之国外的开源软件都慢慢的转向LINUX平台，我们这些跟着微软走的people也该学着跨跨平台了，为了大家今后更好的发展，可以自学一下python或者是ruby） <br>
<br>
讲完性能，接下来讲讲它的应用环境，MONGODB有好处，那么自然也有它不足的地方，相对于关系性数据库来说，它的安全性就大打折扣了，所以我们应该有选择性的使用，譬如网站的日志记录，网站访问统计等可以使用MONGODB来进行数据存储。 <br>
<br>
讲完应用，接下来讲讲它的语法: <br>
<br>
sql: select * from t_user <br>
<br>
mongodb: db.t_user.find() <br>
<br>
sql: select count(*) from t_user <br>
<br>
mongodb: db.t_user.find().count() <br>
<br>
sql: select top 10 * from t_user where id &lt; (select min(id) from (select top 10 id from t_user order by id desc)) order by id desc //分页取第11条到第20条的数据 <br>
<br>
mongodb: db.t_user.find().limit(10).skip(10).sort({}) <br>
<br>
sql: update t_user set username='lucky' where id=1 <br>
<br>
mongodb: db.t_user.update({id:1},{username:"lucky"}) <br>
<br>
sql: delete t_user where id=1 <br>
<br>
mongodb: db.t_user.delete({id:1}) <br>
<br>
...... <br>
<br>
还有很多语句在这里就不一一写出了，大家可以查看帮助，命令：db.help()-查看与数据库操作有关的帮助，db.t_user.help()-查看与表有关的帮助 <br>
<br>
mongodb针对目前几乎所有的语言都有相关的驱动，针对.net的驱动名称为mongodb.driver，大家去google上搜一下便有下载链接，还会有相应的实例，mongodb在.net下的应用看看它里面包含的实例就能搞定了，挺简单的，唯独group by语法相对复杂一点，如果有能够帮得上大家的地方，可以给我留言。 <br>
<br>
部分参考自： <br>
http://www.cnblogs.com/zengen/archive/2010/09/29/1838402.html <br>
http://blog.csdn.net/firetaker/archive/2010/08/12/5806833.aspx
<p><a href="http://feedads.g.doubleclick.net/~a/5LZZ7K1aGdznxfk0481l__I7LEA/0/da"><img src="http://feedads.g.doubleclick.net/~a/5LZZ7K1aGdznxfk0481l__I7LEA/0/di" border="0" ismap></a><br>
<a href="http://feedads.g.doubleclick.net/~a/5LZZ7K1aGdznxfk0481l__I7LEA/1/da"><img src="http://feedads.g.doubleclick.net/~a/5LZZ7K1aGdznxfk0481l__I7LEA/1/di" border="0" ismap></a></p><img src="http://feeds.feedburner.com/~r/LatestFromChinaonrails/~4/cV7Owh7tH_o" height="1" width="1"><img src="http://www1.feedsky.com/t1/421512868/RubyonRails_q5tb/feedsky/s.gif?r=http://feedproxy.google.com/~r/LatestFromChinaonrails/~3/cV7Owh7tH_o/4523.html" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/feedsky/RubyonRails_q5tb/421512868/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/feedsky/RubyonRails_q5tb/421512868/art01.gif"></a></p>