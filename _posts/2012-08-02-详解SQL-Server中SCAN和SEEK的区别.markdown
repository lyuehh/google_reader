---
layout: post
title:  "详解SQL Server中SCAN和SEEK的区别"
date:   2012-08-02 10:44:27
author: 杨玉廷
categories: program
---

## 详解SQL Server中SCAN和SEEK的区别
### by 杨玉廷
### at 2012-08-02 10:44:27
### original <http://hp.dewen.org/?p=1091>

<p>SQL SERVER使用扫描（scan）和查找（seek）这两种算法从数据表和索引中读取数据。这两种算法构成了查询的基础，几乎无处不在。Scan会扫描并且返回整个表或整个索引。 而seek则更有效率，根据谓词（predicate），只返索引内的一个或多个范围内的数据。下面将以如下的查询语句作为例子来分析scan和seek：</p>
<pre><code>select OrderDate from Orders where OrderKey = 2</code></pre>
<div style="text-align:center;margin-bottom:5px;margin-top:5px"><img src="http://hp.dewen.org/wp-content/uploads/2012/08/sqlserver_logo.png" alt="SQL Server中SCAN和SEEK的区别" title="SQL Server中SCAN和SEEK的区别" border="0"></div>
<p><span></span></p>
<h2 style="font-size:16px;text-transform:none">Scan</h2>
<p>使用Scan的方式，SQL Server 会去读取Orders表中的每一行数据，读取的时候评估是否满足谓词 “where order＝2”。如果满足（数据行符合条件），则返回该行。这个例子里，我们将这个谓词称作“residual predicate”。为了得到最优的性能，SQL会尽可能地在扫描中使用“residual predicate”。但如果residual predicate的开销过于昂贵，SQL Server可能会使用单独的“filter iterator”. “residual predicate”以where关键字的形式出现在文本格式的plan中。对XML格式的plan，则是&lt;predicate&gt;标记的形式。</p>
<p>下面这个扫描的文本格式的plan的结果：</p>
<p>|–Table Scan(OBJECT:([ORDERS]), WHERE:([ORDERKEY]=(2)))</p>
<p>下图说明了扫描的方式：</p>
<p><a href="http://blogs.msdn.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-42-89/8535.image001.png"><img src="http://hp.dewen.org/wp-content/uploads/2012/08/SQL-Server-SCAN-SEEK-01.png" alt="SQL Server中SCAN 和SEEK的区别" border="0"></a></p>
<p>无论数据行是否满足条件，扫描的读取方式都会访问表中的每一个数据，所以scan的成本和表的数据总量是成比例的。 因此，如果表很小或者表内的大多数数据多满足谓词，scan是一种有效率的读取方式。然而如果表很大或者绝大多数的数据并不满足谓词, 那么这种方式会让我们访问到太多不需要的数据页面，并执行更多的额外的IO操作。</p>
<h2 style="font-size:16px;text-transform:none">Seek</h2>
<p>继续以上面的查询为例子，如果在orderkey列上有一个索引，那么seek可能会是一个好的选择。使用seek的访问方式，SQL Server会使用索引直接导向到满足谓词条件的数据行。 这个例子里，我们将这个谓词称为“seek predicate”。 大多数情况下，SQL Server不必将“seek predicate”重新评估为“residual predicate”。 索引会保证“seek”只返回符合条件的数据行。“seek predicate”以seek关键字的形式出现在文本格式的plan中。 对于xml 格式的plan,则以&lt;seekpredicates&gt;标记出现。</p>
<p>下面是使用seek的文本格式的plan的结果：</p>
<p>|–Index Seek(OBJECT:([ORDERS].[OKEY_IDX]), SEEK:([ORDERKEY]=(2)) ORDERED FORWARD)</p>
<p><a href="http://blogs.msdn.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-42-89/3482.image002.jpg"><img src="http://hp.dewen.org/wp-content/uploads/2012/08/SQL-Server-SCAN-SEEK-02.jpg" alt="SQL Server中SCAN 和SEEK的区别" border="0"></a></p>
<p>使用seek时，SQL Server只会直接访问到满足条件的数据行和数据页，因此它的成本只跟满足条件的数据行的及其相应的数据页面数量成比例， 和基表的数据量完全没有关系。因此，如果对于一个选择性很高（通过这个谓词，可以筛选掉表中的大部分数据）的谓词条件，seek是非常高效的。</p>
<p>下面的表格列出了seek和scan这两种查找方式和堆表，聚簇索引和非聚簇索引的各种组合：</p>
<table border="0">
<tbody>
<tr>
<td></td>
<td>Scan</td>
<td>Seek</td>
</tr>
<tr>
<td>Heap</td>
<td>Table Scan</td>
<td></td>
</tr>
<tr>
<td>Clustered Index</td>
<td>Clustered Index Scan</td>
<td>Clustered Index Seek</td>
</tr>
<tr>
<td>Non-Clustered Index</td>
<td>Index Scan</td>
<td>Index Seek</td>
</tr>
</tbody>
</table>
<p></p>
<p><b>本文来源：<a href="http://blogs.msdn.com/b/apgcdsd/archive/2012/08/01/sql-server-scan-seek.aspx">SQL Server中SCAN 和SEEK的区别</a></b></p>