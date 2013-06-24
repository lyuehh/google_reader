---
layout: post
title:  "git rebase 和 merge的区别"
date:   2012-09-04 21:34:54
author: 
categories: program
---

## git rebase 和 merge的区别
### by 
### at 2012-09-04 21:34:54
### original <http://hlee.iteye.com/blog/1671477>

<a href="http://stackoverflow.com/questions/457927/git-workflow-and-rebase-vs-merge-questions">http://stackoverflow.com/questions/457927/git-workflow-and-rebase-vs-merge-questions</a>
<br>
<br><div>引用</div><div>
<br>git fetch用来将自己本地的repo 更新到最新的。
<br>
<br>但是fetch后，并不会显示到本地。  有两种办法， rebase 和 merge. 
<br>
<br>if you want masterA =&gt; masterB =&gt; masterC =&gt; yourworkhere =&gt; (masterD and E merged) =&gt; HEAD then use merge
<br>
<br>if you want masterA =&gt; B =&gt; C =&gt; D =&gt; E =&gt; yourworkhere then use rebase
<br>
<br>好了，区别出来了，就是commit的顺序不一样。 
<br>一般建议用rebase， 这样可以保持upstream上的顺序。 
<br></div>
<br>
<br><div>引用</div><div>
<br>If you haven’t worked with a version control tool that allows for easy branching, you’re probably wondering what the difference is between rebase and merge and why you’d choose one over the other.
<br>
<br>From the standpoint of the end result, a merge and a rebase in Git appear to do the same thing:
<br>
<br>A + B + A’ (merge) = A + A’ + B (rebase)
<br>
<br>Wouldn’t it be simpler to just choose one operation and stick with it?
<br>
<br>The answer of course is no.  Otherwise you wouldn’t have the option.  (If you feel completely contrary, best of luck.  And choose merge.)
<br>
<br>It really only becomes apparent once your development effort becomes hierarchical, either in terms of application lifecycle (such as dev, test and release versions) or in a team structure.  You’re now dealing with multiple branches, each with non-trivial changes that can and will occur independently.
<br>
<br>Rebases are how changes should pass from the top of hierarchy downwards and merges are how they flow back upwards.
<br>
<br>Let’s take a look in more detail at what is actually taking place in each operation for a parent branch A and a child branch B:
<br>
<br>Merge
<br>
<br>A + B + A’ + dA’B
<br>
<br>where A’ are the changes being merged in and dA’B the resolution of merge conflicts from A’ and the set of commits B on the current branch
<br>
<br>The changes introduced by A’ and dA’B are grouped together into one merge commit A” = A’ + dA’B that is added on top of the existing set of commits (A + B) and becomes the head of the current branch:
<br>
<br>A + B + A”
<br>
<br>Rebase
<br>
<br>A + A’ + B + dA’B
<br>
<br>The rebase resets the starting point the branch and reapplies the set of commits B.  Merge conflicts dA’B are combined with these commits so they become grouped together B’ = B + dA’B:
<br>
<br>A + A’ + B’
<br>
<br>If you stare at it, you’ll realize that the rebase guarantees that the changes being brought it in from the other branch come in exactly as-is: A + A’.  Any conflicts are resolved and contained with the associated commits B’ on the current branch.
<br>
<br>Using merge to pull in changes from the higher-level branch mixes those changes with the resolved merge conflicts.  This means that the current branch won’t necessarily have the same state as the one it was based on (from the hierarchical structure).  And since the resolved conflicts are grouped all together in one merge commit, you’ve also made it harder to cleanly cherry-pick individual changes.
<br>
<br>Having development lifecycle tracks and each local developer branch start from known consistent states is critical to reducing and resolving code issues.  Where a change occurs (or suddenly becomes missing) and who is responsible become easier to determine.  By using the rebase to pull in changes, you have that.
<br>
<br>When you’re submitting changes back up the chain, you only want to add your changes on top of the existing commits of the higher-level branch.  Merge is clearly the operation for that.
<br>
<br>Even if you’re a single developer with only a few branches, it’s worth it to get in the habit of using rebase and merge properly.  The basic work pattern will look like:
<br>
<br>Create new branch B from existing branch A
<br>Add/commit changes on branch B
<br>Rebase updates from branch A
<br>Merge changes from branch B onto branch A
<br></div>
<br>
<br><a href="http://gitbook.liuhui998.com/4_2.html">http://gitbook.liuhui998.com/4_2.html</a>
<br>
<br>Pro Git中文版：http://progit.org/book/zh/
<br>Git Merge的一个很好的例子讲解：http://blog.microsuncn.com/?p=2000
<br>Git rebase的一个例子讲解：http://blog.microsuncn.com/?p=1989
<br>关于Git rebase和merge的比较的另一个例子：http://gitguru.com/2009/02/03/rebase-v-merge-in-git/
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1671477#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>