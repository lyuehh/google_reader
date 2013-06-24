---
layout: post
title:  "修改 Android 开机 LOGO"
date:   2010-12-29 11:57:08
author: 
categories: program
---

## 修改 Android 开机 LOGO
### by 
### at 2010-12-29 11:57:08
### original <http://my.oschina.net/zhang3/blog/11637>

1. 制作 initlogo.rle # 使用ImageMagick自带的convert命令，进行raw格式转换
convert -depth 8 logo.png rgb:logo.raw

# android自带的rgb2565工具,对raw文件进行rle565格式转换 
$out/host/linux-x86/bin/rgb2565 -rle initlogo.rle   

#然后将initlogo.rle拷贝到android系统根目录,也就是root目录...