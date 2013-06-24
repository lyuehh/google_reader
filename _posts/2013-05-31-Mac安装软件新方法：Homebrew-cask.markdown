---
layout: post
title:  "Mac安装软件新方法：Homebrew-cask"
date:   2013-05-31 15:00:00
author: 
categories: program
---

## Mac安装软件新方法：Homebrew-cask
### by 
### at 2013-05-31 15:00:00
### original <http://www.yangzhiping.com/tech/homebrew-cask.html>

<p>Homebrew是Ruby社区极富想象力的一个作品，使得Mac下安装Mysql等常用包不再困难。那么，是否也可以通过<code>brew install mysql</code>这样简单的方式来安装chrome浏览器？</p>

<p>近期，phinze的作品<a href="https://github.com/phinze/homebrew-cask">homebrew-cask</a>解决了这一问题。</p>

<h2>安装</h2>

<p>假设你已安装好了<a href="http://mxcl.github.io/homebrew/index_zh-cn.html">Homebrew</a>，安装与使用极其简单，打开终端，输入：</p>

<pre><code>brew tap phinze/homebrew-cask
brew install brew-cask
</code></pre>

<p>然后就可以像brew一样来安装chrome等软件，例如：</p>

<pre><code>brew cask install google-chrome
</code></pre>

<p>再也不需要以前的打开网页、找到链接、下载软件、解压包、放到程序目录，再来启动它这么复杂的步骤了。一键完成！</p>

<h2>创建你的cask</h2>

<p><a href="https://github.com/phinze/homebrew-cask">homebrew-cask</a>一发布，即得到广泛支持，请参考<a href="https://github.com/phinze/homebrew-cask/tree/master/Casks">homebrew cask 已支持软件列表</a>。不过有些特殊软件，它并不支持。如何创建自己的cask？并共享给其它用户？</p>

<p>请参考<a href="https://github.com/phinze/homebrew-cask">homebrew-cask</a>官方文档：<a href="https://github.com/phinze/homebrew-cask/blob/master/CONTRIBUTING.md">How to Contribute</a>。</p>

<p>需要特别注意的是，如何查找软件sha1，命令如下：</p>

<pre><code>openssl sha1 ~/Downloads/Zotero-4.0.8.dmg
</code></pre>

<p>花费半个小时，提交了一堆中国较常用的软件。以<a href="http://www.zotero.org/">Zotero</a>为例，运行：</p>

<pre><code>brew cask create zotero
</code></pre>

<p>会在 <code>/usr/local/Library/Taps/phinze-cask</code>目录下面，创建名为：<code>zotero.rb</code>的文件，然后修改内容如下：</p>

<pre><code>class Zotero &lt; Cask
  url &#39;http://download.zotero.org/standalone/4.0.8/Zotero-4.0.8.dmg&#39;
  homepage &#39;http://www.zotero.org/&#39;
  version &#39;4.0&#39;
  sha1 &#39;1ceedd019fdec48621910d80ea893ff0d75404df&#39;
  link :app, &#39;Zotero.app&#39;
end
</code></pre>

<h2>验证</h2>

<p>在 <code>/usr/local/Library/Taps/phinze-cask</code>目录下面，运行：</p>

<pre><code>rake test  
</code></pre>

<p>最好创建一个独立的gem集合，拿来测试与运行。会检查你的cask是否正确。一些常见的错误有：</p>

<h3>sf地址不对</h3>

<p>请将：</p>

<p><code>http://jaist.dl.sourceforge.net/project/scribus/scribus/1.4.2/scribus-1.4.2.dmg</code></p>

<p>修改为：</p>

<p><code>http://downloads.sourceforge.net/project/scribus/scribus/1.4.2/scribus-1.4.2.dmg</code></p>