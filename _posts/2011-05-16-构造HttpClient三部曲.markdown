---
layout: post
title:  "构造HttpClient三部曲"
date:   2011-05-16 12:05:45
author: iGFW
categories: program
---

## 构造HttpClient三部曲
### by iGFW
### at 2011-05-16 12:05:45
### original <http://igfw.tk/archives/2680>

<p><strong>构造HttpClient三部曲之一：支持代理的Socket封装</strong></p>
<p>最近在重构闪印的网络部分的代码——原因不外乎当时写的时候太着急，很多东西都没有好好的封装，将就着能用就行了。而挪到Ebox中为了传说中的平台无 关，像FileStream，Socket之类的用了大量JUCE的封装，觉得异常恶心，而由于种种原因目前很清闲，于是开始重构这部分的代码(实际上是 自己从头写起……)，目前也整了个大概了，于是可以写写总结：《构造HttpClient三部曲》。</p>
<p><span></span>话说工欲善其事，必先利其器，而构造HttpClient的器就是可支持代理的Socket封装类——当初自己写HttpClient而不是使用 WinInet之类的库很大部分原因就是WinInet只支持Http代理而不支持Socks的代理，所以只能自己动手丰衣足食了。(话说很奇怪微软的很 多东西都是这样只支持HTTP代理，比如C#中比较有名的<a href="http://www.mentalis.org/soft/class.qpx?id=9">ProxySocket</a>都是别人写的)而C++方面貌似也没有支持代理的HTTP库，倒是C方面有个curl，如果自己懒得封装Http倒是可以拿来用。</p>
<p>言归正传，说ProxySocket的封装。大体来说代理服务器分为三种：http,ftp和socks，更有透明代理(比如公司的翻墙代理就是种透明代 理)和非透明代理之分。而这里我只关心http和socks这两种比较常见的非透明代理，至于ftp代理貌似只能从文献中听闻了，基本很少有什么服务器提 供这样的代理。当机器通过代理服务器上网时，整个通讯过程是分为两个部分的：机器和代理通讯，代理和目的地址通讯，这样一来客户端需要关心的就只有：和代 理服务器完成一次对传输协议协商握手过程，在这之后就可以把代理服务器看成目标地址了。</p>
<p> </p>
<p><strong>HTTP代理</strong></p>
<p><strong> </strong>当客户端连接上一个HTTP代理服务器后并通过它发送请求，代理服务器做的事情就是：建立和目标地 址的连接，发送请求，接受反馈并将反馈发回客户端。为了实现这一点，在HTTP协议中规定了这么一个特殊方法：CONNECT。当客户端和HTTP代理服 务器连接后，只需要发送如下格式的HTTP请求即可：</p>
<table border="0" cellspacing="0" cellpadding="2" width="562">
<tbody>
<tr>
<td width="560" valign="top">CONNECT &lt;destanation_address&gt; : &lt;destanition_port&gt; &lt;http_version&gt;&lt;CR&gt;&lt;LF&gt;<br>
Host: &lt;destanation_address&gt; : &lt;destanition_port&gt;&lt;CR&gt;&lt;LF&gt;<br>
&lt;optional_header_line&gt;&lt;CR&gt;&lt;LF&gt;<br>
&lt;optional_header_line&gt;&lt;CR&gt;&lt;LF&gt;<br>
……<br>
&lt;CR&gt;&lt;LF&gt;</td>
</tr>
</tbody>
</table>
<p> </p>
<p>而如果代理需要验证用户名密码则需要将”用户名：密码”进行Base64编码后填入：</p>
<table border="0" cellspacing="0" cellpadding="2" width="564">
<tbody>
<tr>
<td width="562" valign="top">CONNECT &lt;destanation_address&gt; : &lt;destanition_port&gt; &lt;http_version&gt;&lt;CR&gt;&lt;LF&gt;<br>
Host: &lt;destanation_address&gt; : &lt;destanition_port&gt;&lt;CR&gt;&lt;LF&gt;<br>
Authorization: Basic &lt;base64后的验证字段&gt;&lt;CR&gt;&lt;LF&gt;<br>
Proxy-Authorization: &lt;Basic base64后的验证字段&gt;&lt;CR&gt;&lt;LF&gt;<br>
&lt;optional_header_line&gt;&lt;CR&gt;&lt;LF&gt;<br>
……<br>
&lt;CR&gt;&lt;LF&gt;</td>
</tr>
</tbody>
</table>
<p>即：(再次怨念一下狗屎的代码格式化插件)</p>
<div>
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>if</code> <code>(!_proxy_config._host_name.empty())</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code> </code><code>std::string auth = _proxy_config._host_name + </code><code>":"</code> <code>+ _proxy_config._password;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code><code>std::string base64_encode_auth;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code> </code><code>Util::base64Encode(auth,base64_encode_auth);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code> </code><code>sprintf_s(buff,max_buffer_length,</code><code>"CONNECT %s:%d HTTP/1.1\r\nHost: %s:%d\r\nAuthorization: Basic %s\r\nProxy-Authorization: Basic %s\r\n\r\n"</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code> </code><code>_host_name,_port_number,_host_name,_port_number,base64_encode_auth,base64_encode_auth);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>else</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code> </code><code>sprintf_s(buff,max_buffer_length,</code><code>"CONNECT %s:%d HTTP/1.1\r\nHost: %s:%d\r\n\r\n"</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code> </code><code>_host_name,_port_number,_host_name,_port_number);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<p>而在请求后如果收到正确反馈即表示代理连接成功。(一般就是code =  200)这里值得说明的一点是：上述代码只是对Basic这种验证方式做了处理，这种明文传输的形式是很不安全的。当然还有一种验证方式是NTLM，相对 而言比较复杂，不赘述。(实际上是没空去看…)</p>
<p> </p>
<p><strong>Socks4代理</strong></p>
<p><strong> </strong>Socks4的代理连接请求相比Http代理要简单不少，而且Socks4是不支持用户验证的。整个Socks4的请求就包含5个字段而已：</p>
<p>字段一：Socks版本号，即0×04，占一个字节。</p>
<p>字段二：命令码，占一个字节，其中0×01：TCP/IP连接，而0×02：端口绑定。</p>
<p>字段三：网络字节序端口，占两个字节。<strong> </strong></p>
<p>字段四：网络字节序IP地址，占四个字节。</p>
<p>字段五：用户ID字段，可变，以null(0)结尾。</p>
<p>而服务器返回的反馈则更加简单，一共包含4个字段：</p>
<p>字段一：一个空字节</p>
<p>字段二：一个字节，表示反馈状态码，其中0x5A(即90)表示请求被接受。</p>
<p>字段三：两个字节，可被忽略</p>
<p>字段四：四个字节，可被忽略</p>
<p>可以看出实际上整个Sock4的请求和反馈都是异常简单：SOCK4的反馈甚至只有一个字节是有意义的，很轻松就可以搞定：</p>
<div>
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>//Socks4没有用户密码验证</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>struct</code> <code>Sock4Reqeust</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code><code>char</code> <code>VN;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code> </code><code>char</code> <code>CD;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code> </code><code>unsigned </code><code>short</code> <code>port;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code> </code><code>unsigned </code><code>long</code> <code>ip_address;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code> </code><code>char</code> <code>other[256]; </code><code>// 变长</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>} sock4_request; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code>struct</code> <code>Sock4Reply</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code> </code><code>char</code> <code>VN;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code> </code><code>char</code> <code>CD;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code> </code><code>unsigned </code><code>short</code> <code>port;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>16</code></td>
<td><code> </code><code>unsigned </code><code>long</code> <code>ip_address;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>17</code></td>
<td><code>} sock4_reply; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>18</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>19</code></td>
<td><code>sock4_request.VN = 0x04; </code><code>// VN是SOCK版本，应该是4；</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>20</code></td>
<td><code>sock4_request.CD = 0x01; </code><code>// CD是SOCK的命令码，1表示CONNECT请求，2表示BIND请求；</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>21</code></td>
<td><code>sock4_request.port= ntohs(_port_number);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>22</code></td>
<td><code>sock4_request.ip_address = SocketHelper::getIntAddress(_host_name.c_str());</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>23</code></td>
<td><code>sock4_request.other[0] = </code><code>'\0'</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>24</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>25</code></td>
<td><code>if</code> <code>(sock4_request.ip_address == INADDR_NONE)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>26</code></td>
<td><code> </code><code>return</code> <code>false</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>27</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>28</code></td>
<td><code>//发送SOCKS4连接请求</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>29</code></td>
<td><code>int</code> <code>ret =  _socket.write((</code><code>char</code><code>*)&amp;sock4_request,9);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>30</code></td>
<td><code>if</code> <code>(ret &lt;= 0)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>31</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>32</code></td>
<td><code> </code><code>return</code> <code>false</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>33</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>34</code></td>
<td><code>//获得Socks4代理的回复</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>35</code></td>
<td><code>ret = _socket.read((</code><code>char</code> <code>*)&amp;sock4_reply, </code><code>sizeof</code><code>(sock4_reply));</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>36</code></td>
<td><code>if</code> <code>(ret &lt;= 0)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>37</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>38</code></td>
<td><code> </code><code>return</code> <code>false</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>39</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>40</code></td>
<td><code>/*</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>41</code></td>
<td><code>CD是代理服务器答复，有几种可能：</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>42</code></td>
<td><code>90，请求得到允许；</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>43</code></td>
<td><code>91，请求被拒绝或失败；</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>44</code></td>
<td><code>92，由于SOCKS服务器无法连接到客户端的identd（一个验证身份的进程），请求被拒绝；</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>45</code></td>
<td><code>93，由于客户端程序与identd报告的用户身份不同，连接被拒绝。</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>46</code></td>
<td><code>*/</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>47</code></td>
<td><code>return</code> <code>sock4_reply.CD == 90;</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<p>值得注意的是由于代码中是用结构体表示相应的字段集合，要考虑到字节对齐对结构体字段排放的影响需要指定</p>
<p>#pragma pack(1)。强制以一个字节对齐。Socks4还有一种变种叫做Socks4a，具体就不介绍了，可以参看<a href="http://en.wikipedia.org/wiki/SOCKS#SOCKS_4a">这里</a>。</p>
<p> </p>
<p><strong>Socks5代理</strong></p>
<p><strong> </strong>Socks5是Socks4的一个升级版本，增加了很多Socks4不支持的特性，比如对IPv6的支持。一个完整的Socks5代理握手协议可以分为如下五个步骤：</p>
<p>1.客户端发起握手请求，参数为可支持的验证方法列表</p>
<p>2.服务器选择一种验证方式并返回(如果无可支持的验证方式直接返回失败)</p>
<p>3.根据所选验证方式，客户端和服务端进行验证方式上的协商(比如选择了用户名/密码这种方式进行验证，客户端就需要发送用户和密码过去，然后服务器进行验证并返回结果)</p>
<p>4.客户端发起一个类似于Socks4的请求</p>
<p>5.服务端返回一个类似Socks4的反馈</p>
<p>这样经过五个步骤，客户端和服务端的握手就算完成了，接下来就是直接进行数据传输即可。</p>
<p>Socks5可支持的验证方式包括：</p>
<p>0×00：无验证</p>
<p>0×01：GSSAPI</p>
<p>0×02：用户名/密码</p>
<p>0×03-0x7F：IANA指定的方法</p>
<p>0×80-0xFE：保留方法</p>
<p>对于我们这个简单的ProxySocket来说完全可以只关心0×00和0×02即可，其他几种方式一来不常见，二来也没有太大的实现必要。</p>
<p>客户端发起的第一个请求总共包括三个字段：</p>
<p>字段1:Socks版本号，即0×05</p>
<p>字段2:支持的验证方式数，一个字节</p>
<p>字段3:验证方法列表，变长，一个字节表示一个方法</p>
<p>服务端在收到这个请求后，返回一个验证方法的反馈，一共就两个字段：</p>
<p>字段1：Socks版本号，一个字节，即0×05</p>
<p>字段2：选择的验证方法，一个字节，如果在客户端发来的验证方法列表中没有服务端支持的方法，则返回0xFF</p>
<p>这个时候客户端可以根据服务端饭回来的验证方法进行验证：0×00则直接跳过第三步。一个典型的用户名/密码验证请求如下：</p>
<p>字段1：版本号，一个字节，必须指定为0×01</p>
<p>字段2：用户名长度，一个字节</p>
<p>字段3：用户名，变长</p>
<p>字段4：密码长度，一个字节</p>
<p>字段5：密码，变长</p>
<p>服务端返回如下的反馈：</p>
<p>字段1：版本号，一个字节 (这个其实可以忽略)</p>
<p>字段2：状态码，一个字节，0×00表示成功，其他值则表示验证失败，连接需要断开。</p>
<p>这以后的工作就基本和Sock4相似了，客户端发起一个连接请求：</p>
<p>字段1：Socks版本号，一个字节，即0×05</p>
<p>字段2：命令码，一个字节，0×01：TCP/IP连接，0×02：TCP/IP端口绑定，0×03：关联UDP端口</p>
<p>字段3：保留字段，一个字节，必须指定为0×00</p>
<p>字段4：地址类型，一个字节，0×01：IPv4地址，0×03：域名，0×04：IPv6地址</p>
<p>字段5：目标地址：4字节表示的IPv4地址，或者16字节表示的IPv6地址，如果是域名则是变长：一字节表示域名长度，紧随其后的是域名</p>
<p>字段6：网络序的端口号，两个字节</p>
<p>服务器的反馈为：</p>
<p>字段1：Sock协议版本，一个字节，必须为0×05</p>
<p>字段2：状态码，0×00表示请求成功，其余的状态码可参考相应的RFC文档</p>
<p>字段3：保留字段，一个字节，必须制定为0×00</p>
<p>字段4：地址类型，一个字节，0×01：IPv4地址，0×03：域名，0×04：IPv6地址</p>
<p>字段5：目标地址，4字节表示的IPv4地址，或者16字节表示的IPv6地址，如果是域名则是变长：一字节表示域名长度，紧随其后的是域名</p>
<p>字段6：网络序的端口号，两个字节</p>
<p>这样一个完整的Socks5握手协议就算完成了，但是鉴于代码篇幅太长了，这里就不上了，等整个HttpClient介绍完毕后再统一上代码…….（实际上是…某些地方的代码还没整清爽，无颜见公婆啊）</p>
<p> </p>
<p><strong>参考资料：</strong></p>
<p>1.Wiki中关于Sock5的介绍：<a href="http://en.wikipedia.org/wiki/SOCKS">http://en.wikipedia.org/wiki/SOCKS</a></p>
<p>2.《Http Tunneling》：<a href="http://www.codeproject.com/KB/IP/httptunneling.aspx">http://www.codeproject.com/KB/IP/httptunneling.aspx</a></p>
<p>3.《CAsyncProxySocket – CAsyncSocket derived class to connect through proxies》：<a href="http://www.codeproject.com/KB/IP/casyncproxysocket.aspx">http://www.codeproject.com/KB/IP/casyncproxysocket.aspx</a></p>
<p> </p>
<p><strong>构造HttpClient三部曲之二：GET方法实现</strong></p>
<p>继续HttpClient构造的博文，第二篇：GET方法的实现。HTTP协议定义了和服务器交互的不同方法，包括 GET,POST,PUT,DELETE,CONNECT等等，但是最基本的同时也是用的最多的两个方法就是GET和POST。这篇先讲讲GET方法的一 些细节.</p>
<p>HTTP协议的交互主要有请求和响应组成：客户端发起请求，服务端返回响应。而一个简单的HTTP请求又可以分成信息头和信息体。<span style="color:#ff0000">但</span><span style="color:#ff0000">对于GET来说，它的请求只有HTTP消息头而已。</span></p>
<p><strong>HTTP请求之GET</strong></p>
<p>一个最简单的HTTP GET请求可以写成：</p>
<table border="0" cellspacing="0" cellpadding="2" width="400">
<tbody>
<tr>
<td width="400" valign="top">GET &lt;resouce_path&gt; &lt;http_version&gt; &lt;CR&gt;&lt;LF&gt;<br>
HOST : &lt;host_address&gt;&lt;CR&gt;&lt;LF&gt;<br>
&lt;CR&gt;&lt;LF&gt;</td>
</tr>
</tbody>
</table>
<p>而复杂的请求往往会加入很多的请求头域，如：</p>
<table border="0" cellspacing="0" cellpadding="2" width="585">
<tbody>
<tr>
<td width="583" valign="top">GET /logos/2011/hargreaves11-hp-15.jpg HTTP/1.1<br>
Host: www.google.com.hk<br>
Connection: keep-alive<br>
Referer: <a href="http://www.google.com.hk/">http://www.google.com.hk/</a><br>
User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.24 (KHTML, like Gecko) Chrome/11.0.696.60 Safari/534.24<br>
Accept: */*<br>
Accept-Encoding: gzip,deflate,sdch<br>
Accept-Language: zh-CN,zh;q=0.8<br>
Accept-Charset: GBK,utf-8;q=0.7,*;q=0.3</td>
</tr>
</tbody>
</table>
<p>每一个头域都有一个域名，冒号(：)和域值组成。<strong><span style="color:#ff0000">域名是大小写无关</span></strong>， 而域值前面可以添加数个空格。之所以将前面那段字特地高亮是因为碰到很多“不太规范”的HTTP  Server返回的域名经常是不“正规”的：比如将Content-Type写成Content-type—-这只是从视觉上不太美观，但是绝对是符合规 范的，所以客户端在解析的时候特别要注意忽略大小写的影响。</p>
<p>一些典型的头域有：</p>
<p> </p>
<p>Host：指定请求资源的主机和端口</p>
<p>Connection：指定连接状态，HTTP1.1和1.0协议的一个很大区别就是HTTP1.1可以保持连接，HTTP1.1默认是持久连接，一次连接可以发起多个请求。</p>
<p>User-Agent：包含发出请求的用户信息，多用于系统和浏览器检测。关于它的八卦可以参考<a href="http://www.cnblogs.com/passer/archive/2010/02/14/1668297.html?login=1#commentform">这里</a>。</p>
<p>Referer：请求URI的源地址。一般可用于反盗链。</p>
<p>Accept：可接受的文件类型</p>
<p>当然我们也可以往HTTP头中塞入一些自定义的头域，这样的效果和在URL添加请求参数的效果是一样的。</p>
<p> </p>
<p><strong>HTTP响应</strong></p>
<p><strong> </strong>无论是GET方法还是POST方法，HTTP的响应都是一致的：一个HTTP消息头和一个 HTTP消息体。在HTTP消息头的第一行指定了：HTTP版本号，HTTP响应码和详细消息。而接下来就是一个个头域，直到接受到两 个&lt;CR&gt;&lt;LF&gt;为止。一个典型的HTTP相应的消息头如下：</p>
<table border="0" cellspacing="0" cellpadding="2" width="400">
<tbody>
<tr>
<td width="400" valign="top">HTTP/1.1 200 OK
<p>Cache-Control: private, max-age=30</p>
<p>Content-Type: text/html; charset=utf-8</p>
<p>Content-Encoding: gzip</p>
<p>Expires: Mon, 25 May 2009 03:20:33 GMT</p>
<p>Last-Modified: Mon, 25 May 2009 03:20:03 GMT</p>
<p>Vary: Accept-Encoding</p>
<p>Server: Microsoft-IIS/7.0</p>
<p>X-AspNet-Version: 2.0.50727</p>
<p>X-Powered-By: ASP.NET</p>
<p>Date: Mon, 25 May 2009 03:20:02 GMT</p>
<p>Content-Length: 12173</p></td>
</tr>
</tbody>
</table>
<p>对于客户端来说需要着重关注的头域一般只有<span style="color:#ff0000">Content-Length</span>和<span style="color:#ff0000">Transfer-Encoding</span>而 已。如果返回的HTTP消息头中有Content-Length这个头域则说明HTTP体是定长的，客户端只需要继续接受Content-Length指 定的数据长度的内容即可。而如果服务器在返回数据给客户端时是一边生成数据一边发送，那么很可能就会采用chunked的形式，将Transfer- Encoding指定为chunked。这样客户端对HTTP消息体的处理就会稍显复杂。</p>
<p>在HTTP协议的RFC中对chunk传输的定义如下：</p>
<table border="0" cellspacing="0" cellpadding="2" width="400">
<tbody>
<tr>
<td width="400" valign="top">Chunked-Body   = *chunk<br>
last-chunk<br>
trailer<br>
CRLF<br>
chunk          = chunk-size [ chunk-extension ] CRLF<br>
chunk-data CRLF<br>
chunk-size     = 1*HEX<br>
last-chunk     = 1*(“0″) [ chunk-extension ] CRLF<br>
chunk-extension= *( “;” chunk-ext-name [ &quot;=&quot; chunk-ext-val ] )<br>
chunk-ext-name = token<br>
chunk-ext-val  = token | quoted-string<br>
chunk-data     = chunk-size(OCTET)<br>
trailer        = *(entity-header CRLF)</td>
</tr>
</tbody>
</table>
<p>简单来说就是：一个chunked编码的body可以分为四个部分：一系列的chunk段，last  chunk，trailer和CRLF。一个chunk又可以分为chunk-size和chunk-data两部分。通过解析chunk- size(16进制)可以获取到真正的chunk-data长度并进行拼接得到最后的HTTP体内容。RFC中关于chunk的解析伪代码如下：</p>
<pre>length := 0
　　read chunk-size, chunk-ext (if any) and CRLF
　　while (chunk-size &gt; 0) {
　　read chunk-data and CRLF
　　append chunk-data to entity-body
　　length := length + chunk-size
　　read chunk-size and CRLF
　　}
　　read entity-header
　　while (entity-header not empty) {
　　append entity-header to existing header fields
　　read entity-header
　　}
　　Content-Length := length
　　Remove &quot;chunked&quot; from Transfer-Encoding</pre>
<p>为了构造这个HTTPClient我也相应写了C++版的处理方法，不过没有考虑chunk-extension和entity-header的处理(在我测试的几个网站来看都没有这两个值，所以即使写了也无法验证其正确性)，将这部分片段摘录如下：</p>
<div>
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>//接受并解析chunk内容</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>while</code><code>(</code><code>true</code><code>)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code><code>//如果在上次已经查询到第一块chunk的大小</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code> </code><code>if</code> <code>(find_first_chunk)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code> </code><code>if</code> <code>(chunk_size == 0)</code><code>//如果是最后一块了</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code> </code><code>complete = </code><code>true</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code> </code><code>break</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code> </code><code>else</code> <code>//否则分析chunk内容并进行切割</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code> </code><code>size_t</code> <code>length        = body.length();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code> </code><code>size_t</code> <code>first_chunk    = chunk_size_length + 2 + chunk_size + 2;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>16</code></td>
<td><code> </code><code>if</code> <code>(length &gt;= first_chunk) </code><code>//如果已经接受到一整块chunkdata了则进行切割，否则重新接受</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>17</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>18</code></td>
<td><code> </code><code>find_first_chunk        = </code><code>false</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>19</code></td>
<td><code> </code><code>std::string chunk_data    = body.substr(chunk_size_length + 2, chunk_size);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>20</code></td>
<td><code> </code><code>body.erase(0,first_chunk);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>21</code></td>
<td><code> </code><code>if</code> <code>(!response_receiver-&gt;write(chunk_data.c_str(),chunk_data.length()))</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>22</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>23</code></td>
<td><code> </code><code>setErrorCode(HTTPERROR_IO);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>24</code></td>
<td><code> </code><code>break</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>25</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>26</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>27</code></td>
<td><code> </code><code>else</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>28</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>29</code></td>
<td><code> </code><code>if</code> <code>(!continueToReceiveBody(body))</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>30</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>31</code></td>
<td><code> </code><code>setErrorCode(HTTPERROR_TRANSPORT);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>32</code></td>
<td><code> </code><code>break</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>33</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>34</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>35</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>36</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>37</code></td>
<td><code> </code><code>else</code><code>//查找chunk_size</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>38</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>39</code></td>
<td><code> </code><code>size_t</code> <code>index = body.find(</code><code>"\r\n"</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>40</code></td>
<td><code> </code><code>if</code> <code>(index != std::string::npos) </code><code>//找到，做标记</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>41</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>42</code></td>
<td><code> </code><code>find_first_chunk            = </code><code>true</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>43</code></td>
<td><code> </code><code>chunk_size_length            = (</code><code>int</code><code>)index;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>44</code></td>
<td><code> </code><code>std::string raw_chunk_size    = body.substr(0,chunk_size_length);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>45</code></td>
<td><code> </code><code>chunk_size                    = (</code><code>int</code><code>)</code><code>strtoul</code><code>(raw_chunk_size.c_str(),0,16);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>46</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>47</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>48</code></td>
<td><code> </code><code>else</code> <code>//没有找到，继续接受信息</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>49</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>50</code></td>
<td><code> </code><code>if</code> <code>(!continueToReceiveBody(body))</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>51</code></td>
<td><code> </code><code>{</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>52</code></td>
<td><code> </code><code>setErrorCode(HTTPERROR_TRANSPORT);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>53</code></td>
<td><code> </code><code>break</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>54</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>55</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>56</code></td>
<td><code> </code><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>57</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<p>至此一个完整的GET方法HTTP请求响应就完成了，相应的代码checkout这里：<strong><em><a href="http://amaoproject.googlecode.com/svn/trunk/">http</a></em>://amaoproject.googlecode.com/svn/trunk/</strong>。</p>
<p> </p>
<p><strong>构造HttpClient三部曲终篇：POST方法实现</strong></p>
<p>手机新买，新鲜感未过，几乎一天都在安装试用卸载各种搞毛软件中度过，差点忘了要在这周结束掉HttpClient的博文，趁着还有3个小时才12点赶紧写完。</p>
<p><a href="http://www.xiangwangfeng.com/2011/05/09/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E4%B9%8B%E4%BA%8C%EF%BC%9Aget%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0/">上一篇</a>介绍了GET方法的实现，这篇主要就介绍介绍POST。从上层来看，GET和POST最大的区别在于GET是一种从服务器获取数据的请求，而POST是向服务器传送数据，进行站点更新。而从协议上来看，POST和GET的最大区别在于：<span style="color:#ff0000">POST请求是包含HTTP消息体的，较GET能够实现更复杂的数据交互。</span>通过上一篇我们已经知道一个GET请求是只有HTTP消息头的，虽然它也能够带上参数：将参数和请求的URL以?分开，但是一来请求的数据量很少，二来因为请求格式的固定化导致不能有效表达一些复杂的请求，而POST则没有这样的问题了。</p>
<p><strong>HTTP请求之POST</strong></p>
<p>和GET方法不同，POST有自己的HTTP请求体，这样也必然要求POST方法必须在HTTP请求头指明请求体的类型和长度：即content- type和content-length。content-type指明了Http消息体内的数据以怎样的结构进行组织，而content-length 则指明了http消息体的长度。一个典型的POST请求如下：</p>
<table border="0" cellspacing="0" cellpadding="2" width="600">
<tbody>
<tr>
<td width="598" valign="top">POST /cwf_nget?param=[] HTTP/1.1<br>
Host: friend.renren.com<br>
Connection: keep-alive<br>
Referer: <a href="http://friend.renren.com/ajaxproxy.htm">http://friend.renren.com/ajaxproxy.htm</a><br>
Content-Length: 24<br>
Origin: <a href="http://friend.renren.com/">http://friend.renren.com</a><br>
User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.24 (KHTML, like Gecko) Chrome/11.0.696.60 Safari/534.24<br>
Content-Type: application/x-www-form-urlencoded<br>
Accept: */*<br>
Accept-Encoding: gzip,deflate,sdch<br>
Accept-Language: zh-CN,zh;q=0.8<br>
Accept-Charset: GBK,utf-8;q=0.7,*;q=0.3
<p>requestToken=-1000000000</p></td>
</tr>
</tbody>
</table>
<p>例子中的content-type为application/x-www-form-urlencoded，即需要对HTTP体进行URL编码。而 content-length为24，即后面requestToken=-1000000000的长度。当然Content-Type有很多种，比如 text/plain，text/xml等等，具体的意义可以参考<a href="http://www.utoronto.ca/webdocs/HTMLdocs/Book/Book-3ed/appb/mimetype.html">这里</a>。 而唯一值得拿出来说的一种content-type是multipart/form-data的情况。顾名思义这种content-type要求HTTP 体是以多段的格式发送，主要是为了能够通过HTTP协议上传文件。当然也可以只用这种编码方式来分段上传请求数据，不过不免有点脱裤子放屁之嫌。</p>
<p> </p>
<p> </p>
<p>一个典型的以multipart/form-data上传文件的Http包如下：</p>
<table border="0" cellspacing="0" cellpadding="2" width="590">
<tbody>
<tr>
<td width="588" valign="top">POST /upload HTTP/1.1<br>
Host: upload.buzz.163.com<br>
Connection: keep-alive<br>
Referer: <a href="http://t.163.com/">http://t.163.com/</a><br>
Content-Length: 8993<br>
Cache-Control: max-age=0<br>
Origin: <a href="http://t.163.com/">http://t.163.com</a><br>
User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.24 (KHTML, like Gecko) Chrome/11.0.696.60 Safari/534.24<br>
Content-Type: multipart/form-data; boundary=—-WebKitFormBoundaryoF81IsooBo64lBY2<br>
Accept: application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5<br>
Accept-Encoding: gzip,deflate,sdch<br>
Accept-Language: zh-CN,zh;q=0.8<br>
Accept-Charset: GBK,utf-8;q=0.7,*;q=0.3<br>
Cookie:  _ntes_nuid=b243d3e6005fcc75916dcf6a05a1231d;  vjuids=-115573d47.12ba116f81c.0.6e77627f;  ALLYESID4=00101015004134494510218; isGd=0; isFs=0;  OUTFOX_SEARCH_USER_ID=-1937482032@10.130.10.67;  __utmz=187553192.1299506530.4.3.utmcsr=blog.163.com|utmccn=(referral)|utmcmd=referral|utmcct=/lgh_2002/;  USERTRACK=60.186.110.113.1302880928690537;  __utma=187553192.1936313005.1288621751.1301116115.1304778661.6;  MAIL163_SSN=xiang.wangfeng; Province=0571; City=0571; NTES_LOGINED=true;  vjlast=1286897859.1305430125.11;
<p>——WebKitFormBoundaryoF81IsooBo64lBY2<br>
Content-Disposition: form-data; name=”callback”</p>
<p>uploadImageCallback<br>
——WebKitFormBoundaryoF81IsooBo64lBY2<br>
Content-Disposition: form-data; name=”watermark”</p>
<p>t.163.com/amao<br>
——WebKitFormBoundaryoF81IsooBo64lBY2<br>
Content-Disposition: form-data; name=”Filedata”; filename=”head_5685p40.jpg”<br>
Content-Type: image/jpeg</p>
<p>??.JFIF…..x.x..?C………………….<br>
…..<br>
…<br>
..<br>
…<br>
…………………….?C…….    ..    .<br>
.<br>
…………………………………………..?…d.d……….?………………………?B…………<br>
…………..!1A.”Q..2aq?.#態?Rbr偙$4DSUct儞擦佯?……………..曘膇X.s菢t=5Y鰦.?.敠婂糩輾纽脡昍獛.h塱7鶛フ3?xA/.[B}.渀hn提挮mN笢菨扦.跍Ni?胘?祻?牛S喁膷<br>
;.g 鉉`.逽枻%.9歩)&lt;€?芋s    鼠狿鍸h?P竐&gt;犚倳攧侽!Θ.俰F?!Z €u€币雽?j.頎??雙iZG%?  ?堈..Ie湊趻?.r.-l?.n(#洓’b.,$? y@)虏J堧蟁訷YF攬偧I蘵嶮診    繡`|摥薫?. PG?<br>
<span style="color:#ff0000">(此处删去N多字节….)</span></p>
<p>——WebKitFormBoundaryoF81IsooBo64lBY2–</p></td>
</tr>
</tbody>
</table>
<p>HTTP消息头中指明了content-type为multipart/form-data并给出了boundary。boundary的主要作用 是进行分割HTTP消息体中的内容，一般的格式都是前面带几个“-”号，然后跟一堆随机字符。整个HTTP消息体以boundary+两个”-”做结尾。</p>
<p>至于HTTP响应就和GET方法是一样的，不赘述了。关于POST相关的代码可以参考<a href="http://code.google.com/p/amaoproject/source/browse/trunk/httpclient/httpclient/http_client.cpp">这里</a>。</p>
<p> </p>
<p>原文：</p>
<p><a href="http://www.xiangwangfeng.com/2011/05/06/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E4%B9%8B%E4%B8%80%EF%BC%9A%E6%94%AF%E6%8C%81%E4%BB%A3%E7%90%86%E7%9A%84socket%E5%B0%81%E8%A3%85/comment-page-1/#comment-147">http://www.xiangwangfeng.com/2011/05/06/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E4%B9%8B%E4%B8%80%EF%BC%9A%E6%94%AF%E6%8C%81%E4%BB%A3%E7%90%86%E7%9A%84socket%E5%B0%81%E8%A3%85/</a></p>
<p><a href="http://www.xiangwangfeng.com/2011/05/09/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E4%B9%8B%E4%BA%8C%EF%BC%9Aget%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0/comment-page-1/#comment-149">http://www.xiangwangfeng.com/2011/05/09/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E4%B9%8B%E4%BA%8C%EF%BC%9Aget%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0/</a></p>
<p><a href="http://www.xiangwangfeng.com/2011/05/15/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E7%BB%88%E7%AF%87%EF%BC%9Apost%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0/">http://www.xiangwangfeng.com/2011/05/15/%E6%9E%84%E9%80%A0httpclient%E4%B8%89%E9%83%A8%E6%9B%B2%E7%BB%88%E7%AF%87%EF%BC%9Apost%E6%96%B9%E6%B3%95%E5%AE%9E%E7%8E%B0/</a></p>