---
layout: post
title:  "基于注解的 Spring MVC 简单入门"
date:   2010-07-07 16:47:57
author: 
categories: program
---

## 基于注解的 Spring MVC 简单入门
### by 
### at 2010-07-07 16:47:57
### original <http://www.oschina.net/bbs/thread/9608>

以下内容是经过自己整理资料、官方文档所得：

web.xml配置：

&lt;servlet&gt;
	&lt;servlet-name&gt;dispatcher&lt;/servlet-name&gt;
	&lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;
	&lt;init-param&gt;
		&lt;description&gt;加载/WEB-INF/spring-mvc/目...