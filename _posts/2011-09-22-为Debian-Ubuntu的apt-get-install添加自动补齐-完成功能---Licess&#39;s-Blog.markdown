---
layout: post
title:  "为Debian/Ubuntu的apt-get install添加自动补齐/完成功能 - Licess&#39;s Blog"
date:   2011-09-22 12:53:37
author: 
categories: program
---

## 为Debian/Ubuntu的apt-get install添加自动补齐/完成功能 - Licess&#39;s Blog
### by 
### at 2011-09-22 12:53:37
### original <http://blog.licess.org/debianubuntu-apt-get-install-autocompletion/>

<h1><a href="http://blog.licess.org/debianubuntu-apt-get-install-autocompletion/" rel="bookmark" title="Permalink to 《为Debian/Ubuntu的apt-get install添加自动补齐/完成功能》">为Debian/Ubuntu的apt-get install添加自动补齐/完成功能</a></h1>
	 <p>
					<span>2011年05月20日 13:54 | 作者：<a href="http://blog.licess.org/author/licess/" title="由 licess 发布" rel="author">licess</a></span>
				</p>
	 	 
	<div>
		<p>Debian/Ubuntu的apt-get太常用了，不过偶尔可能也会碰到不太熟悉，想不起来的包的名称，除了去<a href="http://www.debian.org/distrib/packages">debian packages</a>去查找，另外的方法就是给Debian/Ubuntu添加自动补齐/完成功能。方法如下：</p>
<p><strong>1、安装bash-completion</strong></p>
<p>apt-get install bash-completion</p>
<p><strong>2、编辑~/.bashrc 文件</strong></p>
<p>添加如下内容：<span></span></p>
<p>if [ -f /etc/bash_completion ]; then<br>
. /etc/bash_completion<br>
fi</p>
<p><strong>3、使其生效</strong></p>
<p>退出SSH，重新登录。</p>
<p>apt-get install build-e  然后TAB一下，自动补齐了吧。</p>
<p>好久不写东西了，都荒草丛生了。</p></div>