---
layout: post
title:  "Bash One-Liners Explained, Part III: All about redirections"
date:   2012-08-30 07:07:15
author: Peteris Krumins
categories: program
---

## Bash One-Liners Explained, Part III: All about redirections
### by Peteris Krumins
### at 2012-08-30 07:07:15
### original <http://feedproxy.google.com/~r/catonmat/~3/GE_GrG9-R2g/bash-one-liners-explained-part-three>

<p>This is the third part of the <strong>Bash One-Liners Explained</strong> article series. In this part I'll teach you all about input/output redirection. I'll use only the best bash practices, various bash idioms and tricks. I want to illustrate how to get various tasks done with just bash built-in commands and bash programming language constructs.</p>
<p>See the <a href="http://www.catonmat.net/blog/bash-one-liners-explained-part-one/">first part</a> of the series for introduction. After I'm done with the series I'll release an ebook (similar to my ebooks on <a href="http://www.catonmat.net/blog/awk-book/">awk</a>, <a href="http://www.catonmat.net/blog/sed-book/">sed</a>, and <a href="http://www.catonmat.net/blog/perl-book/">perl</a>), and also bash1line.txt (similar to my <a href="http://www.catonmat.net/download/perl1line.txt">perl1line.txt</a>).</p>
<p>Also see my other articles about working fast in bash from 2007 and 2008:</p>
<ul><li><a href="http://www.catonmat.net/blog/bash-emacs-editing-mode-cheat-sheet/">Working Productively in Bash's Emacs Command Line Editing Mode (comes with a cheat sheet)</a></li>
<li><a href="http://www.catonmat.net/blog/bash-vi-editing-mode-cheat-sheet/">Working Productively in Bash's Vi Command Line Editing Mode (comes with a cheat sheet)</a></li>
<li><a href="http://www.catonmat.net/blog/the-definitive-guide-to-bash-command-line-history/">The Definitive Guide to Bash Command Line History  (comes with a cheat sheet)</a></li>
</ul>
<p>Let's start.</p>
<h2>Part III: Redirections</h2>
<p>Working with redirections in bash is really easy once you realize that it's all about manipulating file descriptors. When bash starts it opens the three standard file descriptors: stdin (file descriptor 0), stdout (file descriptor 1), and stderr (file descriptor 2). You can open more file descriptors (such as 3, 4, 5, ...), and you can close them. You can also copy file descriptors. And you can write to them and read from them.</p>
<p>File descriptors always point to some file (unless they're closed). Usually when bash starts all three file descriptors, stdin, stdout, and stderr, point to your terminal. The input is read from what you type in the terminal and both outputs are sent to the terminal.</p>
<p>Assuming your terminal is <code>/dev/tty0</code>, here is how the file descriptor table looks like when bash starts:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/initial-fd-table.png"></p>
</center>
<p>When bash runs a command it forks a child process (see <code>man 2 fork</code>) that inherits all the file descriptors from the parent process, then it sets up the redirections that you specified, and execs the command (see <code>man 3 exec</code>).</p>
<p>To be a pro at bash redirections all you need to do is visualize how the file descriptors get changed when redirections happen. The graphics illustrations will help you.</p>
<p><strong>1. Redirect the standard output of a command to a file</strong></p>
<pre>
$ command &gt;file
</pre>
<p>Operator <code>&gt;</code> is the output redirection operator. Bash first tries to open the file for writing and if it succeeds it sends the stdout of <code>command</code> to the newly opened file. If it fails opening the file, the whole command fails.</p>
<p>Writing <code>command &gt;file</code> is the same as writing <code>command 1&gt;file</code>. The number <code>1</code> stands for stdout, which is the file descriptor number for standard output.</p>
<p>Here is how the file descriptor table changes. Bash opens <code>file</code> and replaces file descriptor 1 with the file descriptor that points to <code>file</code>. So all the output that gets written to file descriptor 1 from now on ends up being written to <code>file</code>:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdout.png"></p>
</center>
<p>In general you can write <code>command n&gt;file</code>, which will redirect the file descriptor <code>n</code> to <code>file</code>.</p>
<p>For example,</p>
<pre>
$ ls &gt; file_list
</pre>
<p>Redirects the output of the <code>ls</code> command to the <code>file_list</code> file.</p>
<p><strong>2. Redirect the standard error of a command to a file</strong></p>
<pre>
$ command 2&gt; file
</pre>
<p>Here bash redirects the stderr to file. The number <code>2</code> stands for stderr.</p>
<p>Here is how the file descriptor table changes:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stderr.png"></p>
</center>
<p>Bash opens <code>file</code> for writing, gets the file descriptor for this file, and it replaces file descriptor 2 with the file descriptor of this file. So now anything written to stderr gets written to file.</p>
<p><strong>3. Redirect both standard output and standard error to a file</strong></p>
<pre>
$ command &amp;&gt;file
</pre>
<p>This one-liner uses the <code>&amp;&gt;</code> operator to redirect both output streams - stdout and stderr - from <code>command</code> to <code>file</code>. This is bash's shortcut for quickly redirecting both streams to the same destination.</p>
<p>Here is how the file descriptor table looks like after bash has redirected both streams:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdout-stderr.png"></p>
</center>
<p>As you can see both stdout and stderr now point to <code>file</code>. So anything written to stdout and stderr gets written to <code>file</code>.</p>
<p>There are several ways to redirect both streams to the same destination. You can redirect each stream one after another:</p>
<pre>
$ command &gt;file 2&gt;&amp;1
</pre>
<p>This is a much more common way to redirect both streams to a file. First stdout is redirected to <code>file</code>, and then stderr is duplicated to be the same as stdout. So both streams end up pointing to <code>file</code>.</p>
<p>When bash sees several redirections it processes them from left to right. Let's go through the steps and see how that happens. Before running any commands bash's file descriptor table looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/initial-fd-table.png"></p>
</center>
<p>Now bash processes the first redirection <code>&gt;file</code>. We've seen this before and it makes stdout point to <code>file</code>:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdout.png"></p>
</center>
<p>Next bash sees the second redirection <code>2&gt;&amp;1</code>. We haven't seen this redirection before. This one duplicates file descriptor 2 to be a copy of file descriptor 1 and we get:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdout-stderr.png"></p>
</center>
<p>Both streams have been redirected to <code>file</code>.</p>
<p>However be careful here! Writing:</p>
<pre>
command &gt;file 2&gt;&amp;1
</pre>
<p>Is not the same as writing:</p>
<pre>
$ command 2&gt;&amp;1 &gt;file
</pre>
<p>The order of redirects matters in bash! This command redirects only the standard output to the file. The stderr will still print to the terminal. To understand why that happens, let's go through the steps again. So before running the command the file descriptor table looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/initial-fd-table.png"></p>
</center>
<p>Now bash processes redirections left to right. It first sees <code>2&gt;&amp;1</code> so it duplicates stderr to stdout. The file descriptor table becomes:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/duplicate-stderr-stdout.png"></p>
</center>
<p>Now bash sees the second redirect <code>&gt;file</code> and it redirects stdout to <code>file</code>:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/duplicate-stderr-stdout-stdout-file.png"></p>
</center>
<p>Do you see what happens here? Stdout now points to <code>file</code> but the stderr still points to the terminal! Everything that gets written to stderr still gets printed out to the screen! So be very, very careful with the order of redirects!</p>
<p>Also note that in bash, writing this:</p>
<pre>
$ command &amp;&gt;file
</pre>
<p>Is exactly the same as:</p>
<pre>
$ command &gt;&amp;file
</pre>
<p>The first form is preferred however.</p>
<p><strong>4. Discard the standard output of a command</strong></p>
<pre>
$ command &gt; /dev/null
</pre>
<p>The special file <code>/dev/null</code> discards all data written to it. So what we're doing here is redirecting stdout to this special file and it gets discarded. Here is how it looks from the file descriptor table's perspective:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdout-dev-null.png"></p>
</center>
<p>Similarly, by combining the previous one-liners, we can discard both stdout and stderr by doing:</p>
<pre>
$ command &gt;/dev/null 2&gt;&amp;1
</pre>
<p>Or just simply:</p>
<pre>
$ command &amp;&gt;/dev/null
</pre>
<p>File descriptor table for this feat looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdout-stderr-dev-null.png"></p>
</center>
<p><strong>5. Redirect the contents of a file to the stdin of a command</strong></p>
<pre>
$ command &lt;file
</pre>
<p>Here bash tries to open the file for reading before running any commands. If opening the file fails, bash quits with error and doesn't run the command. If opening the file succeeds, bash uses the file descriptor of the opened file as the stdin file descriptor for the command.</p>
<p>After doing that the file descriptor table looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/redirect-stdin.png"></p>
</center>
<p>Here is an example. Suppose you want to read the first line of the file in a variable. You can simply do this:</p>
<pre>
$ read -r line &lt; file
</pre>
<p>Bash's built-in <code>read</code> command reads a single line from standard input. By using the input redirection operator <code>&lt;</code> we set it up to read the line from the file.</p>
<p><strong>6. Redirect a bunch of text to the stdin of a command</strong></p>
<pre>
$ command &lt;&lt;EOL
your
multi-line
text
goes
here
EOL
</pre>
<p>Here we use the here-document redirection operator <code>&lt;&lt;MARKER</code>. This operator instructs bash to read the input from stdin until a line containing only <code>MARKER</code> is found. At this point bash passes the all the input read so far to the stdin of the command.</p>
<p>Here is a common example. Suppose you've copied a bunch of URLs to the clipboard and you want to remove <code>http://</code> part of them. A quick one-liner to do this would be:</p>
<pre>
$ sed &#39;s|http://||&#39; &lt;&lt;EOL
http://url1.com
http://url2.com
http://url3.com
EOL
</pre>
<p>Here the input of a list of URLs is redirected to the <code>sed</code> command that strips <code>http://</code> from the input.</p>
<p>This example produces this output:</p>
<pre>
url1.com
url2.com
url3.com
</pre>
<p><strong>7. Redirect a single line of text to the stdin of a command</strong></p>
<pre>
$ command &lt;&lt;&lt; &quot;foo bar baz&quot;
</pre>
<p>For example, let's say you quickly want to pass the text in your clipboard as the stdin to a command. Instead of doing something like:</p>
<pre>
$ echo "clipboard contents" | command
</pre>
<p>You can now just write:</p>
<pre>
$ command &lt;&lt;&lt; &quot;clipboard contents&quot;
</pre>
<p>This trick changed my life when I learned it!</p>
<p><strong>8. Redirect stderr of all commands to a file forever</strong></p>
<pre>
$ exec 2&gt;file
$ command1
$ command2
$ ...
</pre>
<p>This one-liner uses the built-in <code>exec</code> bash command. If you specify redirects after it, then they will last forever, meaning until you change them or exit the script/shell.</p>
<p>In this case the <code>2&gt;file</code> redirect is setup that redirects the stderr of the current shell to the <code>file</code>. Running commands after setting up this redirect will have the stderr of all of them redirected to file. It's really useful in situations when you want to have a complete log of all errors that happened in the script, but you don't want to specify <code>2&gt;file</code> after every single command!</p>
<p>In general <code>exec</code> can take an optional argument of a command. If it's specified, bash replaces itself with the command. So what you get is only that command running, and there is no more shell.</p>
<p><strong>9. Open a file for reading using a custom file descriptor</strong></p>
<pre>
$ exec 3&lt;file
</pre>
<p>Here we use the <code>exec</code> command again and specify the <code>3&lt;file</code> redirect to it. What this does is opens the file for reading and assigns the opened file-descriptor to the shell's file descriptor number <code>3</code>. The file descriptor table now looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/custom-fd.png"></p>
</center>
<p>Now you can read from the file descriptor <code>3</code>, like this:</p>
<pre>
$ read -u 3 line
</pre>
<p>This reads a line from the file that we just opened as fd <code>3</code>.</p>
<p>Or you can use regular shell commands such as <code>grep</code> and operate on file descriptor <code>3</code>:</p>
<pre>
$ grep &quot;foo&quot; &lt;&amp;3
</pre>
<p>What happens here is file descriptor <code>3</code> gets duplicated to file descriptor <code>1</code> - stdin of grep. Just remember that once you read the file descriptor it's been exhausted and you need to close it and open it again to use it. (You can't rewind an fd in bash.)</p>
<p>After you're done using fd <code>3</code>, you can close it this way:</p>
<pre>
$ exec 3&gt;&amp;-
</pre>
<p>Here the file descriptor <code>3</code> is duped to <code>-</code>, which is bash's special way to say "close this fd".</p>
<p><strong>10. Open a file for writing using a custom file descriptor</strong></p>
<pre>
$ exec 4&gt;file
</pre>
<p>Here we simply tell bash to open file for writing and assign it number <code>4</code>. The file descriptor table looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/custom-fd-writing.png"></p>
</center>
<p>As you can see file descriptors don't have to be used in order, you can open any file descriptor number you like from 0 to 255.</p>
<p>Now we can simply write to the file descriptor <code>4</code>:</p>
<pre>
$ echo &quot;foo&quot; &gt;&amp;4
</pre>
<p>And we can close the file descriptor <code>4</code>:</p>
<pre>
$ exec 4&gt;&amp;-
</pre>
<p>It's so simple now once we learned how to work with custom file descriptors!</p>
<p><strong>11. Open a file both for writing and reading</strong></p>
<pre>
$ exec 3&lt;&gt;file
</pre>
<p>Here we use bash's diamond operator <code>&lt;&gt;</code>. The diamond operator opens a file descriptor for both reading and writing.</p>
<p>So for example, if you do this:</p>
<pre>
$ echo &quot;foo bar&quot; &gt; file   # write string &quot;foo bar&quot; to file &quot;file&quot;.
$ exec 5&lt;&gt; file           # open &quot;file&quot; for rw and assign it fd 5.
$ read -n 3 var &lt;&amp;5       # read the first 3 characters from fd 5.
$ echo $var
</pre>
<p>This will output <code>foo</code> as we just read the first 3 chars from the file.</p>
<p>Now we can write some stuff to the file:</p>
<pre>
$ echo -n + &gt;&amp;5           # write &quot;+&quot; at 4th position.
$ exec 5&gt;&amp;-               # close fd 5.
$ cat file
</pre>
<p>This will output <code>foo+bar</code> as we wrote the <code>+</code> char at 4th position in the file.</p>
<p><strong>12. Send the output from multiple commands to a file</strong></p>
<pre>
$ (command1; command2) &gt;file
</pre>
<p>This one-liner uses the <code>(commands)</code> construct that runs the commands a sub-shell. A sub-shell is a child process launched by the current shell.</p>
<p>So what happens here is the commands <code>command1</code> and <code>command2</code> get executed in the sub-shell, and bash redirects their output to <code>file</code>.</p>
<p><strong>13. Execute commands in a shell through a file</strong></p>
<p>Open two shells. In shell 1 do this:</p>
<pre>
mkfifo fifo
exec &lt; fifo
</pre>
<p>In shell 2 do this:</p>
<pre>
exec 3&gt; fifo;
echo &#39;echo test&#39; &gt;&amp;3
</pre>
<p>Now take a look in shell 1. It will execute <code>echo test</code>. You can keep writing commands to <code>fifo</code> and shell 1 will keep executing them.</p>
<p>Here is how it works.</p>
<p>In shell 1 we use the <code>mkfifo</code> command to create a named pipe called <code>fifo</code>. A named pipe (also called a FIFO) is similar to a regular pipe, except that it's accessed as part of the file system. It can be opened by multiple processes for reading or writing. When processes are exchanging data via the FIFO, the kernel passes all data internally without writing it to the file system. Thus, the FIFO special file has no contents on the file system; the file system entry merely serves as a reference point so that processes can access the pipe using a name in the file system.</p>
<p>Next we use <code>exec &lt; fifo</code> to replace current shell's stdin with <code>fifo</code>.</p>
<p>Now in shell 2 we open the named pipe for writing and assign it a custom file descriptor 3. Next we simply write <code>echo test</code> to the file descriptor 3, which goes to <code>fifo</code>.</p>
<p>Since shell 1's stdin is connected to this pipe it executes it! Really simple!</p>
<p><strong>14. Access a website through bash</strong></p>
<pre>
$ exec 3&lt;&gt;/dev/tcp/www.google.com/80
$ echo -e &quot;GET / HTTP/1.1\n\n&quot; &gt;&amp;3
$ cat &lt;&amp;3
</pre>
<p>Bash treats the <code>/dev/tcp/host/port</code> as a special file. It doesn't need to exist on your system. This special file is for opening tcp connections through bash.</p>
<p>In this example we first open file descriptor 3 for reading and writing and point it to <code>/dev/tcp/www.google.com/80</code> special file, which is a connection to <code>www.google.com</code> on port 80.</p>
<p>Next we write <code>GET / HTTP/1.1\n\n</code> to file descriptor 3. And then we simply read the response back from the same file descriptor by using <code>cat</code>.</p>
<p>Similarly you can create a UDP connection through <code>/dev/udp/host/port</code> special file.</p>
<p>With <code>/dev/tcp/host/port</code> you can even write things like port scanners in bash!</p>
<p><strong>15. Prevent overwriting the contents of a file when redirecting output</strong></p>
<pre>
$ set -o noclobber
</pre>
<p>This turns on the <code>noclobber</code> option for the current shell. The noclobber option prevents you from overwriting existing files with the &gt; operator.</p>
<p>If you try redirecting output to a file that exists, you'll get an error:</p>
<pre>
$ program &gt; file
bash: file: cannot overwrite existing file
</pre>
<p>If you're 100% sure that you want to overwrite the file, use the <code>&gt;|</code> redirection operator:</p>
<pre>
$ program &gt;| file
</pre>
<p>This succeeds as it overrides the noclobber option.</p>
<p><strong>16. Redirect standard input to a file and print it to standard output</strong></p>
<pre>
$ command | tee file
</pre>
<p>The <code>tee</code> command is super handy. It's not part of bash but you'll use it often. It takes an input stream and prints it both to standard output and to a file.</p>
<p>In this example it takes the stdout of command, puts it in file, and prints it to stdout.</p>
<p>Here is a graphical illustration of how it works:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/tee.png"></p>
</center>
<p><strong>17. Send stdout of one process to stdin of another process</strong></p>
<pre>
$ command1 | command2
</pre>
<p>This is simple piping. I'm sure everyone is familiar with this. I'm just including it here for completeness. Just to remind you, a pipe connects stdout of <code>command1</code> with the stdin of <code>command2</code>.</p>
<p>It can be illustrated with a graphic:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/pipe.png"></p>
</center>
<p>As you can see, everything sent to file descriptor 1 (stdout) of command1 gets redirected through a pipe to file descriptor 0 (stdin) of command2.</p>
<p>You can read more about pipes in <code>man 2 pipe</code>.</p>
<p><strong>18. Send stdout and stderr of one process to stdin of another process</strong></p>
<pre>
$ command1 |&amp; command2
</pre>
<p>This works on bash versions starting 4.0. The <code>|&amp;</code> redirection operator sends both stdout and stderr of command1 over a pipe to stdin of command2.</p>
<p>As the new features of bash 4.0 aren't widely used, the old, and more portable way to do the same is:</p>
<pre>
$ command1 2&gt;&amp;1 | command2
</pre>
<p>Here is an illustration that shows what happens with file descriptors:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/pipe-stdout-stderr.png"></p>
</center>
<p>First command1's stderr is redirected to stdout, and then a pipe is setup between command1's stdout and command2's stdin.</p>
<p><strong>19. Give file descriptors names</strong></p>
<pre>
$ exec {filew}&gt;output_file
</pre>
<p>Named file descriptors is a feature of bash 4.1. Named file descriptors look like <code>{varname}</code>. You can use them just like regular, numeric, file descriptors. Bash internally chooses a free file descriptor and assigns it a name.</p>
<p><strong>20. Order of redirections</strong></p>
<p>You can put the redirections anywhere in the command you wish. Check out these 3 examples, they all do the same:</p>
<pre>
$ echo hello &gt;/tmp/example

$ echo &gt;/tmp/example hello

$ &gt;/tmp/example echo hello
</pre>
<p>Got to love bash!</p>
<p><strong>21. Swap stdout and stderr</strong></p>
<pre>
$ command 3&gt;&amp;1 1&gt;&amp;2 2&gt;&amp;3
</pre>
<p>Here we first duplicate file descriptor 3 to be a copy of stdout. Then we duplicate stdout to be a copy of stderr, and finally we duplicate stderr to be a copy of file descriptor 3, which is stdout. As a result we've swapped stdout and stderr.</p>
<p>Let's go through each redirection with illustrations. Before running the command, we've file descriptors pointing to the terminal:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/initial-fd-table.png"></p>
</center>
<p>Next bash setups <code>3&gt;&amp;1</code> redirection. This creates file descriptor 3 to be a copy of file descriptor 1:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/fd3-copy-of-fd1.png"></p>
</center>
<p>Next bash setups <code>1&gt;&amp;2</code> redirection. This makes file descriptor 1 to be a copy of file descriptor 2:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/fd1-copy-of-fd2.png"></p>
</center>
<p>Next bash setups <code>2&gt;&amp;3</code> redirection. This makes file descriptor 2 to be a copy of file descriptor 3:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/fd2-copy-of-fd3.png"></p>
</center>
<p>If we want to be nice citizens we can also close file descriptor 3 as it's no longer needed:</p>
<pre>
$ command 3&gt;&amp;1 1&gt;&amp;2 2&gt;&amp;3 3&gt;&amp;-
</pre>
<p>The file descriptor table then looks like this:</p>
<center><p><img src="http://www.catonmat.net/images/bash-redirections/fd1-fd2-swap.png"></p>
</center>
<p>As you can see, file descriptors 1 and 2 have been swapped.</p>
<p><strong>22. Send stdout to one process and stderr to another process</strong></p>
<pre>
$ command &gt; &gt;(stdout_cmd) 2&gt; &gt;(stderr_cmd)
</pre>
<p>This one-liner uses process substitution. The <code>&gt;(...)</code> operator runs the commands in <code>...</code> with stdin connected to the read part of an anonymous named pipe. Bash replaces the operator with the filename of the anonymous pipe.</p>
<p>So for example, the first substitution <code>&gt;(stdout_cmd)</code> might return <code>/dev/fd/60</code>, and the second substitution might return <code>/dev/fd/61</code>. Both of these files are named pipes that bash created on the fly. Both named pipes have the commands as readers. The commands wait for someone to write to the pipes so they can read the data.</p>
<p>The command then looks like this:</p>
<pre>
$ command &gt; /dev/fd/60 2&gt; /dev/fd/61
</pre>
<p>Now these are just simple redirections. Stdout gets redirected to <code>/dev/fd/60</code>, and stderr gets redirected to <code>/dev/fd/61</code>.</p>
<p>When the command writes to stdout, the process behind <code>/dev/fd/60</code> (process <code>stdout_cmd</code>) reads the data. And when the command writes to stderr, the process behind <code>/dev/fd/61</code> (process <code>stderr_cmd</code>) reads the data.</p>
<p><strong>23. Find the exit codes of all piped commands</strong></p>
<p>Let's say you run several commands all piped together:</p>
<pre>
$ cmd1 | cmd2 | cmd3 | cmd4
</pre>
<p>And you want to find out the exit status codes of all these commands. How do you do it? There is no easy way to get the exit codes of all commands as bash returns only the exit code of the last command.</p>
<p>Bash developers thought about this and they added a special <code>PIPESTATUS</code> array that saves the exit codes of all the commands in the pipe stream.</p>
<p>The elements of the <code>PIPESTATUS</code> array correspond to the exit codes of the commands. Here's an example:</p>
<pre>
$ echo 'pants are cool' | grep 'moo' | sed 's/o/x/' | awk '{ print $1 }'
$ echo ${PIPESTATUS[@]}
0 1 0 0
</pre>
<p>In this example <code>grep 'moo'</code> fails, and the 2nd element of the <code>PIPESTATUS</code> array indicates failure.</p>
<h2>Shoutouts</h2>
<p>Shoutouts to bash hackers wiki for their <a href="http://wiki.bash-hackers.org/howto/redirection_tutorial">illustrated redirection tutorial</a> and <a href="http://wiki.bash-hackers.org/scripting/bashchanges">bash version changes</a>. </p>
<h2>Enjoy!</h2>
<p>Enjoy the article and let me know in the comments what you think about it! If you think that I forgot some interesting bash one-liners related to redirections, let me know in the comments below!</p>
<div>
<a href="http://feeds.feedburner.com/~ff/catonmat?a=GE_GrG9-R2g:VDz0PS9tIqE:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/catonmat?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=GE_GrG9-R2g:VDz0PS9tIqE:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/catonmat?i=GE_GrG9-R2g:VDz0PS9tIqE:F7zBnMyn0Lo" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=GE_GrG9-R2g:VDz0PS9tIqE:gIN9vFwOqvQ"><img src="http://feeds.feedburner.com/~ff/catonmat?i=GE_GrG9-R2g:VDz0PS9tIqE:gIN9vFwOqvQ" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=GE_GrG9-R2g:VDz0PS9tIqE:XiUCZPyL81w"><img src="http://feeds.feedburner.com/~ff/catonmat?d=XiUCZPyL81w" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=GE_GrG9-R2g:VDz0PS9tIqE:DhrJZwOgkxs"><img src="http://feeds.feedburner.com/~ff/catonmat?d=DhrJZwOgkxs" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/catonmat/~4/GE_GrG9-R2g" height="1" width="1">