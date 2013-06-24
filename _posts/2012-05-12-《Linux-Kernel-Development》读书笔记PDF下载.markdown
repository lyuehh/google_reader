---
layout: post
title:  "《Linux Kernel Development》读书笔记PDF下载"
date:   2012-05-12 14:22:53
author: 
categories: program
---

## 《Linux Kernel Development》读书笔记PDF下载
### by 
### at 2012-05-12 14:22:53
### original <http://simple-is-better.com/news/866>

<p>
	这个读书笔记目录系列尝试以PDF文件的形式提供，也是我的LaTeX初学成果。</p>
<p>
	<a href="http://www.kuaipan.cn/file/id_7640343092660687.html">《Linux Kernel Development》读书笔记1</a></p>
<p>
	第1章：Linux内核概览<br>
	第2章：内核概览</p>
<h1 align="center">
	Linux Kernel Development读书笔记</h1>
<h3 align="center">
	mdyang <a href="javascript:void(0);" name="tthFrefAAB"></a><sup>1</sup> <a href="javascript:void(0);" name="tthFrefAAC"></a><sup>2</sup></h3>
<div>
	 </div>
<p>
	本文档是《Linux Kernel Development》英文版第三版的读书笔记。</p>
<div>
	 </div>
<p>
	目录：</p>
<div>
	 </div>
<ol>
	<li>
		Introduction to the Linux Kernel (Linux内核概览)
		<div>
			 </div>
	</li>
	<li>
		Getting Started with the Kernel (内核入门)
		<div>
			 </div>
	</li>
	<li>
		Process Management (进程管理)
		<div>
			 </div>
	</li>
	<li>
		Process Scheduling (进程调度)
		<div>
			 </div>
	</li>
	<li>
		System Calls (系统调用)
		<div>
			 </div>
	</li>
	<li>
		Kernel Data Structures (内核数据结构)
		<div>
			 </div>
	</li>
	<li>
		Interrupts and Interrupt Handlers (中断与中断处理器)
		<div>
			 </div>
	</li>
	<li>
		Bottom Halves and Deferring Work (下半部和推后执行的工作)
		<div>
			 </div>
	</li>
	<li>
		An Introduction to Kernel Synchronization (内核同步概览)
		<div>
			 </div>
	</li>
	<li>
		Kernel Synchronization Methods (内核同步方法)
		<div>
			 </div>
	</li>
	<li>
		Timers and Time Management (定时器与时间管理)
		<div>
			 </div>
	</li>
	<li>
		Memory Management (内存管理)
		<div>
			 </div>
	</li>
	<li>
		The Virtual Filesystem (虚拟文件系统)
		<div>
			 </div>
	</li>
	<li>
		The Block I/O Layer (块I/O层)
		<div>
			 </div>
	</li>
	<li>
		The Process Address Space (进程地址空间)
		<div>
			 </div>
	</li>
	<li>
		The Page Cache and Page Writeback (页缓存与页写回)
		<div>
			 </div>
	</li>
	<li>
		Devices and Modules (设备与模块)
		<div>
			 </div>
	</li>
	<li>
		Debugging (调试)
		<div>
			 </div>
	</li>
	<li>
		Portability (可移植性)
		<div>
			 </div>
	</li>
	<li>
		Patches, Hacking, and the Community (补丁、技巧与Linux社区)</li>
</ol>
<div>
	 </div>
<p>
	<strong>Chapter 1 <br>
	第1章：Linux内核概览</strong></p>
<p>
	<strong>1.1 Linux的历史</strong></p>
<p>
	<strong>1.1.1 Unix的历史</strong></p>
<p>
	Unix由Dennis Ritchie和Ken Thompson在1969年创造。Unix的前身是Multics，一个失败的多用户操作系统。1969年，贝尔实验室启动了一个文件系统项目，最终演化为Unix，Unix最早在PDP-7系统上运行，后来被移植到PDP-11。1973年，Unix被使用C语言重写。</p>
<p>
	最早广为使用的Unix系统是Unix第六版，简称V6。除贝尔实验室外也有一些其他公司开发了自家的Unix，1977年贝尔实验室将这些分支系统整合后发布了Unix System III。1982年，AT&amp;T发布了System V。</p>
<p>
	众多Unix开发者中数UC Berkeley的开发者最有影响力，这些发行被统称为BSD（Berkeley Software Distribution）。BSD的第一个发布1BSD发布于1977年，第二年发布了2BSD。这两版都是对贝尔版Unix再打包并添加一些应用软件后发布的。首个完全独立开发的BSD Unix是3BSD，发布于1979年。现在已经有了Darwin、FreeBSD、NetBSD、OpenBSD等多个BSD Unix版本。</p>
<p>
	Unix的设计初衷是设计一个简单、强大、鲁棒的操作系统，这主要体现在以下几点：</p>
<ol>
	<li>
		与当时的操作系统动辄就有上千个系统调用相比，Unix只有数百个系统调用</li>
	<li>
		Unix的思想是“一切都是文件”，简化了对数据的操作。无论是读写文件还是对I/O设备进行操作，都可以使用诸如<tt>write()</tt>、<tt>lseek()</tt>和<tt>close()</tt>这样的函数完成</li>
	<li>
		Unix使用C写成，具有良好的可移植性</li>
	<li>
		Unix创建进程的性能极好，系统调用<tt>fork()</tt>也是独一无二的</li>
	<li>
		Unix具有简单、鲁棒的IPC（Inter-Process Communication，进程间通信）机制</li>
</ol>
<p>
	今天的Unix系统已经是一个抢占式的（preemptive）多任务操作系统，支持多线程、虚拟内存，请求调页（demand paging）、动态共享库与TCP/IP网络连接等现代操作系统特性。</p>
<p>
	<strong>1.1.2 Linux的历史</strong></p>
<p>
	Linux是芬兰的Linus Torvalds在1991年写成的一个基于80386架构的操作系统。那时Linus是赫尔辛基大学的一名学生，由于当时缺乏为个人电脑设计的操作系统，他便自己编写了一个操作系统，称为Linux. 今天，Linux已经能够在Alpha、ARM、PowerPC、SPARC、x86-64以及很多其它架构的硬件上运行，从有房间那么大的巨型机到嵌入式设备都能见到Linux的身影。</p>
<p>
	Linux是一个类Unix（Unix-like）的系统，这意味着Linux实现了Unix具备的很多特性（例如Linux实现了Unix API），但与其它Unix系统不同，Linux并不是基于Unix代码实现的。</p>
<p>
	Linux的另一个显著特性是它并不由某个人或某家公司控制，Linux是社区协作的产物。虽然创始人Linus依然被尊为Linux的创造者，并直到现在依然在维护Linux内核代码，但整个Linux内核的开发与更新工作是由整个社区推动的。Linux内核在GPL General Public License (GPL) v2.0许可证下发布。因此，任何人都可以下载Linux的内核源代码，并且进行修改。但前提是经过修改的代码也必须遵守GPL v2.0的条款。</p>
<p>
	<strong>1.1.3 Linux内核概览</strong></p>
<p>
	Linux操作系统是指是系统运行起来所需的最基本的程序和工具的集合，包括内核、设备驱动、引导加载器（boot loader）、命令行Shell以及其他用户界面、文件系统和系统工具。其中用户界面是最外围的部分，直接与用户交互，而内核则是最核心的部分。</p>
<p>
	Linux中有用户态（User Mode）和内核态（Kernel Mode）的区分。内核程序是在内核态下执行的，用户程序是在用户态下执行的，在这两个状态之间转换需要进行上下文切换（Context Switch）。用户态下的程序使用系统提供的功能是通过系统调用（System Call）实现的。比如对于库函数<tt>printf()</tt>，它进行的操作是将输出数据格式化写入缓冲区后，调用系统调用<tt>write()</tt>输出缓冲区内容到控制台。当应用程序调用系统调用时，我们说内核正在代表应用执行；或是说应用程序正在内核态下执行系统调用，内核在进程上下文中运行。</p>
<p>
	内核也负责系统硬件的管理。几乎所有的架构中都使用<em>中断</em>这个概念处理外部硬件请求（例如键盘敲击）。Linux内核负责管理中断请求，并调用中断处理程序（handler）处理中断请求。</p>
<p>
	<strong>1.1.4 Linux内核与传统Unix内核的对比</strong></p>
<p>
	<strong>单内核（Monolithic Kernel）与微内核（Microkernel）的比较</strong></p>
<p>
	单内核是指将内核的所有功能实现为一个整体的二进制可执行镜像（image），在同一个进程空间内执行。</p>
<p>
	微内核则反其道行之，将不同的功能实现为不同的模块，在不同的进程空间内执行。Windows NT和Mach（即Mac OS X）都是微内核的代表。这种设计使得需要使用IPC机制处理这些进程之间的通信。</p>
<p>
	Linux是单内核的操作系统，但与Unix不同，Linux从微内核的思想中借鉴一些东西，将不同的系统功能分别编译为不同的共享库，这些库可以在调用请求发生时被动态加载进内核的进程空间。</p>
<p>
	以下是Linux与Unix相比不同的地方：</p>
<ul>
	<li>
		Linux支持内核模块的动态链接</li>
	<li>
		Linux支持SMP（Symmetrical Multiprocessor）</li>
	<li>
		Linux内核是抢占式的</li>
	<li>
		Linux并不区分进程和线程，它们都是进程。同一进程内的多个线程被视作共享某些资源的多个进程</li>
	<li>
		Linux提供面向对象的设备模型，可管理设备类，支持热插拔，并通过用户空间设备文件系统（user space device filesystem）管理设备</li>
	<li>
		Linux摒弃了Unix中一些败笔的设计，例如STREAMS</li>
	<li>
		Linux是开放、自由、免费的，你可以免费获得Linux源代码，对其进行修改，甚至还可以将这些修改贡献至社区
		<div>
			 </div>
	</li>
</ul>
<p>
	<strong>Linux内核版本号</strong></p>
<p>
	Linux内核版本号分为四段：2(主版本号).6(次版本号).26(Revision版本号).1(稳定版本号)</p>
<p>
	主版本号代表大的升级，次版本号代表小的升级。次版本号为偶数代表为稳定版，奇数代表开发版。例如1.3经bug修复后发布了2.0稳定版，2.5经稳定后发布了2.6稳定版。</p>
<p>
	到2004年有了一些变化，因为2.6的Linux内核已经包括了现代操作系统所需具备的全部特性，并且稳定、高效。因此决定延长2.6版本的生命周期，推迟2.7版的开发。自此Linux内核版本一直稳定在2.6版，只是更新Revision版本号、稳定版本号。Revision版本号更新代表着相对上一个Revision进行了较多修改，并引入了新特性。稳定版本号升级代表进行了重要bug修复。</p>
<p>
	主要的Linux内核开发社区是<em>Linux Kernel Mailing List</em>（也称为<em>lkml</em>）</p>
<p>
	<strong>Chapter 2 <br>
	第2章：内核入门</strong></p>
<p>
	Linux内核源代码使用<em>Git</em>进行版本管理，因此可通过<tt>git clone</tt>命令获得Linux内核源代码。使用<tt>tar</tt>解压即可安装源码、并阅读、修改。可以使用patch命令对代码进行增量式修改，例如添加特性、版本切换等。需要注意的是由于<tt>/usr/src/linux</tt>下的源代码是供GCC等编译器编译代码时使用的头文件所在的地方，因此对内核代码的修改最好不要在该位置进行。可以在自己的用户文件夹下新建一个目录，并将源码安装至此进行修改、编译。</p>
<p>
	内核编译的配置文件以<tt>CONFIG</tt>开头，为<tt>CONFIG_FEATURE</tt>的格式，例如对于SMP，其config文件为<tt>CONFIG_SMP</tt>。配置通常是二元选择（<em>yes</em>和<em>no</em>）或三元选择（<em>yes</em>、<em>no</em>和<em>module</em>）。对于设定编译为module的模块，该模块会被编译为动态链接库。也有一些选项需要执行数值或是字符串。</p>
<p>
	内核代码中提供了一些编辑配置文件的工具，例如<tt>make config</tt>就启动一个最简单的基于文本的命令行工具，对于每个编译选项提出问题，用户需要对于每个问题选择编译选项（yes, no或是module）。还有一些图形化的编辑工具，例如<tt>make menuconfig</tt>和<tt>make gconfig</tt>等。也可以直接编辑<tt>.config</tt>文件或是使用<tt>make defconfig</tt>应用默认设置。</p>
<p>
	配置好的<tt>.config</tt>文件也可以留到以后版本的Linux源代码中使用，使用前需要执行<tt>make oldconfig</tt>进行验证、升级。</p>
<p>
	配置项<tt>CONFIG_IKCONFIG_PROC</tt>将在编译时把当前的配置文件压缩为<tt>/proc/config.gz</tt>，下次使用时，可以直接使用这个文件：</p>
<pre>
$ zcat /proc/config.gz &gt; .config$ make oldconfig</pre>
<p>
	使用<tt>make</tt>命令编译内核，可以通过<tt>&gt;</tt>将编译过程中的控制台输出导入至文件：</p>
<pre>
$ make &gt; ../detritus</pre>
<p>
	这将把输入导入至文本文件<tt>../detritus</tt>。而如果使用<tt>/dev/null</tt>则可以在事实上关闭控制台输出（<tt>/dev/null</tt>吞噬任何对它的输入）：</p>
<pre>
$ make &gt; /dev/null</pre>
<p>
	出于编译模块间依赖性的考虑<tt>make</tt>默认每次只处理一个模块的变异任务，否则可能发生大量编译错误。而Linux内核源代码的依赖性处理得很好，因此可以进行并行编译，使用<tt>make -jn</tt>同时编译<tt>n</tt>个模块。例如使用<tt>make -j32</tt>可同时启动32个模块的编译任务。</p>
<p>
	安装编译好的内核根据系统使用的boot loader不同而不同，例如在grub下，将<tt>arch/i386/boot/bzImage</tt>拷贝至<tt>/boot</tt>，重命名为<tt>vmlinuz-</tt><em>version</em>，编辑<tt>/boot/grub/grub.conf</tt>，为新的内核加入启动项。使用LILO的系统则需要编辑<tt>/etc/lilo.conf</tt>。</p>
<p>
	而为内核安装新的模块则很简单，只需要执行<tt>make modules_install</tt>即可。</p>
<p>
	<strong>2.1 内核代码与普通C代码的不同之处</strong></p>
<p>
	写内核代码和写普通应用程序的C代码主要有如下几个不同之处</p>
<ul>
	<li>
		内核代码中不能使用标准C程序库，也不能<tt>#include</tt>标准C库的头文件</li>
	<li>
		内核源代码使用GNU C编写，而不是ANSI C</li>
	<li>
		不像用户代码，内核源代码在执行时没有内存访问保护</li>
	<li>
		内核缺乏对浮点操作的良好支持</li>
	<li>
		对于内核代码，每个进程的栈空间大小是固定不可变的</li>
	<li>
		同步、并发控制是每时每刻都需要仔细处理的问题：抢占式的异步中断处理、SMP支持、多线程无处不在</li>
	<li>
		必须保证可移植性</li>
</ul>
<p>
	<strong>2.1.1 不能使用标准C程序库</strong></p>
<p>
	主要处于两个原因：</p>
<ol>
	<li>
		标准C程序库作为为应用程序代码设计的程序库，运行在内核之上，因此在内核代码中使用标准C程序库陷入了“先有鸡还是先有蛋”的悖论</li>
	<li>
		内核代码需要短小精悍，加入大量的库将会使模块变得臃肿</li>
</ol>
<p>
	但实际上，很多常用的libc库函数都包含在了Linux内核源代码中，例如<tt>lib/string.c</tt>中实现了常用的字符串处理函数，只需在头文件中包含<tt>&lt;linux/string.h&gt;</tt>即可使用它们。</p>
<p>
	Linux源代码中<tt>#include</tt>语句的基准目录是<tt>include/</tt>，例如<tt>#include &lt;linux/inotify.h&gt;</tt>包含的<tt>.h</tt>文件实际位于<tt>include/linux/inotify.h</tt>。</p>
<p>
	<strong>2.1.2 GNU C</strong></p>
<p>
	要编译Linux内核源代码，需要使用gcc 3.2及以上版本，最好是4.4及以上版本。 使用内联函数提高代码执行效率。注意内联函数会增大生成的二进制代码体积。 在<tt>if</tt>语句中使用gcc分支优化指令<tt>likely()</tt>和<tt>unlikely()</tt>，例如对于<tt>if (err) {...}</tt>，由于error并不经常发生，因此可以写作<tt>if(unlikely(error)) {...}</tt>使得gcc能够对其进行分支优化。</p>
<p>
	<strong>2.1.3 无内存访问保护</strong></p>
<p>
	用户态下内存访问越界时，内核将捕获这个错误，向进程发送<tt>SIGSEGV</tt>信号，并杀死进程。但当内核代码发生访问越界时是没有这些保护措施的。因此编写内核代码时需要万分小心。</p>
<p>
	<strong>2.1.4 浮点操作支持不佳</strong></p>
<p>
	用户态下的整点/浮点数运算操作的转换由内核完成，内核本身并不能很好地处理浮点运算操作，因此除非万不得已，不要使用浮点数，进行浮点操作。</p>
<p>
	<strong>2.1.5 固定大小的栈空间</strong></p>
<p>
	在x86架构下，对于32位机栈空间为8KB，64位机则为16KB。</p>
<p>
	<strong>2.1.6 同步和并发控制</strong></p>
<ul>
	<li>
		Linux是抢占式的多任务操作系统，内核代码负责切换执行进程，因此必须处理同步与并发处理</li>
	<li>
		Linux支持SMP，因此内核代码可能在多个处理器上同时执行，因此</li>
	<li>
		中断是异步发生的，程序可能在任何时候被中断，因此</li>
	<li>
		Linux内核本身的执行也可能被抢占，因此</li>
</ul>
<p>
	<strong>2.1.7 可移植性</strong></p>
<p>
	Linux是跨平台的操作系统，因此其源代码必须具有良好的可移植性。一些原则需要注意，例如对于small-endian和big-endian保持中立，对是32/64位平台都能够良好支持，对不同的内存页尺寸都能良好支持。</p>
<hr>
<p>
	<strong>Footnotes:</strong></p>
<p>
	<a href="javascript:void(0);"><sup>1</sup></a><a href="http://www.cnblogs.com/mdyang"><tt>http://www.cnblogs.com/mdyang</tt></a></p>
<p>
	<a href="javascript:void(0);"><sup>2</sup></a><a href="http://www.mdyang.com/"><tt>http://www.mdyang.com</tt></a></p>
<hr>
<p>
	<small>File translated from T<sub>E</sub>X by <a href="http://hutchinson.belmont.ma.us/tth/">T<sub>T</sub>H</a>, version 4.01.<br>
	On 10 May 2012, 00:28.</small><img alt="" height="1" src="http://www.cnblogs.com/mdyang/aggbug/2487850.html?type=1" width="1"></p>
<p>
	<a href="http://www.cnblogs.com/mdyang/archive/2012/05/07/linux_kernel_dev_notes.html">本文链接</a></p>

                    <hr>
                    <p>
                        在微博上关注：
                        <a href="http://t.sina.com.cn/pythoncn">新浪</a>,
                        <a href="http://t.sina.com.cn/pythoncn">腾讯</a>
                         
                        <a href="http://simple-is-better.com/tougao">投稿</a>
                    </p>
                    <p><strong>最新招聘</strong></p>
                    <ul>
                        <li><a href="http://simple-is-better.com/jobs/detail-330">[北京] Python开发工程师</a> - 创新工场</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-146">[杭州] Python开发工程师(至少熟悉一个 web 框架)</a> - 杭州美巴科技有限公司</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-328">[上海] Python开发工程师</a> - ITALKI</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-313">[上海] Python 程序开发(Django, Scrapy, 待遇 6-15k)</a> - 若邻网</li>
                        <li><a href="http://simple-is-better.com/jobs/detail-219">[深圳] Python 程序开发(Django, 网络爬虫开发, 数据分析)</a> - 北京创新在线网络技术有限公司</li>
                    </ul>
                    <p><a href="http://simple-is-better.com/jobs/">更多&gt;&gt;</a></p><img src="http://www1.feedsky.com/t1/638052723/simple-is-better/feedsky/s.gif?r=http://simple-is-better.com/news/866" border="0" height="0" width="0">