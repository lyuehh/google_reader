---
layout: post
title:  "优化 AIX 7 磁盘性能，第 1 部分: 磁盘 I/O 概述和长期监控工具（sar、nmon 和 topas）"
date:   2010-12-22 20:59:40
author: 
categories: program
---

## 优化 AIX 7 磁盘性能，第 1 部分: 磁盘 I/O 概述和长期监控工具（sar、nmon 和 topas）
### by 
### at 2010-12-22 20:59:40
### original <http://www.ibm.com/developerworks/cn/aix/library/au-aix7optimize1/index.html?ca=drs->

根据之前的有关 AIX 5L 的文章，基于对 AIX 7 beta 的研究，了解如何配置和监控 AIX 7。本文讨论对直接 I/O、并发 I/O 和异步 I/O 的支持，以及每种 I/O 实现方法的最佳实践。本系列分为三部分，讨论 AIX 磁盘和 I/O 子系统，重点关注在优化磁盘 I/O 性能时遇到的各种挑战。尽管磁盘调优很可能没有 CPU 或者内存优化那么激动人心，但它是优化服务器性能的关键方面。事实上，部分原因是因为磁盘 I/O 是最薄弱的子系统环节，与任何其他子系统相比，可以通过更多的措施提高磁盘 I/O 性能。