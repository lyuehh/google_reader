---
layout: post
title:  "Firebug Console API 与命令行"
date:   2011-05-29 20:53:05
author: 愚人码头
categories: program
---

## Firebug Console API 与命令行
### by 愚人码头
### at 2011-05-29 20:53:05
### original <http://www.css88.com/archives/3678>

前几天有人问我关于firebug console的问题，其实我平时用的最多也就是console.log,相当于alert()。还真没深入了解其他的api，今天在jsmix上看到了，所以转载过来分享一下 Console API 当打开 firebug (也包括 Chrome 等浏览器的自带调试工具)，window 下面会注册一个叫做 console 的对象，它提供多种方法向控制台输出信息，供开发人员调试使用。下面是这些方法的一个简单介绍，适时地运用它们，对于提高开发效率很有帮助。 console.log(object[, object, ...]) 使用频率最高的一条语句：向控制台输出一条消息。支持 C 语言 printf 式的格式化输出。当然，也可以不使用格式化输出来达到同样的目的。下面两行代码执行的结果是相同的： console.log(“The %s jumped over %d tall buildings”, animal, count); console.log(“The”, animal, “jumped over”, count, “tall buildings”); console.debug(object[, object, ...]) 向控制台输出一条信息，它包括一个指向该行代码位置的超链接。 console.info(object[, object, ...]) 向控制台输出一条信息，该信息包含一个表示“信息”的图标，和指向该行代码位置的超链接。 console.warn(object[, object, ...]) 同 info。区别是图标与样式不同。 console.error(object[, object, ...]) 同 info。区别是图标与样式不同。error 实际上和 throw [...]