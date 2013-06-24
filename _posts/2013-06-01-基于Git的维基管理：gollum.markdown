---
layout: post
title:  "基于Git的维基管理：gollum"
date:   2013-06-01 15:00:00
author: 
categories: program
---

## 基于Git的维基管理：gollum
### by 
### at 2013-06-01 15:00:00
### original <http://www.yangzhiping.com/tech/gollum.html>

<p>在旧文<a href="http://www.yangzhiping.com/tech/github.html">如何高效利用Github</a>中，曾介绍过<a href="https://github.com/gollum/gollum">gollum</a>。可惜，很多朋友没有理解这一工具的强大之处，使用流程不对，思维仍然停留在Github诞生之前的时代。</p>

<p>就像<a href="http://www.zotero.org/">Zotero</a>一样，明明是非常强大的知识管理工具，但是因为错误使用流程，没有享受到其优点。借昨晚重新搭建Wiki过程，介绍一下<a href="https://github.com/gollum/gollum">gollum</a>的最佳使用流程。</p>

<h2>前提</h2>

<p>假设你已阅读过<a href="http://www.yangzhiping.com/tech/writing-space.html">理想的写作环境：Git+Github+Markdown+Jekyll</a>一文，熟悉相关知识，并且已经成功在Github上搭建了一个个人博客。比如，我的博客的Git地址是：</p>

<pre><code>git@github.com:ouyangzhiping/ouyangzhiping.github.com.git
</code></pre>

<p>请记得改成你的相应地址。</p>

<h2>开通Wiki</h2>

<p>登陆<a href="http://github.com">Github</a>，找到你所开通的Github项目的<code>Settings</code>栏目，开通<code>Wikis</code>，如果只希望别人可读不可写，勾选：<code>Restrict edits to Collaborators only</code>。如下图所示：</p>

<p><img src="http://www.yangzhiping.com/images/tech/gollum/github_wiki.png" alt="Github Wiki"></p>

<h2>关键步骤</h2>

<p>关键步骤来了！很多没有将<a href="https://github.com/gollum/gollum">gollum</a>强大之处发挥出来的朋友卡在这一步。</p>

<p>打开命令提示符，<code>git clone</code>下相应维基的git地址，请格外注意，地址是在你的个人博客的git地址之前加上<code>wiki</code>：</p>

<pre><code>cd ~/dev/learn
git clone git@github.com:ouyangzhiping/ouyangzhiping.github.com.wiki.git wiki
</code></pre>

<p>clone成功之后，进入wiki相应目录，上述Git Clone的网址也可以在开通Wiki之后的<code>Git Access</code>栏目查看。</p>

<pre><code>cd wiki
</code></pre>

<p>新建Gemfile文件，内容如下：</p>

<pre><code>source &quot;http://rubygems.org&quot;
gem &#39;redcarpet&#39;
gem &quot;grit&quot;, &#39;~&gt; 2.5.0&#39;, git: &#39;https://github.com/gitlabhq/grit.git&#39;, ref: &#39;42297cdcee16284d2e4eff23d41377f52fc28b9d&#39;
gem &#39;gollum&#39;,git: &#39;https://github.com/gollum/gollum.git&#39;
</code></pre>

<p>运行：</p>

<pre><code>bundle install 
</code></pre>

<p>即已安装成功<code>gollum</code>等。然后运行：</p>

<pre><code>gollum
</code></pre>

<p>就可以在本地启动成功维基。打开网址：<code>http://0.0.0.0:4566/</code>，如下图所示：</p>

<p><img src="http://www.yangzhiping.com/images/tech/gollum/gollum.png" alt="Gollum"></p>

<h2>云端备份</h2>

<p>现在，可以在<code>wiki</code>目录下面，像任何一个git库操作一样，正常提交本地对维基内容的修改。一切将自动保存在你的Github上的个人博客网站的<code>wiki</code>目录下面。如：</p>

<pre><code>cd ~/dev/learn/wiki
git add .
git commit -am"维基的一些变更“
git push
</code></pre>

<h2>配置随机启动</h2>

<p>安装<a href="http://pow.cx/">pow</a>之后，即可配置随机启动。在根目录下创建<code>config.ru</code>文件，填入以下内容：</p>

<pre><code>#!/usr/bin/ruby
require 'rubygems'
require 'gollum/app'

gollum_path = '/Users/ouyang/dev/github/ouyang/wiki'

disable :run

configure :development, :staging, :production do
 set :raise_errors, true
 set :show_exceptions, true
 set :dump_errors, true
 set :clean_trace, true
end

$path = gollum_path
Precious::App.set(:gollum_path, gollum_path)
Precious::App.set(:wiki_options, {})
run Precious::App
</code></pre>

<p>请记住，将gollum_path 改为你的wiki所在目录。更多配置参见：<a href="https://gist.github.com/ouyangzhiping/5693143">gist5693143</a></p>

<p>然后使用<code>pow</code>链接到该wiki目录。就可以使用类似于<code>wiki.dev</code>网址打开维基。</p>

<h2>优势</h2>

<p>这么写维基的好处是什么呢？</p>

<ul>
<li>一切变更，都有git来保存内容与版本更迭，不再担心写废；</li>
<li>对于不熟悉Git、Github的Windows用户来说，可以直接登陆Github，在线编辑维基；</li>
<li>一切都是开源，成本等于零；</li>
<li>支持Markdown、Rst等常见格式；</li>
<li>云端备份，学到哪，写到哪；走到哪，写到哪。</li>
</ul>


<p>写博客与写维基的区别是什么？前者凸显创作；后者适合整理。很多入门级别、复用别人的操作，并不适合放在个人博客上。即使你不是技术人员，如果需要大量写作，也不妨尝试一下Git、Markdown与Github，给你带来的惊喜会大于你的学习成本。同时，你可以采取一种我实践多年的<code>主题学习法</code>来写维基，主要步骤如下：</p>

<ul>
<li>购买大量读物；</li>
<li>快速扫描目录，了解核心知识点所在，假设自己要写一本书，将这个目录写出来；</li>
<li>实践两个好的课程，了解实践中的知识点怎么归入自己这本书中;</li>
<li>未来不断往自己这本书中填内容。</li>
</ul>