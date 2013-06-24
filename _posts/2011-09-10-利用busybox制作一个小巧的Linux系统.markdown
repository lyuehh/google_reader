---
layout: post
title:  "利用busybox制作一个小巧的Linux系统"
date:   2011-09-10 19:15:17
author: Richard
categories: program
---

## 利用busybox制作一个小巧的Linux系统
### by Richard
### at 2011-09-10 19:15:17
### original <http://wowubuntu.com/busybox-make-a-small-linux.html>

<p>1. 下载busybox和linux kernel的源码.</p>
<p>busybox的源码地址: <a href="http://www.linuxidc.com/Linux/2011-08/40704.htm">http://www.linuxidc.com/Linux/2011-08/40704.htm</a><br>
linux kernel的源码地址:<a href="http://www.kernel.org/pub/linux/kernel/v2.6/"> http://www.kernel.org/pub/linux/kernel/v2.6/</a><span></span></p>
<p>我选择的busybox版本是: busybox-1.16.0.tar.bz2</p>
<p>linux kernel的版本是: linux-2.6.28.tar.bz2</p>
<p>2. 接下来我们先编译linux内核</p>
<p>我将下载下来的内核源代码压缩包拷贝到/usr/src目录下，然后进入到这个目录将其解压,命令如下:</p>
<blockquote><p># tar xjf linux-2.6.28.tar.bz2</p></blockquote>
<p>然后创建一个目录，用来保存编译内核产生的目标文件</p>
<blockquote><p># pwd</p>
<p>/usr/src</p>
<p># mkdir linux-2.6.28-obj</p></blockquote>
<p>linux-2.6.28-obj现在是一个空目录，编译内核时会将目标文件输出保存到这个目录下。</p>
<p>linux-2.6.28 是刚才linux-2.6.28.tar.bz2文件解压出来的目录。</p>
<p>然后我们开始编译linux内核,输入如下所示的命令:</p>
<blockquote><p># cd /usr/src/linux-2.6.28 (进入到内核源码树目录)</p>
<p># make O=/usr/src/linux-2.6.28-obj menuconfig (配置内核)</p></blockquote>
<p>配置内核时，里面的选项有很多,如果不确定的话就将所有选项都编译进内核，当然最好能针对性的配置内核，这样产生出的内核镜像不至于太大。还有一点就是配置时一定要将选定的选项编译进内核，而不要编译成模块。同时，为了支持initrd内存盘文件系统，有两个选项是必须的。</p>
<p>一个是General Setup –&gt; Initial RAM filesystem and RAM disk support</p>
<p>另一个是 Device Drivers –&gt; Block Devices –&gt; RAM block device support</p>
<p>这个选项的子选项保持默认就可以了，如下图所示:</p>
<p>然后退出配置界面，在退出时会提示你是否保存刚才的配置，选择yes就可以了(因为我们在配置时指明了O=/usr/src/linux-2.6.28-obj 目录，所以配置文件会保存到这个目录下，文件名为.config)</p>
<p>接下来我们开始编译内核:</p>
<blockquote><p># make O=/usr/src/linux-2.6.28-obj (生成内核镜像和模块)</p></blockquote>
<p>通常，我们编译内核是为了更新内核，但这里我们只是为了编译出一个内核镜像，所以就不调用make install命令来安装内核了。</p>
<p>好！内核编译完成，我们将编译好的内核镜像拷贝到主目录下，以供后面使用。</p>
<blockquote><p># cp /usr/src/linux-2.6.28-obj/arch/x86/boot/bzImage ~ (拷贝内核镜像到root用户的主目录下)</p></blockquote>
<p>3.编译busybox</p>
<p>接下来我们开始编译busybox。（我的busybox-1.16.0.tar.bz2存放到了/root目录下）</p>
<blockquote><p># tar xf busybox-1.16.0.tar.bz2 (解压busybox压缩包)</p>
<p># cd busybox-1.16.0 (进入到解压后的busybox源码目录)</p>
<p># make menuconfig (配置busybox)</p></blockquote>
<p>注意配置时，一定要选择静态链接选项，该选项位于:</p>
<p>Busybox Settings –&gt; Build Options –&gt; Build Busybox as a static binary</p>
<p>接下来，我们安装busybox</p>
<blockquote><p># make install (busybox默认安装到了其源码树目录的名字为_install的目录中)</p>
<p># cd _install (进入安装了busybox的目录)</p></blockquote>
<p>当我们进入了busybox后发现了熟悉的linux目录结构，但只有这些是不够的还需要手工添加一些基本的配置文件。</p>
<p>4. 在busybox中添加配置文件并生成initrd镜像</p>
<p>这时，我们处在/root/busybox-1.16.0/_install 目录下。</p>
<p>好了，开始我们的配置~</p>
<blockquote><p># mkdir proc sys etc dev (创建四个空目录，linux内核需要)</p>
<p># cd dev</p>
<p># mknod console c 5 1 (创建一个控制台字符设备文件)<br>
# mknod null c 1 3 (创建一个0设备文件)</p>
<p># cd ..</p>
<p># cd etc</p>
<p># vim fstab (输入如下图内容)</p>
<p># mkdir init.d</p>
<p># vim init.d/rcS (输入如下内容)</p>
<p># chmod +x init.d/rcS (给rcS文件加上可执行权限)</p>
<p># vim inittab (输入如下内容)</p>
<p># cd ..</p>
<p># pwd (打印当前目录)</p>
<p>/root/busybox-1.16.0/_install</p></blockquote>
<p>此时表明我们处在busybox安装文件的根目录下</p>
<blockquote><p># rm linuxrc (删除linuxrc链接文件)</p></blockquote>
<p>然后新创建一个指向busybox文件的链接文件我们输入如下图所示命令来创建initrd镜像.</p>
<blockquote><p># cd ..</p>
<p># cp initrd.gz ~ (将其拷贝到主目录)</p></blockquote>
<p>至此我们就得到了两个镜像文件：</p>
<p>bzImage : linux内核镜像文件</p>
<p>initrd.gz : 内存盘根文件系统镜像文件</p>
<p>好！接下来我们在一个grub引导器下，来引导这个系统：</p>
<p>嗯。为了方便起见，我将生成的这两个文件拷贝到了/boot下</p>
<p>只要在grub提示符下输入如下图所示的三个命令，你的mini linux就能引导开了~~</p>
<p> </p>
<p><img src="http:///home/liuxiaochun/Desktop/110817071960422.png" alt=""></p>
# 本文采用<a href="http://creativecommons.org/licenses/by/2.5/cn/">CC协议</a>进行授权，转载本文请注明<a href="http://wowubuntu.com/busybox-make-a-small-linux.html" title="Permalink">本文链接<a>。/  23  条留言
<hr>
- <a href="http://twitter.com/ubuntu_tips">Twitter</a> 
/ <a href="https://www.google.com/profiles/wowubuntucom">Buzz</a>
/ <a href="http://t.sina.com.cn/utips">微博</a> 
/ <a href="http://ask.wowubuntu.com">问答</a> 
/ <a href="http://wowubuntu.com/submit">投稿</a>
/ <a href="http://wowubuntu.com/join">加入我们</a> wow0slx6bcs721xo1udcc<br>- 高性价比 <a href="http://wowubuntu.com/vps.html">Ubuntu VPS</a> / 本站架设于<a href="http://www.photonvps.com/billing/aff.php?aff=129"> PhotonVPS </a> / 定制 <a href="http://tto.me/kfstee">Ubuntu T-Shirt</a><table cellspacing="0" cellpadding="2" border="0" width="100%" style="clear:both">
    
    <tr>
        <td><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwowubuntu.com%2Flinux-20-year.html&amp;from=http%3A%2F%2Fwowubuntu.com%2Fbusybox-make-a-small-linux.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:13px!important">庆祝 Linux 20 周岁</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwowubuntu.com%2Flinux3.html&amp;from=http%3A%2F%2Fwowubuntu.com%2Fbusybox-make-a-small-linux.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:13px!important">Linux Kernel 3.0, 不会有重大变化</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwowubuntu.com%2Flinux-20.html&amp;from=http%3A%2F%2Fwowubuntu.com%2Fbusybox-make-a-small-linux.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:13px!important">Linux 20周年开发者庆典活动!</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwowubuntu.com%2Flinux_zfs.html&amp;from=http%3A%2F%2Fwowubuntu.com%2Fbusybox-make-a-small-linux.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:13px!important">Linux 上的原生 ZFS 文件系统模块将于下月发布</font>
                    </a>
                </td>
            </tr>
            <tr>
                <td style="margin:0!important;padding:0!important;line-height:20px!important">
                    <img border="0" src="http://static.wumii.com/images/widget/widget_solidPoint.gif">
                    <a style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fwowubuntu.com%2F5linux-news-site.html&amp;from=http%3A%2F%2Fwowubuntu.com%2Fbusybox-make-a-small-linux.html">
                        <font size="-1" color="#333333" style="line-height:1.65em;font-size:13px!important">推荐 5 个Linux新闻网站</font>
                    </a>
                </td>
            </tr>
    
    <tr>
        <td align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table></a></a>