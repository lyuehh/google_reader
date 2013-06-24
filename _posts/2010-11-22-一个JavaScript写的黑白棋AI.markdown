---
layout: post
title:  "一个JavaScript写的黑白棋AI"
date:   2010-11-22 10:31:00
author: 赖勇浩
categories: program
---

## 一个JavaScript写的黑白棋AI
### by 赖勇浩
### at 2010-11-22 10:31:00
### original <http://blog.csdn.net/lanphaday/archive/2010/11/22/6026315.aspx>

首先，这个代码不是我写的，但注释是我加上去的。作者是shaofei cheng，他的网站：http://shaofei.name。第二，目前这个代码只是使用了 alpha-beta 剪枝，棋力还弱，有很大的优化空间。但是代码写得非常清晰，如果有朋友对人机弈棋方面的课题有兴趣又还没有入门，这份代码作为一个例子是很棒的。第三，目前计算机只能搜索 3 层，我觉得加上迭代深化和历史启发算法之后，搜索到 5 层是不成问题的。现代 JavaScript 的性能不错。第四，作者在代码里展示了不少技巧，值得学习和借鉴，哪怕不懂 JavaScript 也很容易看懂代码（我也不懂)。第五，试试这个 AI 的棋力：http://shaofei.name/OthelloAI/othello.html 以下是代码：<img src="http://www1.feedsky.com/t1/461436907/lanphaday/csdn.net/s.gif?r=http://blog.csdn.net/lanphaday/archive/2010/11/22/6026315.aspx" border="0" height="0" width="0"><p><a href="http://www1.feedsky.com/r/l/csdn.net/lanphaday/461436907/art01.html"><img border="0" ismap src="http://www1.feedsky.com/r/i/csdn.net/lanphaday/461436907/art01.gif"></a></p>