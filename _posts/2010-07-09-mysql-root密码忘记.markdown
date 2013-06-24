---
layout: post
title:  "mysql root密码忘记"
date:   2010-07-09 12:40:26
author: 
categories: program
---

## mysql root密码忘记
### by 
### at 2010-07-09 12:40:26
### original <http://my.oschina.net/huxuanhui/blog/5987>

停止mysql服务 service mysqld stop
找到 mysqld_safe所在目录，这个可以通过 rpm -ql mysql-server 来找到，
然后运行 mysqld_safe --skip-grant-tables （注意这里是两条中划线，网上很多都是一条，一条不行）
然后mysql -u root -p
在 Enter password: 这里直接回车
use mysql;
update user set password=pass...