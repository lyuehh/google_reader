---
layout: post
title:  "jQuery的TDD框架 - QUnit"
date:   2010-10-31 10:37:00
author: Tony  Qu
categories: program
---

## jQuery的TDD框架 - QUnit
### by Tony  Qu
### at 2010-10-31 10:37:00
### original <http://www.cnblogs.com/tonyqus/archive/2010/10/31/jquery_tdd_qunit.html>

<p><p>作者：Tony Qu</p>  <p> </p>  <p>我今天只讨论基于JavaScript的TDD，可能有些公司是用C#做js单元测试的，但我不认为这是个好主意，很多js运行时的东西让js来返回会更直观，且易于维护。</p>  <p> </p>  <p>在讨论jQuery TDD之前，我们先来了解下什么才是一个标准的TDD框架。作为标准的TDD框架，必须满足这么几个要求：</p>  <p>1. 即使测试脚本出错了也要能继续运行接下来的脚本</p>  <p>2. 能够不依赖被测试代码写测试用例，即使代码没有实现也可以先写测试用例</p>  <p>3. 能够显示详细的错误信息和位置</p>  <p>4. 能够统计通过和未通过的用例的数量</p>  <p>5. 有专门的可视化界面用于统计和跟踪测试用例</p>  <p>6. 易于上手，通过一些简单的指导就可以马上开始写测试代码。</p>  <p> </p>  <p>以上这些要求QUnit都做到了， 这也是我推荐QUnit的原因。</p>  <p> </p>  <p>QUnit目前已经可以脱离jQuery独立运行，这也是它成功的另外一个原因，即兼容性好，其实严格意义上它已经不是一个jQuery的测试框架了，而是JavaScript测试框架。有意思的是你会发现QUnit的注释曾经发生过微小的变化，如下</p>  <p><a href="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/QUnit_2.gif"><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="QUnit" border="0" alt="QUnit" src="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/QUnit_thumb.gif" width="644" height="129"></a> </p>  <p>这也说明QUnit的代码是做过专门的调整，使之能脱离JQuery运行。</p>  <p>  </p>  <p><u>下载Qunit</u></p>  <p>下载qunit的代码可以去<a title="http://github.com/jquery/qunit" href="http://github.com/jquery/qunit">http://github.com/jquery/qunit</a>，那里的代码是最新的。</p>  <p> </p>  <p><u>如何使用QUnit</u></p>  <p>使用QUnit很简单，只需要下面这些html代码，初始的设置就完成了。</p>  <div style="border-bottom:gray 1px solid;border-left:gray 1px solid;padding-bottom:4px;line-height:12pt;background-color:#f4f4f4;margin:20px 0px 10px;padding-left:4px;width:97.5%;padding-right:4px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;max-height:200px;font-size:8pt;overflow:auto;border-top:gray 1px solid;border-right:gray 1px solid;padding-top:4px">   <pre style="border-bottom-style:none;padding-bottom:0px;line-height:12pt;border-right-style:none;background-color:#f4f4f4;margin:0em;padding-left:0px;width:100%;padding-right:0px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;border-top-style:none;color:black;font-size:8pt;border-left-style:none;overflow:visible;padding-top:0px"><span style="color:#0000ff">&lt;!</span><span style="color:#800000">DOCTYPE</span> <span style="color:#ff0000">html</span> <span style="color:#ff0000">PUBLIC</span> <span style="color:#0000ff">&quot;-//W3C//DTD XHTML 1.0 Transitional//EN&quot;</span> <span style="color:#0000ff">&quot;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&quot;</span><span style="color:#0000ff">&gt;</span>
<span style="color:#0000ff">&lt;</span><span style="color:#800000">html</span> <span style="color:#ff0000">xmlns</span><span style="color:#0000ff">=&quot;http://www.w3.org/1999/xhtml&quot;</span> <span style="color:#0000ff">&gt;</span>
<span style="color:#0000ff">&lt;</span><span style="color:#800000">head</span><span style="color:#0000ff">&gt;</span>
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">title</span><span style="color:#0000ff">&gt;</span>My Foo Tests<span style="color:#0000ff">&lt;/</span><span style="color:#800000">title</span><span style="color:#0000ff">&gt;</span>    
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">link</span> <span style="color:#ff0000">href</span><span style="color:#0000ff">=&quot;qunit.css&quot;</span> <span style="color:#ff0000">type</span><span style="color:#0000ff">=&quot;text/css&quot;</span> <span style="color:#ff0000">rel</span><span style="color:#0000ff">=&quot;stylesheet&quot;</span><span style="color:#0000ff">/&gt;</span>    
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">script</span> <span style="color:#ff0000">language</span><span style="color:#0000ff">=&quot;javascript&quot;</span> <span style="color:#ff0000">src</span><span style="color:#0000ff">=&quot;jquery-1.4.2.js&quot;</span> <span style="color:#ff0000">type</span><span style="color:#0000ff">=&quot;text/javascript&quot;</span> <span style="color:#0000ff">/&gt;</span>
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">script</span> <span style="color:#ff0000">language</span><span style="color:#0000ff">=&quot;javascript&quot;</span> <span style="color:#ff0000">src</span><span style="color:#0000ff">=&quot;qunit.js&quot;</span> <span style="color:#ff0000">type</span><span style="color:#0000ff">=&quot;text/javascript&quot;</span><span style="color:#0000ff">/&gt;</span>
<span style="color:#0000ff">&lt;/</span><span style="color:#800000">head</span><span style="color:#0000ff">&gt;</span>
<span style="color:#0000ff">&lt;</span><span style="color:#800000">body</span><span style="color:#0000ff">&gt;</span>
      <span style="color:#0000ff">&lt;</span><span style="color:#800000">h1</span> <span style="color:#ff0000">id</span><span style="color:#0000ff">=&quot;qunit-header&quot;</span><span style="color:#0000ff">&gt;</span>QUnit Test Suite<span style="color:#0000ff">&lt;/</span><span style="color:#800000">h1</span><span style="color:#0000ff">&gt;</span>  
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">h2</span> <span style="color:#ff0000">id</span><span style="color:#0000ff">=&quot;qunit-banner&quot;</span><span style="color:#0000ff">&gt;&lt;/</span><span style="color:#800000">h2</span><span style="color:#0000ff">&gt;</span>  
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">div</span> <span style="color:#ff0000">id</span><span style="color:#0000ff">=&quot;qunit-testrunner-toolbar&quot;</span><span style="color:#0000ff">&gt;&lt;/</span><span style="color:#800000">div</span><span style="color:#0000ff">&gt;</span>  
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">h2</span> <span style="color:#ff0000">id</span><span style="color:#0000ff">=&quot;qunit-userAgent&quot;</span><span style="color:#0000ff">&gt;&lt;/</span><span style="color:#800000">h2</span><span style="color:#0000ff">&gt;</span>  
    <span style="color:#0000ff">&lt;</span><span style="color:#800000">ol</span> <span style="color:#ff0000">id</span><span style="color:#0000ff">=&quot;qunit-tests&quot;</span><span style="color:#0000ff">&gt;&lt;/</span><span style="color:#800000">ol</span><span style="color:#0000ff">&gt;</span>  
<span style="color:#0000ff">&lt;/</span><span style="color:#800000">body</span><span style="color:#0000ff">&gt;</span>
<span style="color:#0000ff">&lt;/</span><span style="color:#800000">html</span><span style="color:#0000ff">&gt;</span></pre>
</div>

<p></p>

<p>QUnit不仅仅为你提供了测试脚本函数，还为你的单元测试提供了一个标准化的测试界面，如下图所示，红色的表示这个测试用例没有通过，绿色的表示通过。每一个框比表示一个测试函数，里面可能有多个断言语句的结果，标题中（x,y,z）表示总共有z个断言，y个是正确的，x个是错误的。</p>

<p><a href="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/image_2.png"><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="image" border="0" alt="image" src="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/image_thumb.png" width="598" height="484"></a> </p>

<p>刚才只是初步看了下界面，现在我们来学习如何使用，我从<a title="http://github.com/jquery/jquery/raw/master/test/unit/core.js" href="http://github.com/jquery/jquery/raw/master/test/unit/core.js">http://github.com/jquery/jquery/raw/master/test/unit/core.js</a>下载了一个core.js的范例测试代码，这个是jQuery用来测试它的核心模块的代码。</p>

<p>在&lt;head&gt;&lt;/head&gt;中加入&lt;script language=&quot;javascript&quot; src=&quot;core.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;，注意一定要在qunit.js声明后面，因为core.js中用到了qunit.js定义的函数。</p>

<p>这时你再运行刚才我们创建的html文件，你就会看到类似上图的结果，这就是core.js的测试结果，是不是一清二楚？如果是测试你自己的函数，你就可以根据红色的错误提示跟踪问题所在，直到把所有的测试结果都变成绿色。</p>

<p><u></u></p>

<p><u></u></p>

<p><u></u></p>

<p><u>测试脚本怎么写</u></p>

<p>测试脚本的写法，我建议你参考<a title="http://docs.jquery.com/Qunit#Reference_Test_Suites" href="http://docs.jquery.com/Qunit#Reference_Test_Suites">http://docs.jquery.com/Qunit#Reference_Test_Suites</a>，这里面有提到一些测试用例的文件，你可以通过它们来研究怎么写测试用例，尽管有些测试用例已经跑不通了。</p>

<p>比较常用的函数有：</p>

<p>expect(amount) - 指定某个函数中会有多少个断言，通常写在测试函数开头。</p>

<p>module(name) - 模块是测试函数的集合，使用该函数可以在UI中将测试函数按模块归类。</p>

<p>ok(state, message) – 布尔型断言，message是专门显示在QUnit界面上，用来区分不同的断言的</p>

<p>equals(actual, expected, message) - 相等断言，actual和expected的值相等时才能通过。</p>

<p>same(actual, expected, message) - 完全相等断言，和equals的区别在于它会比较子元素，对于数组和一些自定义对象的比较十分有用。</p>

<p>这些是最常用的，其他的大家可以自己参考Qunit官方文档。</p>

<p> </p>

<p><u>第一个QUnit测试用例</u></p>

<p>假设我们写个这样的函数，计算a+b的结果，如下</p>

<div style="border-bottom:gray 1px solid;border-left:gray 1px solid;padding-bottom:4px;line-height:12pt;background-color:#f4f4f4;margin:20px 0px 10px;padding-left:4px;width:97.5%;padding-right:4px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;max-height:200px;font-size:8pt;overflow:auto;border-top:gray 1px solid;border-right:gray 1px solid;padding-top:4px">
  <pre style="border-bottom-style:none;padding-bottom:0px;line-height:12pt;border-right-style:none;background-color:#f4f4f4;margin:0em;padding-left:0px;width:100%;padding-right:0px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;border-top-style:none;color:black;font-size:8pt;border-left-style:none;overflow:visible;padding-top:0px"><span style="color:#0000ff">function</span> CalculateAPlusB(a,b)
{
    <span style="color:#0000ff">return</span> a+b;
}</pre>
</div>

<p>在页面中加入一个单独的js引用专门用来写单元测试的function，比如叫test.js</p>

<div style="border-bottom:gray 1px solid;border-left:gray 1px solid;padding-bottom:4px;line-height:12pt;background-color:#f4f4f4;margin:20px 0px 10px;padding-left:4px;width:97.5%;padding-right:4px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;max-height:200px;font-size:8pt;overflow:auto;border-top:gray 1px solid;border-right:gray 1px solid;padding-top:4px">
  <pre style="border-bottom-style:none;padding-bottom:0px;line-height:12pt;border-right-style:none;background-color:#f4f4f4;margin:0em;padding-left:0px;width:100%;padding-right:0px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;border-top-style:none;color:black;font-size:8pt;border-left-style:none;overflow:visible;padding-top:0px">&lt;script language=<span style="color:#006080">&quot;javascript&quot;</span> src=<span style="color:#006080">&quot;test.js&quot;</span> type=<span style="color:#006080">&quot;text/javascript&quot;</span>/&gt;</pre>
</div>

<p>具体的测试代码如下</p>

<div style="border-bottom:gray 1px solid;border-left:gray 1px solid;padding-bottom:4px;line-height:12pt;background-color:#f4f4f4;margin:20px 0px 10px;padding-left:4px;width:97.5%;padding-right:4px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;max-height:200px;font-size:8pt;overflow:auto;border-top:gray 1px solid;border-right:gray 1px solid;padding-top:4px">
  <pre style="border-bottom-style:none;padding-bottom:0px;line-height:12pt;border-right-style:none;background-color:#f4f4f4;margin:0em;padding-left:0px;width:100%;padding-right:0px;font-family:consolas,&#39;Courier New&#39;,courier,monospace;border-top-style:none;color:black;font-size:8pt;border-left-style:none;overflow:visible;padding-top:0px">test(<span style="color:#006080">&quot;basic calculation&quot;</span>, <span style="color:#0000ff">function</span>() {
        equals(CalculateAPlusB(1,5),6,<span style="color:#006080">&quot;1+5=6&quot;</span>);
        equals(CalculateAPlusB(1.2,5.5),6.7,<span style="color:#006080">&quot;1.2+5.5=6.7&quot;</span>);
        equals(CalculateAPlusB(-1,10),9,<span style="color:#006080">&quot;-1+10=9&quot;</span>);
    });

test(<span style="color:#006080">&quot;pass null test&quot;</span>, <span style="color:#0000ff">function</span>() {
        ok(isNaN(CalculateAPlusB(<span style="color:#0000ff">null</span>,5)),<span style="color:#006080">&quot;pass null as the first argument&quot;</span>);
        ok(isNaN(CalculateAPlusB(5,<span style="color:#0000ff">null</span>)),<span style="color:#006080">&quot;pass null as the second argument&quot;</span>);
        ok(isNaN(CalculateAPlusB(<span style="color:#0000ff">null</span>,<span style="color:#0000ff">null</span>)),<span style="color:#006080">&quot;no argument pass in&quot;</span>);
    });</pre>
</div>

<p>其中test方法是qunit用来定义测试方法的语句，其第一个参数表示这个测试用例的名称，第二个参数就是具体的实现。equals是用来比较期望值和实际值是否一致，ok是用来判断结果是否为真。</p>

<p><u></u></p>

<p>如果一切顺利，你将看到类似下面的结果。</p>

<p><a href="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/qunit1_2.jpg"><img style="border-right-width:0px;display:inline;border-top-width:0px;border-bottom-width:0px;border-left-width:0px" title="qunit1" border="0" alt="qunit1" src="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/qunit1_thumb.jpg" width="640" height="366"></a> </p>

<p>这时应该恭喜自己，因为所有的测试结果都是绿颜色的，这表示这些测试都通过了。 当然这里只是举2个例子，你可以写更多的测试用例来测试这个方法，比如测测值溢出的情况。</p>

<p> </p>

<p><u>参考资料</u></p>

<p><a title="http://www.lostechies.com/blogs/chad_myers/archive/2008/08/28/getting-started-with-jquery-qunit-for-client-side-javascript-testing.aspx" href="http://www.lostechies.com/blogs/chad_myers/archive/2008/08/28/getting-started-with-jquery-qunit-for-client-side-javascript-testing.aspx">http://www.lostechies.com/blogs/chad_myers/archive/2008/08/28/getting-started-with-jquery-qunit-for-client-side-javascript-testing.aspx</a></p>

<p><a href="http://docs.jquery.com/Qunit">http://docs.jquery.com/Qunit</a></p>

<p><a title="http://www.swift-lizard.com/2009/11/24/test-driven-development-with-jquery-qunit/" href="http://www.swift-lizard.com/2009/11/24/test-driven-development-with-jquery-qunit/">http://www.swift-lizard.com/2009/11/24/test-driven-development-with-jquery-qunit/</a></p>

<p><a title="http://www.agiledata.org/essays/tdd.html" href="http://www.agiledata.org/essays/tdd.html">http://www.agiledata.org/essays/tdd.html</a></p>

<p><a title="http://www.oncoding.cn/2010/javascript-unit-testing-qunit/" href="http://www.oncoding.cn/2010/javascript-unit-testing-qunit/">http://www.oncoding.cn/2010/javascript-unit-testing-qunit/</a></p>

<p> </p>

<p><font color="#800000">广告：长期招聘.NET职位，上海、北京、大连均有职位，有兴趣请发简历到 <a href="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/r_gmailicon_2.jpg"><img style="border-bottom:0px;border-left:0px;display:inline;border-top:0px;border-right:0px" title="r_gmailicon" border="0" alt="r_gmailicon" src="http://images.cnblogs.com/cnblogs_com/tonyqus/WindowsLiveWriter/b3c5301eb9e5_6CFA/r_gmailicon_thumb.jpg" width="161" height="21"></a> </font></p><img src="http://www.cnblogs.com/tonyqus/aggbug/1848420.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/tonyqus/">Tony  Qu</a> 发表于 2010-10-31 10:37 <a href="http://www.cnblogs.com/tonyqus/archive/2010/10/31/jquery_tdd_qunit.html">原文链接</a></p><p>评论: 2　<a href="http://www.cnblogs.com/tonyqus/archive/2010/10/31/jquery_tdd_qunit.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/tonyqus/archive/2010/10/31/jquery_tdd_qunit.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/79190/">因腾讯360之争而出的搞笑漫画(组图)</a><span style="color:gray">(2010-10-31 20:52)</span><br>· <a href="http://news.cnblogs.com/n/79187/">隐藏在硬件中的后门</a><span style="color:gray">(2010-10-31 20:18)</span><br>· <a href="http://news.cnblogs.com/n/79185/">无法替换MicroSD卡，Samsung Omnia 7用户表哭</a><span style="color:gray">(2010-10-31 20:17)</span><br>· <a href="http://news.cnblogs.com/n/79184/">360与腾讯之争——娱乐化时代的产物</a><span style="color:gray">(2010-10-31 20:06)</span><br>· <a href="http://news.cnblogs.com/n/79182/">评论：相比微软Windows 更多消费者拥抱苹果产品</a><span style="color:gray">(2010-10-31 19:57)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/79114/">【重磅新闻】微软Silverlight战略发生转移 回归HTML5</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>