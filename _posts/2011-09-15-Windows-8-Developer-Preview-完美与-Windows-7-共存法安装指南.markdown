---
layout: post
title:  "Windows 8 Developer Preview 完美与 Windows 7 共存法安装指南"
date:   2011-09-15 18:20:33
author: 
categories: program
---

## Windows 8 Developer Preview 完美与 Windows 7 共存法安装指南
### by 
### at 2011-09-15 18:20:33
### original <http://www.cnbeta.com/articles/155176.htm>

<div><a rel="nofollow" href="http://www.cnbeta.com/topics/480.htm"><img src="http://img.cnbeta.com/topics/win7.gif" alt="Windows 8" name="sign" align="right"></a>
        <p><span style="font-weight:bold">感谢Bob Bao的投递</span><br>
9月13号，微软的Build大会上发布了 Windows 8 Developer Preview 确实让人激动了一把，可是第二天，广大开发人员就面临了如何安装的问题。微软明确说明这个版本只适用于开发者，所以请那些喜欢看电影聊天的还是不要安装为好，对你们没什么用处。还有微软对于这个版本说实话，制作的也是稍加简陋，在第二天下载好，想通过虚拟机安装，我就遇到了很多问题，主要有以下几个：</p>
		<p>1.CPU不支持 64位，只能玩32位版本<br>
2.CPU没有开启虚拟机支持模式 (VT)<br>
3.几个常用虚拟机软件中只有 VMWare 8 ， VirtualBox 和 Hyper-V 支持Windows 8 DP版，其他
如 VMWare 6/7, VirtualPC 都不支持，Windows 8 安装时都报了 HAL_ERROR 硬件不兼容。（如果有条件的话，还
是使用虚拟机，最好是部属在 Hyper-V 上。）<br>
<p>而且在VirtualBox安装了之后，Windows 8 DP不一定能够正确识别你的硬件，（我就碰到了不识别网络适配器，没网那还玩个啥开发）</p>
<p>所以，我就在寻思是否可以实现和现有Win 7操作系统共存的模式，做成双引导系统。</p>
<p>国外有个大牛在昨天已经实现，不妨参考下：<a rel="nofollow" title="http://www.hanselman.com/blog/GuideToInstallingAndBootingWindows8DeveloperPreviewOffAVHDVirtualHardDisk.aspx" href="http://www.hanselman.com/blog/GuideToInstallingAndBootingWindows8DeveloperPreviewOffAVHDVirtualHardDisk.aspx"><span style="color:#0000ff">http://www.hanselman.com/blog/GuideToInstallingAndBootingWindows8DeveloperPreviewOffAVHDVirtualHardDisk.aspx</span></a><span style="color:#0000ff"> </span>将Win8安装在VHD上，然后直接引导启动VHD中的Windows 
8实现双系统，这样好处是那天我们不要Win8了，直接删除那个VHD文件即可。而且Win8不会改变现有的分区，做到绝对安全。</p>
<p>不过我还是给出一个比较简单人性化的步骤：</p>
<ol><li>准备安装文件： 准备一个大于5GB的移动硬盘或者U盘 （如果安装没有开发工具的版本，4GB左右就够了），如果是U盘的话，下载<strong> </strong><a rel="nofollow" href="http://www.microsoftstore.com/store/msstore/html/pbPage.Help_Win7_usbdvd_dwnTool"><strong>Windows 
7 USB/DVD download tool</strong></a>工作将Windows 8 DP 
ISO做成一个可引导启动盘。如果是移动硬盘的话，需要如下设置：<ul><li>（我选择这种方案）保证硬盘里有个分区有足够大的空间，或者独立分一块5-10GB的空间出来。然后在Windows 系统管理里面，磁盘管理中将这个分区设置为 
激活 状态：</li>
<li>
<p><img src="http://img.cnbeta.com/newsimg/110915/18203301339697619.png" alt=""></p>
</li>
<li>然后将ISO用虚拟光驱读取，将里面的所有文件都拷贝到这个分区（包括隐藏文件）</li>
<li>或者我们也可以准备一个DVD9的盘，刻录（不推荐，国内不好买到，而且买到也不一定好用）</li>
<li>或者我们还可以通过网络，本地磁盘共享等，在通过一个启动盘引导然后访问运行Winload.exe 我没测过，不一定好试。</li>
</ul>
</li>
<li>开始安装： 首先检查你的电脑主板是否支持USB设备引导，如果是，在开机的时候选择通过USB引导，你就可以顺利进入Windows 8 DP安装界面。 <span style="color:#ff0000">注：如果你无法正常进入，比如他报告HAL错误等，那只能恭喜了，你的硬件太前卫了，他不支持。</span></li>
<li>在你顺利进入到选择分区的时候，你需要按Shift + F10 调出命令行，OK这个时候我们就需要创建VHD (<a rel="nofollow" href="http://en.wikipedia.org/wiki/VHD_%28file_format%29">Virtual Hard Disk</a>) 
和挂载VHD了。<ul><li>用命令行DIR等先在已有的分区上找一个足够大的分区，个人建议找一个还有50GB空闲空间以上的分区，记住他的盘符，比如D:盘</li>
<li>进入DISKPART工具，命令行输入 DISKPART</li>
<li>用命令 create vdisk file=&quot;d:\win8.vhd&quot; type=expandable maximum=50000 
在D:盘下创建一个最大空间 50GB 动态增加的VHD文件 （当然也可以用固定大小的 type=fixed）。<span style="color:#ff0000">注
意：不要以为他是动态增加空间，我就能在只剩小于50GB的分区中创建它，如过这样，虽然一般的虚拟机是可以读取引导这个VHD里的系统或者文件的，但是
直接通过物理机器引导启动还是不行的，因为引导启动时她要根据VHD的设置来创建一个逻辑分区，所以如果你已经不足50GB了，而他还是要占用50GB的
空间时，就会报空间不足的错误。所以千万要记住，一定选择一个空间足够多的分区，然后设置一个合理的最大值</span><span style="color:#ff0000">。</span></li>
<li>
<p><img src="http://img.cnbeta.com/newsimg/110915/1820341862120681.png" alt=""></p>
</li>
<li>然后选择这个VHD文件并且挂载他：</li>
<li>
<p><img src="http://img.cnbeta.com/newsimg/110915/18203421001529194.png" alt=""></p>
</li>
<li>在选择安装分区的界面中点击 refresh 刷新下，找到你刚才挂载的分区，确认大小后，选择它，下一步开始正式安装</li>
<li>上述创建VHD步骤也可以在你已有Win7中 磁盘管理里面做，菜单，动作，创建VHD</li>
<li>
<p><img src="http://img.cnbeta.com/newsimg/110915/18203431627107219.png" alt=""></p>
</li>
</ul>
</li>
<li>重启，安装成功，正确引导： Windows 8 安装好之后，他会生成自己的Boot Manager，而这个Boot 
Manager是支持直接在VHD引导Win8的，所以你应该可以看到下面的界面：<ul><li>
<p><img src="http://img.cnbeta.com/newsimg/110915/18203442058842376.png" alt=""></p>
选择你要进入的系统即可。</li>
</ul>
</li>
<li>如果你熟悉<a rel="nofollow" href="http://technet.microsoft.com/zh-cn/library/cc709667%28v=ws.10%29.aspx"><strong>BCDEdit</strong></a>的话，你也可以自己修改Boot 
Manager内容或者用Windows 7 的Boot Manager。这里推荐<a rel="nofollow" href="http://en.wikipedia.org/wiki/EasyBCD">EasyBCD</a>工具，有可视化界面操作简单，比命令行的 
bcdedit 直观很多。<span style="color:#ff0000">（建议熟悉的同学操作，不然很容易弄得进不了系统）</span></li>
</ol>
<p>好了，大致就简单这几步，整个安装过程大概20分钟不到，最后看下登陆后我的截图：</p>
<p><img style="width:567px;height:355px" src="http://img.cnbeta.com/newsimg/110915/18203451995833363.png" alt=""></p>
<p><img style="width:568px;height:355px" src="http://img.cnbeta.com/newsimg/110915/18203462090571087.png" alt=""></p>
<p><img style="width:572px;height:359px" src="http://img.cnbeta.com/newsimg/110915/18203571137717270.png" alt=""></p>
<p>附:我在Win 8 DP 上安了Office 2010 目前看来很稳定，Windows 的一脉相承做的还算地道。不过真也说明一点， Win 
8也就是和Win 7一样的核子加了点东西。主要还是关注与他的Metro Style的开发。</p></p></div>