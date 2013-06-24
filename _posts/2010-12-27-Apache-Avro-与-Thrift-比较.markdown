---
layout: post
title:  "Apache Avro 与 Thrift 比较"
date:   2010-12-27 14:52:31
author: fankong
categories: program
---

## Apache Avro 与 Thrift 比较
### by fankong
### at 2010-12-27 14:52:31
### original <http://www.tbdata.org/archives/1307>

<p>Avro和Thrift都是跨语言，基于二进制的高性能的通讯中间件. 它们都提供了数据序列化的功能和RPC服务. 总体功能上类似，但是哲学不一样. Thrift出自Facebook用于后台各个服务间的通讯,Thrift的设计强调统一的编程接口的多语言通讯框架. Avro出自Hadoop之父Doug Cutting, 在Thrift已经相当流行的情况下Avro的推出，其目标不仅是提供一套类似Thrift的通讯中间件更是要建立一个新的，标准性的云计算的数据交换和存储的Protocol。 这个和Thrift的理念不同，Thrift认为没有一个完美的方案可以解决所有问题，因此尽量保持一个Neutral框架，插入不同的实现并互相交互。而Avro偏向实用，排斥多种方案带来的 可能的混乱，主张建立一个统一的标准，并不介意采用特定的优化。Avro的创新之处在于融合了显式,declarative的Schema和高效二进制的数据表达，强调数据的自我描述，克服了以往单纯XML或二进制系统的缺陷。Avro对Schema动态加载功能，是Thrift编程接口所不具备的，符合了Hadoop上的Hive/Pig及NOSQL 等既属于ad hoc，又追求性能的应用需求.</p>
<h2>语言绑定</h2>
<p>目前阶段Thrift比Avro支持的语言更丰富.</p>
<p>Thrift:  C++, C#, Cocoa, Erlang, Haskell, Java, Ocami, Perl, PHP, Python, Ruby, Smalltalk.</p>
<p>Avro:   C, C++, Java, Python, Ruby, PHP.</p>
<h2>数据类型</h2>
<p>从常见的数据类型的角度来说， Avro和Thrift非常接近，功能上并没有什么区别。</p>
<table border="1" cellspacing="0" cellpadding="0" width="99%">
<tbody>
<tr>
<td width="21%" valign="top"><strong> </strong></td>
<td width="23%" valign="top"><strong> Avro</strong><strong> </strong></td>
<td width="26%" valign="top"><strong> Thrift</strong><strong> </strong></td>
<td width="28%" valign="top"><strong> </strong></td>
</tr>
<tr>
<td width="21%" valign="top">基本类型</td>
<td width="23%" valign="top"></td>
<td width="26%" valign="top"></td>
<td width="28%" valign="top"><tt> </tt>
<p><tt>true</tt> or <tt>false</tt></p></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">N/A</td>
<td width="26%" valign="top"></td>
<td width="28%" valign="top">8-bit signed integer</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">N/A</td>
<td width="26%" valign="top">I16</td>
<td width="28%" valign="top">16-bit signed integer</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">int</td>
<td width="26%" valign="top">I32</td>
<td width="28%" valign="top">32-bit signed integer</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">long</td>
<td width="26%" valign="top">I64</td>
<td width="28%" valign="top">64-bit signed integer</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">float</td>
<td width="26%" valign="top">N/A</td>
<td width="28%" valign="top">32-bit floating point</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">double</td>
<td width="26%" valign="top">double</td>
<td width="28%" valign="top">64-bit floating point</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">bytes</td>
<td width="26%" valign="top">binary</td>
<td width="28%" valign="top">Byte sequence</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">string</td>
<td width="26%" valign="top">string</td>
<td width="28%" valign="top">Character sequence</td>
</tr>
<tr>
<td width="21%" valign="top">复杂类型</td>
<td width="23%" valign="top"></td>
<td width="26%" valign="top"></td>
<td width="28%" valign="top"></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">record</td>
<td width="26%" valign="top">struct</td>
<td width="28%" valign="top">用户自定义类型</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">enum</td>
<td width="26%" valign="top">enum</td>
<td width="28%" valign="top"></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">array&lt;T&gt;</td>
<td width="26%" valign="top">list&lt;T&gt;</td>
<td width="28%" valign="top"></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">N/A</td>
<td width="26%" valign="top">set&lt;T&gt;</td>
<td width="28%" valign="top"></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">map&lt;string,T&gt;</td>
<td width="26%" valign="top">map&lt;T1,T2&gt;</td>
<td width="28%" valign="top">Avro map的key
<p>必须是string</p></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">union</td>
<td width="26%" valign="top">union</td>
<td width="28%" valign="top"></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">fixed</td>
<td width="26%" valign="top">N/A</td>
<td width="28%" valign="top">固定大小的byte array
<pre>e.g. md5(16);</pre>
</td>
</tr>
<tr>
<td width="21%" valign="top">RPC服务</td>
<td width="23%" valign="top"></td>
<td width="26%" valign="top"></td>
<td width="28%" valign="top"></td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">protocol</td>
<td width="26%" valign="top">service</td>
<td width="28%" valign="top">RPC服务类型</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">error</td>
<td width="26%" valign="top">exception</td>
<td width="28%" valign="top">RPC异常类型</td>
</tr>
<tr>
<td width="21%" valign="top"></td>
<td width="23%" valign="top">namespace</td>
<td width="26%" valign="top">namespace</td>
<td width="28%" valign="top">域名</td>
</tr>
</tbody>
</table>
<h2>开发流程</h2>
<p>从开发者角度来说，Avro和Thrift也相当类似，</p>
<p>1)       同一个服务分别用Avro和Thrift来描述</p>
<p>Avro.idl:</p>
<p>protocol SimpleService {</p>
<p>record Message {</p>
<p>string topic;</p>
<p>bytes content;</p>
<p>long    createdTime;</p>
<p>string id;</p>
<p>string ipAddress;</p>
<p>map&lt;string&gt; props;</p>
<p>}</p>
<p>int publish(string context,array&lt;Message&gt; messages);</p>
<p>}</p>
<p>Thrift.idl:</p>
<p>struct Message {</p>
<p>1: string topic</p>
<p>2: binary content</p>
<p>3: i64    createdTime</p>
<p>4: string id</p>
<p>5: string ipAddress</p>
<p>6: map&lt;string,string&gt; props</p>
<p>}</p>
<p>service SimpleService {</p>
<p>i32 publish(1:string context,2:list&lt;Message&gt; messages);</p>
<p>}</p>
<p>2)       Avro和Thrift都支持IDL代码生成功能</p>
<p>java idl avro.idl idl.avro</p>
<p>java org.apache.avro.specific.SpecificCompiler idl.avro avro-gen</p>
<p>目标目录生成Message.java和SimpleService.java</p>
<p>thrift -gen java thrift.idl</p>
<p>同样的,目标目录生成Message.java和SimpleService.java</p>
<p>3)       客户端代码</p>
<p>Avro client :</p>
<p>URL url = new URL ( “http”, HOST, PORT, “/”);</p>
<p>Transceiver trans = new HttpTransceiver(url);</p>
<p>SimpleService proxy=</p>
<p>= (SimpleService)SpecificRequestor.getClient(SimpleService.class, transceiver);</p>
<p>…</p>
<p>Thrift client :</p>
<p>TTransport transport = new TFramedTransport(new TSocket(HOST,PORT));</p>
<p>TProtocol protocol = new TCompactProtocol(transport);</p>
<p>transport.open();</p>
<p>SimpleService.Client client = new SimpleService.Client(protocol);</p>
<p>…</p>
<p>4)       服务器端 Avro和Thrift都生成接口需要实现：</p>
<p>Avro server:</p>
<p>public static class ServiceImpl implements SimpleService {</p>
<p>..</p>
<p>}</p>
<p>Responder responder = new SpecificResponder(SimpleService.class, new ServiceImpl());</p>
<p>Server server = new HttpServer(responder, PORT);</p>
<p>Thrift server:</p>
<p>public static class ServerImpl implements SimpleService.Iface {</p>
<p>..</p>
<p>}</p>
<p>TServerTransport serverTransport=new TServerSocket(PORT);</p>
<p>TServer server=new TSimpleServer(processor,serverTransport,new TFramedTransport.Factory(), new TCompactProtocol.Factory());</p>
<p>server.serve();</p>
<h2>Schema处理</h2>
<p>Avro和Thrift处理Schema方法截然不同。</p>
<p>Thrift是一个面向编程的系统, 完全依赖于IDL-&gt;Binding Language的代码生成。 Schema也“隐藏”在生成的代码中了，完全静态。为了让系统识别处理一个新的数据源，必须走编辑IDL，代码生成，编译载入的流程。</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a1.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a1.jpg" alt="" width="556" height="203"></a></p>
<p>与此对照，虽然Avro也支持基于IDL的Schema描述,但Avro内部Schema还是显式的,存在于JSON格式的文件当中，Avro可以把IDL格式的Schema转化成JSON格式的。</p>
<p>Avro支持2种方式。Avro-specific方式和Thrift的方式相似，依赖代码生成产生特定的类，并内嵌JSON Schema.　Avro-generic方式支持Schema的动态加载，用通用的结构(map)代表数据对象，不需要编译加载直接就可以处理新的数据源。</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a2.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a2.jpg" alt="" width="556" height="410"></a></p>
<h2>Serialization</h2>
<p>对于序列化Avro制定了一个协议，而Thrift的设计目标是一个框架，它没有强制规定序列化的格式。</p>
<p>Avro规定一个标准的序列化的格式，即无论是文件存储还是网络传输，数据的Schema(in JASON)都出现在数据的前面。数据本身并不包含任何Metadata(Tag). 在文件储存的时候，schema出现在文件头中。在网络传输的时候Schema出现在初始的握手阶段.这样的好处一是使数据self describe,提高了数据的透明度和可操作性，二是减少了数据本身的信息量提高存储效率，可谓一举二得了</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a3.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a3.jpg" alt="" width="556" height="69"></a></p>
<p>Avro的这种协议提供了很多优化的机会:</p>
<ul>
<li>对数据作Projection，通过扫描schema只对感兴趣的部分作反序列化。</li>
</ul>
<ul>
<li>支持schema的versioning和mapping ,不同的版本的Reader和Writer可以通过查询schema相互交换数据(schema的aliases支持mapping),这比thrift采用的给每个域编号的方法优越多了</li>
</ul>
<p>Avro的Schema允许定义数据的排序Order并在序列化的时候遵循这个顺序。这样话不需要反序列化就可以直接对数据进行排序,在Hadoop里很管用.</p>
<p>另外一个Avro的特性是采用block链表结构，突破了用单一整型表示大小的限制。比如Array或Map由一系列Block组成，每个Block包含计数器和对应的元素，计数器为0标识结束。</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a4.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a4.jpg" alt="" width="556" height="156"></a></p>
<p>Thrift提供了多种序列化的实现：</p>
<p>TCompactProtocol: 最高效的二进制序列化协议，但并不是所有的绑定语言都支持。</p>
<p>TBinaryProtocol: 缺省简单二进制序列化协议.</p>
<p>与Avro不同，Thrift的数据存储的时候是每个Field前面都是带Tag的，这个Tag用于标识这个域的类型和顺序ID（IDL中定义，用于Versioning）。在同一批数据里面，这些Tag的信息是完全相同的，当数据条数大的时候这显然就浪费了。</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a51.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a51.jpg" alt="" width="556" height="69"></a></p>
<h2>RPC服务</h2>
<p>Avro提供了</p>
<p>HttpServer : 缺省,基于Jetty内核的服务.</p>
<p>NettyServer: 新的基于Netty的服务.</p>
<p>Thrift提供了：</p>
<p>TThreadPolServer: 多线程服务</p>
<p>TNonBlockingServer: 单线程 non blocking的服务</p>
<p>THsHaServer: 多线程 non blocking的服务</p>
<h2>Benchmarking</h2>
<p>测试环境：2台4核 Intel  Xeon 2.66GHz， 8G memory, Linux, 分别做客户端，服务器。</p>
<p>Object definition:</p>
<p>record Message {</p>
<p>string topic;</p>
<p>bytes payload;</p>
<p>long createdTime;</p>
<p>string id;</p>
<p>string ipAddress;</p>
<p>map&lt;string,string &gt; props;</p>
<p>}</p>
<p>Actual instance:</p>
<p>msg.createdTime : System.<em>nanoTime</em>();</p>
<p>msg.ipAddress : “127.0.0.1″;</p>
<p>msg.topic : “pv”;</p>
<p>msg.payload : byte[100]</p>
<p>msg.id : UUID.<em>randomUUID</em>().toString();</p>
<p>msg.props : <strong>new</strong> HashMap&lt;String,String&gt;();</p>
<p>msg.props.put(“author”, “tjerry”);</p>
<p>msg.props.put(“date”, <strong>new</strong> Date().toString());</p>
<p>msg.props.put(“status”, “new”);</p>
<p>Serialization size</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a6.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a6.jpg" alt="" width="559" height="328"></a></p>
<p>Avro的序列化产生的结果最小</p>
<p>Serialization speed</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a7.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a7.jpg" alt="" width="559" height="328"></a></p>
<p>Thrift-binary因为序列化方式简单反而看上去速度最快.</p>
<p>Deserialization speed</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/a8.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/a8.jpg" alt="" width="559" height="328"></a></p>
<p>这里 Thrift的速度很快， 因与它内部实现采用zero-copy的改进有关.不过在RPC综合测试里这一优势</p>
<p>似乎并未体现出来.</p>
<p>序列化测试数据采集利用了<a href="http://code.google.com/p/thrift-protobuf-compare/">http://code.google.com/p/thrift-protobuf-compare/</a>所提供的框架,</p>
<p>原始输出：</p>
<p>Starting</p>
<p>,   Object create,       Serialize,  /w Same Object,     Deserialize, and Check Media,   and Check All,      Total Time, Serialized Size</p>
<p>avro-generic        ,      8751.30500,     10938.00000,      1696.50000,     16825.00000,     16825.00000,     16825.00000,     27763.00000,        221</p>
<p>avro-specific       ,      8566.88000,     10534.50000,      1242.50000,     18157.00000,     18157.00000,     18157.00000,     28691.50000,        221</p>
<p>thrift-compact      ,      6784.61500,     11665.00000,      4214.00000,      1799.00000,      1799.00000,      1799.00000,     13464.00000,        227</p>
<p>thrift-binary       ,      6721.19500,     12386.50000,      4478.00000,      1692.00000,      1692.00000,      1692.00000,     14078.50000,        273</p>
<p>RPC测试用例：</p>
<p>客户端向服务器发送一组固定长度的message,为了能够同时测试序列和反序列，服务器收到后将原message返回给客户端．</p>
<p>array&lt;Message&gt; publish(string context,　array&lt;Message&gt; messages);</p>
<p>测试使用了Avro Netty Server和 Thrift HaHa Server因为他们都是基于异步IO的并且适用于高并发的环境。</p>
<p>结果</p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/avro1.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/avro1.jpg" alt="" width="937" height="685"></a></p>
<p><a href="http://www.tbdata.org/wp-content/uploads/2010/12/avro21.jpg"><img src="http://www.tbdata.org/wp-content/uploads/2010/12/avro21.jpg" alt="" width="937" height="685"></a></p>
<p>从这个测试来看，再未到达网络瓶颈前，Avro Netty比Thrift HsHa服务提供了更高的吞吐率和更快的响应，另外 avro占用的内存高些。</p>
<p>通过进一步实验,发现不存在绝对的Avro和Thrift服务哪一个更快,决定于给出的test case,或者说与程序的用法有关,比如当前测试用例是Batch模式，大量发送fine grained的对象(接近后台tt,hadoop的用法),这个情况下Avro有优势. 但是对于每次只传一个对象的chatty客户端,情况就出现逆转变成Thrift更高效了.还有当数据结构里blob比例变大的情况下,Avro和Thrift的差别也在减小.</p>
<h2>Conclusion</h2>
<ul>
<li> Thrift适用于程序对程序静态的数据交换，要求schema预知并相对固定。</li>
</ul>
<ul>
<li> Avro在Thrift基础上增加了对schema动态的支持且性能上不输于Thrift。</li>
</ul>
<ul>
<li> Avro显式schema设计使它更适用于搭建数据交换及存储的通用工具和平台,特别是在后台。</li>
</ul>
<ul>
<li> 目前Thrift的优势在于更多的语言支持和相对成熟。</li>
</ul>
<p><img src="http://www.alidata.org/DOCUME%7E1/huankong/LOCALS%7E1/Temp/moz-screenshot.png" alt=""></p>