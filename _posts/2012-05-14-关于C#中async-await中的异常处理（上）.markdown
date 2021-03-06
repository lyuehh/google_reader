---
layout: post
title:  "关于C#中async/await中的异常处理（上）"
date:   2012-05-14 22:16:32
author: jeffz@live.com (老赵)
categories: program
---

## 关于C#中async/await中的异常处理（上）
### by jeffz@live.com (老赵)
### at 2012-05-14 22:16:32
### original <http://blog.zhaojie.me/2012/04/exception-handling-in-csharp-async-await-1.html>

<p>在同步编程中，一旦出现错误就会抛出异常，我们可以使用try…catch来捕捉异常，而未被捕获的异常则会不断向上传递，形成一个简单而统一的错误处理机制。不过对于异步编程来说，异常处理一直是件麻烦的事情，这也是C#中async/await或是<a href="https://github.com/JeffreyZhao/jscex">Jscex</a>等异步编程模型的优势之一。但是，同步的错误处理机制，并不能完全避免异步形式的错误处理方式，这需要一定实践规范来保证，至少我们需要了解async/await到底是如何捕获和分发异常的。在开发Jscex的过程中，我也在C#内部邮件邮件列表中了解了很多关于TPL和C#异步特性的问题，错误处理也是其中之一。在此记录一下吧。</p>

<h1>使用try…catch捕获异常</h1>

<p>首先我们来看下这段代码：</p>

<pre><span style="color:blue">static async </span><span style="color:#2b91af">Task </span>ThrowAfter(<span style="color:blue">int </span>timeout, <span style="color:#2b91af">Exception </span>ex)
{
    <span style="color:blue">await </span><span style="color:#2b91af">Task</span>.Delay(timeout);
    <span style="color:blue">throw </span>ex;
}

<span style="color:blue">static void </span>PrintException(<span style="color:#2b91af">Exception </span>ex)
{
    <span style="color:#2b91af">Console</span>.WriteLine(<span style="color:maroon">&quot;Time: {0}\n{1}\n============&quot;</span>, _watch.Elapsed, ex);
}

<span style="color:blue">static </span><span style="color:#2b91af">Stopwatch </span>_watch = <span style="color:blue">new </span><span style="color:#2b91af">Stopwatch</span>();

<span style="color:blue">static async </span><span style="color:#2b91af">Task </span>MissHandling()
{
    <span style="color:blue">var </span>t1 = ThrowAfter(1000, <span style="color:blue">new </span><span style="color:#2b91af">NotSupportedException</span>(<span style="color:maroon">&quot;Error 1&quot;</span>));
    <span style="color:blue">var </span>t2 = ThrowAfter(2000, <span style="color:blue">new </span><span style="color:#2b91af">NotImplementedException</span>(<span style="color:maroon">&quot;Error 2&quot;</span>));

    <span style="color:blue">try
    </span>{
        <span style="color:blue">await </span>t1;
    }
    <span style="color:blue">catch </span>(<span style="color:#2b91af">NotSupportedException </span>ex)
    {
        PrintException(ex);
    }
}

<span style="color:blue">static void </span>Main(<span style="color:blue">string</span>[] args)
{
    _watch.Start();

    MissHandling();

    <span style="color:#2b91af">Console</span>.ReadLine();
}</pre>

<p>这段代码的输出如下：</p>

<pre>Time: 00:00:01.2058970
System.NotSupportedException: Error 1
   at AsyncErrorHandling.Program.d__0.MoveNext() in ...\Program.cs:line 16
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.GetResult()
   at AsyncErrorHandling.Program.d__3.MoveNext() in ...\Program.cs:line 33
============</pre>

<p>在MissingHandling方法中，我们首先使用ThrowAfter方法开启两个任务，它们会分别在一秒及两秒后抛出两个不同的异常。但是在接下来的try中，我们只对t1进行await操作。很容易理解，t1抛出的NotSupportedException将被catch捕获，耗时大约为1秒左右——当然，从上面的数据可以看出，其实t1在被“捕获”时已经耗费了1.2时间，误差较大。这是因为程序刚启动，TPL内部正处于“热身”状态，在调度上会有较大开销。这里反倒是另一个问题倒更值得关注：t2在两秒后抛出的NotImplementedException到哪里去了？</p>

<h1>未捕获的异常</h1>

<p>C#的async/await功能基于TPL的Task对象，每个await操作符都是“等待”一个Task完成。在之前（或者说如今）的TPL中，Task对象的析构函数会查看它的Exception对象有没有被“访问”过，如果没有，且Task对象出现了异常，则会抛出这个异常，最终导致的结果往往便是进程退出。因此，我们必须小心翼翼地处理每一个Task对象的错误，不得遗漏。<a href="http://msdn.microsoft.com/en-us/library/hh367887(v=VS.110).aspx#core">在.NET 4.5中这个行为被改变了</a>，对于任何没有被检查过的异常，便会触发TaskSchedular.UnobservedTaskException事件——如果您不监听这个事件，未捕获的异常也就这么无影无踪了。</p>

<p>为此，我们对Main方法进行一个简单的改造。</p>

<pre><span style="color:blue">static void </span>Main(<span style="color:blue">string</span>[] args)
{
    <span style="color:#2b91af">TaskScheduler</span>.UnobservedTaskException += (_, ev) =&gt; PrintException(ev.Exception);

    _watch.Start();

    MissHandling();

    <span style="color:blue">while </span>(<span style="color:blue">true</span>)
    {
        <span style="color:#2b91af">Thread</span>.Sleep(1000);
        <span style="color:#2b91af">GC</span>.Collect();
    }
}</pre>

<p>改造有两点，一是响应TaskScheduler.UnobservedTaskException，这自然不必多说。还有一点便是不断地触发垃圾回收，以便Finalizer线程调用析构函数。如今这段代码除了打印出之前的信息之外，还会输出以下内容：</p>

<pre>Time: 00:00:03.0984560
System.AggregateException: A Task&#39;s exception(s) were not observed either by Waiting on the Task or accessing its Exception property. As a result, the unobserved exception was rethrown by the finalizer thread. ---&gt; System.NotImplementedException: Error 2
   at AsyncErrorHandling.Program.d__0.MoveNext() in ...\Program.cs:line 16
   --- End of inner exception stack trace ---
---&gt; (Inner Exception #0) System.NotImplementedException: Error 2
   at AsyncErrorHandling.Program.d__0.MoveNext() in ...\Program.cs:line 16&lt;---
============</pre>

<p>从上面的信息中可以看出，UnobservedTaskException事件并非在“抛出”异常后便立即触发，而是在某次垃圾收集过程，从Finalizer线程里触发并执行。从中也不难得出这样的结论：便是该事件的响应方法不能过于耗时，更加不能阻塞，否则便会对程序性能造成灾难性的影响。</p>

<p>那么假如我们要同时处理t1和t2中抛出的异常该怎么做呢？此时便是Task.WhenAll方法上场的时候了：</p>

<pre><span style="color:blue">static async </span><span style="color:#2b91af">Task </span>BothHandled()
{
    <span style="color:blue">var </span>t1 = ThrowAfter(1000, <span style="color:blue">new </span><span style="color:#2b91af">NotSupportedException</span>(<span style="color:maroon">&quot;Error 1&quot;</span>));
    <span style="color:blue">var </span>t2 = ThrowAfter(2000, <span style="color:blue">new </span><span style="color:#2b91af">NotImplementedException</span>(<span style="color:maroon">&quot;Error 2&quot;</span>));
    
    <span style="color:blue">try
    </span>{
        <span style="color:blue">await </span><span style="color:#2b91af">Task</span>.WhenAll(t1, t2);
    }
    <span style="color:blue">catch </span>(<span style="color:#2b91af">NotSupportedException </span>ex)
    {
        PrintException(ex);
    }
}</pre>

<p>如果您执行这段代码，会发现其输出与第一段代码相同，但其实不同的是，第一段代码中t2的异常被“遗漏”了，而目前这段代码t1和t2的异常都被捕获了，只不过await语句仅仅“抛出”了“其中一个”异常而已。</p>

<p>WhenAll是一个辅助方法，它的输入是n个Task对象，输出则是个返回它们的结果数组的Task对象。新的Task对象会在所有输入全部“结束”后才完成。在这里“结束”的意思包括成功和失败（取消也是失败的一种，即抛出了OperationCanceledException）。换句话说，假如这n个输入中的某个Task对象很快便失败了，也必须等待其他所有输入对象成功或是失败之后，新的Task对象才算完成。而新的Task对象完成后又可能会有两种表现：</p>

<ul>
  <li>所有输入Task对象都成功了：则返回它们的结果数组。 </li>

  <li>至少一个输入Task对象失败了：则抛出“其中一个”异常。 </li>
</ul>

<p>全部成功的情况自不必说，那么在失败的情况下，什么叫做抛出“其中一个”异常？如果我们要处理所有抛出的异常该怎么办？下次我们继续讨论这方面的问题。</p>

<h1>相关文章</h1>

<ul>
  <li>关于C#中async/await中的异常处理（上） </li>

  <li><a href="http://blog.zhaojie.me/2012/04/exception-handling-in-csharp-async-await-2.html">关于C#中async/await中的异常处理（下）</a></li>
</ul>