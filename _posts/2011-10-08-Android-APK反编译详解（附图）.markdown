---
layout: post
title:  "Android APK反编译详解（附图）"
date:   2011-10-08 18:10:00
author: iGFW
categories: program
---

## Android APK反编译详解（附图）
### by iGFW
### at 2011-10-08 18:10:00
### original <http://igfw.tk/archives/5722>

<p><strong>一、反编译Apk得到Java源代码</strong><br>
首先要下载两个工具：dex2jar和JD-GUI</p>
<p>前者是将apk中的classes.dex转化成Jar文件，而JD-GUI是一个反编译工具，可以直接查看Jar包的源代码。以下是下载地址：</p>
<p><strong>dex2jar：</strong><a href="http://laichao.googlecode.com/files/dex2jar-0.0.7-SNAPSHOT.zip"><span>http://laichao.googlecode.com/files/dex2jar-0.0.7-SNAPSHOT.zip</span></a></p>
<p><strong>JD-GUI：</strong><a href="http://laichao.googlecode.com/files/jdgui.zip"><span>http://laichao.googlecode.com/files/jdgui.zip</span></a></p>
<p><span></span></p>
<p><strong>具体步骤：</strong></p>
<p>首先将apk文件，将后缀改为zip，解压，得到其中的classes.dex，它就是java文件编译再通过dx工具打包而成的；解压下载的 dex2jar，将classes.dex复制到dex2jar.bat所在目录。在命令行下定位到dex2jar.bat所在目录</p>
<ul>
<li>运行 dex2jar.bat classes.dex 生成 classes.dex.dex2jar.jar</li>
</ul>
<p><img src="http://foxlog.co.cc/usedPictures/20110921/1.gif" alt="">生成jar文件</p>
<ul>
<li>运行JD-GUI（jd-gui.exe），打开上面生成的jar包，即可看到源代码了</li>
</ul>
<p><img src="http://foxlog.co.cc/usedPictures/20110921/2.gif" alt=""><br>
HelloAndroid源码（编译前的apk源码对照）如下：</p>
<p><strong><img src="http://foxlog.co.cc/usedPictures/20110921/3.gif" alt=""></strong><strong><br>
</strong></p>
<p><strong>二、反编译apk生成程序的源代码和图片、XML配置、语言资源等文件</strong></p>
<p>如果是只是汉化软件，这将特别有用。</p>
<p>首先还是下载工具，这次用到的是apktool</p>
<p>下载地址：<a href="http://code.google.com/p/android-apktool/downloads/list"><span>http://code.google.com/p/android-apktool/downloads/list</span></a></p>
<p>下载<span>：</span><a href="http://android-apktool.googlecode.com/files/apktool1.4.1.tar.bz2"><span>apktool1.4.1.tar.bz2</span></a> 和 <a href="http://android-apktool.googlecode.com/files/apktool-install-windows-r04-brut1.tar.bz2"><span>apktool-install-windows-r04-brut1.tar.bz2</span></a>（两个包都下载）</p>
<p><strong>具体步骤：</strong></p>
<p>将下载的两个包解压到同一个文件夹下，应该会有三个文件：aapt.exe，apktool.bat，apktool.jar</p>
<p>在命令行下定位到apktool.bat文件夹，输入以下命令：apktool d C:\*.apk C:\*文件夹，如下图：</p>
<p><img src="http://foxlog.co.cc/usedPictures/20110921/4.gif" alt=""></p>
<p><span>命令行解释：apktool   d   [apk文件 ]   [输出文件夹]</span></p>
<p>反编译的文件如下（AndroidManifest.xml为例）：</p>
<p><img src="http://foxlog.co.cc/usedPictures/20110921/5.gif" alt=""></p>
<p>特别注意：你要反编译的文件一定要放在C盘的根目录里</p>
<p>将反编译完的文件重新打包成apk，很简单，输入<span>apktool b c:\***文件夹（你编译出来文件夹）</span>即可，命令如下：</p>
<p><img src="http://foxlog.co.cc/usedPictures/20110921/6.gif" alt=""></p>
<p>打包apk后的文件在目录C:\HelloAndroid下，生成了两个文件夹： build和dist</p>
<p>其中，打包生成的HelloAndroid.apk，在上面的dist文件夹下，Ok</p>
<p><strong>注：</strong>本android反编译教程，是在Windows 7  Ultimate  64bit ，测试通过</p>
<p><a href="http://d.download.csdn.net/down/3556426/sunboy_2050"><strong><span style="font-size:small">上述反编译工具包下载</span></strong></a></p>
<p>本文原文：<a href="http://blog.csdn.net/sunboy_2050/article/details/6727581">http://blog.csdn.net/sunboy_2050/article/details/6727581</a> 略有改动。</p>
<p><strong>来源</strong>：<a href="http://www.foxhouse.tk/android-apk%E5%8F%8D%E7%BC%96%E8%AF%91%E8%AF%A6%E8%A7%A3%EF%BC%88%E9%99%84%E5%9B%BE%EF%BC%89/">http://www.foxhouse.tk/android-apk%E5%8F%8D%E7%BC%96%E8%AF%91%E8%AF%A6%E8%A7%A3%EF%BC%88%E9%99%84%E5%9B%BE%EF%BC%89/</a></p>