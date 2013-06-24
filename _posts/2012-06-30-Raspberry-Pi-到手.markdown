---
layout: post
title:  "Raspberry Pi 到手"
date:   2012-06-30 23:06:19
author: Jian Lin
categories: program
---

## Raspberry Pi 到手
### by Jian Lin
### at 2012-06-30 23:06:19
### original <http://blog.linjian.org/articles/get-raspberry-pi/>

<p>　　我五月初订购的 <a href="http://www.raspberrypi.org/">Raspberry Pi</a> 到手了，比预计时间早一些。<br>
　　二月底 Raspberry Pi 正式发布时，<a href="http://blog.xiang.li/">idealee</a> 同学专门给我发邮件说过它。我当时没太在意，因为大学时做竞赛项目也用过一些小主板、小 PC，包括 <a href="http://www.via.com.tw/en/initiatives/spearhead/pico-itx/index.jsp">VIA Pico-ITX</a> 和 <a href="http://www.compactpc.com.tw/ebox-2300.htm">eBox</a>，故而对 Raspberry Pi 的“小”没什么感觉。但五一时偶然在所里见到一块 Raspberry Pi，试用了一下，又听了同学神侃，我发现 Raspberry Pi 还是有一些优势的，包括价格、开放性、可折腾性等。加之它有慈善组织背景，于是决定订购一块，研究加收藏。<br>
<a href="https://picasaweb.google.com/lh/photo/wJJxZ_dq3nR5Be2dQLWPXtMTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="http://linjian.org/imgcache/getimg.php?u=%3DcGcq5SaQ1SeyJXZiB3chJ1LwQjNz9SUqN1XzQlNC9kaI9ycSNUQBFUQBFUQB9SSM92Z6tWWQhTLU9CMvhVRlREUHZVQ10yLt92YuQnblRnbvNmclNXdlx2Zv92ZuYDas9yL6MHc0RHa" height="480" width="640" title="" alt="Raspberry Pi 到手"></a><br>
　　到手之后简单测试，感觉 Raspberry Pi 的性能还是符合我的预期的。官方提供的映像使用起来基本没有障碍，我手头的外设也都工作良好。专门试了一下 GPIO，只需要读写 /sys 文件系统就可以实现控制交互。只有一点不爽的：Raspberry Pi 的视频输出接口是 HDMI 和 Composite RCA（“AV”的“V”），而我周围的显示器都是 VGA 的，不光没有 HDMI，连 DVI 都没有，只能使用 HDMI-VGA 转接头。一个转接头的价格快赶上 Raspberry Pi 本身了，好在实验室某果粉手里有一个，借来先用。通过廉价的山寨 USB 视频采集卡接入 Composite RCA 信号也是一个办法，但这又不是玩 FC，PAL/NTSC 那点分辨率看看视频还行，操作终端和桌面可能会是煎熬。（这句话反过来说似乎也对？）<br>
　　顺便聊聊相关的产品。我前不久关注过 <a href="http://www.arduino.cc/">Arduino</a>，因为连续几期《无线电》杂志几乎被它占领了。最初看到 Raspberry Pi 上那两排针脚时，我曾期望它能够完全涵盖 Arduino 的能力。不过查看相关资料后发现，<a href="http://www.raspberrypi.org/archives/1171">Raspberry Pi 并没有侵占 Arduino 市场的意思</a>。它只有 8 个非 0 即 1 的 GPIO，其余针脚是 UART、I2C、SPI 等接口，而非板载 ADC、DAC。这也好，EE 和 CS 本来就是有分工、有合作（如 <a href="http://omer.me/2012/05/introducing-ponte/">Ponte</a>）。<br>
　　最近另一款吸引眼球的小设备是 <a href="http://www.engadget.com/2012/06/07/mk802-android-4-0-mini-pc-hands-on-impressions/">MK802 Android 4.0 Mini PC</a>，它基于 <a href="http://rhombus-tech.net/allwinner_a10/">Allwinner A10</a> 解决方案，主要面向家庭娱乐市场。不考虑开放性等理想因素，对于非折腾型用户，MK802 的性价比可能比 Raspberry Pi 更高一些。当然，Android 系的东西要想折腾都是有可能的，CSK 同学几天前就给出了<a href="http://www.csksoft.net/blog/post/288.html">一个例子</a>。<a href="http://twitter.com/lonepig">@lonepig</a> 同学根据 Android 市场现状估计，MK802 这类产品还有降价空间。<br>
　　后续仍然关注 Raspberry Pi 的 26 根针脚，以及 DSI、CSI 接口的潜力。</p>
<hr>原文链接：<a href="http://blog.linjian.org/articles/get-raspberry-pi/">http://blog.linjian.org/articles/get-raspberry-pi/</a>，作者：<a href="http://linjian.org/">林健</a>。
<br>本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/cn/">知识共享署名-非商业性使用-相同方式共享 3.0 中国大陆许可协议</a>进行许可。
<br>如何您认为本作品有价值，可以向作者<a href="http://linjian.org/donate/">捐款</a>以表支持。
<br>(Digital fingerprint:  993d4981d6d552510db9a08493b2dbec)