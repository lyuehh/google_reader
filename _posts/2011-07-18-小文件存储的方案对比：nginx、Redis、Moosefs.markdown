---
layout: post
title:  "小文件存储的方案对比：nginx、Redis、Moosefs"
date:   2011-07-18 00:20:56
author: nosqlfan
categories: program
---

## 小文件存储的方案对比：nginx、Redis、Moosefs
### by nosqlfan
### at 2011-07-18 00:20:56
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/536689372/6253001/1/item.html>

<p>大量小文件存储一直是典型的应用场景之一，下面是一篇转载文章，对大量小文件存储分别采用<span><a href="http://blog.nosqlfan.com/tags/nginx" title="查看 Nginx 的全部文章">Nginx</a></span>+<span><a href="http://blog.nosqlfan.com/tags/nfs" title="查看 NFS 的全部文章">NFS</a></span>、<span><a href="http://blog.nosqlfan.com/tags/redis" title="查看 Redis 的全部文章">Redis</a></span>和<span><a href="http://blog.nosqlfan.com/tags/%e5%88%86%e5%b8%83%e5%bc%8f%e6%96%87%e4%bb%b6%e7%b3%bb%e7%bb%9f" title="查看 分布式文件系统 的全部文章">分布式文件系统</a></span><span><a href="http://blog.nosqlfan.com/tags/moosefs" title="查看 Moosefs 的全部文章">Moosefs</a></span>三种方案的优缺点进行了分析。</p>
<blockquote><p>现在有3KW的数据，单条数据都很小的，如果按key-value来看的话，key就是32位的MD5字符串，value按平均算大概是100字节左右。</p>
<p>现在需要将这些数据做缓存以在高并非的时候依然可以快速响应。</p>
<p>因为这些数据基本没有冷热数据之分，所以需要将全部数据都放到缓存中。</p>
<h3>1、直接生成静态文件，利用nginx对静态文件的高效做静态缓存。</h3>
<ul>
<li>当时服务器硬件资源有限，所以就采用这种方式，一直源用至今。</li>
<li>服务器间通过NFS来共享</li>
<li>太多小文件，不方便管理</li>
<li>NFS不方便运维与扩展</li>
<li>文件内容很小(100字节左右)，3KW大概就是2.5G大小左右
<ul>
<li>不过文件存储的时候和硬盘分区的族大小有关，在这里磁盘分区的族大小为8K，所以尽管文件内容只有100字节，但是实际存储到磁盘上的时候单个文件其实是8K</li>
<li>所以3KW的文件世界占的磁盘空间大约为：200G左右（<strong>严重浪费空间啊</strong>）</li>
</ul>
</li>
</ul>
<h3>2、Redis(V2.2.11)【KV数据库】</h3>
<ul>
<li>听同事说开启VM会使性能急剧下降，所以基本无视VM，数据全放内存。</li>
<li>key为32位MD5字符串</li>
<li>测试数据：10W数据大概占内存20M</li>
<li>测试数据：500W数据大概占内存1G，持久化的rdb数据文件大概350M</li>
<li>推算3KW数据：内存6G，持久化的rdb数据文件大概为2G(压缩了？)</li>
<li>因为Redis在持久化的时候内存会加倍，和考虑到数据的增长，所以需要1台20G内存的机器基本就没问题了（容灾啥的另算）。</li>
<li>Redis非常快，如果硬件条件没问题，基本选这个最好了。</li>
</ul>
<h3>3、Moosefs(MFS)【分布式文件存储系统】</h3>
<ul>
<li>mfs支持NFS的方式mount到本地直接操作（如使用mfs，则现在的架构基本不用改）</li>
<li>最基本的需要一台主控服务器(Master Server)、一台数据服务器(Chunk Server)</li>
<li>文件和目录的索引需要全部加载到主控服务器的内存中，所以对主控服务器的内存有一定的要求</li>
<li>写入30W文件，到20W的时候写入就开始下降得厉害了（我的5400转的笔记本硬盘）</li>
<li>30W文件，4W目录，主控服务器占用大概120M内存</li>
<li>文件存储方式貌似和普遍的文件一样单个存储的（不确定），30W文件大概占了2.4G的磁盘空间（同样是8K一个文件）。</li>
<li>小文件一样很多，不方便维护与迁移(不知是否我设置不对？)。</li>
<li>内存、硬盘都占用得比较多，而且性能相对来说不是很出众。</li>
<li>所以这个基本不考虑了。</li>
</ul>
<p>版权信息：</p>
<p>作者：<a href="http://www.twitter.com/QLeelulu">QLeelulu</a><br>
出处：<a href="http://qleelulu.cnblogs.com/">http://QLeelulu.cnblogs.com/</a><br>
本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文连接，否则保留追究法律责任的权利</p></blockquote>
<p style="font-weight:bold"><span style="padding-top:5px;float:left">技术传播，需要你我共同努力！</span><a href="http://twitter.com/share?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html&amp;text=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan" title="Twitter" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKEJk/custom.png"></a><a href="http://v.t.sina.com.cn/share/share.php?title=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html" title="新浪微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVKrzm/custom.png"></a><a href="http://v.t.qq.com/share/share.php?title=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html" title="腾讯微博" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJX6o/custom.png"></a><a href="http://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?title=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan%20&amp;url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html" title="QQ空间" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJSKI/custom.png"></a><a href="http://www.douban.com/recommend/?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html&amp;title=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan" title="豆瓣9点" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJrri/custom.png"></a><a href="http://xianguo.com/service/submitdigg?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html&amp;title=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan%20&amp;content=utf-8" title="鲜果" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVJ4v4/custom.png"></a><a href="http://share.renren.com/share/buttonshare.do?link=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html" title="人人网" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVI86k/custom.png"></a><a href="http://www.facebook.com/sharer.php?u=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html&amp;title=%E5%B0%8F%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E7%9A%84%E6%96%B9%E6%A1%88%E5%AF%B9%E6%AF%94%EF%BC%9Anginx%E3%80%81Redis%E3%80%81Moosefs%20@nosqlfan" title="FaceBook" style="text-decoration:none;margin:2px"><img src="http://pic.yupoo.com/iammutex/B8hVHr67/custom.png"></a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">您可能还喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Redis 测试引擎将升级提速" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2383.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/07/12/17143319.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis 测试引擎将升级提速</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Bump的Redis应用经验" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2460.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/07/17/17864990.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Bump的Redis应用经验</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="REDIS TO GO：一个Redis存储服务" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1940.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/14/12130942.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">REDIS TO GO：一个Redis存储服务</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Redis命令参考中文版翻译" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2404.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis命令参考中文版翻译</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Redis内存存储结构分析" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F1694.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2469.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/06/09/11638941.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Redis内存存储结构分析</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="5" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/536689372/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/536689372/6253001/1/item.html" border="0" height="0" width="0">