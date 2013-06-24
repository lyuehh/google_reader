---
layout: post
title:  "git tags 使用 远程创建git tags 删除（小抄版）"
date:   2012-08-01 21:13:10
author: 
categories: program
---

## git tags 使用 远程创建git tags 删除（小抄版）
### by 
### at 2012-08-01 21:13:10
### original <http://hlee.iteye.com/blog/1616461>

update
<br>0.5 获取远程tag
<br><pre name="code">
#my_abc是tag号
git clone http://git.abc.net/git/abc.git
git checkout my_abc OR git checkout -b new_branch my_abc</pre>
<br>
<br>创建
<br>1. 本地创建tags
<br>
<br><pre name="code">git tag -a v1.1 -m "new release"</pre>
<br>
<br>1.5 查看本地tags
<br>
<br><pre name="code">git tag -l</pre>
<br>
<br>2. push到服务器端
<br><pre name="code">
git push --tags</pre>
<br>
<br>2.2.1 创建和删除远程branch
<br><pre name="code">git push origin head:newbranch_name  

#删除远程分支，其它开发者要git branch －d －r 分支  
git push origin :newbranch_name  
</pre>
<br>
<br>2.3 删除本地tag
<br>
<br><pre name="code">git tag -d v1.1</pre>
<br>
<br>3. 删除服务器端远程tag
<br>
<br><pre name="code">git push origin :refs/tags/v1.1</pre>
<br>
<br>4. 查看远程服务器端
<br>
<br><pre name="code">$ git remote show origin</pre>
<br><div>引用</div><div>* remote origin
<br>  URL: git://github.com/rails/rails.git
<br>  Remote branch merged with &#39;git pull&#39; while on branch master
<br>    master
<br>  Tracked remote branches
<br>    1-2-stable 2-0-stable 2-1-stable 2-2-stable 3-0-unstable master</div>
<br>
<br><pre name="code">$ git ls-remote --heads origin</pre>
<br><div>引用</div><div>5b3f7563ae1b4a7160fda7fe34240d40c5777dcd  refs/heads/1-2-stable
<br>71926912a127da29530520d435c83c48778ac2b2  refs/heads/2-0-stable
<br>2b158543247a150e8ec568becf360e7376f8ab84  refs/heads/2-1-stable
<br>b0792a3e7be88e3060af19bab01cd3d26d347e4c  refs/heads/2-2-stable
<br>d6b9f8410c990b3d68d1970f1461a1d385d098d7  refs/heads/3-0-unstable
<br>f04346d8b999476113d5e5a30661e07899e3ff80  refs/heads/master</div>
<br>
<br>
<br>给同行个<a href="http://blog.ashchan.com/archive/2008/06/30/tags-on-git/">链接</a>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1616461#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>