---
layout: post
title:  "如何在安装双启动后卸载 Windows 或者 Linux"
date:   2013-05-27 14:54:04
author: 童海波
categories: program
---

## 如何在安装双启动后卸载 Windows 或者 Linux
### by 童海波
### at 2013-05-27 14:54:04
### original <http://blog.jobbole.com/40278/?utm_source=rss&utm_medium=rss&utm_campaign=%25e5%25a6%2582%25e4%25bd%2595%25e5%259c%25a8%25e5%25ae%2589%25e8%25a3%2585%25e5%258f%258c%25e5%2590%25af%25e5%258a%25a8%25e5%2590%258e%25e5%258d%25b8%25e8%25bd%25bd-windows-%25e6%2588%2596%25e8%2580%2585-linux>

<p>英文原文：<a href="http://lifehacker.com/how-to-uninstall-windows-or-linux-after-dual-booting-508710422">How to Uninstall Windows or Linux After Dual-Booting</a> 编译：<a href="http://www.oschina.net/translate/how-to-uninstall-windows-or-linux-after-dual-booting?p=2#comments">oschina</a></p>
<p>关于如何在同一台电脑上运行多个操作系统的文章数不胜数，比如有的文章介绍了如何同时安装Windows和Linux，有的文章介绍了如何同时安装Windows和OS X，还有一些其他的文章。但是，当你想卸载其中的某个操作系统，你应该怎么办呢？下面，我们就介绍一下你安装了“Windows+Linux”双系统后，如何卸载Windows或Linux。</p>
<p>这个过程其实非常简单，但很多人安装双系统后第一次遇到这类问题时总会向我们求助，因此我们决定把方法写在这里，以便大家能够很容易看到。当你想卸载某个操作系统时，只需要找到它在哪里，然后把对应的分区删除就可以了。当然，不同的操作系统操作起来可能不太一样，所以我们给出三种解决办法。</p>
<p><em>注意：在开始操作之前，一定要备份你要保留的那个操作系统。因为，一旦你操作失误，很有可能就会删错系统，后果会相当相当严重。</em></p>
<p><a href="http://cdn2.jobbole.com/2013/05/23073054_NDKx.jpg" rel="lightbox[40278]" title="如何在安装双启动后卸载 Windows 或者 Linux"><img title="如何在安装双启动后卸载 Windows 或者 Linux" src="http://cdn2.jobbole.com/2013/05/23073054_NDKx.jpg" alt="如何在安装双启动后卸载 Windows 或者 Linux"></a></p>
<p><strong>保留Windows删除Linux</strong></p>
<p>如果你想将Linux从机器中删除，只保留Windows，你需要进行以下几步操作：</p>
<ol>
<li>从开始菜单（或开始屏幕）找到“Disk Management”（磁盘管理工具）并启动.</li>
<li>找到Linux分区。在Windows下无法识别Linux分区，所以，你需要按照驱动器和分区大小去判断，确认好以后继续下面的步骤。</li>
<li>右键选好的分区，然后选择“删除卷”，这将会把分区删除成自由空间，如果你再选择自由空间，选择删除卷，就会变成未分配空间。</li>
<li>在Windows分区上右键，选择“扩展卷”，扩展它以填补删除Linux所留下的自由空间。</li>
<li>最后，插入Windows恢复光盘（或恢复USB驱动器），并从它启动。选择“修复计算机”，去“疑难解答”，然后输入一个命令提示符。键入以下命令
<pre>bootrec.exe /fixmbr</pre>
<p>这将删除Linux的bootloader和恢复Windows’。</p></li>
<li>重新启动你的电脑，你会发现，它直接引导进入Windows，不会有Linux分区。</li>
</ol>
<p>如果你设置了双启动不同或把一个单独的硬盘驱动器上的Linux，又或者有其他的操作系统在驱动器上的，操作方式可能会略有不同。但对于大多数人来说，这些指令就足够了。</p>
<p><a href="http://cdn2.jobbole.com/2013/05/23073056_F2FU.jpg" rel="lightbox[40278]" title="如何在安装双启动后卸载 Windows 或者 Linux"><img title="如何在安装双启动后卸载 Windows 或者 Linux" src="http://cdn2.jobbole.com/2013/05/23073056_F2FU.jpg" alt="如何在安装双启动后卸载 Windows 或者 Linux"></a></p>
<p><strong>保留 OS X 并移除 Windows 或 Linux</strong></p>
<p>如果你在用 Mac，删除其它分区非常简易。同样的，假设你的分区都在同一块硬盘上。</p>
<p><em>更新</em>：读者 <a href="http://martyf81.kinja.com/" rel="nofollow">MartyF81</a> <a href="http://lifehacker.com/if-you-are-running-windows-in-bootcamp-on-an-apple-you-508866761" rel="nofollow">指出</a> ，最好的卸载 Windows 的方法就是使用 Boot Camp 内置的卸载器。你只需打开 Boot Camp 助理，并跟随提示逐步完成。</p>
<p>若需要移除 Linux 分区(或者你在 Hackintosh) 上，你需要做：</p>
<ol>
<li>打开“/应用/实用工具”中的“磁盘工具”。</li>
<li>在左侧面板上点击你的硬盘（硬盘，而非分区），并进入“分区”分页。找到要移除的 Windows 或 Linux 分区（你可能在创建时添加了卷）。</li>
<li>点击你要移除的分区，然后点击窗口底部的小减号按钮。这样分区将从你的系统中移除。</li>
<li>点击你的 Mac 分区的一角并下拉，这样它将占据出现的自由空间。完成操作后点击应用。</li>
</ol>
<p>你的 Mac 可能会花一点时间完成一些必要的步骤，当它结束后，你的系统将恢复原来的 Macintosh。如果你的系统中安装了 rEFIt，你可以不管它——这不会损害任何东西——不过你只需要 <a href="http://refit.sourceforge.net/doc/c1s3_remove.html" rel="nofollow">删除几个文件</a>就可以移除它。</p>
<p><a href="http://cdn2.jobbole.com/2013/05/23073056_W1Jk.jpg" rel="lightbox[40278]" title="如何在安装双启动后卸载 Windows 或者 Linux"><img title="如何在安装双启动后卸载 Windows 或者 Linux" src="http://cdn2.jobbole.com/2013/05/23073056_W1Jk.jpg" alt="如何在安装双启动后卸载 Windows 或者 Linux"></a></p>
<p><strong>保留 Linux 并删除 Windows</strong></p>
<p>如果你喜爱冒险，决定全部时间都是用 Linux，那么你要做的事非常简单。发行版和你特别安装版的说明可能有些不同，但对于传统 Ubuntu 安装，应该类似于下面：</p>
<ol>
<li>插入你所用 Linux 发行版的 Live CD 或 USB 并启动其分区管理器(例如 <a href="http://gparted.sourceforge.net/" rel="nofollow">Gparted</a>)。在 Gparted 的菜单中找到你的 Windows 分区——它将被列为 NTFS 分区。</li>
<li>右键点击 Windows 分区，在菜单中选择“删除”。你的电脑中可能还有其它与 Windows 相关的分区，如 “System Reserved” 和恢复分区。要是愿意，你也可以删除这些分区（但请在删除恢复分区前确定你有恢复盘）</li>
<li>右键点击你的 Linux 分区，选择“调整/移动”。调整它的大小，以使用你硬盘上的自由空间。</li>
<li>点击工具栏上的“确认所有操作”按钮，执行所选任务。。它可能警告你电脑将无法启动，但对多数 Linux 安装这都不成问题（如果有问题，查阅 <a href="http://gparted.org/display-doc.php?name=help-manual&amp;lang=C#gparted-fix-grub-boot-problem" rel="nofollow">此文</a>以修复)。可能要耗些时间，没办法。</li>
</ol>
<p>结束之后，你的硬盘里应该就只剩下Linux系统了。这时启动菜单中还会有些Windows系统痕迹，不删掉也没什么影响，但如果你想把这些痕迹清除，你可以在Linux中启动“终端”，并运行：</p>
<pre>sudo update-grub</pre>
<p>这样就可以把Windows的痕迹清除了。</p>
<p>就像我们说过的那样，每一个双系统都“跟雪花一样美丽且唯一”，所以在不同的环境下操作步骤也不尽然与我们提到的这几种一模一样，你需要酌情处理。希望我们的文章能够给你带去帮助。</p>
<p>结语：一定一定不要忘记先备份，祝你好运！</p>
<h4>相关文章</h4>
<ul>
<li><a href="http://blog.jobbole.com/34764/">王垠：漫谈Linux、Windows和Mac</a></li>
<li><a href="http://blog.jobbole.com/29546/">Ubuntu桌面生存指南(1)：选择 Linux</a></li>
<li><a href="http://blog.jobbole.com/22838/">流行Linux和Windows脚本语言列表</a></li>
<li><a href="http://blog.jobbole.com/1574/">受够了Linux服务器 重返Windows</a></li>
<li><a href="http://blog.jobbole.com/26741/">王垠：从工具的奴隶到工具的主人</a></li>
<li><a href="http://blog.jobbole.com/29668/">Ubuntu桌面生存指南(2)：Ubuntu桌面体验简介</a></li>
<li><a href="http://blog.jobbole.com/27956/">Linux 灾难恢复</a></li>
<li><a href="http://blog.jobbole.com/1215/">计算机的10大经典错误</a></li>
<li><a href="http://blog.jobbole.com/9597/">开发人员为什么需要Mac？</a></li>
<li><a href="http://blog.jobbole.com/1511/">Windows 8将带来什么</a></li>
</ul>
<p><a href="http://blog.jobbole.com/40278/">如何在安装双启动后卸载 Windows 或者 Linux</a>，首发于<a href="http://blog.jobbole.com">博客 - 伯乐在线</a>。</p>