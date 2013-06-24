---
layout: post
title:  "Seafile：Dropbox / box.net 的开源替代"
date:   2013-04-20 23:22:14
author: 
categories: program
---

## Seafile：Dropbox / box.net 的开源替代
### by 
### at 2013-04-20 23:22:14
### original <http://simple-is-better.com/news/989>

<p>
	介绍下我现在在做的项目 Seafile， 及 Python 在其中的应用。</p>
<p>
	Seafile 是一个文件同步和协作平台。它有 Dropbox 类似的文件同步功能， 但是针对团队文件同步和协作做了优化。你可以用它在你自己的服务器上搭建文件同步和协作服务。 Seafile 是目前最好的 Dropbox, Box.com 等类似产品的开源替代。</p>
<p>
	Seafile 的创新特色包括:</p>
<ol>
	<li>
		<p>
			群组功能，用户可以创建和加入群组, 在群组中共享文件。这对团队协作很有用。</p>
	</li>
	<li>
		<p>
			文件组织成资料库。每个资料库可以单独同步和共享。</p>
	</li>
	<li>
		<p>
			在线文件协作，包括文件在线预览、评论、推荐等等。 Markdown, text, 源代码等文本格式可以直接在线编辑。</p>
	</li>
</ol>
<p>
	Seafile 采用了类似 GIT 的数据模型和分布式同步技术，但是针对自动同步和大文件管理做了优化。</p>
<p>
	Seafile 底层是用 C 来写的。客户端的本地界面和服务器界面用 Python 来实现。 服务器端采用了 django 框架，并且用 django restful framework 2.0 来实现 Rest API 接口， 以便以后移动端使用。</p>
<p>
	客户端界面包括一个任务栏图标，点击后会在浏览器中弹出一个基于 web 的管理界面。 任务栏图标不同的平台需要分别实现。 web 的管理界面各个平台就可以共用同一个代码， 目前用 web.py 来实现。虽然 python 是跨平台的，但是实际上在 Mac 和 Windows 下让 python 部分能正常的工作花了我们不少调试的时间 (如果大家以后有类似的需求可以咨询我们)。</p>
<p>
	对 Seafile 内部技术感兴趣的话，可以看看下面的链接里的资料：</p>
<ul>
	<li>
		<a href="https://github.com/haiwen/seafile/wiki/Seafile-data-model">Seafile 数据模型</a></li>
	<li>
		<a href="https://github.com/haiwen/seafile/wiki/Seafile-synchronization-algorithm">Seafile 同步算法</a></li>
	<li>
		<a href="https://github.com/haiwen/ccnet/blob/master/HACKING">底层通信协议简单介绍</a></li>
</ul>
<p>
	产品主页 <a href="http://seafile.com/">http://seafile.com</a></p>
<p>
	项目主页 <a href="https://github.com/haiwen/seafile">https://github.com/haiwen/seafile</a></p>
<p>
	Seafile 1.4 版服务器包已经有数千的下载。同时我们只收到少数的同步问题报告。这些问题都已经被定位和解决。所以同步功能已经相当的稳定。  </p>
<p>
	Seafile 1.5 版包括了企业 LDAP 支持，命令行客户端等新功能。</p>

                    <p># 来源：<a href="https://groups.google.com/group/python-cn/" title="Seafile：Dropbox / box.net 的开源替代">Python-cn 邮件列表</a></p>
                    <hr>
                    <p>
                        在微博上关注：
                        <a href="http://t.sina.com.cn/pythoncn">新浪</a>,
                        <a href="http://t.qq.com/simple-is-better">腾讯</a>
                         
                        <a href="http://simple-is-better.com/tougao">投稿</a>
                    </p>
                    <p><strong>最新招聘</strong></p>
                    <ul>
                        <li><a href="http://simple-is-better.com/jobs/detail-461">[上海] OpenStack研发工程师/架构师</a> - 携程</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-460">[上海] 后端python开发工程师</a> - 点动科技</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-55">[杭州] python 程序开发( Django，2年+ )</a> - 杭州三舍贸易有限公司</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-448">[北京] 云计算项目实施经理</a> - 易云捷讯科技（北京）有限公司</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-429">[北京] 急！产品经理</a> - 易云捷讯科技（北京）有限公司</li>
                    </ul>
                    <p><a href="http://simple-is-better.com/jobs/">更多&gt;&gt;</a></p><img src="http://www1.feedsky.com/t1/731880698/simple-is-better/feedsky/s.gif?r=http://simple-is-better.com/news/989" border="0" height="0" width="0">