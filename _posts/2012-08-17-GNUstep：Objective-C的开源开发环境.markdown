---
layout: post
title:  "GNUstep：Objective-C的开源开发环境"
date:   2012-08-17 11:27:29
author: 杨玉廷
categories: program
---

## GNUstep：Objective-C的开源开发环境
### by 杨玉廷
### at 2012-08-17 11:27:29
### original <http://hp.dewen.org/?p=1246>

<p>Objective-C近几年变得越来流行，TIOBE世界编程语言排行榜中<a href="http://www.dewen.org/topic/6">Objective-C</a>的排名不断上升，同时也反应出Apple平台的开发受到越来越多的人关注。Objective-C语言作为Apple世界的官方语言，无论是MAC开发还是iOS开发，甚至系统层的编写它都能够胜任。融合了C和Smalltalk的面向对象特性，Objective-C变得简单、高效（比起<a href="http://www.dewen.org/topic/3">C++</a>等面向对象语言）。</p>
<p>一般情况下，要想玩玩Objective-C，可以购买一台MAC电脑+免费的Xcode，就拥有了一整套<a href="http://hp.dewen.org/?p=1246">Objective-C的开发环境</a>，就可以为你的iPhone、iPad、iTouch、Mac机编写应用了。Apple的东西，做工精细的同时也价格不菲，难怪有人说：“单反毁三代，苹果毁一生”。本文要给大家推荐一个<strong>开源的Objective-C开发环境——GNUstep</strong>，从此以后你既可以在Linux平台上玩ObjC，也可以在Windows平台上玩ObjC了。</p>
<div style="text-align:center;margin-bottom:5px;margin-top:5px"><img src="http://hp.dewen.org/wp-content/uploads/2012/08/gnustep-header14.jpg" alt="GNUstep：Objective-C的开源开发环境" title="GNUstep：Objective-C的开源开发环境" border="0"></div>
<p><span></span></p>
<h2 style="font-size:16px;text-transform:none">1. 一点历史</h2>
<p>简单来说，GNUstep 是使用OpenStep 界面的开源(Open Source) 计划，目的为提供跨平台的面向对象的开发环境。</p>
<p>早在1985 年，Steve Jobs 离开苹果电脑(Apple) 后成立了NeXT 公司，并于1988 年推出了NeXT 电脑，使用NeXTStep 为操作系统。在当时，NeXTStep 是相当先进的系统。 以Unix (BSD) 为基础，使用PostScript 提供高品质的图形界面，并以Objective-C 语言提供完整的面向对象环境。</p>
<p>尽管NeXT 在软件上的优异，其硬体销售成绩不佳，不久之后，NeXT 便转型为软件公司。1994 年，NeXT 与Sun(Sun Microsystem) 合作推出OpenStep 界面，目标为跨平台的面向对象程式开发环境。NeXT 接着推出使用OpenStep 界面的OPENSTEP 系统，可在Mach, Microsoft Windows NT, Sun Solaris 及HP/UX 上执行。1996 年，苹果电脑买下NeXT，做为苹果电脑下一代操作系统的基础。 OPENSTEP 系统便演进成为MacOS X 的Cocoa 环境。</p>
<p>在1995 年，自由软体基金会(Free Software Fundation) 开始了<strong>GNUstep</strong> 计划，目的在使用OpenStep 界面，以提供Linux/BSD 系统一个完整的程式发展环境。但由于OpenStep 介面过于庞大，开发人力不足，及许多技术在当时尚未成熟(如Display PostScript)，所以直到目前为止，GNUstep 才算是一个完整的编程开发环境。</p>
<p>尽管<a href="http://hp.dewen.org/?p=1246">OpenStep </a>早在1994 年便提出，其界面及架构在现今仍相当先进及实用，使得开发GNUstep 程序相当容易。</p>
<h2 style="font-size:16px;text-transform:none">2. 简介</h2>
<p><strong>GNUstep 使用Objective-C 语言，是<a href="http://www.dewen.org/topic/2">C 语言</a>加上SmallTalk 的面向对象的功能。结合两者的优点，又不至于像C++复杂。</strong></p>
<p>GNUstep 提供两个主要的程序库，Foundation 及AppKit。Foundation 处理非图形介面的部份。如字串, 档案, 网路, 基本资料结构, 多行绪等, 又称之为GNUstep Base。AppKit 则处理图形介面的部份, 包含视窗, 使用者界面等, 又称之为GNUstep GUI。</p>
<p>由于GNUstep 具有跨平台的特性，有关绘图及字型的部份，则交由GNUstep Back 来处理。使用者可依所使用的操作系统，选择适当的后端处理(Backend)。 GNUstep GUI 会自行处理与Back 相关的功能，程式开发者只要使用GUI 程式库，便可适用于各种后端上， 完全不用考虑平台问题。</p>
<h2 style="font-size:16px;text-transform:none">3. Linux下安装GNUstep</h2>
<p>在此仅介绍一下Ubuntu下面的安装，其它安装方法，参考GNUStep官方文档：<a href="http://www.gnustep.org/resources/documentation/User/GNUstep/gnustep-howto_toc.html">http://www.gnustep.org/resources/documentation/User/GNUstep/gnustep-howto_toc.html</a>。</p>
<p><strong>Step1：让gcc支持objective-C的编译</strong></p>
<p>$apt-get install gobjc<br>
$gcc -x objective-c hello.m -o hello<br>
hello.m 随便写几个c函数，编译通过就OK了。例如：<br>
int main(int argc, const char* argv[])<br>
 {<br>
 printf(“Hello Objective-C！”);<br>
 return 0;<br>
 }</p>
<p><strong>Step2：安装GNUstep</strong></p>
<p>$apt-get install gnustep<br>
$apt-get install gnustep-devel<br>
$apt-get install libgnustep-base-dev</p>
<p><strong>Step3：配置GNUSTEP_MAKEFILES和执行GNUstep.sh，自动加载其它环境路径</strong></p>
<p>$chmod +x /usr/share/GNUstep/Makefiles/GNUstep.sh<br>
$export GNUSTEP_MAKEFILES=/usr/share/GNUstep/Makefiles<br>
$source /usr/share/GNUstep/Makefiles/GNUstep.sh</p>
<p>也可以把后面两条放在.bashrc或.bash_profile中，这样就不用每次登陆或者新开终端都敲一遍了。</p>
<p><strong>Step4：编译一个简单的hello程序，测试下看GNUstep安装的是否OK</strong></p>
<p>hello.m内容如下：<br>
#import <br>
int main(int argc, const char* argv[])<br>
 {<br>
 NSAutoreleasePool* pool = [[NSAutoreleasePool alloc] init];<br>
 NSLog(@”Hello GNUstep!!\n”);<br>
 [pool release];<br>
 return 0;<br>
 }</p>
<p>编译一下：</p>
<p>$gcc -Wall -g -o hello hello.m -I/usr/include/GNUstep/ -fconstant-string-class=NSConstantString  -lobjc -lgnustep-base<br>
$./hello<br>
2012-08-16 17:54:08.315 hello[12915] Hello GNUstep!!</p>
<p>编译没有出错，运行hello看到”Hello….”，恭喜你！GNUstep安装成功！</p>
<h2 style="font-size:16px;text-transform:none">4. Windows下安装GNUstep</h2>
<p>Windows下安装GNUstep是比较简单了，直接在此下载安装包：<a href="http://www.gnustep.org/experience/Windows.html">http://www.gnustep.org/experience/Windows.html</a>，内置了MSYS系统。直接从菜单“开始”-&gt;”所有程序” -&gt; “GNUstep” -&gt; “shell”， 创建Objective-C文件hello.m（内容同上），用如下命令编译：</p>
<p>$ gcc -o hello hello.m -I /GNUstep/System/Library/Headers -L /GNUstep/System/Library/Libraries/ -fconstant-string-class=NSConstantString -lobjc -lgnustep-base</p>
<h2 style="font-size:16px;text-transform:none">5. 参考资料</h2>
<p>官方主页：<a href="http://www.gnustep.org/">http://www.gnustep.org/</a></p>
<p><b>本文作者：david++<br>
原文地址：<a href="http://game-lab.org/?p=370">每周一荐：Objective-C的开源开发环境GNUstep</a></b></p>