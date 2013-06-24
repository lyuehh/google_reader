---
layout: post
title:  "linux命令终极系列之（find）"
date:   2011-04-16 09:58:00
author: C++技术中心
categories: program
---

## linux命令终极系列之（find）
### by C++技术中心
### at 2011-04-16 09:58:00
### original <http://www.cppblog.com/API/archive/2011/04/16/144344.html>

<font size="4"><strong>一、find 命令格式<br>
</strong></font>
<p><font size="3"><strong><br>
1、find命令的一般形式为；<br>
</strong></font></p>
<div><code>find pathname -options [-print -exec -ok ...]</code></div>
<p><font size="3"><strong><br>
2、find命令的参数；<br>
</strong></font></p>
<div><code>pathname: find命令所查找的目录路径。例如用.来表示当前目录，用/来表示系统根目录。<br>-print： find命令将匹配的文件输出到标准输出。<br>-exec： find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为&#39;command&#39; {  } \;，注意{   }和\；之间的空格。<br>-ok： 和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执行。</code></div>
<p><font size="3"><strong><br>
3、find命令选项<br>
</strong></font><br>
</p>
<div><code>-name <br><br>按照文件名查找文件。<br><br>-perm <br>按照文件权限来查找文件。<br><br>-prune <br>使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略。<br><br>-user <br>按照文件属主来查找文件。<br><br>-group <br>按照文件所属的组来查找文件。<br><br>-mtime -n +n <br>按照文件的更改时间来查找文件， - n表示文件更改时间距现在n天以内，+ n表示文件更改时间距现在n天以前。find命令还有-atime和-ctime 选项，但它们都和-m time选项。<br><br>-nogroup <br>查找无有效所属组的文件，即该文件所属的组在/etc/groups中不存在。<br><br>-nouser <br>查找无有效属主的文件，即该文件的属主在/etc/passwd中不存在。<br>-newer file1 ! file2 <br><br>查找更改时间比文件file1新但比文件file2旧的文件。<br>-type <br><br>查找某一类型的文件，诸如：<br><br>b - 块设备文件。<br>d - 目录。<br>c - 字符设备文件。<br>p - 管道文件。<br>l - 符号链接文件。<br>f - 普通文件。<br><br>-size n：[c] 查找文件长度为n块的文件，带有c时表示文件长度以字节计。<br>-depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。<br>-fstype：查找位于某一类型文件系统中的文件，这些文件系统类型通常可以在配置文件/etc/fstab中找到，该配置文件中包含了本系统中有关文件系统的信息。<br><br>-mount：在查找文件时不跨越文件系统mount点。<br>-follow：如果find命令遇到符号链接文件，就跟踪至链接所指向的文件。<br>-cpio：对匹配的文件使用cpio命令，将这些文件备份到磁带设备中。</code></div>
<p>另外,下面三个的区别:<br>
</p>
<div><code>   -amin n<br>　　查找系统中最后N分钟访问的文件<br><br>　　-atime n<br>　　查找系统中最后n*24小时访问的文件<br><br>　　-cmin n<br>　　查找系统中最后N分钟被改变文件状态的文件<br><br>　　-ctime n<br>　　查找系统中最后n*24小时被改变文件状态的文件<br><br>   　-mmin n<br>　　查找系统中最后N分钟被改变文件数据的文件<br><br>　　-mtime n<br>　　查找系统中最后n*24小时被改变文件数据的文件</code></div>
<p><font size="3"><strong><br>
4、使用exec或ok来执行shell命令<br>
</strong></font></p>
<p>    使用find时，只要把想要的操作写在一个文件里，就可以用exec来配合find查找，很方便的</p>
<p>    在有些操作系统中只允许-exec选项执行诸如l s或ls -l这样的命令。大多数用户使用这一选项是为了查找旧文件并删除它们。建议在真正执行rm命令删除文件之前，最好先用ls命令看一下，确认它们是所要删除的文件。</p>
<p>    exec选项后面跟随着所要执行的命令或脚本，然后是一对儿{ }，一个空格和一个\，最后是一个分号。为了使用exec选项，必须要同时使用print选项。如果验证一下find命令，会发现该命令只输出从当前路径起的相对路径及文件名。</p>
<p>    例如：为了用ls -l命令列出所匹配到的文件，可以把ls -l命令放在find命令的-exec选项中</p>
<div><code># find . -type f -exec ls -l {  } \;<br>-rw-r--r--    1 root     root        34928 2003-02-25  ./conf/httpd.conf<br>-rw-r--r--    1 root     root        12959 2003-02-25  ./conf/magic<br>-rw-r--r--    1 root     root          180 2003-02-25  ./conf.d/README</code></div>
<p>上面的例子中，find命令匹配到了当前目录下的所有普通文件，并在-exec选项中使用ls -l命令将它们列出。<br>
在/logs目录中查找更改时间在5日以前的文件并删除它们：</p>
<div><code>$ find logs -type f -mtime +5 -exec rm {  } \;</code></div>
<p>    <strong>记住：</strong>在shell中用任何方式删除文件之前，应当先查看相应的文件，一定要小心！当使用诸如mv或rm命令时，可以使用-exec选项的安全模式。它将在对每个匹配到的文件进行操作之前提示你。</p>
<p>在下面的例子中， find命令在当前目录中查找所有文件名以.LOG结尾、更改时间在5日以上的文件，并删除它们，只不过在删除之前先给出提示。</p>
<div><code>$ find . -name &quot;*.conf&quot;  -mtime +5 -ok rm {  } \;<br>&lt; rm ... ./conf/httpd.conf &gt; ? n</code></div>
<p>按y键删除文件，按n键不删除。</p>
<p>任何形式的命令都可以在-exec选项中使用。</p>
<p>在下面的例子中我们使用grep命令。find命令首先匹配所有文件名为“ passwd*”的文件，例如passwd、passwd.old、passwd.bak，然后执行grep命令看看在这些文件中是否存在一个sam用户。</p>
<div><code># find /etc -name &quot;passwd*&quot; -exec grep &quot;sam&quot; {  } \;<br>sam:x:501:501::/usr/sam:/bin/bash</code></div>
<p><font size="4"><strong><br>
二、find命令的例子；<br>
</strong></font></p>
<p><font size="3"><strong><br>
1、查找当前用户主目录下的所有文件：<br>
</strong></font></p>
<p>下面两种方法都可以使用</p>
<div><code>$ find $HOME -print<br>$ find ~ -print</code></div>
<br>
<font size="3"><strong><br>
2、让当前目录中文件属主具有读、写权限，并且文件所属组的用户和其他用户具有读权限的文件；<br>
</strong></font>
<div><code>$ find . -type f -perm 644 -exec ls -l {  } \;</code></div>
<p><font size="3"><strong><br>
3、为了查找系统中所有文件长度为0的普通文件，并列出它们的完整路径；<br>
</strong></font></p>
<div><code>$ find / -type f -size 0 -exec ls -l {  } \;</code></div>
<p><font size="3"><strong><br>
4、查找/var/logs目录中更改时间在7日以前的普通文件，并在删除之前询问它们；<br>
</strong></font></p>
<div><code>$ find /var/logs -type f -mtime +7 -ok rm {  } \;</code></div>
<p><font size="3"><strong><br>
5、为了查找系统中所有属于root组的文件；<br>
</strong></font></p>
<div><code>$find . -group root -exec ls -l {  } \;<br>-rw-r--r--    1 root     root          595 10月 31 01:09 ./fie1</code></div>
<p><font size="3"><strong><br>
6、find命令将删除当目录中访问时间在7日以来、含有数字后缀的admin.log文件。<br>
</strong></font></p>
<p>该命令只检查三位数字，所以相应文件的后缀不要超过999。先建几个admin.log*的文件 ，才能使用下面这个命令</p>
<div><code>$ find . -name &quot;admin.log[0-9][0-9][0-9]&quot; -atime -7  -ok<br>rm {  } \;<br>&lt; rm ... ./admin.log001 &gt; ? n<br>&lt; rm ... ./admin.log002 &gt; ? n<br>&lt; rm ... ./admin.log042 &gt; ? n<br>&lt; rm ... ./admin.log942 &gt; ? n</code></div>
<p><font size="3"><strong><br>
7、为了查找当前文件系统中的所有目录并排序；<br>
</strong></font></p>
<div><code>$ find . -type d | sort</code></div>
<p><font size="3"><strong><br>
8、为了查找系统中所有的rmt磁带设备；<br>
</strong></font></p>
<div><code>$ find /dev/rmt -print</code></div>
<p><font size="4"><strong><br>
三、xargs<br>
</strong></font></p>
<p>xargs - build and execute command lines from standard input</p>
<p>在使用find命令的-exec选项处理匹配到的文件时，
find命令将所有匹配到的文件一起传递给exec执行。但有些系统对能够传递给exec的命令长度有限制，这样在find命令运行几分钟之后，就会出现
溢出错误。错误信息通常是“参数列太长”或“参数列溢出”。这就是xargs命令的用处所在，特别是与find命令一起使用。</p>
<p>    find命令把匹配到的文件传递给xargs命令，而xargs命令每次只获取一部分文件而不是全部，不像-exec选项那样。这样它可以先处理最先获取的一部分文件，然后是下一批，并如此继续下去。</p>
<p>    在有些系统中，使用-exec选项会为处理每一个匹配到的文件而发起一个相应的进程，并非将匹配到的文件全部作为参数一次执行；这样在有些情况下就会出现进程过多，系统性能下降的问题，因而效率不高；</p>
<p>    而使用xargs命令则只有一个进程。另外，在使用xargs命令时，究竟是一次获取所有的参数，还是分批取得参数，以及每一次获取参数的数目都会根据该命令的选项及系统内核中相应的可调参数来确定。</p>
<p>来看看xargs命令是如何同find命令一起使用的，并给出一些例子。</p>
<p>下面的例子查找系统中的每一个普通文件，然后使用xargs命令来测试它们分别属于哪类文件</p>
<div><code>#find . -type f -print | xargs file<br>./.kde/Autostart/Autorun.desktop: UTF-8 Unicode English text<br>./.kde/Autostart/.directory:      ISO-8859 text\<br>......</code></div>
<p>在整个系统中查找内存信息转储文件(core dump) ，然后把结果保存到/tmp/core.log 文件中：</p>
<div><code>$ find / -name &quot;core&quot; -print | xargs echo &quot;&quot; &gt;/tmp/core.log</code></div>
<p>上面这个执行太慢，我改成在当前目录下查找</p>
<div><code>#find . -name &quot;file*&quot; -print | xargs echo &quot;&quot; &gt; /temp/core.log<br># cat /temp/core.log<br>./file6</code></div>
<p>在当前目录下查找所有用户具有读、写和执行权限的文件，并收回相应的写权限：</p>
<div><code># ls -l<br>drwxrwxrwx    2 sam      adm          4096 10月 30 20:14 file6<br>-rwxrwxrwx    2 sam      adm             0 10月 31 01:01 http3.conf<br>-rwxrwxrwx    2 sam      adm             0 10月 31 01:01 httpd.conf<br><br># find . -perm -7 -print | xargs chmod o-w<br># ls -l<br>drwxrwxr-x    2 sam      adm          4096 10月 30 20:14 file6<br>-rwxrwxr-x    2 sam      adm             0 10月 31 01:01 http3.conf<br>-rwxrwxr-x    2 sam      adm             0 10月 31 01:01 httpd.conf</code></div>
<p>用grep命令在所有的普通文件中搜索hostname这个词：</p>
<div><code># find . -type f -print | xargs grep "hostname"<br>./httpd1.conf:#     different IP addresses or hostnames and have them handled by the<br>./httpd1.conf:# VirtualHost: If you want to maintain multiple domains/hostnames<br>on your</code></div>
<p>用grep命令在当前目录下的所有普通文件中搜索hostnames这个词：</p>
<div><code># find . -name \* -type f -print | xargs grep "hostnames"<br>./httpd1.conf:#     different IP addresses or hostnames and have them handled by the<br>./httpd1.conf:# VirtualHost: If you want to maintain multiple domains/hostnames<br>on your</code></div>
<p>注意，在上面的例子中， \用来取消find命令中的*在shell中的特殊含义。</p>
<p>find命令配合使用exec和xargs可以使用户对所匹配到的文件执行几乎所有的命令。</p>
<p><font size="4"><strong><br>
四、find 命令的参数<br>
</strong></font></p>
<p>下面是find一些常用参数的例子，有用到的时候查查就行了，像上面前几个贴子，都用到了其中的的一些参数，也可以用man或查看论坛里其它贴子有find的命令手册</p>
<p><font size="3"><strong><br>
1、使用name选项<br>
</strong></font></p>
<p>文件名选项是find命令最常用的选项，要么单独使用该选项，要么和其他选项一起使用。</p>
<p>可以使用某种文件名模式来匹配文件，记住要用引号将文件名模式引起来。</p>
<p>不管当前路径是什么，如果想要在自己的根目录$HOME中查找文件名符合*.txt的文件，使用~作为 'pathname'参数，波浪号~代表了你的$HOME目录。</p>
<div><code>$ find ~ -name "*.txt" -print</code></div>
<p>想要在当前目录及子目录中查找所有的‘ *.txt’文件，可以用：</p>
<div><code>$ find . -name "*.txt" -print</code></div>
<p>想要的当前目录及子目录中查找文件名以一个大写字母开头的文件，可以用：</p>
<div><code>$ find . -name "[A-Z]*" -print</code></div>
<p>想要在/etc目录中查找文件名以host开头的文件，可以用：</p>
<div><code>$ find /etc -name "host*" -print</code></div>
<p>想要查找$HOME目录中的文件，可以用：</p>
<div><code>$ find ~ -name "*" -print 或find . -print</code></div>
<p>要想让系统高负荷运行，就从根目录开始查找所有的文件。</p>
<div><code>$ find / -name "*" -print</code></div>
<p>如果想在当前目录查找文件名以两个小写字母开头，跟着是两个数字，最后是.txt的文件，下面的命令就能够返回名为ax37.txt的文件：</p>
<div><code>$find . -name "[a-z][a-z][0--9][0--9].txt" -print</code></div>
<p><font size="3"><strong><br>
2、用perm选项<br>
</strong></font></p>
<p>按照文件权限模式用-perm选项,按文件权限模式来查找文件的话。最好使用八进制的权限表示法。</p>
<p>如在当前目录下查找文件权限位为755的文件，即文件属主可以读、写、执行，其他用户可以读、执行的文件，可以用：</p>
<div><code>$ find . -perm 755 -print</code></div>
<p>还有一种表达方法：在八进制数字前面要加一个横杠-，表示都匹配，如-007就相当于777，-006相当于666</p>
<div><code># ls -l<br>-rwxrwxr-x    2 sam      adm             0 10月 31 01:01 http3.conf<br>-rw-rw-rw-    1 sam      adm         34890 10月 31 00:57 httpd1.conf<br>-rwxrwxr-x    2 sam      adm             0 10月 31 01:01 httpd.conf<br>drw-rw-rw-    2 gem      group        4096 10月 26 19:48 sam<br>-rw-rw-rw-    1 root     root         2792 10月 31 20:19 temp<br><br># find . -perm 006<br># find . -perm -006<br>./sam<br>./httpd1.conf<br>./temp</code></div>
<p>-perm mode:文件许可正好符合mode</p>
<p>-perm +mode:文件许可部分符合mode</p>
<p>-perm -mode: 文件许可完全符合mode</p>
<p><font size="3"><strong><br>
3、忽略某个目录<br>
</strong></font></p>
<p>如果在查找文件时希望忽略某个目录，因为你知道那个目录中没有你所要查找的文件，那么可以使用-prune选项来指出需要忽略的目录。在使用-prune选项时要当心，因为如果你同时使用了-depth选项，那么-prune选项就会被find命令忽略。</p>
<p>如果希望在/apps目录下查找文件，但不希望在/apps/bin目录下查找，可以用：</p>
<div><code>$ find /apps -path "/apps/bin" -prune -o -print</code></div>
<p><font size="3"><strong><br>
4、使用find查找文件的时候怎么避开某个文件目录<br>
</strong></font></p>
<p>比如要在/usr/sam目录下查找不在dir1子目录之内的所有文件</p>
<div><code>find /usr/sam -path "/usr/sam/dir1" -prune -o -print </code></div>
<div><code>find [-path ..] [expression] 在路径列表的后面的是表达式</code></div>
<p>-path "/usr/sam" -prune -o -print 是 -path "/usr/sam" -a -prune -o<br>
-print 的简写表达式按顺序求值, -a 和 -o 都是短路求值，与 shell 的 &amp;&amp; 和 || 类似如果 -path
&quot;/usr/sam&quot; 为真，则求值 -prune , -prune 返回真，与逻辑表达式为真；否则不求值 -prune，与逻辑表达式为假。如果
-path &quot;/usr/sam&quot; -a -prune 为假，则求值 -print ，-print返回真，或逻辑表达式为真；否则不求值
-print，或逻辑表达式为真。</p>
<p>这个表达式组合特例可以用伪码写为</p>
<div><code>if -path &quot;/usr/sam&quot;  then<br>          -prune<br>else<br>          -print</code></div>
<p>避开多个文件夹</p>
<div><code>find /usr/sam \( -path /usr/sam/dir1 -o -path /usr/sam/file1 \) -prune -o -print</code></div>
<p>圆括号表示表达式的结合。<br>
</p>
<div><code>\ 表示引用，即指示 shell 不对后面的字符作特殊解释，而留给 find 命令去解释其意义。</code></div>
<p>查找某一确定文件，-name等选项加在-o 之后</p>
<div><code>#find /usr/sam  \(-path /usr/sam/dir1 -o -path /usr/sam/file1 \) -prune -o -name &quot;temp&quot; -print</code></div>
<p><font size="3"><strong><br>
5、使用user和nouser选项<br>
</strong></font></p>
<p>按文件属主查找文件，如在$HOME目录中查找文件属主为sam的文件，可以用：</p>
<div><code>$ find ~ -user sam -print</code></div>
<p>在/etc目录下查找文件属主为uucp的文件：</p>
<div><code>$ find /etc -user uucp -print</code></div>
<p>为了查找属主帐户已经被删除的文件，可以使用-nouser选项。这样就能够找到那些属主在/etc/passwd文件中没有有效帐户的文件。在使用-nouser选项时，不必给出用户名； find命令能够为你完成相应的工作。</p>
<p>例如，希望在/home目录下查找所有的这类文件，可以用：<br>
</p>
<div><code>$ find /home -nouser -print</code></div>
<p><font size="3"><strong><br>
6、使用group和nogroup选项<br>
</strong></font></p>
<p>就像user和nouser选项一样，针对文件所属于的用户组， find命令也具有同样的选项，为了在/apps目录下查找属于gem用户组的文件，可以用：</p>
<div><code>$ find /apps -group gem -print</code></div>
<p>要查找没有有效所属用户组的所有文件，可以使用nogroup选项。下面的find命令从文件系统的根目录处查找这样的文件</p>
<div><code>$ find / -nogroup-print</code></div>
<p><font size="3"><strong><br>
7、按照更改时间或访问时间等查找文件<br>
</strong></font></p>
<p>如果希望按照更改时间来查找文件，可以使用mtime,atime或ctime选项。如果系统突然没有可用空间了，很有可能某一个文件的长度在此期间增长迅速，这时就可以用mtime选项来查找这样的文件。</p>
<p>用减号-来限定更改时间在距今n日以内的文件，而用加号+来限定更改时间在距今n日以前的文件。</p>
<p>希望在系统根目录下查找更改时间在5日以内的文件，可以用：</p>
<div><code>$ find / -mtime -5 -print</code></div>
<p>为了在/var/adm目录下查找更改时间在3日以前的文件，可以用：</p>
<div><code>$ find /var/adm -mtime +3 -print</code></div>
<p><font size="3"><strong><br>
8、查找比某个文件新或旧的文件<br>
</strong></font></p>
<p>如果希望查找更改时间比某个文件新但比另一个文件旧的所有文件，可以使用-newer选项。它的一般形式为：</p>
<div><code>newest_file_name ! oldest_file_name</code></div>
<p>其中，！是逻辑非符号。</p>
<p>查找更改时间比文件sam新但比文件temp旧的文件：</p>
<p>例：有两个文件</p>
<div><code>-rw-r--r--    1 sam      adm             0 10月 31 01:07 fiel<br>-rw-rw-rw-    1 sam      adm         34890 10月 31 00:57 httpd1.conf<br>-rwxrwxr-x    2 sam      adm             0 10月 31 01:01 httpd.conf<br>drw-rw-rw-    2 gem      group        4096 10月 26 19:48 sam<br>-rw-rw-rw-    1 root     root         2792 10月 31 20:19 temp<br><br># find -newer httpd1.conf  ! -newer temp -ls<br>1077669    0 -rwxrwxr-x   2 sam      adm             0 10月 31 01:01 ./httpd.conf<br>1077671    4 -rw-rw-rw-   1 root     root         2792 10月 31 20:19 ./temp<br>1077673    0 -rw-r--r--   1 sam      adm             0 10月 31 01:07 ./fiel</code></div>
<p>查找更改时间在比temp文件新的文件：</p>
<div><code>$ find . -newer temp -print</code></div>
<br>
<font size="3"><strong><br>
9、使用type选项<br>
</strong></font>
<p>在/etc目录下查找所有的目录，可以用：</p>
<div><code>$ find /etc -type d -print</code></div>
<p>在当前目录下查找除目录以外的所有类型的文件，可以用：<br>
</p>
<div><code>$ find . ! -type d -print</code></div>
<p>在/etc目录下查找所有的符号链接文件，可以用<br>
</p>
<div><code>$ find /etc -type l -print</code></div>
<p><font size="3"><strong><br>
10、使用size选项<br>
</strong></font></p>
<p>可以按照文件长度来查找文件，这里所指的文件长度既可以用块（block）来计量，也可以用字节来计量。以字节计量文件长度的表达形式为N c；以块计量文件长度只用数字表示即可。</p>
<p>在按照文件长度查找文件时，一般使用这种以字节表示的文件长度，在查看文件系统的大小，因为这时使用块来计量更容易转换。<br>
在当前目录下查找文件长度大于1 M字节的文件：<br>
</p>
<div><code>$ find . -size +1000000c -print</code></div>
<p>在/home/apache目录下查找文件长度恰好为100字节的文件：</p>
<div><code>$ find /home/apache -size 100c -print</code></div>
<p>在当前目录下查找长度超过10块的文件（一块等于512字节）： </p>
<div><code>$ find . -size +10 -print</code></div>
<p><font size="3"><strong><br>
11、使用depth选项<br>
</strong></font></p>
<p>在使用find命令时，可能希望先匹配所有的文件，再在子目录中查找。使用depth选项就可以使find命令这样做。这样做的一个原因就是，当在使用find命令向磁带上备份文件系统时，希望首先备份所有的文件，其次再备份子目录中的文件。</p>
<p>在下面的例子中， find命令从文件系统的根目录开始，查找一个名为CON.FILE的文件。</p>
<p>它将首先匹配所有的文件然后再进入子目录中查找。</p>
<div><code>$ find / -name "CON.FILE" -depth -print</code></div>
<p><font size="3"><strong><br>
12、使用mount选项<br>
</strong></font></p>
<p>在当前的文件系统中查找文件（不进入其他文件系统），可以使用find命令的mount选项。</p>
<p>从当前目录开始查找位于本文件系统中文件名以XC结尾的文件：</p>
<div><code>$ find . -name "*.XC" -mount -print</code></div>
<p><br></p><img src="http://www.cppblog.com/API/aggbug/144344.html" width="1" height="1"><br><br><div align="right"><a style="text-decoration:none" href="http://www.cppblog.com/API/">C++技术中心</a> 2011-04-16 09:58 <a href="http://www.cppblog.com/API/archive/2011/04/16/144344.html#Feedback" style="text-decoration:none">发表评论</a></div>