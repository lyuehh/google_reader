---
layout: post
title:  "Flex数据访问：WebService"
date:   2010-10-08 22:41:00
author: Asharp
categories: program
---

## Flex数据访问：WebService
### by Asharp
### at 2010-10-08 22:41:00
### original <http://www.cnblogs.com/zhaozhan/archive/2010/10/08/1846170.html>

<p>作者: <a href="http://www.cnblogs.com/zhaozhan/">Asharp</a> 发表于 2010-10-08 22:41 <a href="http://www.cnblogs.com/zhaozhan/archive/2010/10/08/1846170.html">原文链接</a> 阅读: 630 评论: 2</p>
<p>      WebService组件用于访问SOAP Web服务，此类服务时带有方法的软件模块，Web服务方法通常称为“操作（option）”，操作可以带参数（requet）。Web服务接口通过 Web 服务描述语言 (WSDL) 进行定义。通过 Web 服务提供的标准相容方式，在不同平台上运行的软件模块可以相互交互。</p>
<p>     Flex 支持格式设置为 SOAP 消息且通过 HTTP 传输的 Web 服务请求和结果。SOAP 提供基于 XML 格式的定义，用于在 Web 服务客户端（如使用 Flex 构建的应用程序）和 Web 服务之间交换结构化和类型化信息。</p>
<p>     在.NET和Flex的数据交互可以通过Web Services访问string，object，datatable，List&lt;&gt;,ArrayList等。</p>
<p>     .NET和Flex的数据示例：</p>
<p>     1、返回对象</p>
<p>     定义返回对象的Web Method</p>
<div>
<pre><span> 1:         </span>[<span style="color:#2b91af">WebMethod</span>]</pre>
<pre><span> 2:  </span>       <span style="color:blue">public </span><span style="color:#2b91af">Employee </span>GetEmployee()</pre>
<pre><span> 3:  </span>       {</pre>
<pre><span> 4:  </span>           <span style="color:blue">return new </span><span style="color:#2b91af">Employee</span></pre>
<pre><span> 5:  </span><span style="color:#2b91af">           </span>{</pre>
<pre><span> 6:  </span>               id = 1,</pre>
<pre><span> 7:  </span>               name = <span style="color:#a31515">"Shawn"</span>,</pre>
<pre><span> 8:  </span>               age = 25</pre>
<pre><span> 9:  </span>           };</pre>
<pre><span>10:  </span>       }</pre>
<pre><span>11:  </span> </pre>
</div>

<p>     Flex前端代码：</p>
<div>
<pre><span> 1:  </span>&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;</pre>
<pre><span> 2:  </span><span style="color:blue">&lt;mx:Application </span>xmlns:mx="<span style="color:#990000">http://www.adobe.com/2006/mxml</span>" layout="<span style="color:#990000">absolute</span>"<span style="color:blue">&gt;</span></pre>
<pre><span> 3:  </span><span style="color:blue">    </span><span style="color:#006633">&lt;mx:Script&gt;</span></pre>
<pre><span> 4:  </span><span style="color:#006633">        </span>&lt;![CDATA[</pre>
<pre><span> 5:  </span>            <span style="color:#0033ff">import </span>mx.rpc.events.FaultEvent;</pre>
<pre><span> 6:  </span>            <span style="color:#0033ff">import </span>mx.rpc.events.ResultEvent;</pre>
<pre><span> 7:  </span>            <span style="color:#0033ff">import </span>mx.controls.Alert;</pre>
<pre><span> 8:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>onResult(event:ResultEvent):<span style="color:#0033ff">void</span></pre>
<pre><span> 9:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>10:  </span>                Alert.show(event.result.name);</pre>
<pre><span>11:  </span>            }</pre>
<pre><span>12:  </span> </pre>
<pre><span>13:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>onFault(event:FaultEvent):<span style="color:#0033ff">void</span></pre>
<pre><span>14:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>15:  </span>               Alert.show(<span style="color:#990000">"调</span>+event.message);</pre>
<pre><span>16:  </span>            }</pre>
<pre><span>17:  </span> </pre>
<pre><span>18:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>GetEmployee():<span style="color:#0033ff">void</span></pre>
<pre><span>19:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>20:  </span>                <span style="color:#0033ff">this</span>.MyService.GetEmployee.send();</pre>
<pre><span>21:  </span>            }</pre>
<pre><span>22:  </span>        ]]&gt;</pre>
<pre><span>23:  </span>    <span style="color:#006633">&lt;/mx:Script&gt;</span></pre>
<pre><span>24:  </span><span style="color:#006633">    </span><span style="color:blue">&lt;mx:Button </span>label="<span style="color:#990000">Get Employee</span>" click="GetEmployee()"<span style="color:blue">/&gt;</span></pre>
<pre><span>25:  </span><span style="color:blue">    </span></pre>
<pre><span>26:  </span><span style="color:blue">    &lt;mx:WebService </span>id="<span style="color:#990000">MyService</span>" wsdl="<span style="color:#990000">http://localhost:4081/Flex.asmx?WSDL</span>" useProxy="<span style="color:#990000">false</span>" result="onResult(event)" fault="onFault(event)"<span style="color:blue">&gt;</span></pre>
<pre><span>27:  </span><span style="color:blue">        &lt;mx:operation </span>name="<span style="color:#990000">GetEmployee</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>28:  </span><span style="color:blue">    &lt;/mx:WebService&gt;</span></pre>
<pre><span>29:  </span><span style="color:blue">&lt;/mx:Application&gt;</span></pre>
<pre><span>30:  </span></pre>
</div>

<p>     运行结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/zhaozhan/WindowsLiveWriter/FlexWebService_131B7/image_2.png"><img title="image" style="display:inline;border:0px" src="http://images.cnblogs.com/cnblogs_com/zhaozhan/WindowsLiveWriter/FlexWebService_131B7/image_thumb.png" border="0" alt="image" width="201" height="107"></a> </p>
<p>     2、返回DataTable</p>
<p>     定义返回DataTable的Web Method：</p>
<div>
<pre><span> 1:          </span>[<span style="color:#2b91af">WebMethod</span>]</pre>
<pre><span> 2:  </span>        <span style="color:blue">public </span><span style="color:#2b91af">DataTable </span>GetDataTable()</pre>
<pre><span> 3:  </span>        {</pre>
<pre><span> 4:  </span>            <span style="color:#2b91af">DataTable </span>dt = <span style="color:blue">new </span><span style="color:#2b91af">DataTable</span>(<span style="color:#a31515">"Employees"</span>);</pre>
<pre><span> 5:  </span>            dt.Columns.Add(<span style="color:#a31515">"id"</span>, <span style="color:blue">typeof</span>(<span style="color:blue">int</span>));</pre>
<pre><span> 6:  </span>            dt.Columns.Add(<span style="color:#a31515">"name"</span>, <span style="color:blue">typeof</span>(<span style="color:blue">string</span>));</pre>
<pre><span> 7:  </span>            dt.Columns.Add(<span style="color:#a31515">"age"</span>, <span style="color:blue">typeof</span>(<span style="color:blue">int</span>));</pre>
<pre><span> 8:  </span> </pre>
<pre><span> 9:  </span>            <span style="color:#2b91af">DataRow </span>dr = dt.NewRow();</pre>
<pre><span>10:  </span>            dr[<span style="color:#a31515">"id"</span>] = 1;</pre>
<pre><span>11:  </span>            dr[<span style="color:#a31515">"name"</span>] = <span style="color:#a31515">"Shawn"</span>;</pre>
<pre><span>12:  </span>            dr[<span style="color:#a31515">"age"</span>] = 25;</pre>
<pre><span>13:  </span>            dt.Rows.Add(dr);</pre>
<pre><span>14:  </span> </pre>
<pre><span>15:  </span>            dr = dt.NewRow();</pre>
<pre><span>16:  </span>            dr[<span style="color:#a31515">"id"</span>] = 2;</pre>
<pre><span>17:  </span>            dr[<span style="color:#a31515">"name"</span>] = <span style="color:#a31515">"Jack"</span>;</pre>
<pre><span>18:  </span>            dr[<span style="color:#a31515">"age"</span>] = 23;</pre>
<pre><span>19:  </span>            dt.Rows.Add(dr);</pre>
<pre><span>20:  </span> </pre>
<pre><span>21:  </span>            <span style="color:blue">return </span>dt;</pre>
<pre><span>22:  </span>        }</pre>
<pre><span>23:  </span> </pre>
</div>

<p>    Flex前端代码：</p>
<div>
<pre><span> 1:  </span>&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;</pre>
<pre><span> 2:  </span><span style="color:blue">&lt;mx:Application </span>xmlns:mx="<span style="color:#990000">http://www.adobe.com/2006/mxml</span>" layout="<span style="color:#990000">absolute</span>"<span style="color:blue">&gt;</span></pre>
<pre><span> 3:  </span><span style="color:blue">    </span><span style="color:#006633">&lt;mx:Script&gt;</span></pre>
<pre><span> 4:  </span><span style="color:#006633">        </span>&lt;![CDATA[</pre>
<pre><span> 5:  </span>            <span style="color:#0033ff">import </span>mx.rpc.events.FaultEvent;</pre>
<pre><span> 6:  </span>            <span style="color:#0033ff">import </span>mx.rpc.events.ResultEvent;</pre>
<pre><span> 7:  </span>            <span style="color:#0033ff">import </span>mx.controls.Alert;</pre>
<pre><span> 8:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>onResult(event:ResultEvent):<span style="color:#0033ff">void</span></pre>
<pre><span> 9:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>10:  </span> </pre>
<pre><span>11:  </span>            }</pre>
<pre><span>12:  </span> </pre>
<pre><span>13:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>onFault(event:FaultEvent):<span style="color:#0033ff">void</span></pre>
<pre><span>14:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>15:  </span>               Alert.show(<span style="color:#990000">"调</span>+event.message);</pre>
<pre><span>16:  </span>            }</pre>
<pre><span>17:  </span> </pre>
<pre><span>18:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>GetDataTable():<span style="color:#0033ff">void</span></pre>
<pre><span>19:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>20:  </span>                <span style="color:#0033ff">this</span>.MyService.GetDataTable.send();</pre>
<pre><span>21:  </span>            }</pre>
<pre><span>22:  </span>        ]]&gt;</pre>
<pre><span>23:  </span>    <span style="color:#006633">&lt;/mx:Script&gt;</span></pre>
<pre><span>24:  </span><span style="color:#006633">    </span></pre>
<pre><span>25:  </span><span style="color:#006633">    </span><span style="color:blue">&lt;mx:Panel </span>width="<span style="color:#990000">400</span>"<span style="color:blue">&gt;</span></pre>
<pre><span>26:  </span><span style="color:blue">        &lt;mx:DataGrid </span>id="<span style="color:#990000">gvEmployee</span>" dataProvider="<span style="color:#990000">{</span><span style="color:#0033ff">this</span>.MyService.GetDataTable.lastResult.Tables.Employees.Rows<span style="color:#990000">}</span>" width="<span style="color:#990000">100%</span>"<span style="color:blue">&gt;</span></pre>
<pre><span>27:  </span><span style="color:blue">            &lt;mx:columns&gt;</span></pre>
<pre><span>28:  </span><span style="color:blue">                &lt;mx:DataGridColumn </span>headerText="<span style="color:#990000">ID</span>" dataField="<span style="color:#990000">id</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>29:  </span><span style="color:blue">                &lt;mx:DataGridColumn </span>headerText="<span style="color:#990000">Name</span>" dataField="<span style="color:#990000">name</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>30:  </span><span style="color:blue">                &lt;mx:DataGridColumn </span>headerText="<span style="color:#990000">Age</span>" dataField="<span style="color:#990000">age</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>31:  </span><span style="color:blue">            &lt;/mx:columns&gt;</span></pre>
<pre><span>32:  </span><span style="color:blue">        &lt;/mx:DataGrid&gt;</span></pre>
<pre><span>33:  </span><span style="color:blue">        &lt;mx:ControlBar&gt;</span></pre>
<pre><span>34:  </span><span style="color:blue">            &lt;mx:Button </span>label="<span style="color:#990000">Get DataTable</span>" click="GetDataTable()"<span style="color:blue">/&gt;</span></pre>
<pre><span>35:  </span><span style="color:blue">        &lt;/mx:ControlBar&gt;</span></pre>
<pre><span>36:  </span><span style="color:blue">    &lt;/mx:Panel&gt;</span></pre>
<pre><span>37:  </span><span style="color:blue">    </span></pre>
<pre><span>38:  </span><span style="color:blue">    &lt;mx:WebService </span>id="<span style="color:#990000">MyService</span>" wsdl="<span style="color:#990000">http://localhost:4081/Flex.asmx?WSDL</span>" useProxy="<span style="color:#990000">false</span>" result="onResult(event)" fault="onFault(event)"<span style="color:blue">&gt;</span></pre>
<pre><span>39:  </span><span style="color:blue">        &lt;mx:operation </span>name="<span style="color:#990000">GetDataTable</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>40:  </span><span style="color:blue">    &lt;/mx:WebService&gt;</span></pre>
<pre><span>41:  </span><span style="color:blue">&lt;/mx:Application&gt;</span></pre>
<pre><span>42:  </span></pre>
</div>

<p>    运行结果：</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/zhaozhan/WindowsLiveWriter/FlexWebService_131B7/image_4.png"><img title="image" style="display:inline;border:0px" src="http://images.cnblogs.com/cnblogs_com/zhaozhan/WindowsLiveWriter/FlexWebService_131B7/image_thumb_1.png" border="0" alt="image" width="405" height="239"></a> </p>
<p>     3、返回List&lt;&gt;</p>
<div>
<pre><span> 1:          </span>[<span style="color:#2b91af">WebMethod</span>]</pre>
<pre><span> 2:  </span>        <span style="color:blue">public </span><span style="color:#2b91af">List</span>&lt;<span style="color:#2b91af">Employee</span>&gt; GetEmployeeList()</pre>
<pre><span> 3:  </span>        {</pre>
<pre><span> 4:  </span>            <span style="color:blue">return new </span><span style="color:#2b91af">List</span>&lt;<span style="color:#2b91af">Employee</span>&gt;</pre>
<pre><span> 5:  </span>            {</pre>
<pre><span> 6:  </span>                <span style="color:blue">new </span><span style="color:#2b91af">Employee</span></pre>
<pre><span> 7:  </span><span style="color:#2b91af">                </span>{</pre>
<pre><span> 8:  </span>                    id = 1,</pre>
<pre><span> 9:  </span>                    name = <span style="color:#a31515">"Shawn"</span>,</pre>
<pre><span>10:  </span>                    age = 25</pre>
<pre><span>11:  </span>                },</pre>
<pre><span>12:  </span>                <span style="color:blue">new </span><span style="color:#2b91af">Employee</span></pre>
<pre><span>13:  </span><span style="color:#2b91af">                </span>{</pre>
<pre><span>14:  </span>                    id = 2,</pre>
<pre><span>15:  </span>                    name = <span style="color:#a31515">"Jack"</span>,</pre>
<pre><span>16:  </span>                    age = 23</pre>
<pre><span>17:  </span>                }</pre>
<pre><span>18:  </span>            };</pre>
<pre><span>19:  </span>        }</pre>
<pre><span>20:  </span> </pre>
</div>

<p>     Flex前端：</p>
<div>
<pre><span> 1:  </span>&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;</pre>
<pre><span> 2:  </span><span style="color:blue">&lt;mx:Application </span>xmlns:mx="<span style="color:#990000">http://www.adobe.com/2006/mxml</span>" layout="<span style="color:#990000">absolute</span>"<span style="color:blue">&gt;</span></pre>
<pre><span> 3:  </span><span style="color:blue">    </span><span style="color:#006633">&lt;mx:Script&gt;</span></pre>
<pre><span> 4:  </span><span style="color:#006633">        </span>&lt;![CDATA[</pre>
<pre><span> 5:  </span>            <span style="color:#0033ff">import </span>mx.collections.ArrayCollection;</pre>
<pre><span> 6:  </span>            <span style="color:#0033ff">import </span>mx.rpc.events.FaultEvent;</pre>
<pre><span> 7:  </span>            <span style="color:#0033ff">import </span>mx.rpc.events.ResultEvent;</pre>
<pre><span> 8:  </span>            <span style="color:#0033ff">import </span>mx.controls.Alert;</pre>
<pre><span> 9:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>onResult(event:ResultEvent):<span style="color:#0033ff">void</span></pre>
<pre><span>10:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>11:  </span>                <span style="color:#6699cc">var </span>arr:ArrayCollection = <span style="color:#0033ff">this</span>.MyService.GetEmployeeList.lastResult <span style="color:#0033ff">as </span>ArrayCollection;</pre>
<pre><span>12:  </span>                gvEmployee.dataProvider=arr;</pre>
<pre><span>13:  </span>            }</pre>
<pre><span>14:  </span> </pre>
<pre><span>15:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>onFault(event:FaultEvent):<span style="color:#0033ff">void</span></pre>
<pre><span>16:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>17:  </span>               Alert.show(<span style="color:#990000">"调</span>+event.message);</pre>
<pre><span>18:  </span>            }</pre>
<pre><span>19:  </span> </pre>
<pre><span>20:  </span>            <span style="color:#0033ff">private </span><span style="color:#339966">function </span>GetEmployeeList():<span style="color:#0033ff">void</span></pre>
<pre><span>21:  </span><span style="color:#0033ff">            </span>{</pre>
<pre><span>22:  </span>                <span style="color:#0033ff">this</span>.MyService.GetEmployeeList.send();</pre>
<pre><span>23:  </span>            }</pre>
<pre><span>24:  </span>        ]]&gt;</pre>
<pre><span>25:  </span>    <span style="color:#006633">&lt;/mx:Script&gt;</span></pre>
<pre><span>26:  </span><span style="color:#006633">    </span></pre>
<pre><span>27:  </span><span style="color:#006633">    </span><span style="color:blue">&lt;mx:Panel </span>width="<span style="color:#990000">400</span>"<span style="color:blue">&gt;</span></pre>
<pre><span>28:  </span><span style="color:blue">        &lt;mx:DataGrid </span>id="<span style="color:#990000">gvEmployee</span>" width="<span style="color:#990000">100%</span>"<span style="color:blue">&gt;</span></pre>
<pre><span>29:  </span><span style="color:blue">            &lt;mx:columns&gt;</span></pre>
<pre><span>30:  </span><span style="color:blue">                &lt;mx:DataGridColumn </span>headerText="<span style="color:#990000">ID</span>" dataField="<span style="color:#990000">id</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>31:  </span><span style="color:blue">                &lt;mx:DataGridColumn </span>headerText="<span style="color:#990000">Name</span>" dataField="<span style="color:#990000">name</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>32:  </span><span style="color:blue">                &lt;mx:DataGridColumn </span>headerText="<span style="color:#990000">Age</span>" dataField="<span style="color:#990000">age</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>33:  </span><span style="color:blue">            &lt;/mx:columns&gt;</span></pre>
<pre><span>34:  </span><span style="color:blue">        &lt;/mx:DataGrid&gt;</span></pre>
<pre><span>35:  </span><span style="color:blue">        &lt;mx:ControlBar&gt;</span></pre>
<pre><span>36:  </span><span style="color:blue">            &lt;mx:Button </span>label="<span style="color:#990000">Get List</span>" click="GetEmployeeList()"<span style="color:blue">/&gt;</span></pre>
<pre><span>37:  </span><span style="color:blue">        &lt;/mx:ControlBar&gt;</span></pre>
<pre><span>38:  </span><span style="color:blue">    &lt;/mx:Panel&gt;</span></pre>
<pre><span>39:  </span><span style="color:blue">    </span></pre>
<pre><span>40:  </span><span style="color:blue">    &lt;mx:WebService </span>id="<span style="color:#990000">MyService</span>" wsdl="<span style="color:#990000">http://localhost:4081/Flex.asmx?WSDL</span>" useProxy="<span style="color:#990000">false</span>" result="onResult(event)" fault="onFault(event)"<span style="color:blue">&gt;</span></pre>
<pre><span>41:  </span><span style="color:blue">        &lt;mx:operation </span>name="<span style="color:#990000">GetEmployeeList</span>"<span style="color:blue">/&gt;</span></pre>
<pre><span>42:  </span><span style="color:blue">    &lt;/mx:WebService&gt;</span></pre>
<pre><span>43:  </span><span style="color:blue">&lt;/mx:Application&gt;</span></pre>
<pre><span>44:  </span></pre>
</div>

<p>     运行结果：</p>
<p> <a href="http://images.cnblogs.com/cnblogs_com/zhaozhan/WindowsLiveWriter/FlexWebService_131B7/image_4.png"><img title="image" style="display:inline;border:0px" src="http://images.cnblogs.com/cnblogs_com/zhaozhan/WindowsLiveWriter/FlexWebService_131B7/image_thumb_1.png" border="0" alt="image" width="405" height="239"></a></p><img src="http://www.cnblogs.com/zhaozhan/aggbug/1846170.html?type=1" width="1" height="1" alt=""><p>评论: 2　<a href="http://www.cnblogs.com/zhaozhan/archive/2010/10/08/1846170.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/zhaozhan/archive/2010/10/08/1846170.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/76732/">超级网银已试运行满月 专家担心安全隐患</a><span style="color:gray">(2010-10-09 16:06)</span><br>· <a href="http://news.cnblogs.com/n/76731/">58同城CEO姚劲波：营收今年翻番</a><span style="color:gray">(2010-10-09 15:42)</span><br>· <a href="http://news.cnblogs.com/n/76730/">Opera正式宣布加入Open Screen Project</a><span style="color:gray">(2010-10-09 15:37)</span><br>· <a href="http://news.cnblogs.com/n/76729/">全球最佳雇主CEO：坚持最愚蠢的愚蠢想法</a><span style="color:gray">(2010-10-09 15:35)</span><br>· <a href="http://news.cnblogs.com/n/76727/">Ubuntu预计明年4月推出中国定制版</a><span style="color:gray">(2010-10-09 15:24)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/76477/">远离.NET</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>