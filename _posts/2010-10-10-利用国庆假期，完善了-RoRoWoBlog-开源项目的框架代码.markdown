---
layout: post
title:  "利用国庆假期，完善了 RoRoWoBlog 开源项目的框架代码"
date:   2010-10-10 10:06:00
author: 李锡远
categories: program
---

## 利用国庆假期，完善了 RoRoWoBlog 开源项目的框架代码
### by 李锡远
### at 2010-10-10 10:06:00
### original <http://www.cnblogs.com/taven/archive/2010/10/10/1847057.html>

<p><a href="http://www.cnblogs.com/taven/"><img src="http://pic.cnblogs.com/face/u59940.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/taven/">李锡远</a> 发表于 2010-10-10 10:06 <a href="http://www.cnblogs.com/taven/archive/2010/10/10/1847057.html">原文链接</a> 阅读: 1395 评论: 20</p><div>本开源项目当前使用框架如下：</div>
<div>前台表现：Asp.net MVC 2</div>
<div>数据持久层：ADO.Net Entity Framework 4.0</div>
<div>依赖注入容器：Unity 2.0</div>
<div>开发工具：VS2010</div>
<div> </div>
<div>开源项目地址：<a href="http://rorowo.codeplex.com/">http://rorowo.codeplex.com/</a></div>
<div> </div>
<div>我这次国庆的时间  主要改了以下内容：<br>1、改为POCO，使EF的实体纯净<br>2、增加 IoCHelper类，把IoC的代码改为可以同时支持多种IoC<br>3、修改基础结构层，使其可以支持多种ORM框架</div>
<div>4、修正继承自 DefaultControllerFactory 的自定义控制器，使MVC控制器可以支持依赖注入，充分发挥IoC自动装载的特性</div>
<div>5、正式启用规约接口 ISpecification 作为查询条件</div>
<div> </div>
<div>项目结构图：</div>
<div><img border="0" alt="" src="http://images.cnblogs.com/cnblogs_com/taven/201010/2010-10-10_101321.png" width="272" height="369"></div>
<div> </div>
<div>要创建数据库，请在 RoRoWo.Blog.Infrastructure 打开 RoRoWoDB.edmx 模型视图，在视图显示页面中，点击鼠标右键，选择 “根据模型生成数据库”可以得到创建数据库的SQL，然后修改相关Config中的数据库连接字符串就可以了。</div>
<div> </div>
<div>欢迎广大技术好友下载源码，一起探讨、拍砖哈！</div>
<div>我更多的希望能通过技术好友，学到更多...</div>
<div>如有在DDD（领域驱动设计）方面感兴趣的朋友，希望加个QQ，我的号码：17020415</div>
<div> </div>
<div>在这里我还要感谢两位朋友，一位是 邓智伟(xianrendzw) ，还有一位是 薛飞(xuefly)，感谢他们在这个项目中所做出的贡献。</div>
<div> </div>
<div>  
<div>//-------------------------------------------------------------------</div>
<div>【注意】</div>
<div>我发现国内还是有很多朋友对免费的代码管理平台不是很熟悉。</div>
<div> </div>
<div>什么是CodePlex？</div>
<div>CodePlex是微软提供的一个源码管理平台，它是集开源社区、版本控制为一体的平台（目前在此平台上的项目必须开源）。</div>
<div> </div>
<div>CodePlex可以做什么？</div>
<div>首先，你可以使用TFC或SVN的客户端将你的开发项目签入到CodePlex，而无需自己搭建一个专门的源码管理服务器了；同时你也可以多人一起开发，相互之间可以看到其他成员对代码的修改情况（用过版本控制工具的人都清楚了）。</div>
<div> </div>
<div>如何下载最新版本的源码？</div>
<div>如果用户没有Release最新版本，我们如何下载最新版本的源码呢？可以这样：</div>
<div>进入一个开源项目，然后点击“Source Code” 菜单链接，这时我们可以看到最后的一次代码签入日期，和 Change Set 编号，点击最新的编号，将进入一个相关文件签入的列表页面，在上面有“Download”和“Browse” 两个链接，点击“Download”，即可下载当前版本的源码了（你也可以进入到某个日期的一次代码签入，下载过去某天版本的代码）。</div>
<div>//-------------------------------------------------------------------</div></div>
<div> </div>
<div> </div>
<div> </div><img src="http://www.cnblogs.com/taven/aggbug/1847057.html?type=1" width="1" height="1" alt=""><p>评论: 20　<a href="http://www.cnblogs.com/taven/archive/2010/10/10/1847057.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/taven/archive/2010/10/10/1847057.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76814/">乔布斯亲自回信 称魅族偷窃创意和知识产权</a><span style="color:gray">(2010-10-10 20:57)</span><br>· <a href="http://news.cnblogs.com/n/76813/">中国出台电子书产业发展意见</a><span style="color:gray">(2010-10-10 20:56)</span><br>· <a href="http://news.cnblogs.com/n/76812/">学生和年轻网民成为中国团购主力军</a><span style="color:gray">(2010-10-10 20:54)</span><br>· <a href="http://news.cnblogs.com/n/76811/">方舟子遇袭案判决结果已出</a><span style="color:gray">(2010-10-10 20:52)</span><br>· <a href="http://news.cnblogs.com/n/76810/">商界沉浮 谁是真正的王者？</a><span style="color:gray">(2010-10-10 20:48)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/cmt/archive/2010/10/10/1847077.html">博客园电子期刊2010年9月刊发布</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>