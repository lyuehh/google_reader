---
layout: post
title:  "brew unable to unlink old 'README.md' (Permission denied)"
date:   2013-02-27 23:50:15
author: 
categories: program
---

## brew unable to unlink old 'README.md' (Permission denied)
### by 
### at 2013-02-27 23:50:15
### original <http://hlee.iteye.com/blog/1816959>

rvm install 2.0.0
<br>met problem about <a href="https://github.com/mxcl/homebrew/issues/2906">brew</a> 
<br>
<br><div>引用</div><div>brew unable to unlink old 'README.md' (Permission denied)</div>
<br>
<br>solve problem by:
<br>
<br><pre name="code">

sudo chmod 777 /usr/local
cd /usr/local
git remote add git://github.com/mxcl/homebrew.git
git reset --hard
git clean -f -d
git pull origin master
brew update
sudo chmod 775 /usr/local
</pre>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1816959#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>