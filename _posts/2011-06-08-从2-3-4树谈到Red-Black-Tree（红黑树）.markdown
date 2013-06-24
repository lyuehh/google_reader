---
layout: post
title:  "从2-3-4树谈到Red-Black Tree（红黑树）"
date:   2011-06-08 12:00:00
author: v_JULY_v
categories: program
---

## 从2-3-4树谈到Red-Black Tree（红黑树）
### by v_JULY_v
### at 2011-06-08 12:00:00
### original <http://blog.csdn.net/v_JULY_v/archive/2011/06/08/6531399.aspx>

从2-3-4树谈到红黑树译者：July。编程艺术室出品。出处：http://blog.csdn.net/v_JULY_v。在上一篇文章里已提到，“当关键字数m=2（m=2的意思是，mmin=2，m可以&gt;=2）时的B树是最简单的（有很多人会因此误认为B树就是二叉查找树，但二叉查找树就是二叉查找树，B树就是B树，B树的真正最准确的定义为：一棵含有m（m&gt;=2）个关键字的平衡多路查找树）。每个内结点可能因此而含有2个、3个或4个子女，亦即一棵2-3-4树，然而在实际中，通常采用大得多的t值。”本文，咱们就从2-3-4树开始谈起，然后谈至红黑树。虽然红黑树在本blog已有过非常详尽的阐述。但个人此后对红黑树又有了不少新的认识，雨打风吹去，雨后斜阳，已体味到另一番意境。Ok，本文大部分内容翻译自此文档：Left-Leaning Red-Black Trees, Dagstuhl Workshop on Data Structures, Wadern, Germany, February, 2008.这<img src="http://www1.feedsky.com/t1/528898952/v_JULY_v/csdn.net/s.gif?r=http://blog.csdn.net/v_JULY_v/archive/2011/06/08/6531399.aspx" border="0" height="0" width="0">