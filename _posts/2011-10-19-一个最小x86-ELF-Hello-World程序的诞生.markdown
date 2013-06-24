---
layout: post
title:  "一个最小x86 ELF Hello World程序的诞生"
date:   2011-10-19 22:57:39
author: yuanyi
categories: program
---

## 一个最小x86 ELF Hello World程序的诞生
### by yuanyi
### at 2011-10-19 22:57:39
### original <http://item.feedsky.com/~feedsky/heikezhi/~8608072/577089957/6713895/1/item.html>

<p>注：这里的最小是指我能做到的</p>
<p>最终大小: <strong>142字节</strong></p>
<p><strong>介绍</strong></p>
<p>这篇文章可以算是我在Ubuntu Linux上尝试创建一个最小的x86 ELF二进制Hello World文件的记录，你也可以把它当作一篇指南，我的尝试先是从c开始，然后转向x86汇编，最后以16进制编辑器搞定，但我的最终成果实际上只能打印"Hi World"，这纯粹是为了让最终的数字看着更顺眼一些而已，最终的x86 ELF二进制虽然已经被破坏的不成样子，但最重要的是它仍然可以照常运行。</p>
<p><strong>开始</strong></p>
<ul>
<li>如果你也想跟我一起来试试，那你要做的第一件事就应该是先配置环境：</li>
<li>安装Ubuntu (或随便别的你喜欢的发行版)</li>
<li>运行: <strong>sudo apt-get install g++ gcc nasm</strong></li>
<li>查看系统版本</li>
</ul>
<pre>
user@computer:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 8.04.1
Release:        8.04
Codename:       hardy
user@computer:~$ uname -a
Linux ryanh-desktop 2.6.24-19-generic #1 SMP Wed Jun 18 14:43:41 UTC 2008
 i686 GNU/Linux
user@computer:~$ gcc --version
gcc (GCC) 4.2.3 (Ubuntu 4.2.3-2ubuntu7)
Copyright (C) 2007 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
user@computer:~$ nasm -version
NASM version 0.99.06-20071101 compiled on Nov 15 2007
</pre>
<p>我的尝试从C开始，下面是我写的C程序，chello.c</p>
<pre name="code">
#include 
int main()
{
  printf ("Hi World\n");
  return0;
}
</pre>
<p>编译: </p>
<pre>
user@computer:~$ gcc -o chello chello.c
user@computer:~$ ./chello
Hi World
</pre>
<p>我最初得到的可执行文件大小为6363字节，你可以使用readelf来查看ELF文件的头信息，命令: </p>
<pre>
user@computer:~$ readelf -h chello
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Intel 80386
  Version:                           0x1
  Entry point address:               0x80482f0
  Start of program headers:          52 (bytes into file)
  Start of section headers:          3220 (bytes into file)
  Flags:                             0x0
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         7
  Size of section headers:           40 (bytes)
  Number of section headers:         36
  Section header string table index: 33
</pre>
<p>ldd也是个很有用的命令，它可以显示这个c程序链接了哪些动态库:</p>
<pre>
user@computer:~$ ldd chello
    linux-gate.so.1 =&gt;  (0xb7f77000)
    libc.so.6 =&gt; /lib/tls/i686/cmov/libc.so.6 (0xb7e18000)
    /lib/ld-linux.so.2 (0xb7f78000)
</pre>
<p>file命令可以告诉你这个文件的基本信息</p>
<pre>
user@computer:~$ file chello
chello: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), for GNU/Linux 2.6.8, dynamically linked (uses shared libs), not stripped
</pre>
<p>我们看到file命令返回了"not stripped"，这就是说这个二进制包含了用于调试的符号信息，让我们使用strip命令给它做个瘦身：</p>
<pre name="code">
user@computer:~$ strip -s chello
</pre>
<p>经过瘦身之后，现在这个二进制的大小变成了2984字节，还是没法接受，是时候做出艰难决定了，于是我放弃了C以及printf，转而使用nasm x86汇编，下面是hello.asm：</p>
<pre name="code">
        SECTION .data
msg:	db "Hi World",10
len:	equ $-msg

	SECTION .text

        global main
main:
	mov	edx,len
	mov	ecx,msg
	mov	ebx,1
	mov	eax,4

	int	0x80
	mov	ebx,0
	mov	eax,1
	int	0x80
</pre>
<p>编译</p>
<pre>
user@computer:~$ nasm -f elf hello.asm
user@computer:~$ gcc -o hello hello.o -nostartfiles -nostdlib -nodefaultlibs
user@computer:~$ strip -s hello
user@computer:~$ ./hello
Hi World
</pre>
<p>strip之前是770字节，之后是448字节，但是这个二进制仍然包含一些无用的头部和section信息，现在找个你最顺手的16进制编辑器打开这个二进制，我一般用curses hexeditor和ghex2。</p>
<p><img src="http://heikezhi.com/wp-content/uploads/2011/10/screen1.png" alt="" title="screen1" width="642" height="410"></p>
<p>删掉0xAD后面的内容后，现在大小变成了173字节</p>
<p><img src="http://heikezhi.com/wp-content/uploads/2011/10/screen2.png" alt="" title="screen1" width="642" height="410"></p>
<p><img src="http://heikezhi.com/wp-content/uploads/2011/10/asmtobin.png" alt="" title="screen3" width="642" height="410"></p>
<p>可以看到0x7后面有一块无用空间，所有我们把保存Hi World字符串的数据块从0xA4-0xAC移到了0x7然后把0x86对字符串的引用从0xA4改为新的地址0x7，最后删除0xA2和0xA3.</p>
<p><img src="http://heikezhi.com/wp-content/uploads/2011/10/screen3.png" alt="" title="screen1" width="642" height="410"></p>
<p>现在文件大小应该变成了164字节，是时候进入最终环节了，剩下的部分我需要做些解释，基本上，我要做的就是不断尝试改变ELF的头部，但是避免出现segfault fault，我加了许多jmp并完全破坏了原本的可执行文件，尽管如此，它还是可以运行的，这里是一些有用的技巧： 在x86汇编中 0xD9D0，也就是nop操作符，如果你需要填充空白时这个指令很有用，另外0xEB后面跟一个有符号字节来完成相对跳转，你真的应该看看intel x86的<a href="http://www.intel.com/products/processor/manuals/">汇编指令文档</a> <a href="http://download.intel.com/design/processor/manuals/253666.pdf">A-M</a> <a href="http://download.intel.com/design/processor/manuals/253667.pdf">N-Z</a>。</p>
<pre name="code">
typedef struct {
    unsigned char   e_ident[EI_NIDENT];
    Elf32_Half      e_type;
    Elf32_Half      e_machine;
    Elf32_Word      e_version;
    Elf32_Addr      e_entry;
    Elf32_Off       e_phoff;
    Elf32_Off       e_shoff;
    Elf32_Word      e_flags;
    Elf32_Half      e_ehsize;
    Elf32_Half      e_phentsize;
    Elf32_Half      e_phnum;
    Elf32_Half      e_shentsize;
    Elf32_Half      e_shnum;
    Elf32_Half      e_shtrndx;
} Elf32_Ehdr;
</pre>
<p><img src="http://heikezhi.com/wp-content/uploads/2011/10/screen4_path.gif" alt="" title="screen1" width="642" height="410"></p>
<p><strong>结论</strong></p>
<p>最终大小: 142 字节</p>
<p><a href="http://timelessname.com/elfbin/helloworld.tar.gz">helloworld.tar.gz</a></p>
<p>我确信肯定还有办法让它更小，应该还是可以从头部移掉一些没用的数据，但是我不想花太多时间钻研ELF的头部格式，另一个办法或许是使用a.out格式来代替ELF格式。</p>
<p>如果你有意见，建议或是批评欢迎给我邮件：henszey#gmail.com </p>
<p>-----------<br>
本文来自："<a href="http://timelessname.com/elfbin/">Smallest x86 ELF Hello World</a>"，作者：henszey</p>
<p>想和我们一道传播黑客精神？<a href="http://heikezhi.com/join">快来加入吧！</a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="4"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">无觅猜您也喜欢：</font></b></td>
    </tr>
    
        <tr>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="Hello world!" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F03%2F11%2Fhello-world%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F10%2F19%2Fsmallest-x86-elf-hello-world%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Hello world!</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="破解我吧：一次ELF文件的解构之旅" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F09%2F15%2Fhackme-deconstructing-an-elf-file%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F10%2F19%2Fsmallest-x86-elf-hello-world%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/images/blogWidget/wordpress_default.gif" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">破解我吧：一次ELF文件的解构之旅</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="debugCSS: 超简单的CSS/(x)html诊断工具" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F10%2F11%2Fdebugcss-intro%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F10%2F19%2Fsmallest-x86-elf-hello-world%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/10/18/9391408.png" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">debugCSS: 超简单的CSS/(x)html诊断工具</font>
                    </a>
                </td>
                <td width="102" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="轻松为任意程序代码添加语法高亮" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fheikezhi.com%2F2011%2F08%2F01%2Fquick-tip-how-to-add-syntax-highlighting-to-any-project%2F&amp;from=http%3A%2F%2Fheikezhi.com%2F2011%2F10%2F19%2Fsmallest-x86-elf-hello-world%2F">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:96px!important;height:96px!important" src="http://static.wumii.com/site_images/2011/08/01/20424846.jpg" width="96px" height="96px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:102px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">轻松为任意程序代码添加语法高亮</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="4" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/577089957/heikezhi/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/heikezhi/~8608072/577089957/6713895/1/item.html" border="0" height="0" width="0">