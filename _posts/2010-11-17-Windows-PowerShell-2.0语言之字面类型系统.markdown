---
layout: post
title:  "Windows PowerShell 2.0语言之字面类型系统"
date:   2010-11-17 00:31:00
author: 天行健@中国元素
categories: program
---

## Windows PowerShell 2.0语言之字面类型系统
### by 天行健@中国元素
### at 2010-11-17 00:31:00
### original <http://www.cnblogs.com/fuhj02/archive/2010/11/17/1879408.html>

<p><p>PowerShell语言允许通过一个字面类型（type Literals）来访问类型，它是用括号抬起的类型名，返回.NET底层的System.Type对象实例，如： </p>
<pre>PS C:\&gt; [System.Int32]

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Int32                                    System.ValueType

PS C:\&gt; [System.String]

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     String                                   System.Object

PS C:\&gt; [System.Diagnostics.Process]

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    Process                                  System.ComponentM...
                               System.ComponentM...</pre>
<p>不必在任何情况下都重复System命名空间的前缀，这时因为所用的大多数类型都在命名空间下。PowerShell允许省略这个前缀，如： </p>
<pre>PS C:\&gt; [Diagnostics.Process]

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    Process                                  System.ComponentM...
为了方便，我们定义了如下常用类型别名。
（1）int、short和long：分别参考System.Int32、System.Int16和Sytem.Int64类型。
（2）byte和sbyte：对应无符号的System.Int32和有符号的System.SByte类型。
（3）void：对应没有类型。
（4）string：对应System.String类型。
（5）float和single：对应System.Single而double对应于Sytem.Double类型。
（6）decimal：对应System.Decimal的类型。
（7）regex：对应System.Text.RegularExpressions.Regex。
（8）adsi：对应System.DirectoryServices.DirectoryEntry。
（9）wmi、wmiclass和wmisearcher：分别对应System.Management.
ManagementObject、System.Management.
ManagementClass和System.ManagementObjectSearcher。</pre>
<p>不需要记忆任何别名的含义，可以查询，如： </p>
<pre>PS C:\&gt; ([regex]).Fullname
System.Text.RegularExpressions.Regex
PS C:\&gt; ([adsi]).Fullname
System.DirectoryServices.DirectoryEntry
PS C:\&gt; ([wmisearcher]).Fullname
System.Management.ManagementObjectSearcher
PS C:\&gt; ([wmi]).Fullname
System.Management.ManagementObject</pre>
<p><strong>类型转换</strong></p>
<p>在原始对象前面放置字面类型，通常字面类型会通知Shell完成对象类型的转换。如果正确，将返回一个新的对象。下例将字符串转换为整型： </p>
<pre>PS C:\&gt; $str=&quot;10&quot;
PS C:\&gt; $str.GetType().FullName
System.String
PS C:\&gt; $Num=[int]$str
PS C:\&gt; $Num
10
PS C:\&gt; $Num.GetType().FullName
System.Int32</pre>
<p>如果字符串本身可以转换，转换很方便；否则将会出错。下例在转换出错时抛出异常： </p>
<pre>PS C:\&gt; [int] &quot;not a integer string&quot; </pre>
<pre>Cannot convert value "not a integer string" to type "System.Int32". </pre>
<pre>Error: "输入字符串的格式不正确。"  </pre>
<pre>At line:1 char:6 + [int] &lt;&lt;&lt;&lt; &quot;not a integer string&quot;</pre>
<pre></pre>
<p>PowerShell主要依靠.NET框架类型转换机制并为开发人员提供了方便的类型转换工具，处理类型转换任务的策略如下。 </p>
<p>（1）检查是否可以从旧类型转换到新类型，如果在.NET中可以实现，类型必须相同或其中一个直接或者间接继承。 </p>
<p>（2）如果类型间转换已经被内置到语言中，PowerShell会尝试使用这些方法。 </p>
<p>（3）查找TypeConverter或PSTypeConverter相关类，前者是.NET内置的类型转换机制；后者是PowerShell特定的类型转换对象。二者均通过用正确的.NET属性标记目标自定义类、关联目标类型和编译类型类。很多现在.NET类已与类型转换器关联，PowerShell能够将这些类无缝地在类型间转换。另外，可以在Shell扩展类型系统的配置文件中将类型与类型转换器关联起来。 </p>
<p>（4）尝试从原始对象转换到字符串类型，通过调用目标对象类型的Parse()方法从字符串类型转换到目标类型。 </p>
<p>（5）查找可以接受原始类型对象的新类型结构体，如果找到，则用其构造新对象。 </p>
<p>（6）搜索两种对象类型的显式或隐式转换操作符，适当的操作符会被编译成分别以op_Explicit和op_Implicit命名的显示和隐式静态方法。如果找到了合适的方法，将会调用该方法转换并返回对象。 </p>
<p>（7）检查原始类型是否已经执行.NET的IConvertible接口，如果执行，类型能够转换为诸如int、double及decimal之类的基础类型。 </p>
<p>PowerShell的内置类型转换规则如下。 </p>
<p>（1）每一个对象能够被转换为PSObject，PowerShell会自动执行这个操作。大部分情况下没有参考对象，但是使用指向实际对象的PSObject实例。 </p>
<p>（2）void被转换为转换$null，如： </p>
<pre>PS C:\&gt; ([void] 5) -eq $null
True</pre>
<p>（3）可以把所有的对象和集合类型转换为数组，这里的集合被定义为执行ICollextion接口的.NET类型。.NET框架有多个内置集合类，而且集合类通过开发人员调用接口来执行。这意味着我们能够把任何的集合转换为数组，如： </p>
<pre>PS C:\&gt; ([object[]] 5).GetType().FullName
System.Object[]
PS C:\&gt; $string = New-Object Collections.Specialized.StringCollection
PS C:\&gt; ([object[]] $string).GetType().FullName
System.Object[]</pre>
<p>（4）可以按照任何非空和非null对象为真的方式把所有对象转换为Boolean类型。 </p>
<p>（5）所有对象能够被转换为string类型。 </p>
<p>（6）IDictionary对象能够转换为哈希表，.NET框架中的IDictionary接口被拥有一系列键-值映射的对象来执行。哈希表类经常被用在字典对象中，下例从OrderedDictionary对象转换到哈希表： </p>
<pre>PS C:\&gt; $dict=New-Object Collections.Specialized.OrderedDictionary
PS C:\&gt; $dict[&quot;item1&quot;]=3
PS C:\&gt; $hash=[Hashtable] $dict
PS C:\&gt; $hash

Name                           Value
----                           -----
item1                          3

PS C:\&gt; $hash.GetType().FullName
System.Collections.Hashtable
PS C:\&gt; $dict.GetType().FullName
System.Collections.Specialized.OrderedDictionary</pre>
<p>把变量转换为RSReference时需要创建一个指向原型的基准变量，修改基准值即修改原型，如： </p>
<pre>PS C:\&gt; $a=3
PS C:\&gt; $b=[ref]$a
PS C:\&gt; $b.Value=4
PS C:\&gt; $b.Value
4
PS C:\&gt; $a
4</pre>
<p>我们将会在第7章中详细介绍。 </p>
<p>（7）适当格式的字符串可以被转换为XML文档，如： </p>
<pre>PS C:\&gt; $doc=[xml] &quot;&lt;root&gt;&lt;item&gt;2&lt;/item&gt;&lt;/root&gt;&quot;
PS C:\&gt; $doc.root.item
2</pre>
<p>（8）脚本块可以转换为.NET的委派委派是指向方法或脚本块的对象类型，通常会作用于对象触发事件处理代码。使用可触发事件的.NET类型时，通常需要把脚本块转换为委派。 </p>
<p>在从包含名称、路径或查询字符串转换时，对象能够被创建和获取。 </p>
<p>（9）字符串转换为字符数组，如： </p>
<pre>PS C:\&gt; [char[]] &quot;Chars&quot;
C
h
a
r
s</pre>
<p>（10）字符串可以转换为正则表达式，这时字符串被作为匹配模式，如： </p>
<pre>PS C:\&gt; ([regex] &quot;a.*?b&quot;).GetType().FullName
System.Text.RegularExpressions.Regex</pre>
<p>（11）字符串可以创建WMI对象（参见第22章），如： </p>
<pre>PS C:\&gt; [wmisearcher] &#39;select * from Win32_Process&#39;
Scope     : System.Management.ManagementScope
Query     : System.Management.ObjectQuery
Options   : System.Management.EnumerationOptions
Site      :
Container :

PS C:\&gt; [WMI]&#39;\\.\root\cimv2:Win32_Process.Handle=0&#39;
……
Description                : System Idle Process
……

PS C:\&gt; ([wmiclass] &quot;Win32_Share&quot;).GetType().FullName
System.Management.ManagementClass</pre>
<p>（12）字符串可以转换为活动目录实例，转换进程使用字符串作为DircetoryEntry路径。下例使用PSBase替代适配器视图获取真实的对象类型： </p>
<pre>PS C:\&gt; $user=[ADSI]&quot;WinNT://./Server,admin&quot;
PS C:\&gt; $user.PSBase.GetType().FullName
System.DirectoryServices.DirectoryEntry</pre>
<p>（13）字符串可以指向.NET类型，输入字符串被作为类型名，同样可以省略System命名空间前缀，如： </p>
<pre>PS C:\&gt; [type] &quot;System.Diagnostics.Process&quot;

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    Process                                  System.ComponentM...

PS C:\&gt; [type] &quot;Diagnostics.Process&quot;

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    Process                                  System.ComponentM...</pre>
<p><strong>访问静态成员</strong></p>
<p>字面类型可以用于访问静态属性和方法，为此使用双冒号（::）来分割字面类型和成员名。下例获取一个静态属性值： </p>
<pre>PS C:\&gt; [datetime]::Today

2008年12月16日 0:00:00</pre>
<p>调用静态方法与此类似，下例调用Parse方法创建double对象： </p>
<pre>PS C:\&gt; [double]::Parse(&quot;2.5&quot;)</pre>
<p>2.5 </p>
<p><strong>总 结</strong></p>
<p>PowerShell作为强大且复杂的语言，并没有过于严格或对新手不友好。因为它把所有的对象类型通过适配器和扩展统一存取，所以用户能够找到快速入手的途径。最常用的对象是获取实体扩展，从而提高了脚本编写者的创造性，并降低了学习的难度。 </p>
<p>在需要时类型扩展系统在引擎中转换对象，从而节省了宝贵的时间并增加了脚本的可读性。所有这一切使得PowerShell带有.NET和Windows的原始动力，成为强大的混类脚本语言。</p>
<p>赛迪网地址：<a title="http://tech.ccidnet.com/art/33093/20100610/2082895_1.html" href="http://tech.ccidnet.com/art/33093/20100610/2082895_1.html">http://tech.ccidnet.com/art/33093/20100610/2082895_1.html</a></p>
<p>作者: 付海军 <br>出处：<a href="http://fuhj02.cnblogs.com">http://fuhj02.cnblogs.com</a> <br>版权：本文版权归作者和博客园共有 <br>转载：欢迎转载，为了保存作者的创作热情，请按要求【转载】，谢谢 <br>要求：未经作者同意，必须保留此段声明；必须在文章中给出原文连接；否则必究法律责任 <br>个人网站: <a href="http://txj.shell.tor.hu">http://txj.shell.tor.hu</a></p><img src="http://www.cnblogs.com/fuhj02/aggbug/1879408.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/fuhj02/">天行健@中国元素</a> 发表于 2010-11-17 00:31 <a href="http://www.cnblogs.com/fuhj02/archive/2010/11/17/1879408.html">原文链接</a></p><p>评论: 2　<a href="http://www.cnblogs.com/fuhj02/archive/2010/11/17/1879408.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/fuhj02/archive/2010/11/17/1879408.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/81387/">美国“人造太阳”点火 释放能量达高130万兆焦</a><span style="color:gray">(2010-11-17 12:36)</span><br>· <a href="http://news.cnblogs.com/n/81386/">微软将为其社交游戏服务建立统一平台</a><span style="color:gray">(2010-11-17 11:53)</span><br>· <a href="http://news.cnblogs.com/n/81385/">百度生活产品“百度身边”今日公测上线</a><span style="color:gray">(2010-11-17 11:47)</span><br>· <a href="http://news.cnblogs.com/n/81384/">沈南鹏：微博应用平台的三大投资机会</a><span style="color:gray">(2010-11-17 11:29)</span><br>· <a href="http://news.cnblogs.com/n/81383/">《星际争霸II》创造盗版新纪录 - 下载量突破15.77PB</a><span style="color:gray">(2010-11-17 11:27)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/81361/">微软Imagine Cup 2011，用科技解决全球最棘手问题</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>