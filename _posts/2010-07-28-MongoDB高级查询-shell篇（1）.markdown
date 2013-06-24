---
layout: post
title:  "MongoDB高级查询-shell篇（1）"
date:   2010-07-28 23:20:22
author: 
categories: program
---

## MongoDB高级查询-shell篇（1）
### by 
### at 2010-07-28 23:20:22
### original <http://www.javaeye.com/topic/723842>

<div style="color:#000000;font-family:Verdana,Arial,Helvetica,sans-serif;font-size:12px;background-color:#ffffff;margin:8px">
<div style="color:#000000;font-family:Verdana,Arial,Helvetica,sans-serif;font-size:12px;background-color:#ffffff;margin:8px">
<div style="color:#000000;font-family:Verdana,Arial,Helvetica,sans-serif;font-size:12px;background-color:#ffffff;margin:8px">
<h1 style="margin-left:0cm;text-indent:0cm;text-align:left">
<span lang="EN-US"><span style="font-size:x-large">MongoDB</span></span><span><span style="font-size:x-large">高级查询</span><span lang="EN-US"><span style="font-size:x-large">-shell</span></span><span style="font-size:x-large">篇</span></span>
</h1>
<p style="margin:0px"><span lang="EN-US"> </span></p>
<p style="margin:0px"><span><span style="font-size:small"><strong></strong></span></span></p>
<span><strong>
<p style="margin:0px"><span><span style="font-weight:normal"><span style="font-size:medium">翻译自</span></span><span lang="EN-US"><a href="http://www.mongodb.org/display/DOCS/Advanced+Queries"><span><span style="font-weight:normal"><span style="font-size:medium">http://www.mongodb.org/display/DOCS/Advanced+Queries</span></span></span></a></span><span style="font-weight:normal"><span style="font-size:medium">部分内容。</span></span><span lang="EN-US"></span></span></p>
<p style="margin:0px"><span><span style="font-weight:normal"><span style="font-size:medium">其实内容并不难理解，主要照顾英语苦手的兄弟们，也方便自己。</span></span><span lang="EN-US"></span></span></p>
<p style="margin:0px"><span><span style="font-weight:normal"><span style="font-size:medium">这里主要是讲</span></span><span lang="EN-US"><span style="font-weight:normal"><span style="font-size:medium">MongoDB</span></span></span><span style="font-weight:normal"><span style="font-size:medium">在控制台中如何进行高级查询，既有教程内容，也有ME动手实验的经验，搞懂了这些规则，对于你再使用其他语言（</span></span><span lang="EN-US"><span style="font-weight:normal"><span style="font-size:medium">Java</span></span></span><span style="font-weight:normal"><span style="font-size:medium">，</span></span><span lang="EN-US"><span style="font-weight:normal"><span style="font-size:medium">ruby</span></span></span><span style="font-weight:normal"><span style="font-size:medium">，</span></span><span lang="EN-US"><span style="font-weight:normal"><span style="font-size:medium">python</span></span></span><span style="font-weight:normal"><span style="font-size:medium">等）实现查询时有莫大的帮助，因为基础的是相通的，只是不同的语言实现接口略有差异而已。</span></span></span></p>
<p style="margin:0px"><span style="font-size:medium">还有一句想提醒大家，<span style="color:#ff0000">多动手实验</span>，才是硬道理。</span></p>
</strong></span>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium">&lt;,&gt;,&gt;=,&lt;=</span></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">这四个就不用解释了，最常用的，也是最简单的。</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.collection.find({ </span><span lang="EN-US">"field"</span><span lang="EN-US"> : { $gt: value } } )<span>   </span></span><span lang="EN-US">// </span><span>大于<span lang="EN-US"><span>  </span>: field &gt; value</span></span></p>
<p style="line-height:15.6pt"><span lang="EN-US">db.collection.find({ </span><span lang="EN-US">"field"</span><span lang="EN-US"> : { $lt: value } } )<span>   </span></span><span lang="EN-US">// </span><span>小于<span lang="EN-US"><span>  </span>:<span>  </span>field &lt; value</span></span></p>
<p style="line-height:15.6pt"><span lang="EN-US">db.collection.find({ </span><span lang="EN-US">"field"</span><span lang="EN-US"> : { $gte: value } } )<span>  </span></span><span lang="EN-US">// </span><span>大于等于 <span lang="EN-US">: field &gt;= value</span></span></p>
<p style="line-height:15.6pt"><span lang="EN-US">db.collection.find({ </span><span lang="EN-US">"field"</span><span lang="EN-US"> : { $lte: value } } )<span>  </span></span><span lang="EN-US">// </span><span>小于等于 <span lang="EN-US">: field &lt;= value</span></span></p>
<p style="line-height:15.6pt"> </p>
<p style="line-height:15.6pt"><strong><span style="color:#333399"><span style="font-size:medium">如果要同时满足多个条件，记得要这样用：</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.collection.find({ </span><span lang="EN-US">"field"</span><span lang="EN-US"> : { $gt: value1, $lt: value2 } } )<span>    </span></span><span lang="EN-US">// value1 &lt; field &lt; value</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-weight:bold"><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$ne  </span><span style="white-space:pre"><span style="color:#333399">	</span></span></span></span><span style="font-size:medium"><span style="color:#333399">不等于</span></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { x : { $ne : 3 } } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">条件相当于</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">x&lt;&gt;3,</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">即</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">x</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">不等于</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">3</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">。</span></strong></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US"><strong><span style="font-size:medium"><span style="color:#333399">$mod    </span></span></strong></span><strong><span style="font-size:medium"><span style="color:#333399">取模运算</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { a : { $mod : [ 10 , 1 ] } } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">条件相当于</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">a % 10 == 1 </span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">即</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">a</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">除以</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">10</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">余数为</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">1</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">的。</span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$nin</span></span><span style="font-size:medium"><span style="color:#333399"> </span><span style="white-space:pre"><span style="color:#333399">	</span></span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">不属于</span></strong></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find({j:{$nin: [2,4,6]}})</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">条件相当于 </span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">j </span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">不等于 </span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">[2,4,6] </span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">中的任何一个。</span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="font-weight:bold"><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$in</span></span><span style="font-size:medium"><span style="color:#333399">     </span></span></span><span style="font-size:medium"><span style="color:#333399">属于</span></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find({j:{$in: [2,4,6]}})</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">条件相当于</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">j</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">等于</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">[2,4,6]</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">中的任何一个。</span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span lang="EN-US"><strong><span style="font-size:medium"><span style="color:#333399">$all </span><span style="white-space:pre"><span style="color:#333399">	</span></span></span></strong><span style="color:#333399"><strong></strong></span></span><strong><span style="font-size:medium"><span style="color:#333399">全部属于</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { a: { $all: [ 2, 3 ] } } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><strong><span style="font-size:medium"><span style="color:#333399">与</span></span></strong><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$in</span></span></span></strong><strong><span style="font-size:medium"><span style="color:#333399">类似，但必须是</span></span></strong><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">[]</span></span></span></strong><strong><span style="font-size:medium"><span style="color:#333399">的值全部都存在。</span></span></strong></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="font-weight:bold"><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$size</span></span><span style="font-size:medium"><span style="color:#333399">     </span></span></span><span style="font-size:medium"><span style="color:#333399">数量，尺寸</span></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { a : { $size: 1 } } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">条件相当于</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">a</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">的值的数量是</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">1</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">（</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">a</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">必须是数组，一个值的情况不能算是数量为</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">1</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">的数组）。</span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span lang="EN-US"><strong><span style="font-size:medium"><span style="color:#333399">$exists</span></span></strong><strong><span style="font-size:medium"><span style="color:#333399">   </span></span></strong></span><strong><span style="font-size:medium"><span style="color:#333399">字段存在</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { a : { $exists : </span><span lang="EN-US">true</span><span lang="EN-US"> } } )</span></p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { a : { $exists : </span><span lang="EN-US">false</span><span lang="EN-US"> } } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">true</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">返回存在字段</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">a</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">的数据，</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">false</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">返回不存在字度</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">a</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">的数据。</span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="font-weight:bold"><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$type</span></span><span style="font-size:medium"><span style="color:#333399">     </span></span></span><span style="font-size:medium"><span style="color:#333399">字段类型</span></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { a : { $type : 2 } } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">条件是</span></strong></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">a</span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399">类型符合的话返回数据。</span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><br></span></p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">参数类型如下图：</span></strong></span></p>
<p style="margin:0px"><span style="font-size:medium"><strong><br></strong></span></p>
<table style="margin-left:.9pt;border-collapse:collapse" border="1" cellspacing="0" cellpadding="0"><tbody>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="text-align:left;line-height:13.0pt" align="left"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">Type Name</span></strong></span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399"><strong></strong></span></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="text-align:left;line-height:13.0pt" align="left"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">Type Number</span></strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Double</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>1</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>String</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>2</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Object</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>3</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Array</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>4</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Binary data</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>5</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Object id</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>7</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Boolean</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>8</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Date</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>9</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Null</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>10</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Regular expression</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>11</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>JavaScript code</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>13</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Symbol</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>14</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>JavaScript code with scope</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>15</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>32-bit integer</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>16</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Timestamp</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>17</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>64-bit integer</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>18</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Min key</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>255</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
<tr>
<td style="width:5.0cm;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="189" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>Max key</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
<td style="width:223.8pt;padding:1.4pt 1.4pt 1.4pt 1.4pt" width="298" valign="top">
<p style="line-height:13.0pt"><span lang="EN-US"><span style="font-size:medium"><strong>127</strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong></strong></span></span></p>
</td>
</tr>
</tbody></table>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">Regular Expressions</span></strong></span><span style="font-size:medium"><strong><span style="color:#333399">    </span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">正则表达式</span></strong></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.customers.find( { name : /acme.*corp/i } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399">类似</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">sql</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">中的</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">like</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">方法。</span></strong></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><br></span></strong></p>
<p style="margin:0px"><span style="color:#333399"><strong></strong></span><span style="font-size:medium"><strong><span style="color:#333399">行开始 </span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">/^ </span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">行结束 </span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">$/</span></strong></span></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><br></span></strong></p>
<p style="margin:0px"><span lang="EN-US"><span style="color:#333399"><strong></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">这里要特别特别特别地注意一点，关乎查询效率：</span></strong></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span><span style="background-color:#ffff99"><span style="font-size:small">While /^a/, /^a.</span></span><strong><span style="background-color:#ffff99"><span style="font-size:small">/, and</span></span></strong><span style="background-color:#ffff99"><span style="font-size:small"> </span></span><strong><span style="background-color:#ffff99"><span style="font-size:small">/^a.</span></span></strong><span style="background-color:#ffff99"><span style="font-size:small">$/ are equivalent and will all use an index in the same way, the later two require scanning the whole string so they will be slower. The first format can stop scanning after the prefix is matched.</span></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">意思大概就是指在查询以</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">a</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">开头字符串时，可以有三种形式，</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium"> </span></span></strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">/^a/, /^a./,</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">和</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">/^a.$/ </span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">。后面两种形式会扫描整个字符串，查询速度会变慢。第一种形式会在查到符合的开头后停止扫描后面的字符。</span></span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><strong><br></strong></span></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">所以要特别注意。</span></span></strong></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><strong><br></strong></span></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">几个附加参数：</span></span></strong></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><strong><br></strong></span></span></p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">i</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">的意思是忽略大小写。（这个很重要，很常用）</span></span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><strong><br></strong></span></span></p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">m</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">的意思是支持多行。（不过</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">ME</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">没有尝试过）</span></span></strong></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><strong><br></strong></span></span></p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">x</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">的意思是扩展。（也没用过）</span></span></strong></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-weight:bold"><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">$or</span></span><span style="font-size:medium"><span style="color:#333399">  </span></span></span><span style="font-size:medium"><span style="color:#333399">或</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399"> </span></span></span><span style="font-size:medium"><span style="color:#333399">（注意：</span></span><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399">MongoDB 1.5.3</span></span></span><span style="font-size:medium"><span style="color:#333399">后版本可用）</span></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.foo.find( { $or : [ { a : 1 } , { b : 2 } ] } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:14px"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">符合条件</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">a=1</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">的或者符合条件</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">b=2</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">的数据都会查询出来。</span></span></strong></span></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium"><br></span></span></strong></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">与其他字段一起查询：</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.foo.find( { name : </span><span lang="EN-US">"bob"</span><span lang="EN-US"> , $or : [ { a : 1 } , { b : 2 } ] } )</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="font-size:14px"><span style="font-size:medium"><strong><span style="color:#333399">符合条件</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">name</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">等于</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">bob</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">，同时符合其他两个条件中任意一个的数据。</span></strong></span></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">Value in an Array</span></span></strong></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">   </span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">数组中的值</span></span></strong></span></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium"><br></span></span></strong></p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">例如数据库中存在这样的数据：</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span>{ "_id" : ObjectId("4c503405645fa23b31e11631"), "colors" : [ "red", "black" ] }</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">查询</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span lang="EN-US">db.things.find( { colors : </span><span lang="EN-US">"red"</span><span lang="EN-US"> } );</span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><strong><span style="color:#333399"><span style="font-size:medium">即可查到上面那条数据。</span></span></strong></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin:0px"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin:0px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">$elemMatch</span></span></strong></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">   </span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">要素符合</span></span></strong></span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span>t.find( { x : { $elemMatch : { a : 1, b : { $gt : 1 } } } } ) </span></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><strong><span style="font-size:medium"><span style="color:#333399">结果：</span></span></strong></p>
<p style="margin:0px"> </p>
<p style="margin:0px"><span style="line-height:20px"><span lang="EN-US">{ </span><span lang="EN-US">"_id"</span><span lang="EN-US"> : ObjectId(</span><span lang="EN-US">"4b5783300334000000000aa9"</span><span lang="EN-US">),</span></span></p>
<p style="line-height:15.6pt"><span lang="EN-US"><span>  </span></span><span lang="EN-US">"x"</span><span lang="EN-US"> : [ { </span><span lang="EN-US">"a"</span><span lang="EN-US"> : 1, </span><span lang="EN-US">"b"</span><span lang="EN-US"> : 3 }, 7, { </span><span lang="EN-US">"b"</span><span lang="EN-US"> : 99 }, { </span><span lang="EN-US">"a"</span><span lang="EN-US"> : 11 } ]</span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span lang="EN-US">}</span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="line-height:normal"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">x</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">其中一个要素符合那个检索条件就可以被检索出来。（不过一般谁用像</span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">x</span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">这样的结构去保存数据呢？）</span></strong></span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="line-height:normal"><span lang="EN-US"><span style="font-size:medium"><strong><span style="font-size:medium"><span style="color:#333399">Value in an Embedded Object</span></span></strong></span><span style="font-size:medium"><strong><span style="font-size:medium"><span style="color:#333399">    </span></span></strong></span></span><span style="font-size:medium"><strong><span style="font-size:medium"><span style="color:#333399">内嵌对象中的值</span></span></strong></span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="font-size:medium"><span style="color:#333399"><br></span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><strong><span style="font-size:medium"><span style="color:#333399">例如数据库中存在这样的数据：</span></span></strong></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span><strong></strong></span><span>{ "_id" : ObjectId("4c503773645fa23b31e11632"), "author" : { "name" : "Dan Brown", "age" : 38 }, "book" : "The Lost Symbol" }</span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><strong><span style="font-size:medium"><span style="color:#333399">查询：</span></span></strong></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span><strong></strong></span><span style="line-height:normal"><span lang="EN-US">db.postings.find( { </span><span lang="EN-US">"author.name"</span><span lang="EN-US"> : </span><span lang="EN-US">"Dan Brown"</span><span lang="EN-US"> } );</span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><strong><span style="color:#333399"><span style="font-size:medium">即可查到上面那条数据。</span></span></strong></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="color:#333399"><span style="font-size:medium"><strong></strong></span></span><span style="line-height:normal"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">查询内嵌对象的属性，记得要加上</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">“”</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">，字段是</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">“</span></span></strong></span></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">author.name</span></span></strong></span></span><strong><span lang="EN-US"><span style="font-size:medium"><span style="color:#333399"><span style="font-size:medium">”</span></span></span></span></strong><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">，而不是</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">author.name</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">。</span></span></strong></span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="line-height:normal"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399">$not</span><span style="white-space:pre"><span style="color:#333399">	</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399">不是</span></strong></span></span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="line-height:normal"><span><span style="font-size:medium"><strong></strong></span></span></span><span>db.customers.find( { name : { $not : /acme.*corp/i } } );</span></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"> </p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><strong><span style="color:#333399"><span style="font-size:medium">这是一个与其他查询条件组合使用的操作符，不会单独使用。</span></span></strong></p>
<p style="margin-top:7.5pt;margin-right:0cm;margin-bottom:7.5pt;margin-left:0cm;line-height:15.6pt"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="color:#333399"><span style="font-size:medium"><strong></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">只要你理解了前面的查询操作即可，只是再加上了</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">$not</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">，结果就是得到了没有</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">$not</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">的相反结果集。</span></span></strong></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="color:#333399"><span style="font-size:medium"><br></span></span></p>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"><span style="font-size:14px"><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">$where</span></span></strong></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">  </span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">貌似是</span></span></strong></span><span lang="EN-US"><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">JavaScript</span></span></strong></span></span><span style="font-size:medium"><strong><span style="color:#333399"><span style="font-size:medium">特用的操作符。（先不解释了）</span></span></strong></span></span></p>
<div><br></div>
<div>PS:竟然太长了，一篇发布不了，哎~~~~</div>
</div>
<p> </p>
</div>
<p style="margin-top:12.0pt;margin-right:0cm;margin-bottom:6.0pt;margin-left:0cm"> </p>
</div>
          
          <br><br>
          作者: <a href="http://pangwu86.javaeye.com">pangwu86</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/723842" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>