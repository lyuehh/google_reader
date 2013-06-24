---
layout: post
title:  "Ubuntu Server 自动安装部署 Rails 环境"
date:   2010-09-05 13:59:36
author: 李华顺
categories: program
---

## Ubuntu Server 自动安装部署 Rails 环境
### by 李华顺
### at 2010-09-05 13:59:36
### original <http://www.linuxidc.com/Linux/2010-09/28398.htm>

<p>你需要经常安装部署 Rails 到新的 Web 服务器吗？</p><p>你是否还在经历每次从安装 Ruby Rubygems MySQL... 再到安装 Web Server 组件... 反复烦躁的安装流程呢？</p><p>现在你可以改用这个脚本实现在 Ubuntu Server 上面部署 Ruby1.8 + Rails + Passenger + Nginx 的环境：</p><p>#!/bin/sh<br>echo &quot;=== YTRIPTHIRD UBUNTU INSTALL ===&quot;<br>cd /tmp# Ruby</p>...