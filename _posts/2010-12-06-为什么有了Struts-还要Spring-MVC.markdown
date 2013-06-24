---
layout: post
title:  "为什么有了Struts 还要Spring MVC"
date:   2010-12-06 16:51:39
author: 
categories: program
---

## 为什么有了Struts 还要Spring MVC
### by 
### at 2010-12-06 16:51:39
### original <http://my.oschina.net/dzt/blog/10722>

很多人学习了ssh ，都晓得struts最早被人用来控制表现层，但是struts已经有了mvc 为什么还有spring mvc呢？     所以我稍微查阅了一些资料，总结了一下他们的区别：   1. 机制。spring mvc 的入口是serclet， 而struts是filter（这里要指出，filter和servlet是不同的。以前认为filter是servlet的一种特殊），这样就导...