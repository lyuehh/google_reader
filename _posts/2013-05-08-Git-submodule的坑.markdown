---
layout: post
title:  "Git submodule的坑"
date:   2013-05-08 21:44:00
author: 
categories: program
---

## Git submodule的坑
### by 
### at 2013-05-08 21:44:00
### original <http://blog.devtang.com/blog/2013/05/08/git-submodule-issues/>

<h2>前言</h2>

<p>随着近几年的发展，Git已经成为开源界的标准的版本控制工具。开源界的重量级项目，如Linux, Android, Eclipse, Gnome, KDE, Qt, ROR, Debian，无一例外的都是使用git来进行版本控制。如果你还不会Git，那么恕我直言，你已经out了，赶紧抽空充充电吧。本文并不打算做Git入门级介绍，想学习git的同学，推荐国内作者蒋鑫写的<a href="http://book.douban.com/subject/6526452/">《Git权威指南》</a>。</p>

<p>对于一些比较大的工程，为了便于复用，常常需要抽取子项目。例如我开发的猿题库客户端现在包括3门考试，客户端涉及的公共UI、公共底层逻辑、公共的第三方库、以及公共的答题卡扫描算法就被我分别抽取成了子项目。这些子项目都以git submodule的形式，增加到工程中。</p>

<p>在使用了git submodule一段时间后，我发现了一些submodule的问题，在此分享给大家。</p>




<h2>更新submodule的坑</h2>

<p>submodule项目和它的父项目本质上是2个独立的git仓库。只是父项目存储了它依赖的submodule项目的版本号信息而已。如果你的同事更新了submodule，然后更新了父项目中依赖的版本号。你需要在git pull之后，调用 git submodule update来更新submodule信息。</p>

<p>这儿的坑在于，如果你git pull之后，忘记了调用 git submodule update，那么你极有可能再次把旧的submodule依赖信息提交上去。对于那些习惯使用 git commit -a的人来说，这种危险会更大一些。所以建议大家:</p>

<ol>
<li>git pull之后，立即执行git status, 如果发现submodule有修改，立即执行git submodule update</li>
<li>尽量不要使用 git commit -a， git add命令存在的意义就是让你对加入暂存区的文件做二次确认，而 git commit -a相当于跳过了这个确认过程。</li>
</ol>


<p>更复杂一些，如果你的submodule又依赖了submodule，那么很可能你需要在git pull 和 git submodule update之后，再分别到每个submodule中再执行一次git submodule update，这里可以使用 git submodule foreach命令来实现： <code>git submodule foreach git submodule update</code></p>

<h2>修改submodule的坑</h2>

<p>有些时候你需要对submodule做一些修改，很常见的做法就是切到submodule的目录，然后做修改，然后commit和push。</p>

<p>这里的坑在于，默认git submodule update并不会将submodule切到任何branch，所以，默认下submodule的HEAD是处于游离状态的(‘detached HEAD’ state)。所以在修改前，记得一定要用git checkout master将当前的submodule分支切换到master，然后才能做修改和提交。</p>

<p>如果你不慎忘记切换到master分支，又做了提交，可以用cherry-pick命令挽救。具体做法如下：</p>

<ol>
<li>用 <code>git checkout master</code> 将HEAD从游离状态切换到 master 分支, 这时候，git会报Warning说有一个提交没有在branch上，记住这个提交的change-id（假如change-id为 aaaa)</li>
<li>用 <code>git cherry-pick aaaa</code> 来将刚刚的提交作用在master分支上</li>
<li>用 <code>git push</code> 将更新提交到远程版本库中</li>
</ol>


<p>以下是相关命令的操作示范和命令行输出结果：</p>

<span></span><div><table><tr><td><pre><span>1</span>
<span>2</span>
<span>3</span>
<span>4</span>
<span>5</span>
<span>6</span>
<span>7</span>
<span>8</span>
<span>9</span>
<span>10</span>
<span>11</span>
<span>12</span>
<span>13</span>
</pre></td><td><pre><code><span>➜ ui_common git:<span>(</span>df697f9<span>)</span> git checkout master
</span><span>Warning: you are leaving 1 commit behind, not connected to
</span><span>any of your branches:
</span><span>
</span><span>  df697f9 forget to check out master
</span><span>
</span><span>If you want to keep them by creating a new branch, this may be a good <span>time</span>
</span><span>to <span>do </span>so with:
</span><span>
</span><span> git branch new_branch_name df697f911e5a0f09d883f8f360977e470c53d81e
</span><span>
</span><span>Switched to branch <span>&#39;master&#39;</span>
</span><span>➜ ui_common git:<span>(</span>master<span>)</span> git cherry-pick df697f9
</span></code></pre></td></tr></table></div>


<h2>使用第三方工具</h2>

<p>对于submodule的重度使用者，有几个工具可作推荐：</p>

<ol>
<li><a href="http://source.android.com/source/version-control.html">Repo</a>  Google用于管理Android项目的工具。</li>
<li><a href="http://gitslave.sourceforge.net/">Gitslave</a></li>
<li><a href="https://github.com/apenwarr/git-subtree/">Git Subtree</a></li>
</ol>


<p>以上工具，我都没有实际用过，所以无法提供更多信息。</p>

<h2>Tips</h2>

<p>由于常常使用submodule的相关命令，可以在 ~/.gitconfig文件中将其设置别名，方便操作，我设置的所有相关别名如下：</p>

<span></span><div><table><tr><td><pre><span>1</span>
<span>2</span>
<span>3</span>
<span>4</span>
<span>5</span>
<span>6</span>
<span>7</span>
<span>8</span>
<span>9</span>
<span>10</span>
<span>11</span>
<span>12</span>
<span>13</span>
<span>14</span>
</pre></td><td><pre><code><span><span>[</span><span>alias</span><span>]</span>
</span><span>  <span>st</span> <span>=</span> status -s
</span><span>  <span>ci</span> <span>=</span> commit
</span><span>  <span>l</span> <span>=</span> log --oneline --decorate -12 --color
</span><span>  <span>ll</span> <span>=</span> log --oneline --decorate --color
</span><span>  <span>lc</span> <span>=</span> log --graph --color
</span><span>  <span>co</span> <span>=</span> checkout
</span><span>  <span>br</span> <span>=</span> branch
</span><span>  <span>rb</span> <span>=</span> rebase
</span><span>  <span>dci</span> <span>=</span> dcommit
</span><span>  <span>sbi</span> <span>=</span> submodule init
</span><span>  <span>sbu</span> <span>=</span> submodule update
</span><span>  <span>sbp</span> <span>=</span> submodule foreach git pull
</span><span>  <span>sbc</span> <span>=</span> submodule foreach git co master
</span></code></pre></td></tr></table></div>


<h2>参考链接</h2>

<ol>
<li><p><a href="http://codingkilledthecat.wordpress.com/2012/04/28/why-your-company-shouldnt-use-git-submodules/">《why-your-company-shouldnt-use-git-submodules》</a> (需翻墙)</p></li>
<li><p><a href="http://fiji.sc/Git_submodule_tutorial">《Git_submodule_tutorial》</a></p></li>
</ol>