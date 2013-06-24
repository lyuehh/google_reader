---
layout: post
title:  "git revert merge commit"
date:   2013-05-07 08:10:14
author: 
categories: program
---

## git revert merge commit
### by 
### at 2013-05-07 08:10:14
### original <http://hlee.iteye.com/blog/1860807>

<pre name="code">
0ce2ca0b35f59af267241cf4d40d16a3e13ba6f3
df1acf5f54426d30f12c6b4558c3dd922297aae3
e19b912404ffd3c153ccac3072dbf22396896d2a

#要revert的commit是merge
git revert -m 2 0ce2ca0b35f59af267241cf4d40d16a3e13ba6f3

</pre>
<br>
<br>
<br>如何在commit里，search代码
<br>
<br><pre name="code">git log -p | grep &lt;pattern&gt;


git grep &lt;regexp&gt; $(git rev-list --all)


#Search all revisions for text matching regular expression regexp:

git grep &lt;regexp&gt; $(git rev-list --all)


git log -SFoo -- path_containing_change --since=2009.1.1 --until=2010.1.1
</pre>
<br>
<br>
<br>重置代码为远程
<br>
<br><pre name="code">git fetch origin
git reset --hard origin/master</pre>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1860807#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>