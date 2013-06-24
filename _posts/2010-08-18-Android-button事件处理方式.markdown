---
layout: post
title:  "Android button事件处理方式"
date:   2010-08-18 17:14:53
author: wudaovip
categories: program
---

## Android button事件处理方式
### by wudaovip
### at 2010-08-18 17:14:53
### original <http://www.linuxidc.com/Linux/2010-08/27922.htm>

<p>在Android1.6版本及以后的版本中提供了，对于button的事件处理提供了一种更加简单的机制。通过在布局文件&lt;button&gt;标签中配置android:click属性，同时在代码部分编写想对应的代码即可实现其事件的处理，这样大大简化了代码量，同时便于程序的维护与扩展。</p><p>请看代码：</p><p>布局文件test.xml</p>...