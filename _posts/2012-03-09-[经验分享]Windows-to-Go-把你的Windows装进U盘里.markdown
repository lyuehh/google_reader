---
layout: post
title:  "[经验分享]Windows to Go 把你的Windows装进U盘里"
date:   2012-03-09 10:05:40
author: 
categories: program
---

## [经验分享]Windows to Go 把你的Windows装进U盘里
### by 
### at 2012-03-09 10:05:40
### original <http://www.cnbeta.com/articles/176326.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/480.htm"><img src="http://img.cnbeta.com/topics/win8.png" alt="Windows 8" name="sign" align="right"></a>
        <p><b>感谢<a rel="nofollow" href="http://bbs.win7party.com">Windows 爬梯</a>的投递</b><br>
在看完Windows to Go的展示后，作者通过两台笔记本一台台式机外加一块500G的移动硬盘作为平台。亲自上阵测试Windows to Go，下面把具体流程以及安装经验给大家一起分享一下。</p>
		<p><p align="left">安装Windows to go的先决条件：</p>
<p align="left">1、容量至少为32GB的U盘一枚（或者硬盘）<br>
2、安装好Windows 8的PC</p>
<p align="left">3、Windows 8DVD ISO文件<br>
各个版本的windows8 ISO<a rel="nofollow" href="http://windows.microsoft.com/zh-CN/windows-8/iso">下载</a><br>
4、Imagex.exe （安装 Windows 7Automated Installation Kit之后，可以从C:\Program Files\Windows AIK\Tools\amd64 或者 C:\ProgramFiles\Windows AIK\Tools\x86中找到该文件）</p>
<p align="left">Windows® 自动安装工具包 (AIK)<a rel="nofollow" href="http://www.microsoft.com/downloads/zh-cn/details.aspx?familyid=696dd665-9f76-4177-a811-39c26d3b3b34&amp;displaylang=zh-cn">下载</a></p>
<p align="left">5、建立Windows To Go USB 驱动设备：</p>
<p align="left">亲们，Windows 8支持虚拟光驱的哦</p>
<p align="left">首先插入你的U盘或者硬盘，当然这个是屁话，笔者我这儿有一块500G的移动硬盘，我们需要对硬盘做一些处理，可以通过磁盘管理对硬盘删除，重建分区。</p>
<p align="left">当然我们也可以在命令提示符下对磁盘进行操作，</p>
<p align="left">一、对磁盘进行重新分区操作（其实也可以在磁盘管理下直接对磁盘进行操作，具体方法我们的文章就不细说了，详见：<a rel="nofollow" href="http://bbs.win7party.com/showtopic-132.aspx">爬梯链接</a> 但是如果你是帮妹子做windows to go，你们懂的）</p>
<p align="left">1)右键电脑左下角，选择命令提示符（管理员），用户账户控制选择是。<br>
2)在命令提示符（管理员）下输入：diskpart进入磁盘管理<br>
3)输入listdisk显示磁盘列表，方便在接下来的安装中选择磁盘<br>
4)输入select disk #选择磁盘（以读者为例，#为1，n那么就是select disk 1）<br>
5)输入clean删除磁盘内容（包括文件格式，分区内容）<br>
<img src="http://img.cnbeta.com/newsimg/120309/1029050620483697.jpg"></p>
<p align="left">6)输入create partition primary创建新分区。<br>
7)输入for matfs = ntfs quick格式化分区。<br>
8)输入active<br>
将分区定义为活动分区。<br>
9)输入exit<br>
退出diskpart模式。<br>
<img src="http://img.cnbeta.com/newsimg/120309/1029051539422750.jpg"></p>
<p align="left">二、用虚拟光驱打开下载好的Windows® 自动安装工具包，双击start CD，安装AIK，并指定路径，在这儿笔者的安装路径为：D:\ProgramFiles\Windows AIK，等待安装完成。</p>
<p align="left">找到imagex.exe文件，笔者的在D:\Program Files\WindowsAIK\Tools\x86路径下64位操作系统将后面的X86变为X64.</p>
<p align="left">三、将下载好的用虚拟光驱Windows 8ISO文件打开（亲们，Windows 8支持虚拟光驱的哦，当然你也可以直接解压文件），选择X\source\路径下的install.wim文件（X代表win 8 ISO虚拟光驱盘符，笔者为J盘）将这个文件复制到imagex.exe文件所在的路径下 （笔者的就是这个啦D:\ProgramFiles\Windows AIK\Tools\x86）<br>
<img src="http://img.cnbeta.com/newsimg/120309/1029052428028527.jpg"></p>
<p align="left">四、安装操作。</p>
<p align="left">1、首先将我们的硬盘划分一个盘符吧，右键左下角，选择磁盘管理。右键自己的磁盘，选择更改驱动号和路径，选择添加，选择一个盘符，然后点击确定。<br>
<img style="width:600px;height:474px" src="http://img.cnbeta.com/newsimg/120309/10290631143960555.jpg"></p>
<br>
<img src="http://img.cnbeta.com/newsimg/120309/10290641376193515.jpg"><br>
<img src="http://img.cnbeta.com/newsimg/120309/10290751382487374.jpg"><p align="left">2、返回到命令提示符操作，如果关闭了可以再打开<br>
1)输入d:进入D盘操作。<br>
2)输入cd D:\ProgramFiles\Windows AIK\Tools\x86进入路径。<br>
3)输入imagex.exe/apply install.wim 1 X:\开始安装，（X用自己的移动磁盘盘符代替，笔者的移动磁盘盘符为：L，所以我的命令为：imagex.exe /apply install.wim 1 l:\），下面我们开始“漫长“的等待吧。漫长=11分24秒<br>
4)再经过“漫长“的等待之后，我们输入bcdboot.exe x:\windows/s x:（X用自己的移动磁盘盘符代替，笔者的移动磁盘盘符为：L，所以我的命令为：bcdboot.exe l:\windows /s l:）写入开机引导文件。<br>
<img src="http://img.cnbeta.com/newsimg/120309/1029076938283260.jpg"></p>
<p align="left">五、重启你的电脑<br>
1、你们以为这样就完了么？木有，还木有完。如果你只有一台电脑，而你的电脑有时候Windows 8系统，请记得重启你的计算机，记住，是重启，不是关机，因为Windows 8的混合睡眠模式会让你开机的时候没有BOIS开机启动选项，重启选择USB启动，这就像你第一次安装电脑一样了。<br>
<img src="http://img.cnbeta.com/newsimg/120309/1029087154348195.jpg"></p>
<p align="left">2、在你第一次重启的时候你也许会惊愕的发现不能USB启动了，肿么办？没事重复4.2.4的步骤，重新写入开机引导文件。然后再重启计算机。<br>
<img src="http://img.cnbeta.com/newsimg/120309/10290881649025926.jpg"></p>
<br>
3、再次选择USB启动，这次系统需要输入序列许可号（当然，这一步是可以跳过的），设置计算机名，账户等一些个性化操作。<p align="left">4、好了，链接好互联网，用我们的Windows live 登陆，这时候你会发现你之前在电脑上的各种个性化设置都同步到你的“新“电脑上了，包括网页历史浏览，当然你可以在设置的时候去掉这一项。<br>
<img src="http://img.cnbeta.com/newsimg/120309/1029099535812265.jpg" width="750"></p>
<br>
<img src="http://img.cnbeta.com/newsimg/120309/10290910618183562.png" width="750"><br>
<img src="http://img.cnbeta.com/newsimg/120309/102909111675669511.png" width="750"><br>
<img src="http://img.cnbeta.com/newsimg/120309/102910121150748806.png" width="750"><br>
<img src="http://img.cnbeta.com/newsimg/120309/102910131718999149.png" width="750"><p align="left">5、下面，你的新电脑是需要安装驱动的，你的知道这一点，如果你在家里的电脑和公司的电脑都驱动装好了。现在你就拥有了一个windows to go了。<br>
6、带着你的硬盘或者U盘去其他电脑上得瑟吧。<br>
<img src="http://img.cnbeta.com/newsimg/120309/10291114938983077.jpg"></p>
<br>
<img src="http://img.cnbeta.com/newsimg/120309/102911151095614872.jpg"><br>
六、记住，以后挤公交带一块硬盘吧，或者U盘，秒杀各种笔记本、超级本、还有上网本。<br>
七、前景：Windows 8原生态支持USB3.0技术，速度是USB2.0的10倍，如果你配上一块基于USB3.0的固态硬盘（遇到坏人还可以当板砖使。），那就真的爽爆了。没有移动硬盘或者32G以上U盘的同学建议去正规的商家（比如亚马逊或者京东）买一块，支持7天无条件退货的那种，原因嘛，你们懂的。<br>
<img src="http://img.cnbeta.com/newsimg/120309/102912161868878584.jpg"><br>
附：主要命令行<br>
diskpart:进入磁盘管理<br>
list disk:显示磁盘列表<br>
select disk #：选择磁盘（#代表磁盘号（数字））<br>
clean：删除磁盘内容<br>
create partition primary：创建新分区<br>
format fs=ntfs quick：格式化分区<br>
active：活动分区<br>
exit：D：进入D盘目录<br>
cd D:\Program Files\Windows AIK\Tools\x86:指定路径<br>
imagex.exe /apply install.wim 1 x:\：安装到X盘<br>
bcdboot x:\windows /s X:在X盘上写入引导文件</p></div>