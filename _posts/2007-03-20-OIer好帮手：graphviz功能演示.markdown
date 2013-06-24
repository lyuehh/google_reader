---
layout: post
title:  "OIer好帮手：graphviz功能演示"
date:   2007-03-20 03:35:23
author: matrix67@tom.com(matrix67)
categories: program
---

## OIer好帮手：graphviz功能演示
### by matrix67@tom.com(matrix67)
### at 2007-03-20 03:35:23
### original <http://www.matrix67.com/blog/default.asp?id=200>

<div align="center"><img src="http://www.matrix67.com/blogimage/200703201.gif" border="0" alt=""></div><br>    原来我经常在想，要是有软件能帮我把图论题的数据画出来就好了。后来我想到一个制作这种软件的方法，就是把所有的点的位置设定为圆周上的等分点，这样可以最大限度的保证图象不致于太乱。我没想到居然有程序可以智能地决定哪个点、哪条边放在哪里更好看。<br>    我在OIBH的<a href="http://www.oibh.org/bbs/viewthread.php?tid=14035">这个帖子</a>里找到了这个好东西，它可以帮助OIer将大规模的图论题数据转化为图便于观察。今天我又用到了几次，突然想到把它介绍在我的Blog上。<br>    graphviz的主页设在<a href="http://www.graphviz.org">http://www.graphviz.org</a>，你可以在<a href="http://www.graphviz.org/Download_windows.php">这里</a>下载到最新的Windows版本，目前最新版本的安装程序为graphviz-2.12.exe。安装后你可以在dos下（任何目录中）调用它的命令行模式。<br>    这里，我们使用<a href="http://www.graphviz.org/doc/info/lang.html">dot语言</a>。官方网站上有关于dot语言的<a href="http://www.graphviz.org/Documentation/dotguide.pdf">详细的用户手册</a>，这里我只把常用的一些功能做一下演示。你可以在这篇日志的三个截图中掌握足够的知识来应用graphviz。<br>    先说明一下最顶上的Hello World程序。dot是程序名，参数-Tgif表示以gif格式输出，参数-O表示输出文件的方式设为默认（在当前目录下输出名为noname的文件，其后缀名与参数-T???所设定的类型相同）。下面一行输入的是graphviz所用的dot语言，digraph G表示有向图，花括号里描述图的内容。这样就生成了一个最简单的图。<br><br>    下面一个例子说明了如何输出一个边上有权值的无向图。这是OIer经常要用的东西。size=4,4指定了图的大小，单位为英寸。如果没有这一句的话，默认的图要大得多。你可以另外写一个程序把你的数据按图中的格式转化为dot代码。虽然graphviz可以从外部文件中读入这段代码，但我觉得粘贴进dos窗口更方便一些。<br><br>        <img src="http://www.matrix67.com/blogimage/200703202.gif" border="0" alt=""><br><br>    下面这个例子包含更多的参数，展示了graphviz更多的功能。输出为ps文件更好看一些，因为输出ps文件可以反锯齿（应该是矢量的）。<br><div align="center"><img src="http://www.matrix67.com/blogimage/200703203.gif" border="0" alt=""></div>