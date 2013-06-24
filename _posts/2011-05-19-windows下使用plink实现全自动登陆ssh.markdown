---
layout: post
title:  "windows下使用plink实现全自动登陆ssh"
date:   2011-05-19 10:17:43
author: iGFW
categories: program
---

## windows下使用plink实现全自动登陆ssh
### by iGFW
### at 2011-05-19 10:17:43
### original <http://igfw.tk/archives/2736>

<div>
<p><strong>ssh是什么？</strong><br>
ssh是一个安全通道协议</p>
<p><strong>plink是什么？</strong><br>
plink是这个通道协议的一个实现。</p>
<p><strong>关于plink</strong><br>
官方主页 <a href="http://www.chiark.greenend.org.uk/%7Esgtatham/putty/download.html">http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html</a><br>
下载地址 <a href="http://the.earth.li/%7Esgtatham/putty/latest/x86/plink.exe">http://the.earth.li/~sgtatham/putty/latest/x86/plink.exe</a></p>
<p><strong><span></span><br>
怎么用？</strong><br>
首先您得有个ssh帐号（至于怎么得，看你自己啦）<br>
其次，把plink下载到桌面后，把下面内容保存为ssh.bat，运行之</p>
<div>
<div>
<div><a href="http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/#">帮助</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<div>1</div>
<div>2</div>
</td>
<td>
<div>
<div><code>@</code><code>echo</code> <code>off</code></div>
<div><code>plink -C -</code><code>v</code> <code>-N -D 7070 -l  用户名 -pw 密码 服务器名</code></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
<p><strong>说明：</strong></p>
<ul>
<li> * 换成相应的信息，参数之间是有个空格的。</li>
<li> * -C表示压缩数据 -v表示显示输出信息 -N 表示不打开远程终端 -D 表示映射本地端口 -l 表示登陆名 -pw 表示密码</li>
<li>* 如果要指定远程端口，使用参数-P，如指定远程端口1024，-P 1024</li>
<li>* 初次登陆会问你y/n, 按y</li>
<li> * 密码可能要双引号。如密码是&amp;zerwerjk , 参数-pw要变成 -pw  “&amp;zerwerjk”</li>
<p><strong>针对问y/n，官方的说法是这样的</strong></p>
<blockquote><p>To avoid being prompted for the server host key when  using Plink for an automated connection, you should first make a manual  connection (using either of PuTTY or Plink) to the same server, verify  the host key (see section 2.2 for more information), and select Yes to  add the host key to the Registry. After that, Plink commands connecting  to that server should not give a host key prompt unless the host key  changes.</p></blockquote>
<p>大意是，先登陆一次那个服务器，回答了那个y/n的问题，之后再登陆就不用回答了。</p>
<p><strong>折腾了一段时间，发现我out了，在windows也可以用管道 | , 一个echo y就解决了即</strong></p>
<div>
<div>
<div><a href="http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/#">帮助</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<div>1</div>
<div>2</div>
</td>
<td>
<div>
<div><code>@</code><code>echo</code> <code>off</code></div>
<div><code>echo</code> <code>y|plink -C -</code><code>v</code> <code>-N -D 7070 -l  用户名 -pw 密码 服务器名</code></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
</ul>
<p><strong>阅读更多！</strong><br>
怎么隐藏cmd窗口?<br>
把下面代码保存为ssh.vbs，运行之。</p>
<div>
<div>
<div><a href="http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/#">帮助</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<div>1</div>
<div>2</div>
<div>3</div>
</td>
<td>
<div>
<div><code>dim objShell</code></div>
<div><code>set</code> <code>objShell=wscript.createObject(</code><code>"WScript.Shell"</code><code>)</code></div>
<div><code>iReturnCode=objShell.Run(</code><code>"ssh.bat"</code><code>,0,TRUE)</code></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
<p><strong><br>
更高深的ssh.bat/ssh.vbs版本?</strong><br>
<strong>版本1 在ssh断开之后播放一个声音。</strong></p>
<div>
<div>
<div><a href="http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/#">帮助</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<div>1</div>
<div>2</div>
<div>3</div>
</td>
<td>
<div>
<div><code>@</code><code>echo</code> <code>off</code></div>
<div><code>plink -C -</code><code>v</code> <code>-N -D 7070 -l  用户名 -pw 密码 服务器名</code></div>
<div><code>mplayer supermario_game_over.wav</code></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
<p><strong>说明：</strong><br>
1、要下载命令行版本的mplayer才行。折腾帝哈。<br>
2、把mplayer的路径添加到环境变量。<br>
3、我不知道windows下的播放声音命令是什么，linux下是aplay或者ossplay，有知道的朋友请告知一下。</p>
<p><strong>版本2 在ssh断开之后显示一个窗口</strong><br>
把ssh.vbs内容换成</p>
<div>
<div>
<div><a href="http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/#">帮助</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
</td>
<td>
<div>
<div><code>dim objShell</code></div>
<div><code>set</code> <code>objShell=wscript.createObject(</code><code>"WScript.Shell"</code><code>)</code></div>
<div><code>iReturnCode=objShell.Run(</code><code>"ssh.bat"</code><code>,0,TRUE)</code></div>
<div><code>a=</code><code>"SSH断开了  : ("</code></div>
<div><code>msgbox a ,48,</code><code>"提示"</code></div>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
<p>附上</p>
<p>用户环境变量的设定 计算机==&gt;右键“属性”==&gt;“高级系统设置”==&gt;“环境变量”==&gt;用户环境变量“编辑”</p>
<p>SSH断开了<br>
<a href="http://speedup2012.tk/wp-content/uploads/2011/01/ssh.png"><img title="ssh" src="http://speedup2012.tk/wp-content/uploads/2011/01/ssh-300x225.png" alt="" width="300" height="225"></a></p>
</div>
<p><strong>来源</strong>：<a href="http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/">http://speedup2012.tk/2011/01/use-plink-to-auto-login-ssh-in-windows/</a></p>
<p>感谢<strong>lol</strong>网友的指教</p>
<p>下面内容保存为ssh.bat</p>
<p>@echo off<br>
:relink<br>
echo y|plink -C -N -D 7070 用户名@服务器 -P ssh端口 -pw 密码 -v<br>
goto relink</p>
<p>这样就不用按y回车,-v查看连接(可有可无)relink掉线自动重连,默认22端口的可省略-P</p>
<p>感谢<strong>ming</strong>网友的指教</p>