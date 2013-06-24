---
layout: post
title:  "使用vimux加速ruby开发测试的技术汇总"
date:   2012-10-05 13:46:28
author: 
categories: program
---

## 使用vimux加速ruby开发测试的技术汇总
### by 
### at 2012-10-05 13:46:28
### original <http://coderchina.tumblr.com/post/32925584775>

<p>vimux是VIM的插件，目的是让你在VIM写代码的同时，还能开一个新tmux窗口运行测试。这里是tmux不是screen，我表示遗憾，毕竟我还是很喜欢screen的，:-o.</p>
<p><a href="https://github.com/benmills/vimux"><a href="https://github.com/benmills/vimux">https://github.com/benmills/vimux</a></a></p>
<p>安装方法：</p>
<p><span>Install </span><a href="https://github.com/gmarik/vundle">Vundle</a><span>:</span></p>
<pre><code>git clone <a href="https://github.com/gmarik/vundle.git">https://github.com/gmarik/vundle.git</a> ~/.vim/bundle/vundle</code></pre>
<p>编辑 <strong>.vimrc</strong>:</p>
<p><span>Bundle ‘gmarik/vundle’ </span></p>
<p><span>Bundle ‘benmills/vimux’</span></p>
<p><span>Bundle ‘pgr0ss/vimux-ruby-test’</span></p>
<p>然后在VIM下运行 <strong>:BundleInstall</strong></p>
<p>然后开始我们的测试之旅吧。</p>
<p>随意编辑一个spec文件，运行<span>RunRubyFocusedTest就会开启一个tmux sub session跑当前鼠标所指的test case.</span></p>
<p>感觉还是有点不爽，可以换一个测试插件，vim-vroom。</p>
<p>安装：</p>
<p>Add the following to your <code>.vimrc</code> after vundle setup:</p>
<pre><code>Bundle 'skalnik/vim-vroom' </code></pre>
<p>and remember to run <code>:BundleInstall</code>.</p>
<p>安装完成之后，在你的Rails工程目录，直接打开spec文件，马上就是使用leader + r or R跑了。慢着，想调运vimux插件，只需要加一行自定义参数在vimrc中就可以搞定。</p>
<p><span>let g:vroom_use_vimux = 1</span></p>
<p><span>我拿lazy_high_charts跑了几圈，特别满意。</span></p>
<p><span><img alt="vimux" height="370" src="http://l.ruby-china.org/photo/96810233c7896b7753f4cebfbdb0690a.png" width="680"></span></p>
<p><span><br></span></p>
<p><span><br></span></p>
<p><strong><br></strong></p>