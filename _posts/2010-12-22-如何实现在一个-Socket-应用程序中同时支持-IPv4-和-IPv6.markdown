---
layout: post
title:  "如何实现在一个 Socket 应用程序中同时支持 IPv4 和 IPv6"
date:   2010-12-22 20:59:47
author: 
categories: program
---

## 如何实现在一个 Socket 应用程序中同时支持 IPv4 和 IPv6
### by 
### at 2010-12-22 20:59:47
### original <http://www.ibm.com/developerworks/cn/linux/l-cn-ipv4v6-sockapp/index.html?ca=drs->

当今的网络主流是 IPv4 网络，但随着 IP 地址的日益短缺，IPv6 网络开始渐渐盛行，因此传统的网络编程也需要做一些改进来适应 IPv6 和 IPv4 共存的网络环境。
	本文介绍了一种设计模式来根据用户输入的地址或者域名建立合适的网络连接，并且屏蔽了网络连接细节，提供给用户一个统一的接口进行二次开发。
	在文中还给出了一个基于 OpenSSL https 安全连接的应用来说明该方法的使用细节。