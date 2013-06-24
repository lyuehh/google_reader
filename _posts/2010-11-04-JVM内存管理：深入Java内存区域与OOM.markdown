---
layout: post
title:  "JVM内存管理：深入Java内存区域与OOM"
date:   2010-11-04 11:30:37
author: 
categories: program
---

## JVM内存管理：深入Java内存区域与OOM
### by 
### at 2010-11-04 11:30:37
### original <http://www.javaeye.com/topic/802573>

<p>
</p>
<p style="text-indent:21.0pt"><span lang="EN-US">Java</span><span>与</span><span lang="EN-US">C++</span><span>之间有一堵由内存动态分配和垃圾收集技术所围成的高墙，墙外面的人想进去，墙里面的人却想出来。</span></p>
<p style="text-indent:21.0pt"> </p>
<h2><span>概述：</span></h2>
<p style="text-indent:21.0pt"><span>对于从事</span><span lang="EN-US">C</span><span>、</span><span lang="EN-US">C++</span><span>程序开发的开发人员来说，在内存管理领域，他们即是拥有最高权力的皇帝又是执行最基础工作的劳动人民——拥有每一个对象的“所有权”，又担负着每一个对象生命开始到终结的维护责任。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span>对于</span><span lang="EN-US">Java</span><span>程序员来说，不需要在为每一个</span><span lang="EN-US">new</span><span>操作去写配对的</span><span lang="EN-US">delete/free</span><span>，不容易出现内容泄漏和内存溢出错误，看起来由</span><span lang="EN-US">JVM</span><span>管理内存一切都很美好。不过，也正是因为</span><span lang="EN-US">Java</span><span>程序员把内存控制的权力交给了</span><span lang="EN-US">JVM</span><span>，一旦出现泄漏和溢出，如果不了解</span><span lang="EN-US">JVM</span><span>是怎样使用内存的，那排查错误将会是一件非常困难的事情。</span></p>
<p style="text-indent:21.0pt"> </p>
<h2>
<span lang="EN-US">VM</span><span>运行时数据区域</span>
</h2>
<p style="text-indent:21.0pt"><span lang="EN-US">JVM</span><span>执行</span><span lang="EN-US">Java</span><span>程序的过程中，会使用到各种数据区域，这些区域有各自的用途、创建和销毁时间。根据《</span><span lang="EN-US">Java</span><span>虚拟机规范（第二版）》（下文称</span><span lang="EN-US">VM Spec</span><span>）的规定，</span><span lang="EN-US">JVM</span><span>包括下列几个运行时数据区域：</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p style="margin-left:0cm;text-indent:0cm"><span lang="EN-US"><span>1.</span></span><span>程序计数器（</span><span lang="EN-US">Program Counter Register</span><span>）：</span></p>
<p style="margin-left:21.0pt"><span lang="EN-US"> </span></p>
<p style="margin-left:21.0pt"><span>每一个</span><span lang="EN-US">Java</span><span>线程都有一个程序计数器来用于保存程序执行到当前方法的哪一个指令，对于非</span><span lang="EN-US">Native</span><span>方法，这个区域记录的是正在执行的</span><span lang="EN-US">VM</span><span>原语的地址，如果正在执行的是</span><span lang="EN-US">Natvie</span><span>方法，这个区域则为空（</span><span lang="EN-US">undefined</span><span>）。此内存区域是唯一一个在</span><span lang="EN-US">VM Spec</span><span>中没有规定任何</span><span lang="EN-US">OutOfMemoryError</span><span>情况的区域。</span></p>
<p style="margin-left:21.0pt"><span lang="EN-US"> </span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:0cm;text-indent:0cm"><span lang="EN-US"><span>2.</span></span><span lang="EN-US">Java</span><span>虚拟机栈（</span><span lang="EN-US">Java
Virtual Machine Stacks</span><span>）</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>与程序计数器一样，</span><span lang="EN-US">VM</span><span>栈的生命周期也是与线程相同。</span><span lang="EN-US">VM</span><span>栈描述的是</span><span lang="EN-US">Java</span><span>方法调用的内存模型：每个方法被执行的时候，都会同时创建一个帧（</span><span lang="EN-US">Frame</span><span>）用于存储本地变量表、操作栈、动态链接、方法出入口等信息。每一个方法的调用至完成，就意味着一个帧在</span><span lang="EN-US">VM</span><span>栈中的入栈至出栈的过程。在后文中，我们将着重讨论</span><span lang="EN-US">VM</span><span>栈中本地变量表部分。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>经常有人把</span><span lang="EN-US">Java</span><span>内存简单的区分为堆内存（</span><span lang="EN-US">Heap</span><span>）和栈内存（</span><span lang="EN-US">Stack</span><span>），实际中的区域远比这种观点复杂，这样划分只是说明与变量定义密切相关的内存区域是这两块。其中所指的“堆”后面会专门描述，而所指的“栈”就是</span><span lang="EN-US">VM</span><span>栈中各个帧的本地变量表部分。本地变量表存放了编译期可知的各种标量类型（</span><span lang="EN-US">boolean</span><span>、</span><span lang="EN-US">byte</span><span>、</span><span lang="EN-US">char</span><span>、</span><span lang="EN-US">short</span><span>、</span><span lang="EN-US">int</span><span>、</span><span lang="EN-US">float</span><span>、</span><span lang="EN-US">long</span><span>、</span><span lang="EN-US">double</span><span>）、对象引用（不是对象本身，仅仅是一个引用指针）、方法返回地址等。其中</span><span lang="EN-US">long</span><span>和</span><span lang="EN-US">double</span><span>会占用</span><span lang="EN-US">2</span><span>个本地变量空间（</span><span lang="EN-US">32bit</span><span>），其余占用</span><span lang="EN-US">1</span><span>个。本地变量表在进入方法时进行分配，当进入一个方法时，这个方法需要在帧中分配多大的本地变量是一件完全确定的事情，在方法运行期间不改变本地变量表的大小。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>在</span><span lang="EN-US">VM Spec</span><span>中对这个区域规定了</span><span lang="EN-US">2</span><span>中异常状况：如果线程请求的栈深度大于虚拟机所允许的深度，将抛出</span><span lang="EN-US">StackOverflowError</span><span>异常；如果</span><span lang="EN-US">VM</span><span>栈可以动态扩展（</span><span lang="EN-US">VM Spec</span><span>中允许固定长度的</span><span lang="EN-US">VM</span><span>栈），当扩展时无法申请到足够内存则抛出</span><span lang="EN-US">OutOfMemoryError</span><span>异常。</span><span lang="EN-US"><br><br></span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:0cm;text-indent:0cm"><span lang="EN-US"><span>3.</span></span><span>本地方法栈（</span><span lang="EN-US">Native Method Stacks</span><span>）</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>本地方法栈与</span><span lang="EN-US">VM</span><span>栈所发挥作用是类似的，只不过</span><span lang="EN-US">VM</span><span>栈为虚拟机运行</span><span lang="EN-US">VM</span><span>原语服务，而本地方法栈是为虚拟机使用到的</span><span lang="EN-US">Native</span><span>方法服务。它的实现的语言、方式与结构并没有强制规定，甚至有的虚拟机（譬如</span><span lang="EN-US">Sun
Hotspot</span><span>虚拟机）直接就把本地方法栈和</span><span lang="EN-US">VM</span><span>栈合二为一。和</span><span lang="EN-US">VM</span><span>栈一样，这个区域也会抛出</span><span lang="EN-US">StackOverflowError</span><span>和</span><span lang="EN-US">OutOfMemoryError</span><span>异常。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:0cm"><span lang="EN-US"><br>
4.Java</span><span>堆（</span><span lang="EN-US">Java Heap</span><span>）</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>对于绝大多数应用来说，</span><span lang="EN-US">Java</span><span>堆是虚拟机管理最大的一块内存。</span><span lang="EN-US">Java</span><span>堆是被所有线程共享的，在虚拟机启动时创建。</span><span lang="EN-US">Java</span><span>堆的唯一目的就是存放对象实例，绝大部分的对象实例都在这里分配。这一点在</span><span lang="EN-US">VM
Spec</span><span>中的描述是：所有的实例以及数组都在堆上分配（原文：</span><span lang="EN-US">The heap is the runtime data area from which memory for all class
instances and arrays is allocated</span><span>），但是在逃逸分析和标量替换优化技术出现后，</span><span lang="EN-US">VM Spec</span><span>的描述就显得并不那么准确了。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span lang="EN-US">Java</span><span>堆内还有更细致的划分：新生代、老年代，再细致一点的：</span><span lang="EN-US">eden</span><span>、</span><span lang="EN-US">from survivor</span><span>、</span><span lang="EN-US">to survivor</span><span>，甚至更细粒度的本地线程分配缓冲（</span><span lang="EN-US">TLAB</span><span>）等，无论对</span><span lang="EN-US">Java</span><span>堆如何划分，目的都是为了更好的回收内存，或者更快的分配内存，在本章中我们仅仅针对内存区域的作用进行讨论，</span><span lang="EN-US">Java</span><span>堆中的上述各个区域的细节，可参见本文第二章《</span><span lang="EN-US">JVM</span><span>内存管理：深入垃圾收集器与内存分配策略》。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>根据</span><span lang="EN-US">VM Spec</span><span>的要求，</span><span lang="EN-US">Java</span><span>堆可以处于物理上不连续的内存空间，它逻辑上是连续的即可，就像我们的磁盘空间一样。实现时可以选择实现成固定大小的，也可以是可扩展的，不过当前所有商业的虚拟机都是按照可扩展来实现的（通过</span><span lang="EN-US">-Xmx</span><span>和</span><span lang="EN-US">-Xms</span><span>控制）。如果在堆中无法分配内存，并且堆也无法再扩展时，将会抛出</span><span lang="EN-US">OutOfMemoryError</span><span>异常。</span><span lang="EN-US"><br><br></span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:0cm;text-indent:0cm"><span lang="EN-US"><span>5.</span></span><span>方法区（</span><span lang="EN-US">Method Area</span><span>）</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>叫“方法区”可能认识它的人还不太多，如果叫永久代（</span><span lang="EN-US">Permanent
Generation</span><span>）它的粉丝也许就多了。它还有个别名叫做</span><span lang="EN-US">Non-Heap</span><span>（非堆），但是</span><span lang="EN-US">VM Spec</span><span>上则描述方法区为堆的一个逻辑部分（原文：</span><span lang="EN-US">the method area is logically part of the heap</span><span>），这个名字的问题还真容易令人产生误解，我们在这里就不纠结了。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>方法区中存放了每个</span><span lang="EN-US">Class</span><span>的结构信息，包括常量池、字段描述、方法描述等等。</span><span lang="EN-US">VM Space</span><span>描述中对这个区域的限制非常宽松，除了和</span><span lang="EN-US">Java</span><span>堆一样不需要连续的内存，也可以选择固定大小或者可扩展外，甚至可以选择不实现垃圾收集。相对来说，垃圾收集行为在这个区域是相对比较少发生的，但并不是某些描述那样永久代不会发生</span><span lang="EN-US">GC</span><span>（至少对当前主流的商业</span><span lang="EN-US">JVM</span><span>实现来说是如此），这里的</span><span lang="EN-US">GC</span><span>主要是对常量池的回收和对类的卸载，虽然回收的“成绩”一般也比较差强人意，尤其是类卸载，条件相当苛刻。</span><span lang="EN-US"><br><br></span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:0cm;text-indent:0cm"><span lang="EN-US"><span>6.</span></span><span>运行时常量池（</span><span lang="EN-US">Runtime Constant Pool</span><span>）</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span lang="EN-US">Class</span><span>文件中除了有类的版本、字段、方法、接口等描述等信息外，还有一项信息是常量表</span><span lang="EN-US">(constant_pool table)</span><span>，用于存放编译期已可知的常量，这部分内容将在类加载后进入方法区（永久代）存放。但是</span><span lang="EN-US">Java</span><span>语言并不要求常量一定只有编译期预置入</span><span lang="EN-US">Class</span><span>的常量表的内容才能进入方法区常量池，运行期间也可将新内容放入常量池（最典型的</span><span lang="EN-US">String.intern()</span><span>方法）。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>运行时常量池是方法区的一部分，自然受到方法区内存的限制，当常量池无法在申请到内存时会抛出</span><span lang="EN-US">OutOfMemoryError</span><span>异常。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span lang="EN-US"> </span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:0cm;text-indent:0cm"><span lang="EN-US"><span>7.</span></span><span>本机直接内存（</span><span lang="EN-US">Direct Memory</span><span>）</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>直接内存并不是虚拟机运行时数据区的一部分，它根本就是本机内存而不是</span><span lang="EN-US">VM</span><span>直接管理的区域。但是这部分内存也会导致</span><span lang="EN-US">OutOfMemoryError</span><span>异常出现，因此我们放到这里一起描述。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>在</span><span lang="EN-US">JDK1.4</span><span>中新加入了</span><span lang="EN-US">NIO</span><span>类，引入一种基于渠道与缓冲区的</span><span lang="EN-US">I/O</span><span>方式，它可以通过本机</span><span lang="EN-US">Native</span><span>函数库直接分配本机内存，然后通过一个存储在</span><span lang="EN-US">Java</span><span>堆里面的</span><span lang="EN-US">DirectByteBuffer</span><span>对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在</span><span lang="EN-US">Java</span><span>对和本机堆中来回复制数据。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"><span>显然本机直接内存的分配不会受到</span><span lang="EN-US">Java</span><span>堆大小的限制，但是即然是内存那肯定还是要受到本机物理内存（包括</span><span lang="EN-US">SWAP</span><span>区或者</span><span lang="EN-US">Windows</span><span>虚拟内存）的限制的，一般服务器管理员配置</span><span lang="EN-US">JVM</span><span>参数时，会根据实际内存设置</span><span lang="EN-US">-Xmx</span><span>等参数信息，但经常忽略掉直接内存，使得各个内存区域总和大于物理内存限制（包括物理的和操作系统级的限制），而导致动态扩展时出现</span><span lang="EN-US">OutOfMemoryError</span><span>异常。</span></p>
<p style="margin-top:10.5pt;margin-right:0cm;margin-bottom:10.5pt;margin-left:21.0pt"> </p>
<h2>
<span>实战</span><span lang="EN-US">OutOfMemoryError</span>
</h2>
<p style="text-indent:21.0pt"><span>上述区域中，除了程序计数器，其他在</span><span lang="EN-US">VM Spec</span><span>中都描述了产生</span><span lang="EN-US">OutOfMemoryError</span><span>（下称</span><span lang="EN-US">OOM</span><span>）的情形，那我们就实战模拟一下，通过几段简单的代码，令对应的区域产生</span><span lang="EN-US">OOM</span><span>异常以便加深认识，同时初步介绍一些与内存相关的虚拟机参数。下文的代码都是基于</span><span lang="EN-US">Sun Hotspot</span><span>虚拟机</span><span lang="EN-US">1.6</span><span>版的实现，对于不同公司的不同版本的虚拟机，参数与程序运行结果可能结果会有所差别。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p><strong><span lang="EN-US">Java</span></strong><strong><span>堆</span></strong></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span lang="EN-US">Java</span><span>堆存放的是对象实例，因此只要不断建立对象，并且保证</span><span lang="EN-US">GC Roots</span><span>到对象之间有可达路径即可产生</span><span lang="EN-US">OOM</span><span>异常。测试中限制</span><span lang="EN-US">Java</span><span>堆大小为</span><span lang="EN-US">20M</span><span>，不可扩展，通过参数</span><span lang="EN-US">-XX:+HeapDumpOnOutOfMemoryError</span><span>让虚拟机在出现</span><span lang="EN-US">OOM</span><span>异常的时候</span><span lang="EN-US">Dump</span><span>出内存映像以便分析。（关于</span><span lang="EN-US">Dump</span><span>映像文件分析方面的内容，可参见本文第三章《</span><span lang="EN-US">JVM</span><span>内存管理：深入</span><span lang="EN-US">JVM</span><span>内存异常分析与调优》。）</span><span lang="EN-US"><br><br></span></p>
<p><span>清单</span><span lang="EN-US">1</span><span>：</span><span lang="EN-US">Java</span><span>堆</span><span lang="EN-US">OOM</span><span>测试</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">/**</span></p>
<p><span lang="EN-US"><span> </span>*
  VM Args</span><span>：</span><span lang="EN-US">-Xms20m
  -Xmx20m -XX:+HeapDumpOnOutOfMemoryError</span></p>
<p><span lang="EN-US"><span> </span>*
  @author zzm</span></p>
<p><span lang="EN-US"><span> </span>*/</span></p>
<p><span lang="EN-US">public class HeapOOM {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>static
  class OOMObject {</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  static void main(String[] args) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>List&lt;OOMObject&gt; list = new
  ArrayList&lt;OOMObject&gt;();</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>while (true) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>list.add(new
  OOMObject());</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US">}</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><span>运行结果：</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">java.lang.OutOfMemoryError: Java heap
  space</span></p>
<p><span lang="EN-US">Dumping heap to java_pid3404.hprof ...</span></p>
<p><span lang="EN-US">Heap dump file created [22045981 bytes in
  0.663 secs]</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p><strong><span lang="EN-US">VM</span></strong><strong><span>栈和本地方法栈</span><span lang="EN-US"></span></strong></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span lang="EN-US">Hotspot</span><span>虚拟机并不区分</span><span lang="EN-US">VM</span><span>栈和本地方法栈，因此</span><span lang="EN-US">-Xoss</span><span>参数实际上是无效的，栈容量只由</span><span lang="EN-US">-Xss</span><span>参数设定。关于</span><span lang="EN-US">VM</span><span>栈和本地方法栈在</span><span lang="EN-US">VM Spec</span><span>描述了两种异常：</span><span lang="EN-US">StackOverflowError</span><span>与</span><span lang="EN-US">OutOfMemoryError</span><span>，当栈空间无法继续分配分配时，到底是内存太小还是栈太大其实某种意义上是对同一件事情的两种描述而已，在笔者的实验中，对于单线程应用尝试下面</span><span lang="EN-US">3</span><span>种方法均无法让虚拟机产生</span><span lang="EN-US">OOM</span><span>，全部尝试结果都是获得</span><span lang="EN-US">SOF</span><span>异常。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p style="margin-left:0cm;text-indent:21.0pt"><span lang="EN-US"><span>1.</span></span><span>使用</span><span lang="EN-US">-Xss</span><span>参数削减栈内存容量。结果：抛出</span><span lang="EN-US">SOF</span><span>异常时的堆栈深度相应缩小。</span></p>
<p style="margin-left:0cm;text-indent:21.0pt"><span lang="EN-US"><span>2.</span></span><span>定义大量的本地变量，增大此方法对应帧的长度。结果：抛出</span><span lang="EN-US">SOF</span><span>异常时的堆栈深度相应缩小。</span></p>
<p style="margin-left:0cm;text-indent:21.0pt"><span lang="EN-US"><span>3.</span></span><span>创建几个定义很多本地变量的复杂对象，打开逃逸分析和标量替换选项，使得</span><span lang="EN-US">JIT</span><span>编译器允许对象拆分后在栈中分配。结果：实际效果同第二点。</span></p>
<p><span lang="EN-US"> </span></p>
<p><span>清单</span><span lang="EN-US">2</span><span>：</span><span lang="EN-US">VM</span><span>栈和本地方法栈</span><span lang="EN-US">OOM</span><span>测试（仅作为第</span><span lang="EN-US">1</span><span>点测试程序）</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">/**</span></p>
<p><span lang="EN-US"><span> </span>*
  VM Args</span><span>：</span><span lang="EN-US">-Xss128k</span></p>
<p><span lang="EN-US"><span> </span>*
  @author zzm</span></p>
<p><span lang="EN-US"><span> </span>*/</span></p>
<p><span lang="EN-US">public class JavaVMStackSOF {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>private
  int stackLength = 1;</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  void stackLeak() {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>stackLength++;</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>stackLeak();</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  static void main(String[] args) throws Throwable {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>JavaVMStackSOF oom = new JavaVMStackSOF();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>try {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>oom.stackLeak();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>} catch (Throwable e) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>System.out.println("stack
  length:" + oom.stackLength);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>throw
  e;</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US">}</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><span>运行结果：</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">stack length:2402</span></p>
<p><span lang="EN-US">Exception in thread "main"
  java.lang.StackOverflowError</span></p>
<p><span lang="EN-US"><span>       
  </span>at org.fenixsoft.oom.JavaVMStackSOF.stackLeak(JavaVMStackSOF.java:20)</span></p>
<p><span lang="EN-US"><span>       
  </span>at org.fenixsoft.oom.JavaVMStackSOF.stackLeak(JavaVMStackSOF.java:21)</span></p>
<p><span lang="EN-US"><span>       
  </span>at org.fenixsoft.oom.JavaVMStackSOF.stackLeak(JavaVMStackSOF.java:21)</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span>如果在多线程环境下，不断建立线程倒是可以产生</span><span lang="EN-US">OOM</span><span>异常，但是基本上这个异常和</span><span lang="EN-US">VM</span><span>栈空间够不够关系没有直接关系，甚至是给每个线程的</span><span lang="EN-US">VM</span><span>栈分配的内存越多反而越容易产生这个</span><span lang="EN-US">OOM</span><span>异常。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span>原因其实很好理解，操作系统分配给每个进程的内存是有限制的，譬如</span><span lang="EN-US">32</span><span>位</span><span lang="EN-US">Windows</span><span>限制为</span><span lang="EN-US">2G</span><span>，</span><span lang="EN-US">Java</span><span>堆和方法区的大小</span><span lang="EN-US">JVM</span><span>有参数可以限制最大值，那剩余的内存为</span><span lang="EN-US">2G</span><span>（操作系统限制）</span><span lang="EN-US">-Xmx</span><span>（最大堆）</span><span lang="EN-US">-MaxPermSize</span><span>（最大方法区），程序计数器消耗内存很小，可以忽略掉，那虚拟机进程本身耗费的内存不计算的话，剩下的内存就供每一个线程的</span><span lang="EN-US">VM</span><span>栈和本地方法栈瓜分了，那自然每个线程中</span><span lang="EN-US">VM</span><span>栈分配内存越多，就越容易把剩下的内存耗尽。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p><span>清单</span><span lang="EN-US">3</span><span>：创建线程导致</span><span lang="EN-US">OOM</span><span>异常</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">/**</span></p>
<p><span lang="EN-US"><span> </span>*
  VM Args</span><span>：</span><span lang="EN-US">-Xss2M </span><span>（这时候不妨设大些）</span></p>
<p><span lang="EN-US"><span> </span>*
  @author zzm</span></p>
<p><span lang="EN-US"><span> </span>*/</span></p>
<p><span lang="EN-US">public class JavaVMStackOOM {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>private
  void dontStop() {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>while (true) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  void stackLeakByThread() {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>while (true) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>Thread
  thread = new Thread(new Runnable() {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span>@Override</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span>public void run() {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span><span>       </span>dontStop();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>});</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>thread.start();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  static void main(String[] args) throws Throwable {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>JavaVMStackOOM oom = new
  JavaVMStackOOM();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>oom.stackLeakByThread();</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US">}</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span>特别提示一下，如果读者要运行上面这段代码，记得要存盘当前工作，上述代码执行时有很大令操作系统卡死的风险。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p><span>运行结果：</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">Exception in thread "main"
  java.lang.OutOfMemoryError: unable to create new native thread </span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"><br><br></span></p>
<p><strong><span>运行时常量池</span><span lang="EN-US"></span></strong></p>
<p><strong><span lang="EN-US"> </span></strong></p>
<p style="text-indent:21.0pt"><span>要在常量池里添加内容，最简单的就是使用</span><span lang="EN-US">String.intern()</span><span>这个</span><span lang="EN-US">Native</span><span>方法。由于常量池分配在方法区内，我们只需要通过</span><span lang="EN-US">-XX:PermSize</span><span>和</span><span lang="EN-US">-XX:MaxPermSize</span><span>限制方法区大小即可限制常量池容量。实现代码如下：</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p><span>清单</span><span lang="EN-US">4</span><span>：运行时常量池导致的</span><span lang="EN-US">OOM</span><span>异常</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">/**</span></p>
<p><span lang="EN-US"><span> </span>*
  VM Args</span><span>：</span><span lang="EN-US">-XX:PermSize=10M
  -XX:MaxPermSize=10M</span></p>
<p><span lang="EN-US"><span> </span>*
  @author zzm</span></p>
<p><span lang="EN-US"><span> </span>*/</span></p>
<p><span lang="EN-US">public class RuntimeConstantPoolOOM {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  static void main(String[] args) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>// </span><span>使用</span><span lang="EN-US">List</span><span>保持着常量池引用，压制</span><span lang="EN-US">Full
  GC</span><span>回收常量池行为</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>List&lt;String&gt; list = new
  ArrayList&lt;String&gt;();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>// 10M</span><span>的</span><span lang="EN-US">PermSize</span><span>在</span><span lang="EN-US">integer</span><span>范围内足够产生</span><span lang="EN-US">OOM</span><span>了</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>int i = 0; </span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>while (true) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>list.add(String.valueOf(i++).intern());</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US">}</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><span>运行结果：</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">Exception in thread "main"
  java.lang.OutOfMemoryError: PermGen space</span></p>
<p><span lang="EN-US"><span>       </span>at
  java.lang.String.intern(Native Method)</span></p>
<p><span lang="EN-US"><span>       </span>at
  org.fenixsoft.oom.RuntimeConstantPoolOOM.main(RuntimeConstantPoolOOM.java:18)</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"> </span></p>
<p><strong><span>方法区</span></strong></p>
<p><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span>上文讲过，方法区用于存放</span><span lang="EN-US">Class</span><span>相关信息，所以这个区域的测试我们借助</span><span lang="EN-US">CGLib</span><span>直接操作字节码动态生成大量的</span><span lang="EN-US">Class</span><span>，值得注意的是，这里我们这个例子中模拟的场景其实经常会在实际应用中出现：当前很多主流框架，如</span><span lang="EN-US">Spring</span><span>、</span><span lang="EN-US">Hibernate</span><span>对类进行增强时，都会使用到</span><span lang="EN-US">CGLib</span><span>这类字节码技术，当增强的类越多，就需要越大的方法区用于保证动态生成的</span><span lang="EN-US">Class</span><span>可以加载入内存。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<p><span>清单</span><span lang="EN-US">5</span><span>：借助</span><span lang="EN-US">CGLib</span><span>使得方法区出现</span><span lang="EN-US">OOM</span><span>异常</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">/**</span></p>
<p><span lang="EN-US"><span> </span>*
  VM Args</span><span>：</span><span lang="EN-US">
  -XX:PermSize=10M -XX:MaxPermSize=10M</span></p>
<p><span lang="EN-US"><span> </span>*
  @author zzm</span></p>
<p><span lang="EN-US"><span> </span>*/</span></p>
<p><span lang="EN-US">public class JavaMethodAreaOOM {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  static void main(String[] args) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>while (true) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>Enhancer
  enhancer = new Enhancer();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>enhancer.setSuperclass(OOMObject.class);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>enhancer.setUseCache(false);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>enhancer.setCallback(new
  MethodInterceptor() {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span>public Object intercept(Object obj,
  Method method, Object[] args, MethodProxy proxy) throws Throwable {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span><span>       </span>return
  proxy.invokeSuper(obj, args);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>});</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>enhancer.create();</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>static
  class OOMObject {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US">}</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><span>运行结果：</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">Caused by: java.lang.OutOfMemoryError:
  PermGen space</span></p>
<p><span lang="EN-US"><span>       </span>at
  java.lang.ClassLoader.defineClass1(Native Method)</span></p>
<p><span lang="EN-US"><span>       </span>at
  java.lang.ClassLoader.defineClassCond(ClassLoader.java:632)</span></p>
<p><span lang="EN-US"><span>       </span>at
  java.lang.ClassLoader.defineClass(ClassLoader.java:616)</span></p>
<p><span lang="EN-US"><span>       </span>...
  8 more</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><strong><span>本机直接内存</span></strong></p>
<p><span lang="EN-US"> </span></p>
<p style="text-indent:21.0pt"><span lang="EN-US">DirectMemory</span><span>容量可通过</span><span lang="EN-US">-XX:MaxDirectMemorySize</span><span>指定，不指定的话默认与</span><span lang="EN-US">Java</span><span>堆（</span><span lang="EN-US">-Xmx</span><span>指定）一样，下文代码越过了</span><span lang="EN-US">DirectByteBuffer</span><span>，直接通过反射获取</span><span lang="EN-US">Unsafe</span><span>实例进行内存分配（</span><span lang="EN-US">Unsafe</span><span>类的</span><span lang="EN-US">getUnsafe()</span><span>方法限制了只有引导类加载器才会返回实例，也就是基本上只有</span><span lang="EN-US">rt.jar</span><span>里面的类的才能使用），因为</span><span lang="EN-US">DirectByteBuffer</span><span>也会抛</span><span lang="EN-US">OOM</span><span>异常，但抛出异常时实际上并没有真正向操作系统申请分配内存，而是通过计算得知无法分配既会抛出，真正申请分配的方法是</span><span lang="EN-US">unsafe.allocateMemory()</span><span>。</span></p>
<p style="text-indent:21.0pt"><span lang="EN-US"> </span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">/**</span></p>
<p><span lang="EN-US"><span> </span>*
  VM Args</span><span>：</span><span lang="EN-US">-Xmx20M
  -XX:MaxDirectMemorySize=10M</span></p>
<p><span lang="EN-US"><span> </span>*
  @author zzm</span></p>
<p><span lang="EN-US"><span> </span>*/</span></p>
<p><span lang="EN-US">public class DirectMemoryOOM {</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>private
  static final int _1MB = 1024 * 1024;</span></p>
<p><span lang="EN-US"> </span></p>
<p><span lang="EN-US"><span>       </span>public
  static void main(String[] args) throws Exception {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>Field unsafeField =
  Unsafe.class.getDeclaredFields()[0];</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>unsafeField.setAccessible(true);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>Unsafe unsafe = (Unsafe)
  unsafeField.get(null);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>while (true) {</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span><span>       </span>unsafe.allocateMemory(_1MB);</span></p>
<p><span lang="EN-US"><span>       </span><span>       </span>}</span></p>
<p><span lang="EN-US"><span>       </span>}</span></p>
<p><span lang="EN-US">}</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p><span>运行结果：</span></p>
<table style="border-collapse:collapse;border:none" border="1" cellspacing="0" cellpadding="0"><tbody><tr>
<td style="width:426.1pt;padding:0cm 5.4pt 0cm 5.4pt" width="568" valign="top">
<p><span lang="EN-US">Exception in thread "main"
  java.lang.OutOfMemoryError</span></p>
<p><span lang="EN-US"><span>       </span>at
  sun.misc.Unsafe.allocateMemory(Native Method)</span></p>
<p><span lang="EN-US"><span>       </span>at
  org.fenixsoft.oom.DirectMemoryOOM.main(DirectMemoryOOM.java:20)</span></p>
</td>
</tr></tbody></table>
<p><span lang="EN-US"> </span></p>
<p> </p>
<h1>
<span>总结</span><span style="font-weight:normal" lang="EN-US"></span>
</h1>
<p style="text-indent:21.0pt"><span>到此为止，我们弄清楚虚拟机里面的内存是如何划分的，哪部分区域，什么样的代码、操作可能导致</span><span lang="EN-US">OOM</span><span>异常。虽然</span><span lang="EN-US">Java</span><span>有垃圾收集机制，但</span><span lang="EN-US">OOM</span><span>仍然离我们并不遥远，本章内容我们只是知道各个区域</span><span lang="EN-US">OOM</span><span>异常出现的原因，下一章我们将看看</span><span lang="EN-US">Java</span><span>垃圾收集机制为了避免</span><span lang="EN-US">OOM</span><span>异常出现，做出了什么样的努力。</span></p>

          
          <br><br>
          作者: <a href="http://icyfenix.javaeye.com">IcyFenix</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/802573" style="color:red">已有 <strong>29</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>