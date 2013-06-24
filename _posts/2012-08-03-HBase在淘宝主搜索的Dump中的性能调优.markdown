---
layout: post
title:  "HBase在淘宝主搜索的Dump中的性能调优"
date:   2012-08-03 16:53:54
author: 道凡
categories: program
---

## HBase在淘宝主搜索的Dump中的性能调优
### by 道凡
### at 2012-08-03 16:53:54
### original <http://www.udpwork.com/item/7866.html>

<p>目前HBase已经运用于淘宝主搜索的全量和增量的数据存储，有效的减低的数据库的压力，增强了业务扩展的能力。Dump系统的特点是要求在短时间内处理大量数据，对延时要求高。在实施这个项目过程中，我们积累了一些优化的实践，抛砖引玉，供大家参考。</p>
<p>环境：Hadoop CDH3U4 + HBase 0.92.1</p>
<p>1、  尽可能用LZO</p>
<p>数据使用LZO，不仅可以节省存储空间尤其是可以提高传输的效率，因为数据是在regionserver端作解压的。通过测试，可以明显提高HBASE从HDFS的读的性能。尽量不用GZ的方式，GZ的方式在bulkload时有线程安全问题。</p>
<p>2、  根据场景调整Block size</p>
<p>由于使用我们非常关注随机读的性能，一条记录的长度较小，通过设置blocksize=8k，可以提高随机读的性能。</p>
<p>3、  在系统空闲的时候，启动major compaction</p>
<p>在实际中，我们发现随着region不停的flush，hfile的增多会影响scan的性能，为了能控制影响，我们设置了hbase.hregion.majorcompaction为一个比较大的时间，通过另外的定时脚本在空闲的时候集中做各表的major compaction。这样可以保证scan的性能是平稳的。</p>
<p>4、  调整balance策略</p>
<p>我们采用了表级别的balance，但是上线后依旧发现有时scan，会抛timeout的异常。通过hmaster的日志，发现当hbase的表多并且当有regionserver挂掉的时候，表级别balance的策略会导致大面积的region移动。后来通过增加阈值控制，每次balance的时候，每张表的region移动的数量不超过整张表region数量的5%。</p>
<p>5、  关注HDFS的问题</p>
<p>当有regionserver挂掉后，有时split log会很慢，会超时导致master不停的重新resubmit split task，最终导致某些scan任务抛timeout异常。原因是datanode的连接数太多，具体原因是<a href="https://issues.apache.org/jira/browse/HDFS-3359">https://issues.apache.org/jira/browse/HDFS-3359</a>通过升级hdfs到HADOOP CDH3U4之后，问题解决。</p>
<p>6、  注重rowkey设计</p>
<p>使用hash值+具体的key，并且设置一个巨大的MAX_FILESIZE。固定每个region的范围，防止做split，防止split带来的隐患。</p>
<p>7、  尽可能的用batch操作</p>
<p>通过使用batch的方式，能提高近10倍的性能，使原本单条记录的随机读从20ms左右降至2ms左右，因为batch的内部是按regionserver来发送数据的，所以每次batch的List&lt;Row&gt;的大小，应设置成regionserver的若干倍。</p>
<p>8、  如果可以的话，减少数据的versions</p>
<p>由于我们业务只需要一个版本，设置version=1，可以有效的控制hfile的大小，从而控制scan的性能。</p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7866.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7866.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7866.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>