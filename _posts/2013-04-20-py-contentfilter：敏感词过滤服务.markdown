---
layout: post
title:  "py-contentfilter：敏感词过滤服务"
date:   2013-04-20 23:41:14
author: 
categories: program
---

## py-contentfilter：敏感词过滤服务
### by 
### at 2013-04-20 23:41:14
### original <http://simple-is-better.com/news/990>

<p>
	之前做网站的敏感词过滤服务的时候。没有一个特别合适的项目，适合拿来做后台的服务，要求是以守护进程的方式运行，可以自定义词典组和词典，返回结果以Json形式提供的敏感词列表，包括其所在的字典，足够的轻量级。只找到了一个BBSpam，不过它用的ICE商业应用是需要收费的。于是就用gevent和ahocorasick自己写了一个。</p>
<p>
	在敏感词匹配部分用的是AC自动机的方法，通过上一篇日志的性能比较，选择了Ahocorasick这个工具。在网络编程这部分，直接使用的gevent的WSGIServer，剩下的逻辑就非常简单了，通过WSGIServer获取HTTP Post的发送过来的文本，然后进行关键词匹配，将结果用Json的形式返回。其它的部分就是添加了配置文件，日志和守护进程的部分。代码不多。</p>
<p>
	使用的时候如果没有安装gevent和achorasick的，先安装，然后将py-confilter.tar.gz下载下来，解压，进入bin目录运行sudo python confilterd.py start就可以了，绑定的地址和端口可以在配置文件中设置。客户端发送请求的时候需要以post的方式，在参数中指定g＝词典组名，这个要和配置文件中的词典组名一致，t＝需要过滤的文本。接收请求即可。</p>
<p>
	虽然只是个小模块，但是部署，维护，扩展都很容易。考虑到需要使用关键词过滤的地方也很多，这个项目还是能起到一些作用的。</p>
<p>
	py-contentfilter项目的地址：<a href="http://code.google.com/p/py-contentfilter/">http://code.google.com/p/py-contentfilter/</a></p>

                    <p># 来源：<a href="http://blog.cooder.me/?p=81070&amp;utm_source=rss&amp;utm_medium=rss&amp;utm_campaign=py-contentfilter%25ef%25bc%259a%25e6%2595%258f%25e6%2584%259f%25e8%25af%258d%25e8%25bf%2587%25e6%25bb%25a4%25e6%259c%258d%25e5%258a%25a1" title="py-contentfilter：敏感词过滤服务">弱类型</a></p>
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
                    <p><a href="http://simple-is-better.com/jobs/">更多&gt;&gt;</a></p><img src="http://www1.feedsky.com/t1/731880697/simple-is-better/feedsky/s.gif?r=http://simple-is-better.com/news/990" border="0" height="0" width="0">