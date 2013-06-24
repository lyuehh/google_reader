---
layout: post
title:  "Android 给Button加个监听"
date:   2010-08-21 08:05:30
author: 3gstdy
categories: program
---

## Android 给Button加个监听
### by 3gstdy
### at 2010-08-21 08:05:30
### original <http://www.linuxidc.com/Linux/2010-08/27986.htm>

在Android开发过程中，Button是常用的控件，用起来也很简单，你可以在界面xml描述文档中定义，也可以在程序中创建后加入到界面中，其效果都是一样的。不过最好是在xml文档中定义，因为一旦界面要改变是话，直接修改一下xml就行了，不用修改Java程序，并且在xml中定义层次分明，一目了然。另一个是如果在程序中定义，还要将其加入到界面中，有的还要设置高度宽度，样式之类的，会使程序变得臃肿，开发和维护都不方便。...