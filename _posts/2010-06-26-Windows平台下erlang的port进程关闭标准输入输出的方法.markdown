---
layout: post
title:  "Windows平台下erlang的port进程关闭标准输入输出的方法"
date:   2010-06-26 22:04:12
author: 
categories: program
---

## Windows平台下erlang的port进程关闭标准输入输出的方法
### by 
### at 2010-06-26 22:04:12
### original <http://www.javaeye.com/topic/700032>

<p>为了避免与port进程的通信受一些printf调试信息的影响，通常都要关闭或者替换标准输入输出。 <br><br>Unix平台下，open_port只要指定nouse_stdio参数就可以让port进程使用fd 3、4作为通信信道， <br><br>而在Windows平台下并没有所谓的3、4 fd可用，调用fdopen(3,“rb”)将会出错，这时可以使用 <br><br>dup和dup2手动进行替换，例子如下： <br><br><br>port测试进程C代码</p>
<p> </p>
<pre name="code">#include &quot;stdafx.h&quot;
#include &lt;stdio.h&gt;
#include &lt;iostream&gt;
#include &lt;io.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;windows.h&gt;

int main(int argc, char* argv[])
{
    //复制stdio fd，之后我们将使用复制的句柄与Erlang通信
    FILE* f0 = fdopen(dup(0),&quot;rb&quot;);
    FILE* f1 = fdopen(dup(1),&quot;wb&quot;);

    //Windows下默认为Text模式，按需要设置为BINARY模式，
    //否则\n将会被替换为\r\n，这通常不是我们想要的
    _setmode (_fileno(f0), _O_BINARY);
    _setmode (_fileno(f1), _O_BINARY);

    //用一个临时文件替换stdio,并关闭之
    FILE* ftmp = tmpfile();
    dup2(fileno(ftmp), 0);
    dup2(fileno(ftmp), 1);
    fclose(stdin);
    fclose(stdout);
    fclose(ftmp);
    //测试一些输出
    fwrite(&quot;hello&quot;,1,5,f1);
    fputc(&#39; &#39;,f1);
    fflush(f1);
    printf(&quot;world&quot;);
    fflush(stdout);
    std::cout &lt;&lt; &quot;!!!!!\n&quot;;
    std::cout.flush();
    Sleep(5000);

	return 0;
}
</pre>
<p> </p>
<p> </p>
<p>测试用erlang代码：</p>
<p> </p>
<pre name="code">-module(test).
-compile(export_all).

start() -&gt;
  P = open_port({spawn, &quot;./Debug/teststdio.exe&quot;},[binary]),
  loop(P).
  
loop(P) -&gt;
  receive
    {P, {data, Data}} -&gt;
 	io:format(&quot;~p~n&quot;,[Data]),
 	loop(P)
  after 3000 -&gt;
    timeout
  end.
</pre>
<p> </p>
<p> </p>
<p>测试结果：</p>
<p> </p>
<p>1、替换stdio后：</p>
<pre name="code">Eshell V5.7.5  (abort with ^G)
1&gt; c(test.erl).
{ok,test}
2&gt; test:start().
&lt;&lt;&quot;hello &quot;&gt;&gt;
timeout
3&gt;</pre>
<p> </p>
<p>2、如果没有替换stdio，将会收到</p>
<pre name="code">test:start().
&lt;&lt;&quot;hello &quot;&gt;&gt;
&lt;&lt;&quot;world&quot;&gt;&gt;
&lt;&lt;&quot;!!!!!\r\n&quot;&gt;&gt;
timeout</pre>
<p> </p>
<p>我们调用第三方库时很难确认是否会有stdio输出，这种方式很好的避免了printf信息对port通信的影响</p>
<p> </p>
<p><span style="color:#ff9900"> 注意没有替换stdio部分的</span><span style="color:#ff9900">输出，stdin和stdout没有指定BINARY模式，\n被替换成\r\n了，</span></p>
<p><span style="color:#ff9900"> 再看看前面dup2部分的调用，虽然指定了</span><span style="color:#ff9900">"rb"和"wb"，但这样是无效的，必须手动调用setmode进行设置才会有效</span></p>
<p> </p>
<p> 最后说说windows下使用异步stdio的注意点，需要2个设置：</p>
<p>1、在open_port中指定overlapped_io参数</p>
<p>2、用_get_osfhandle函数得到替换后的io的HANDLE，以供ReadFile和WriteFile操作：</p>
<p> </p>
<pre name="code">    HANDLE hin = (HANDLE)_get_osfhandle(_fileno(f0));
    HANDLE hout= (HANDLE)_get_osfhandle(_fileno(f1));
</pre>
<p> </p>
<p> 在C程序里使用异步io虽然麻烦，但有时候却是必须的。比如windows系统下通过port访问串行口时，如果不使用overlapped模式，读取串口与读取stdio是不能同时进行的。</p>
          
          <br><br>
          作者: <a href="http://arksea.javaeye.com">arksea</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/700032" style="color:red">已有 <strong>1</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>