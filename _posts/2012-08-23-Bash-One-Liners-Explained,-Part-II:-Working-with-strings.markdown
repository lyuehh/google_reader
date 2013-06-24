---
layout: post
title:  "Bash One-Liners Explained, Part II: Working with strings"
date:   2012-08-23 20:36:46
author: Peteris Krumins
categories: program
---

## Bash One-Liners Explained, Part II: Working with strings
### by Peteris Krumins
### at 2012-08-23 20:36:46
### original <http://feedproxy.google.com/~r/catonmat/~3/HL3b-lAPWk8/bash-one-liners-explained-part-two>

<p>This is the second part of the <strong>Bash One-Liners Explained</strong> article series. In this part I'll show you how to do various string manipulations with bash. I'll use only the best bash practices, various bash idioms and tricks. I want to illustrate how to get various tasks done with just bash built-in commands and bash programming language constructs.</p>
<p>See the <a href="http://www.catonmat.net/blog/bash-one-liners-explained-part-one/">first part</a> of the series for introduction. After I'm done with the series I'll release an ebook (similar to my ebooks on <a href="http://www.catonmat.net/blog/awk-book/">awk</a>, <a href="http://www.catonmat.net/blog/sed-book/">sed</a>, and <a href="http://www.catonmat.net/blog/perl-book/">perl</a>), and also bash1line.txt (similar to my <a href="http://www.catonmat.net/download/perl1line.txt">perl1line.txt</a>).</p>
<p>Also see my other articles about working fast in bash from 2007 and 2008:</p>
<ul><li><a href="http://www.catonmat.net/blog/bash-emacs-editing-mode-cheat-sheet/">Working Productively in Bash's Emacs Command Line Editing Mode (comes with a cheat sheet)</a></li>
<li><a href="http://www.catonmat.net/blog/bash-vi-editing-mode-cheat-sheet/">Working Productively in Bash's Vi Command Line Editing Mode (comes with a cheat sheet)</a></li>
<li><a href="http://www.catonmat.net/blog/the-definitive-guide-to-bash-command-line-history/">The Definitive Guide to Bash Command Line History  (comes with a cheat sheet)</a></li>
</ul>
<p>Let's start.</p>
<h2>Part II: Working With Strings</h2>
<p><strong>1. Generate the alphabet from a-z</strong></p>
<pre>
$ echo {a..z}
</pre>
<p>This one-liner uses brace expansion. Brace expansion is a mechanism for generating arbitrary strings. This one-liner uses a sequence expression of the form {x..y}, where x and y are single characters. The sequence expression expands to each character lexicographically between x and y, inclusive.</p>
<p>If you run it, you get all the letters from a-z:</p>
<pre>
$ echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z
</pre>
<p><strong>2. Generate the alphabet from a-z without spaces between characters</strong></p>
<pre>
$ printf "%c" {a..z}
</pre>
<p>This is an awesome bash trick that 99.99% bash users don't know about. If you supply a list of items to the <code>printf</code> function it actually applies the format in a loop until the list is empty! <code>printf</code> as a loop! There is nothing more awesome than that!</p>
<p>In this one-liner the printf format is <code>"%c"</code>, which means "a character" and the arguments are all letters from a-z separated by space. So what <code>printf</code> does is it iterates over the list outputting each character after character until it runs out of letters.</p>
<p>Here is the output if you run it:</p>
<pre>
abcdefghijklmnopqrstuvwxyz
</pre>
<p>This output is without a terminating newline because the format string was <code>"%c"</code> and it doesn't include <code>\n</code>. To have it newline terminated, just add <code>$'\n'</code> to the list of chars to print:</p>
<pre>
$ printf "%c" {a..z} $'\n'
</pre>
<p><code>$'\n'</code> is bash idiomatic way to represent a newline character. <code>printf</code> then just prints chars a to z, and the newline character.</p>
<p>Another way to add a trailing newline character is to echo the output of printf:</p>
<pre>
$ echo $(printf "%c" {a..z})
</pre>
<p>This one-liner uses command substitution, which runs <code>printf "%c" {a..z}</code> and replaces the command with its output. Then <code>echo</code> prints this output and adds a newline itself.</p>
<p>Want to output all letters in a column instead? Add a newline after each character!</p>
<pre>
$ printf "%c\n" {a..z}
</pre>
<p>Output:</p>
<pre>
a
b
...
z
</pre>
<p>Want to put the output from <code>printf</code> in a variable quickly? Use the <code>-v</code> argument:</p>
<pre>
$ printf -v alphabet "%c" {a..z}
</pre>
<p>This puts <code>abcdefghijklmnopqrstuvwxyz</code> in the <code>$alphabet</code> variable.</p>
<p>Similarly you can generate a list of numbers. Let's say from 1 to 100:</p>
<pre>
$ echo {1..100}
</pre>
<p>Output:</p>
<pre>
1 2 3 ... 100
</pre>
<p>Alternatively, if you forget this method, you can use the external <code>seq</code> utility to generate a sequence of numbers:</p>
<pre>
$ seq 1 100
</pre>
<p><strong>3. Pad numbers 0 to 9 with a leading zero</strong></p>
<pre>
$ printf "%02d " {0..9}
</pre>
<p>Here we use the looping abilities of <code>printf</code> again. This time the format is <code>"%02d "</code>, which means "zero pad the integer up to two positions", and the items to loop through are the numbers 0-9, generated by the brace expansion (as explained in the previous one-liner).</p>
<p>Output:</p>
<pre>
00 01 02 03 04 05 06 07 08 09
</pre>
<p>If you use bash 4, you can do the same with the new feature of brace expansion:</p>
<pre>
$ echo {00..09}
</pre>
<p>Older bashes don't have this feature.</p>
<p><strong>4. Produce 30 English words</strong></p>
<pre>
$ echo {w,t,}h{e{n{,ce{,forth}},re{,in,fore,with{,al}}},ither,at}
</pre>
<p>This is an abuse of brace expansion. Just look at what this produces:</p>
<pre>
when whence whenceforth where wherein wherefore wherewith wherewithal whither what then thence thenceforth there therein therefore therewith therewithal thither that hen hence henceforth here herein herefore herewith herewithal hither hat
</pre>
<p>Crazy awesome!</p>
<p>Here is how it works - you can produce permutations of words/symbols with brace expansion. For example, if you do this,</p>
<pre>
$ echo {a,b,c}{1,2,3}
</pre>
<p>It will produce the result <code>a1 a2 a3 b1 b2 b3 c1 c2 c3</code>. It takes the first <code>a</code>, and combines it with <code>{1,2,3}</code>, producing <code>a1 a2 a3</code>. Then it takes <code>b</code> and combines it with <code>{1,2,3}</code>, and then it does the same for <code>c</code>.</p>
<p>So this one-liner is just a smart combination of braces that when expanded produce all these English words!</p>
<p><strong>5. Produce 10 copies of the same string</strong></p>
<pre>
$ echo foo{,,,,,,,,,,}
</pre>
<p>This one-liner uses the brace expansion again. What happens here is <code>foo</code> gets combined with 10 empty strings, so the output is 10 copies of <code>foo</code>:</p>
<pre>
foo foo foo foo foo foo foo foo foo foo foo
</pre>
<p><strong>6. Join two strings</strong></p>
<pre>
$ echo "$x$y"
</pre>
<p>This one-liner simply concatenates two variables together. If the variable <code>x</code> contains <code>foo</code> and <code>y</code> contains <code>bar</code> then the result is <code>foobar</code>.</p>
<p>Notice that <code>"$x$y"</code> were quoted. If we didn't quote it, <code>echo</code> would interpret the <code>$x$y</code> as regular arguments, and would first try to parse them to see if they contain command line switches. So if <code>$x</code> contains something beginning with <code>-</code>, it would be a command line argument rather than an argument to echo:</p>
<pre>
x=-n
y=" foo"
echo $x$y
</pre>
<p>Output:</p>
<pre>
foo
</pre>
<p>Versus the correct way:</p>
<pre>
x=-n
y=" foo"
echo "$x$y"
</pre>
<p>Output:</p>
<pre>
-n foo
</pre>
<p>If you need to put the two joined strings in a variable, you can omit the quotes:</p>
<pre>
var=$x$y
</pre>
<p><strong>7. Split a string on a given character</strong></p>
<p>Let's say you have a string <code>foo-bar-baz</code> in the variable <code>$str</code> and you wish to split it on the dash and iterate over it. You can simply combine <code>IFS</code> with <code>read</code> to do it:</p>
<pre>
$ IFS=- read -r x y z &lt;&lt;&lt; &quot;$str&quot;
</pre>
<p>Here we use the <code>read x</code> command that reads data from stdin and puts the data in the <code>x y z</code> variables. We set <code>IFS</code> to <code>-</code> as this variable is used for field splitting. If multiple variable names are specified to <code>read</code>, <code>IFS</code> is used to split the line of input so that each variable gets a single field of the input.</p>
<p>In this one-liner <code>$x</code> gets <code>foo</code>, <code>$y</code> gets <code>bar</code>, <code>$z</code> gets <code>baz</code>.</p>
<p>Also notice the use of <code>&lt;&lt;&lt;</code> operator. This is the here-string operator that allows strings to be passed to stdin of commands easily. In this case string <code>$str</code> is passed as stdin to <code>read</code>.</p>
<p>You can also put the split fields and put them in an array:</p>
<pre>
$ IFS=- read -ra parts &lt;&lt;&lt; &quot;foo-bar-baz&quot;
</pre>
<p>The <code>-a</code> argument to <code>read</code> makes it put the split words in the given array. In this case the array is <code>parts</code>. You can access array elements through <code>${parts[0]}</code>, <code>${parts[1]}</code>, and <code>${parts[0]}</code>. Or just access all of them through <code>${parts[@]}</code>.</p>
<p><strong>8. Process a string character by character</strong></p>
<pre>
$ while IFS= read -rn1 c; do
    # do something with $c
done &lt;&lt;&lt; &quot;$str&quot;
</pre>
<p>Here we use the <code>-n1</code> argument to <code>read</code> command to make it read the input character at a time. Similarly we can use <code>-n2</code> to read two chars at a time, etc.</p>
<p><strong>9. Replace "foo" with "bar" in a string</strong></p>
<pre>
$ echo ${str/foo/bar}
</pre>
<p>This one-liner uses parameter expansion of form <code>${var/find/replace}</code>. It finds the string <code>find</code> in <code>var</code> and replaces it with <code>replace</code>. Really simple!</p>
<p>To replace all occurrences of "foo" with "bar", use the <code>${var//find/replace}</code> form:</p>
<pre>
$ echo ${str//foo/bar}
</pre>
<p><strong>10. Check if a string matches a pattern</strong></p>
<pre>
$ if [[ $file = *.zip ]]; then
    # do something
fi
</pre>
<p>Here the one-liner does something if <code>$file</code> matches <code>*.zip</code>. This is a simple glob pattern matching, and you can use symbols <code>* ? [...]</code> to do matching. Code <code>*</code> matches any string, <code>?</code> matches a single char, and <code>[...]</code> matches any character in <code>...</code> or a character class.</p>
<p>Here is another example that matches if answer is <code>Y</code> or <code>y</code>:</p>
<pre>
$ if [[ $answer = [Yy]* ]]; then
    # do something
fi
</pre>
<p><strong>11. Check if a string matches a regular expression</strong></p>
<pre>
$ if [[ $str =~ [0-9]+\.[0-9]+ ]]; then
    # do something
fi
</pre>
<p>This one-liner tests if the string <code>$str</code> matches regex <code>[0-9]+\.[0-9]+</code>, which means match a number followed by a dot followed by number. The format for regular expressions is described in <code>man 3 regex</code>.</p>
<p><strong>12. Find the length of the string</strong></p>
<pre>
$ echo ${#str}
</pre>
<p>Here we use parameter expansion <code>${#str}</code> which returns the length of the string in variable <code>str</code>. Really simple.</p>
<p><strong>13. Extract a substring from a string</strong></p>
<pre>
$ str="hello world"
$ echo ${str:6}
</pre>
<p>This one-liner extracts <code>world</code> from <code>hello world</code>. It uses the substring expansion. In general substring expansion looks like <code>${var:offset:length}</code>, and it extracts <code>length</code> characters from <code>var</code> starting at index <code>offset</code>. In our one-liner we omit the <code>length</code> that makes it extract all characters starting at offset <code>6</code>.</p>
<p>Here is another example:</p>
<pre>
$ echo ${str:7:2}
</pre>
<p>Output:</p>
<pre>
or
</pre>
<p><strong>14. Uppercase a string</strong></p>
<pre>
$ declare -u var
$ var="foo bar"
</pre>
<p>The <code>declare</code> command in bash declares variables and/or gives them attributes. In this case we give the variable <code>var</code> attribute <code>-u</code>, which upper-cases its content whenever it gets assigned something. Now if you echo it, the contents will be upper-cased:</p>
<pre>
$ echo $var
FOO BAR
</pre>
<p>Note that <code>-u</code> argument was introduced in bash 4. Similarly you can use another feature of bash 4, which is the <code>${var^^}</code> parameter expansion that upper-cases a string in <code>var</code>:</p>
<pre>
$ str="zoo raw"
$ echo ${str^^}
</pre>
<p>Output:</p>
<pre>
ZOO RAW
</pre>
<p><strong>15. Lowercase a string</strong></p>
<pre>
$ declare -l var
$ var="FOO BAR"
</pre>
<p>Similar to the previous one-liner, <code>-l</code> argument to <code>declare</code> sets the lower-case attribute on <code>var</code>, which makes it always be lower-case:</p>
<pre>
$ echo $var
foo bar
</pre>
<p>The <code>-l</code> argument is also available only in bash 4 and later.</p>
<p>Another way to lowercase a string is to use <code>${var,,}</code> parameter expansion:</p>
<pre>
$ str="ZOO RAW"
$ echo ${str,,}
</pre>
<p>Output:</p>
<pre>
zoo raw
</pre>
<h2>Enjoy!</h2>
<p>Enjoy the article and let me know in the comments what you think about it! If you think that I forgot some interesting bash one-liners related to string operations, let me know in the comments also!</p>
<div>
<a href="http://feeds.feedburner.com/~ff/catonmat?a=HL3b-lAPWk8:8fTEJjSGKnc:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/catonmat?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=HL3b-lAPWk8:8fTEJjSGKnc:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/catonmat?i=HL3b-lAPWk8:8fTEJjSGKnc:F7zBnMyn0Lo" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=HL3b-lAPWk8:8fTEJjSGKnc:gIN9vFwOqvQ"><img src="http://feeds.feedburner.com/~ff/catonmat?i=HL3b-lAPWk8:8fTEJjSGKnc:gIN9vFwOqvQ" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=HL3b-lAPWk8:8fTEJjSGKnc:XiUCZPyL81w"><img src="http://feeds.feedburner.com/~ff/catonmat?d=XiUCZPyL81w" border="0"></a> <a href="http://feeds.feedburner.com/~ff/catonmat?a=HL3b-lAPWk8:8fTEJjSGKnc:DhrJZwOgkxs"><img src="http://feeds.feedburner.com/~ff/catonmat?d=DhrJZwOgkxs" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/catonmat/~4/HL3b-lAPWk8" height="1" width="1">