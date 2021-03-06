---
layout: post
title:  "从MySQL到Redis，提升数据迁移的效率"
date:   2013-01-07 23:10:06
author: 
categories: program
---

## 从MySQL到Redis，提升数据迁移的效率
### by 
### at 2013-01-07 23:10:06
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/706989109/6253001/1/item.html>

<p>做开发的同学都知道，一旦设计到底层存储优化，数据结构甚至数据库的变更，通常都会进行数据迁移的工作。如果系统运行时间过长，数据迁移的数量可能非常庞大。这时候，如何进行高效的数据迁移，实际也是上线质量的直接影响因素之一。</p>
<p>下面内容是转载的一个小技巧（<a href="http://dcw.ca/blog/2013/01/02/mysql-to-redis-in-one-step/">原文</a>），无法适用于各种变化的场景，仅供大家参考。</p>
<p>场景是从<span><a href="http://blog.nosqlfan.com/tags/mysql" title="查看 MySQL 的全部文章">MySQL</a></span>中将数据导入到<span><a href="http://blog.nosqlfan.com/tags/redis" title="查看 Redis 的全部文章">Redis</a></span>的Hash结构中。当然，最直接的做法就是遍历MySQL数据，一条一条写入到Redis中。这样可能没什么错，但是速度会非常慢。而如果能够使MySQL的查询输出数据直接能够与Redis命令行的输入数据协议相吻合，可能就省事多了。</p>
<p>根据什么都测试，他800w的数据迁移，时间从90分钟缩短到2分钟。</p>
<p>废话说了一堆，下面是具体案例。</p>
<p>MySQL数据表结构：</p>
<pre>CREATE TABLE events_all_time (
  id int(11) unsigned NOT NULL AUTO_INCREMENT,
  action varchar(255) NOT NULL,
  count int(11) NOT NULL DEFAULT 0,
  PRIMARY KEY (id),
  UNIQUE KEY uniq_action (action)
);</pre>
<p>Redis存储结构：</p>
<pre>HSET events_all_time [action] [count]</pre>
<p>下面是重点，能过下面SQL语句将MySQL输出直接变更成redis-cli可接收的格式：</p>
<pre>-- events_to_redis.sql

SELECT CONCAT(
  "*4\r\n",
  '$', LENGTH(redis_cmd), '\r\n',
  redis_cmd, '\r\n',
  '$', LENGTH(redis_key), '\r\n',
  redis_key, '\r\n',
  '$', LENGTH(hkey), '\r\n',
  hkey, '\r\n',
  '$', LENGTH(hval), '\r\n',
  hval, '\r'
)
FROM (
  SELECT
  'HSET' as redis_cmd,
  'events_all_time' AS redis_key,
  action AS hkey,
  count AS hval
  FROM events_all_time
) AS t</pre>
<p>然后用管道符重定向输出即可：</p>
<pre>mysql stats_db --skip-column-names --raw &lt; events_to_redis.sql | redis-cli --pipe</pre>
<div style="margin-top:20px;margin-left:70px;line-height:24px;border:1px solid #ccc;text-align:center;width:545px;background:#fff">
<div style="font-size:16px;font-family:Verdana;background:#d20;color:#fff;float:left;border-radius:10px 0 10px 0;padding:3px 12px 4px;line-height:32px;margin-top:14px">42区 VPS</div>
<div>
42qu.com 云主机 , 卖给创业的你 。 <a href="http://vps.42qu.com/by/iammutex/rss" style="text-decoration:none;background:none;color:#02d">点击这里 , 查看详情</a>
</div>
</div>
<table cellspacing="0" cellpadding="2" border="0" width="100%" style="clear:both">
    
    <tr>
        <td><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.cn/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F287.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F4144.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">从MySQL到MongoDB(添加MongoDB循环插入数据方法)</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.cn/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1466.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F4144.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">foursquare 的数据分析系统（Hadoop+Hive+Redis+MongoDB）</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.cn/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F730.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F4144.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">MongoDB GridFS 数据读取效率 benchmark</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.cn/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3231.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F4144.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">Craigslist迁移20亿数据到MongoDB的经验与教训</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.cn/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F401.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F4144.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">用Redis存储大量数据</font>
                    </a>
                </td>
            </tr>
    
    <tr>
        <td align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/706989109/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/706989109/6253001/1/item.html" border="0" height="0" width="0">