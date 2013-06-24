---
layout: post
title:  "各自用一句话来概括MVC、MVP、MVVM的差异特点"
date:   2012-06-13 16:33:00
author: 司徒正美
categories: program
---

## 各自用一句话来概括MVC、MVP、MVVM的差异特点
### by 司徒正美
### at 2012-06-13 16:33:00
### original <http://www.cnblogs.com/rubylouvre/archive/2012/06/13/2548095.html>

<p>MVC：</p><p>用户的请求首先会到达Controller，由Controller从Model获取数据，选择合适的View，把处理结果呈现到View上； </p><p> </p><p>MVP： </p><p>用户的请求首先会到达View，View传递请求到特定的Presenter，Presenter从Model获取数据后，再把处理结果通过接口传递到View。 </p><p> </p><p>MVVM： </p><p>立足于原有MVP框架并且把WPF的新特性(数据绑定DataBind、依赖属性Dependency Property、路由事件Routed Events、命令Command等...)揉合进去。</p><p><img src="http://pic002.cnblogs.com/images/2010/152115/2010102816493731.png" alt=""></p><img src="http://www.cnblogs.com/rubylouvre/aggbug/2548095.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/rubylouvre/archive/2012/06/13/2548095.html">本文链接</a></p>