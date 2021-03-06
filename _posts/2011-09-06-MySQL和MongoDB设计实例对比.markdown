---
layout: post
title:  "MySQL和MongoDB设计实例对比"
date:   2011-09-06 22:43:41
author: nosqlfan
categories: program
---

## MySQL和MongoDB设计实例对比
### by nosqlfan
### at 2011-09-06 22:43:41
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/555157233/6253001/1/item.html>

<p>本文转载自<a href="http://huoding.com/">火丁笔记</a>，文章举了一个<span><a href="http://blog.nosqlfan.com/tags/%e6%95%b0%e6%8d%ae%e5%ba%93%e8%ae%be%e8%ae%a1" title="查看 数据库设计 的全部文章">数据库设计</a></span>的例子，对<span><a href="http://blog.nosqlfan.com/tags/mysql" title="查看 MySQL 的全部文章">MySQL</a></span>和<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>两种存储工具，分别进行了数据库结构设计，在MongoDB的设计上，利用了MongoDB的 <span><a href="http://blog.nosqlfan.com/tags/schema-free" title="查看 schema-free 的全部文章">schema-free</a></span>的特性。</p>
<p>虽然文中的例子不一定是最优的选择。但分享此文，希望提醒大家，换个存储，不仅是换一个存储，更重要的是换一套思维。</p>
<blockquote><p>MySQL是关系型数据库中的明星，MongoDB是文档型数据库中的翘楚。下面通过一个设计实例对比一下二者：假设我们正在维护一个手机产品库，里面除了包含手机的名称，品牌等基本信息，还包含了待机时间，外观设计等参数信息，应该如何存取数据呢？</p>
<h3>如果使用MySQL的话，应该如何存取数据呢？</h3>
<p>如果使用MySQL话，手机的基本信息单独是一个表，另外由于不同手机的参数信息差异很大，所以还需要一个参数表来单独保存。</p>
<pre>CREATE TABLE IF NOT EXISTS `mobiles` (
    `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(100) NOT NULL,
    `brand` VARCHAR(100) NOT NULL,
    PRIMARY KEY (`id`)
);

CREATE TABLE IF NOT EXISTS `mobile_params` (
    `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `mobile_id` int(10) unsigned NOT NULL,
    `name` varchar(100) NOT NULL,
    `value` varchar(100) NOT NULL,
    PRIMARY KEY (`id`)
);

INSERT INTO `mobiles` (`id`, `name`, `brand`) VALUES
(1, 'ME525', '摩托罗拉'),
(2, 'E7'   , '诺基亚');

INSERT INTO `mobile_params` (`id`, `mobile_id`, `name`, `value`) VALUES
(1, 1, '待机时间', '200'),
(2, 1, '外观设计', '直板'),
(3, 2, '待机时间', '500'),
(4, 2, '外观设计', '滑盖');</pre>
<p>注：为了演示方便，没有严格遵守关系型数据库的范式设计。</p>
<p>如果想查询待机时间大于100小时，并且外观设计是直板的手机，需按照如下方式查询：</p>
<pre>SELECT * FROM `mobile_params` WHERE name = &#39;待机时间&#39; AND value &gt; 100;
SELECT * FROM `mobile_params` WHERE name = &#39;外观设计&#39; AND value = &#39;直板&#39;;</pre>
<p>注：参数表为了方便，把数值和字符串统一保存成字符串，实际使用时，MySQL允许在字符串类型的字段上进行数值类型的查询，只是需要进行类型转换，多少会影响一点性能。</p>
<p>两条SQL的结果取交集得到想要的MOBILE_IDS，再到mobiles表查询即可：</p>
<pre>SELECT * FROM `mobiles` WHERE mobile_id IN (MOBILE_IDS)</pre>
<h3>如果使用MongoDB的话，应该如何存取数据呢？</h3>
<p>如果使用MongoDB的话，虽然理论上可以采用和MySQL一样的设计方案，但那样的话就显得无趣了，没有发挥出MongoDB作为文档型数据库的优点，实际上使用MongoDB的话，和MySQL相比，形象一点来说，可以合二为一：</p>
<pre>db.getCollection("mobiles").ensureIndex({
    "params.name": 1,
    "params.value": 1
});

db.getCollection("mobiles").insert({
    "_id": 1,
    "name": "ME525",
    "brand": "摩托罗拉",
    "params": [
        {"name": "待机时间", "value": 200},
        {"name": "外观设计", "value": "直板"}
    ]
});

db.getCollection("mobiles").insert({
    "_id": 2,
    "name": "E7",
    "brand": "诺基亚",
    "params": [
        {"name": "待机时间", "value": 500},
        {"name": "外观设计", "value": "滑盖"}
    ]
});</pre>
<p>如果想查询待机时间大于100小时，并且外观设计是直板的手机，需按照如下方式查询：</p>
<pre>db.getCollection("mobiles").find({
    "params": {
        $all: [
            {$elemMatch: {"name": "待机时间", "value": {$gt: 100}}},
            {$elemMatch: {"name": "外观设计", "value": "直板"}}
        ]
    }
});</pre>
<p>注：查询中用到的<a href="http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries-%24all">$all</a>，<a href="http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries-%24elemMatch">$elemMatch</a>等高级用法的详细介绍请参考官方文档中相关说明。</p>
<p>MySQL需要多个表，多次查询才能搞定的问题，MongoDB只需要一个表，一次查询就能搞定，对比完成，相对MySQL而言，MongoDB显得更胜一筹，至少本例如此。</p></blockquote>
<p>来源：<a href="http://huoding.com/2011/06/08/84">huoding.com</a></p>
<table cellspacing="0" cellpadding="2" border="0" width="100%" style="clear:both">
    
    <tr>
        <td><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1894.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2985.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">又一个MongoDB Schema Design的Slide</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2985.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">Python调用MongoDB使用心得</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1329.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2985.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">MongoDB、HandlerSocket和MySQL性能测试及其结果分析</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F287.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2985.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">从MySQL到MongoDB(添加MongoDB循环插入数据方法)</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1155.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2985.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:14px!important">视觉中国的NoSQL之路：从MySQL到MongoDB</font>
                    </a>
                </td>
            </tr>
    
    <tr>
        <td align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/555157233/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/555157233/6253001/1/item.html" border="0" height="0" width="0">