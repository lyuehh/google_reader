---
layout: post
title:  "如何生成一段Typoglycemia文本（解答）"
date:   2012-11-12 00:32:41
author: jeffz@live.com (老赵)
categories: program
---

## 如何生成一段Typoglycemia文本（解答）
### by jeffz@live.com (老赵)
### at 2012-11-12 00:32:41
### original <http://blog.zhaojie.me/2012/11/how-to-generate-typoglycemia-text-result.html>

<p>关于“<a href="http://blog.zhaojie.me/2012/11/how-to-generate-typoglycemia-text.html">生成Tygolycemia文本</a>”这个问题，已经有许多同学给了答案。不过，似乎大部分同学都只是以“完成”作为目标，并没有在解法的效率和内存占用上做过多考虑。其实对于这种非常简单的问题，给出一个解法并没有太大意义，而是要从中获取一些经验，否则就算反复解决这类简单的问题，得到的进步依然十分有限。</p>

<p>我们很容易想到，解决这个问题只需要从头至尾遍历这个字符串，找到需要重排的单词，并加以处理即可，所以这题的关键在于如何控制内存的占用以及内存复制大小。我们可以举一个最经典的例子：假如我们要将<code>M</code>个<code>N</code>字节的字符串拼接为一个<code>M * N</code>字节的大字符串，则直接使用加号依次进行连接，则会累计开辟及复制<code>2 * N + 3 * N + ... + (M - 1) * N</code>，即规模在<code>M<sup>2</sup> * N</code>的数据（尽管内存会被GC不断回收，但复制是绝对省不下来，而且这对GC来说也是极大负担）。但是，假如我们使用<code>StringBuilder</code>，并直接开辟<code>M * N</code>的容量，则只需要额外复制<code>M * N</code>的数据。更进一步，假如我们使用<code>String.Concat(string[])</code>来拼接字符串，则一点额外的复制操作都不需要就能得到最终的字符串。可见，完成同样的工作，占用的内存空间以及内存复制大小是天差地远的。</p>

<p>估计是被强调太多次的缘故，说起此类字符串操作，大部分同学都倾向于使用<code>StringBuilder</code>，但这真是万能的解决方案吗？其实我很多年前就做过实验（<a href="http://blog.zhaojie.me/2009/11/string-concat-perf-1-benchmark.html">一</a>、<a href="http://blog.zhaojie.me/2009/12/string-concat-perf-2-stringbuilder-implementations.html">二</a>、<a href="http://blog.zhaojie.me/2009/12/string-concat-perf-3-profiling-analysis.html">三</a>），强调了在可以使用<code>String.Concat</code>的情况下，它的性能会比<code>StringBuilder</code>更好。<code>StringBuilder</code>更适合在一些动态的环境里，生成一些长度不固定的字符串。另外，很多同学都不重视<code>StringBuilder</code>的初始容量（同理还有<code>List&lt;T&gt;</code>的初始容量），这都是会影响性能的因素。所以，我们除了要知道<code>StringBuilder</code>这个东西以外，还应该知道它的适用场景，其实这些结论都很简单，完全不用死记硬背，靠常理推断即可。</p>

<p>就拿如今的Typoglycemia问题来说，使用一个<code>StringBuilder</code>并非是一个最好的选择，因为在得到初始字符串的情况下，目标字符串的长度也已经完全可以确定下来，换句话说，我们完全可以直接用一个<code>char[]</code>来保存内容，进行处理，并直接生成字符串，这样我们只需要一份额外的字符串空间即可。当然，用<code>StringBuilder</code>时也可以直接指定足够的容量以避免不断扩容，但直接将字符串转为<code>char[]</code>则意味着一次性的，连续的内存复制，而用<code>StringBuilder</code>以后便会以小段小段方式复制内存，性能高低一目了然。</p>

<p>用<code>char[]</code>进行处理还有额外的好处，便是完全无需生成小段的字符串对象。有的同学处理这个问题的时候直接就用上<code>String.Split</code>来拆分字符串，这其实只会对性能带来负面影响。<a href="http://www.bluebytesoftware.com/blog/2012/10/31/BewareTheString.aspx">著名的Joe Duffy最近便写了一篇文章</a>，谈到字符串处理时别总是为了图省事，以免“将一个IO密集型应用活活变成了CPU密集型”。在这里假如我们用上了<code>StringBuilder</code>，即便没有动态扩容，也势必会将源字符串拆分为一小段一小段，造成额外的内存复制及GC压力。</p>

<p>因此，我们的代码其实大致会是这样的结构：</p>

<pre><span style="color:blue">static string </span>MakeTypoglycemia(<span style="color:blue">string </span>text)
{
    <span style="color:blue">var </span>charArray = text.ToCharArray();

    <span style="color:green">// 处理charArray内容

    </span><span style="color:blue">return new string</span>(charArray);
}</pre>


<p>剩下的，便只要找准位置，并打乱即可。我这里采取的策略其实十分简单，只要设定一个下标<code>index</code>（初始值为0），并重复以下三步即可：</p>

<ol>
  <li>从当前<code>index</code>开始，向后找到第一个字母，作为单词的起始位置。假如找不到字母，则表明处理完毕。 </li>

  <li>从当前<code>index</code>开始，向后寻找单词的分隔字符（空白字符），并随时记录最后一个字母的位置<code>lastLetterIndex</code>（因为空白符前面可能是标点符号）。 </li>

  <li><code>index</code>和<code>lastLetterIndex</code>之间便是需要打乱的单词。打乱后，将<code>index</code>设为分隔字符的下一个位置。 </li>
</ol>

<p>于是<code>MakeTypoglycemia</code>方法的代码便呼之欲出了：</p>

<pre><span style="color:blue">static string </span>MakeTypoglycemia(<span style="color:blue">string </span>text)
{
    <span style="color:blue">var </span>charArray = text.ToCharArray();

    <span style="color:blue">var </span>index = 0;
    <span style="color:blue">do
    </span>{
        index = FindLetter(charArray, index);
        <span style="color:blue">if </span>(index &gt;= charArray.Length) <span style="color:blue">break</span>;

        <span style="color:blue">int </span>lastLetterIndex;
        <span style="color:blue">var </span>spaceIndex = FindSeparator(charArray, index + 1, <span style="color:blue">out </span>lastLetterIndex);

        <span style="color:blue">if </span>(lastLetterIndex - index &gt; 2)
        {
            Shuffle(charArray, index + 1, lastLetterIndex - 1);
        }

        index = spaceIndex + 1;

    } <span style="color:blue">while </span>(index &lt; charArray.Length);

    <span style="color:blue">return new string</span>(charArray);
}</pre>

<p>其中用到两个辅助方法<code>FindLetter</code>及<code>FindSeparator</code>，都十分简单：</p>

<pre><span style="color:blue">static int </span>FindLetter(<span style="color:blue">char</span>[] charArray, <span style="color:blue">int </span>startIndex)
{
    <span style="color:blue">for </span>(<span style="color:blue">var </span>i = startIndex; i &lt; charArray.Length; i++)
    {
        <span style="color:blue">if </span>(<span style="color:#2b91af">Char</span>.IsLetter(charArray[i])) <span style="color:blue">return </span>i;
    }

    <span style="color:blue">return </span>charArray.Length;
}

<span style="color:blue">static int </span>FindSeparator(<span style="color:blue">char</span>[] charArray, <span style="color:blue">int </span>startIndex, <span style="color:blue">out int </span>lastLetterIndex)
{
    lastLetterIndex = -1;

    <span style="color:blue">for </span>(<span style="color:blue">var </span>i = startIndex; i &lt; charArray.Length; i++)
    {
        <span style="color:blue">var </span>ch = charArray[i];

        <span style="color:blue">if </span>(<span style="color:#a31515">&quot; \t\r\n&quot;</span>.IndexOf(ch) &gt;= 0) <span style="color:blue">return </span>i;

        <span style="color:blue">if </span>(<span style="color:#2b91af">Char</span>.IsLetter(ch)) lastLetterIndex = i;
    }

    <span style="color:blue">return </span>charArray.Length;
}</pre>

<p>这里还用到一个<code>Shuffle</code>方法就不多解释了，一个传统的数组随机化的实现而已，只是需要跳过非字母的字符（用于处理“couldn't”这种情况）：</p>

<pre><span style="color:blue">static readonly </span><span style="color:#2b91af">Random </span>Random = <span style="color:blue">new </span><span style="color:#2b91af">Random</span>(<span style="color:#2b91af">DateTime</span>.Now.Millisecond);

<span style="color:blue">static void </span>Shuffle(<span style="color:blue">char</span>[] charArray, <span style="color:blue">int </span>startIndex, <span style="color:blue">int </span>endIndex)
{
    <span style="color:blue">for </span>(<span style="color:blue">var </span>i = startIndex; i &lt;= endIndex; i++)
    {
        <span style="color:blue">if </span>(!<span style="color:#2b91af">Char</span>.IsLetter(charArray[i])) <span style="color:blue">continue</span>;

        <span style="color:blue">var </span>index = Random.Next(startIndex, endIndex + 1);
        <span style="color:blue">if </span>(!<span style="color:#2b91af">Char</span>.IsLetter(charArray[index])) <span style="color:blue">continue</span>;

        <span style="color:blue">var </span>temp = charArray[index];
        charArray[index] = charArray[i];
        charArray[i] = temp;
    }
}</pre>

<p>从中可以看出，其实这样一个高效的实现也可以十分清晰而简单，可以说并没有因为追求效率而引入任何复杂度。使用这种做法，我们只用了一份额外的字符串大小的内存（即一个<code>char[]</code>对象）便得到了目标字符串，那么我们能否把这一份额外的内存开销也省下来呢？答案是肯定的，不过这个问题我们下次再谈。</p>

<p>最后说个有趣的东西：假如不追求效率，并且假设输入文本完全是用空格分割的字母，这个问题还有更好玩的“一行解法”，例如：</p>

<pre><span style="color:blue">static string </span>MakeTypoglycemia(<span style="color:blue">string </span>text)
{
    <span style="color:blue">var </span>random = <span style="color:blue">new </span><span style="color:#2b91af">Random</span>();
    <span style="color:blue">return </span><span style="color:#2b91af">String</span>.Join(<span style="color:#a31515">&quot;&quot;</span>, text.Split(<span style="color:#a31515">' '</span>).Select(s =&gt; s.Length &lt;= 3 ? s : s[0] + <span style="color:blue">new </span><span style="color:#2b91af">String</span>(s.Substring(1, s.Length - 2).OrderBy(_ =&gt; random.Next()).ToArray()) + s[s.Length - 1]));
}</pre>

<p>以上代码完全借助.NET中内置的功能，并没有增加任何辅助方法。从效率的角度来说，这种做法当然不值一提。那么，您能否估计一下，这个做法到底会产生多少额外的内存复制？假设这是一个长度为N的字符串，其中所有的单词长度都大于3，并且总共有M个单词。</p>