---
layout: post
title:  "Chess@home: 打造最厉害的国际象棋AI"
date:   2011-09-09 08:38:46
author: yuanyi
categories: program
---

## Chess@home: 打造最厉害的国际象棋AI
### by yuanyi
### at 2011-09-09 08:38:46
### original <http://item.feedsky.com/~feedsky/heikezhi/~8608072/564698635/6713895/1/item.html>

<p><a href="http://chessathome.org/">Chess@home</a>项目是在一个叫做<a href="http://nodeknockout.com/">Node Knockout</a>的48小时编程竞赛上由<a href="http://joshfire.com/">Joshfire</a>团队打造的一个分布式的Javascript 国际象棋AI应用，他们受到<a href="http://setiathome.berkeley.edu/">SETI@home</a>项目（一个通过分布式计算寻找外星人的项目）的启发，决定利用Node.js在I/O方面的优势，探索使用Javascript来打造一个高度可扩展的分布式计算平台的可能，这样，用户只要通过浏览器进行访问就可以加入计算。</p>
<p>但是应该计算什么东西呢，他们在吉尼斯记录中发现了一个叫做“<a href="http://www.guinnessworldrecords.com/records-3000/largest-networked-chess-computer/">最大象棋AI网络</a>”的条目，目前的记录只有2070个节点，他们认为这条记录是时候被刷新了，于是 Chess@home项目就诞生了，只要访问它们的网站，你就已经加入了一个分布式的象棋AI网络，并且你可以看到你当前对战的电脑数量，咱么样，赶快去杀一盘吧！</p>
<p><img title="Screen shot 2011-09-09 at 8.32.59 AM" src="http://heikezhi.com/wp-content/uploads/2011/09/Screen-shot-2011-09-09-at-8.32.59-AM.png" alt="" width="575" height="325"></p>
<p><strong>挑战</strong></p>
<p>Chess@home同时也分享了他们在打造这一平台时遇到的挑战，技术控请继续往下看：</p>
<p>1. 延迟，国际象棋的标准比赛规则是90分钟，也就是说只要平均一分钟计算出一步就可以了，但这对没有耐心的Web用户显然是不合适的，由于游戏窗口需要与服务器实时交互，但是<a href="http://en.wikipedia.org/wiki/Hadoop">大部分分布式计算架构</a>都没有考虑这种高吞吐量的情况，因此他们需要寻找新方案，WebSocket显然是个不错的选择</p>
<p>2. 并行，为了减低延迟，他们需要寻找一种方法将每一步移动的计算量分割成几千份，以保证每一个Worker的计算量尽可能的小。</p>
<p>3. Javascript性能，State-of-the-art 象棋AI可以每秒扫描1500万个节点（NPS），但是他们使用的开源Javascript AI <a href="https://github.com/glinscott/Garbochess-JS">GarboChess</a>只能做到100K NPS，相差了150倍，不过作为一个48小时的实验项目，单台机器100K节点已经足够他们实现刷新吉尼斯记录的目标了，所以他们没有在这块浪费太多时间。</p>
<p>另外要说的是，打败卡斯帕洛夫的IBM<a href="http://en.wikipedia.org/wiki/Deep_Blue_versus_Garry_Kasparov">深蓝</a>也只有200m NPS，所以他们认为，只要给他们2070个阶段，超越深蓝也不是没有可能。</p>
<p>4.容错/安全，在第三方客户端运行Javascript意味着你不能完全信任它们，不管是从可达性还是可靠性上来说，所以它们将任务单元以一个FIFO的队列保存在了MongoDB中，如果有一个节点在5秒中内没有返回计算结果，那么就将有第二台机器来接手计算。</p>
<p>整个系统的原型设计如下：</p>
<p><img title="chessathome" src="http://heikezhi.com/wp-content/uploads/2011/09/chessathome.jpg" alt="" width="604" height="342"></p>
<p>其中使用了以下技术：</p>
<p>1. <a href="https://github.com/substack/dnode">dnode</a>，一个node的异步双向远程方法调用库，它可以通过<a href="http://socket.io/">socket.io</a>同时支持网络Socket和Websocket通讯，这就保证了服务器端的进程可以使用同样的接口来和客户端或是和其它服务端进程进行通讯。</p>
<p>2. <a href="http://en.wikipedia.org/wiki/Web_Workers">Web Workers</a>，Javascript是单线程的，这就意味着你不可能同时运行多个脚本而不影响UI的响应，Web Workers提供了一种机制，可以让javascript运行高计算量的任务而不阻塞客户端UI和用户交互，并且Web Worker还有一个Node.js的实现<a href="https://github.com/pgriess/node-webworker">node-webworker</a>，这就保证了它们可以在服务端使用同样的接口。</p>
<p>3. <a href="http://www.mongodb.org/">MongoDB</a>，使用MongoDB只是因为<a href="http://www.mongohq.com/">MongoHQ</a>赞助了活动，所以它们决定试试这个键值数据库</p>
<p>4. <a href="http://nodejs.org/">Node</a>，整个架构的核心，Node让它们可以快速开发，并在服务端和客户端共享代码，同时为高性能的I/O提供可靠保障。</p>
<p>5. Master上的AI，尽管这可能会妨碍可扩展性，但是因为APHID算法需要先生成一个小的计算树然后才能在客户端分派任务，所以还是需要对服务器上的AI进行一些调用，这个问题会在以后的版本解决。</p>
<p>最后，如果你对它们的实现感兴趣，可以在Github查看项目源代码：<a href="http://github.com/joshfire/chessathome">http://github.com/joshfire/chessathome</a></p>
<p><a href="http://sylvainzimmer.com/2011/09/06/chessathome-building-largest-chess-ai/">来自这里</a></p>
<p>想和我们一道传播黑客精神？<a href="http://heikezhi.com/join">快来加入吧！</a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">无觅猜您也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="贪吃蛇AI挑战赛第二季" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F06%2F06%2Fsnake-challenge-season-2%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F09%2F08%2Fchessathome-building-largest-chess-ai%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/06/07/11312098.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">贪吃蛇AI挑战赛第二季</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="IfItWereMyHome: 如果我生在这个国家" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F07%2F30%2Fif-it-were-my-home%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F09%2F08%2Fchessathome-building-largest-chess-ai%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/07/31/20225920.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">IfItWereMyHome: 如果我生在这个国家</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="一个绝对新手的绘画和素描学习之旅" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F10%2F10%2Fjourney-of-an-absolute-rookie-paintings-and-sketches%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F09%2F08%2Fchessathome-building-largest-chess-ai%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/10/11/8941925.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">一个绝对新手的绘画和素描学习之旅</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="220美元让你摆脱纸张" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F05%2F28%2Fpaper-free%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F09%2F08%2Fchessathome-building-largest-chess-ai%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/05/29/9990231.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">220美元让你摆脱纸张</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="4" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/564698635/heikezhi/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/heikezhi/~8608072/564698635/6713895/1/item.html" border="0" height="0" width="0">