---
layout: post
title:  "Online Spreadsheet技术历险记"
date:   2012-09-22 16:30:17
author: 
categories: program
---

## Online Spreadsheet技术历险记
### by 
### at 2012-09-22 16:30:17
### original <http://coderchina.tumblr.com/post/32038573275>

<p>做为Ruby Developer拿到这个Case，开头让我有点为难，因为我之前并没有此类经验，总不能让我空想实现方法吧。当然凭着开发企业WEB APP的经验，我觉得可以先看看主流的Online Spreadsheet实现的程度，好对技术壁垒有个充分的认识。经过谷歌的窗口，我筛选出google spreadsheet, zoho spreadsheet, thinkfree office。认真的试用了各项指标，对各家有了一定的了解。然后摆在我面前的问题就是，有无开源的解决方案能解决这个问题。经过又一轮的搜索，ZK Spreadsheet类库到了我的眼前，虽然之前有Java背景，但已经不是我专注的技术了，所以我还是很谨慎的看着这个台湾社区推出的开源类库。ZK ajax+java的方式在企业技术里应该是到达一个技术瓶颈点，没有什么再可以突破的空间。但通过它实现的Spreadsheet appdemo却让我对实现一套Spreadsheet有了不少更真实的认识。</p>
<p>首先，如何render出在线表格，zk使用apache poi移植过来的版本实现Excel的读写。用处就是支持用户上传和导出。</p>
<p>第二，如果实现协作编辑，对于spreadsheet，cell已经是最小单元，最好的办法就是使用Session来控制。但仍然有很多挑战，比如同一个人打开两个session编辑同一个cell的内容，确实很变态，但如何有效显示内容呢。</p>
<p>第三，函数，强大的函数可以摧毁一切和MS Excel的竞争对手，但就是因为有在线的优势，如何裁剪函数就是艺术了。</p>
<p>第四，我一开始还想过torquebox + zk spreadsheet做一个开源的Online spreadsheet project, 但torquebox目前对纯rails, sinatra的支持，好过mashup java project的支持，虽然它文档有过介绍，但我没有走通。 </p>
<p>第五，在一个转换XML case上，java就有<span>JAXP 1.4, JAXB 2.2a, and JAX-WS 2.2，并且还不是完全一样只干一件事的东西。所以java程序员想再一个技术点上只专心的用一个库，真的很难。可以说在这个点上，JSR到底是干什么吃饭的我就想不同了。 keep it simple在java上一点用处都没有，就剩下我对语言的膜拜了。</span></p>
<p><span>分享一个和ruby没关系的话题，希望不让大家失望。反正也在墙外，没啥流量。</span></p>