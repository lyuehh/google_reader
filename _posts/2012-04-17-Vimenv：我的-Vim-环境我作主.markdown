---
layout: post
title:  "Vimenv：我的 Vim 环境我作主"
date:   2012-04-17 23:39:10
author: Toy
categories: program
---

## Vimenv：我的 Vim 环境我作主
### by Toy
### at 2012-04-17 23:39:10
### original <http://linuxtoy.org/archives/vimenv.html>

<p><iframe src="http://feedads.g.doubleclick.net/~ah/f/r45t08ks0fj6sr7aa7q8jurtt8/300/250?ca=1&amp;fh=280#http%3A%2F%2Flinuxtoy.org%2Farchives%2Fvimenv.html" width="100%" height="280" frameborder="0" scrolling="no" marginwidth="0" marginheight="0"></iframe></p><p>相信每位使用 <a href="http://www.vim.org">Vim</a> 的朋友都有一套自己的环境配置。我自然也不例外。
最近，我对自己的 Vim 环境进行了审视，深感具有不少问题，仍有改进之必要。
<span></span></p>

<p><strong>现有问题</strong></p>

<p>细究起来，我的 Vim 环境配置目前主要存在下面两个问题：</p>

<ol>
<li><p>管理不便。这里所说的管理主要针对 Vim Scripts 而言，包括插件、配色方
案、工具等等。它们的安装、删除、升级若是纯粹通过手工来完成的话，的确
显得麻烦，且耗费时间。</p></li>
<li><p>移植性差。我总是会在不同的电脑上使用 Vim。当将 Vim 配置从一台电脑
迁移到另一台电脑时，既不够顺畅，又需要做额外的工作。</p></li>
</ol>

<p><strong>Pathogen + Git submodules 来拯救</strong></p>

<p>要解决上述问题，其实只要 <a href="https://github.com/tpope/vim-pathogen">Pathogen</a> 和 <a href="http://help.github.com/submodules/">Git submodules</a>
就可以完美达成。其中，Pathogen 这个 Vim 插件能对 Vim 的 runtimepath
做到轻松管理。这样，Vim 的各式插件、配色方案、以及工具可以分别归属于自己
单独的位置。而 Git submodules 则使添加、升级 Vim 插件更加方便。这使我的 Vim
环境配置具有非常好的灵活性。</p>

<p><strong>Vimenv：更加趁手</strong></p>

<p>为了使这套方案用起来更加趁手，我用 Ruby 编写了 <a href="https://github.com/xuxiaodong/vimenv">Vimenv</a>
这个简单的脚本。其用途包括：初始化 Vim 环境、添加/移除/列出/更新 Vim
插件、编辑 Vim 配置等。</p>

<p>Vimenv 用法如下：</p>

<pre><code>% git clone git://github.com/xuxiaodong/vimenv.git
% cd vimenv
% ./vimenv -i                   # 初始化 Vim 配置，默认使用同目录下的 _vimrc 及 _vim
% ./vimenv -u                   # 更新插件
% ./vimenv -a majutsushi/tagbar # 添加 tagbar 插件，即去除 https://github.com/ 之后的部分
% ./vimenv -r gist              # 移除 gist 插件
% ./vimenv -l                   # 列出已安装的 Vim 插件
</code></pre>

<p>如果你使用我的 Vim 环境，只需执行前 4 步即可。另外，其下的 _vimrc
可换成你自己的，但别忘了在其中添加下面几行以使 Pathogen 使用正常：</p>

<pre><code>" Pathogen
runtime bundle/pathogen/autoload/pathogen.vim
call pathogen#infect()
call pathogen#helptags()
</code></pre>

<p>欢迎 fork :)</p>
	<p></p>
	<p>分类: <a href="http://linuxtoy.org/category/tips" title="View all posts in Tips" rel="category tag">Tips</a> | 
	<a href="http://linuxtoy.org/archives/vimenv.html">永久链接</a> |
	<a href="http://delicious.com/save?url=http://linuxtoy.org/archives/vimenv.html&amp;title=Vimenv%EF%BC%9A%E6%88%91%E7%9A%84%20Vim%20%E7%8E%AF%E5%A2%83%E6%88%91%E4%BD%9C%E4%B8%BB">收藏到 del.icio.us</a> | 
	<a href="mailto:?Subject=Check+This+Out&amp;body=I+think+you&#39;ll+like+this:+http://linuxtoy.org/archives/vimenv.html">Email 给好友</a> | 
    <a href="http://linuxtoy.org/archives/vimenv.html#comments">无评论</a> |
    <a href="http://linuxtoy.org/faq/donate">捐助本站</a></p>