---
layout: post
title:  "为Chrome for Mac增加用Safari打开当前页面的功能"
date:   2013-03-02 14:33:12
author: ytzong
categories: program
---

## 为Chrome for Mac增加用Safari打开当前页面的功能
### by ytzong
### at 2013-03-02 14:33:12
### original <http://www.99css.com/archives/1274>

<p>此功能类似于Firefox的扩展Open with，换作在Chrome中实现而已</p>
<ol>
<li>打开 <strong>Automator.app</strong></li>
<li>选中 <strong>运行 AppleScript</strong></li>
<li>在右侧 “服务”收到选<strong>没有输入</strong>，位于选<strong>Google Chrome.app</strong></li>
<li>从<a href="https://gist.github.com/John07/4401082">https://gist.github.com/John07/4401082</a>拷贝代码到<strong>运行 AppleScript </strong>框中<br>
<a href="http://www.99css.com/wp-content/uploads/2013/03/open-in-safari.png"><img alt="open-in-safari" src="http://www.99css.com/wp-content/uploads/2013/03/open-in-safari.png" width="877" height="654"></a></li>
<li>保存为 <strong>Open in Safari</strong></li>
<li>打开系统偏好设置，进入键盘，切换到键盘快捷键选项卡，选中服务，在右侧找到 Open in Safari，为其设置快捷键</li>
</ol>
<p>这样，在Chrome中浏览网页时，按下刚才设置的快捷键即可实现在Safari中打开</p>
<p>参考资料：<a href="http://www.quora.com/Google-Chrome/Is-there-an-open-in-Safari-plugin-for-Chrome">http://www.quora.com/Google-Chrome/Is-there-an-open-in-Safari-plugin-for-Chrome</a></p>