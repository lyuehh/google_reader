---
layout: post
title:  "websocket 连接 C Server的尝试"
date:   2012-08-03 16:52:19
author: 吴帅
categories: program
---

## websocket 连接 C Server的尝试
### by 吴帅
### at 2012-08-03 16:52:19
### original <http://www.udpwork.com/item/7867.html>

<p><strong>websocket:</strong>
</p>
<p>WebSocket 规范的目标是在浏览器中实现和服务器端双向通信.双向通信可以拓展浏览器上的应用类型.</p>
<p>它是基于 TCP链接的 全双工通讯，但与普通的TCP又不同，它提供的是消息流，而不是字节流。</p>
<p><strong>基于 HTTP 长连接的“服务器推”技术</strong>

<br>
这项技术是在Ajax之后 备受追捧的一项技术，
<br>
推送技术Server Push的基础思想是将浏览器主动查询信息改为服务器主动发送信息。服务器发送一批数据，浏览器显示这些数据，同时保证与服务器的连接。当服务器需要再次发送一批数据时，浏览器显示数据并保持连接。以后，服务器仍然可以发送批量数据，浏览器继续显示数据，依次类推。
<br>
客户端拉曳(Client Pull)
<br>
在客户端拖曳技术中，服务器发送一批数据，在HTTP响应或文档头标记中插入指令，让浏览器“在5秒内再次装入这些数据”或“10秒内前往某URL装入数据”。当指定的时间达到时，客户端就按照服务器的指示去做，或者刷新当前数据，或者调入新的数据。</p>
<p>在客户端，HTTP会保持一个连接到Server，
<br>
而普通的HTTP，则没有这个链接，它通过 客户端主动像Server发起链接请求，主动获取数据，但这样就会有一个致命的缺点：“会产生很多无用的HTTP请求。”
<br>
比如 Discuz论坛的短消息：
<br>
它通过JS 设置一个时间循环事件，一定时间之后会激活事件向服务器发起一个HTTP请求，取到数据通过js更改当前页面的数据。</p>
<p>如下图</p>
<p><a href="http://imsiren.com/wp-content/uploads/2012/07/1.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/1.png"></a>
<br>
如果在没有新数据的情况下，对于服务器来讲，这个HTTP请求就是无意义的连接，在并发量很大的站点中，会给服务器造成很大的压力。
<br>
于是…服务器推送 出现了。
<br>
它有个别名<strong>Comet</strong>
!
<br>
在client会保持一个HTTP连接到server，这个连接成功之后，server会不断的循环检测数据是否存在，如果存在，那么就像客户端发送这些数据。
<br>
它的优点显而易见。
<br>
现在已经有很多不错的技术在生产环境中使用了：
<br>
<strong>1、 通过 Flash作为中间节点，客户端与server的请求通过Flash来转发。</strong>

<br>
此方法的缺点
<br>
客户端必须安装 Flash 播放器；
<br>
因为 XMLSocket 没有 HTTP 隧道功能，XMLSocket 类不能自动穿过防火墙；
<br>
因为是使用套接口，需要设置一个通信端口，防火墙、代理服务器也可能对非 HTTP 通道端口进行限制；
<br>
<strong>2、Java Applet 套接口</strong>

<br>
在客户端使用 Java Applet，通过 java.net.Socket 或 java.net.DatagramSocket 或 java.net.MulticastSocket 建立与服务器端的套接口连接，从而实现“服务器推”。
<br>
这种方案最大的不足在于 Java applet 在收到服务器端返回的信息后，无法通过 JavaScript 去更新 HTML 页面的内容。
<br>
<strong>3、基于Ajax的轮询</strong>

<br>
AJAX 的出现使得 JavaScript 可以调用 XMLHttpRequest 对象发出 HTTP 请求，JavaScript 响应处理函数根据服务器返回的信息对 HTML 页面的显示进行更新。使用 AJAX 实现“服务器推”与传统的 AJAX 应用不同之处在于：
<br>
服务器端会阻塞请求直到有数据传递或超时才返回。
<br>
客户端 JavaScript 响应处理函数会在处理完服务器返回的信息后，再次发出请求，重新建立连接。
<br>
当客户端处理接收的数据、重新建立连接时，服务器端可能有新的数据到达；这些信息会被服务器端保存直到客户端重新建立连接，客户端会一次把当前服务器端所有的信息取回。
<br>
<strong>4、基于 Iframe 及 htmlfile 的流（streaming）方式</strong>

<br>
iframe 是很早就存在的一种 HTML 标记， 通过在 HTML 页面里嵌入一个隐蔵帧，然后将这个隐蔵帧的 SRC 属性设为对一个长连接的请求，服务器端就能源源不断地往客户端输入数据。</p>
<p>了解了这些，就可以再来谈谈WebSocket了</p>
<p><strong>浏览器支持</strong>
</p>
<ul><li>Chrome                   Supported in version 4+</li>
<li>Firefox                    Supported in version 4+</li>
<li>Internet Explorer      Supported in version 10+</li>
<li>Opera                     Supported in version 10+</li>
<li>Safari                     Supported in version 5+</li>
</ul>
<p><strong>WebSocket 概述</strong>
</p>
<p>WebSocket 协议本质上是一个基于 TCP 的协议。为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息”Upgrade: WebSocket”表明这是一个申请协议升级的 HTTP 请求，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。</p>
<p><a href="http://imsiren.com/wp-content/uploads/2012/07/22.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/22.png"></a></p>
<p>To establish a WebSocket connection, the client sends a WebSocket handshake request, and the server sends a WebSocket handshake response。
<br>
建立一个websocket连接，客户端发送握手请求，服务器返回握手响应，
<br>
客户端发送的数据如下：</p>
<pre>
GET /mychat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat
Sec-WebSocket-Version: 13
Origin: http://example.com
</pre><p>服务器返回的数据如下：</p>
<pre>
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
</pre><p>The handshake resembles HTTP, but actually isn’t. It allows the server to interpret part of the handshake request as HTTP, then switch to WebSocket.
<br>
Note that each line ends with an EOL (end of line) sequence, \n or \r\n. There must be a blank line at the end.
<br>
这个握手很像HTTP，但是实际上却不是，它允许服务器以HTTP的方式解释一部分handshake的请求，然后切换为websocket
<br>
注意，每一行结尾都要以EOL 空白符结束，\n 或者\r\n。
<br>
这里是非常重要的地方！网上资料很少，我就是在这里遇到了问题，开始建立连接之后，永远都不返回句柄，我以为只需要客户端像server发送数据，而server不用返回确认数据，看到这里我才知道我错了！</p>
<div>The client sends a Sec-WebSocket-Key which is base64 encoded. To form a response, the magic string 258EAFA5-E914-47DA-95CA-C5AB0DC85B11 is appended to this (undecoded) key. The resulting string is then hashed with SHA-1, then base64 encoded. Finally, the resulting reply occurs in the header Sec-WebSocket-Accept.
<br>
客户端发送 base64加密的 Sec-WebSocket-Key，服务器接收到这个key之后，将258EAFA5-E914-47DA-95CA-C5AB0DC85B11拼在key之后，生成HSmrc0sMlYUkAGmm5OPpG2HaGWk=258EAFA5-E914-47DA-95CA-C5AB0DC85B11，然后将得到的结果进行sha-1哈希，最后base64加密，作为Sec-WebSocket-Accept的值 发送给客户端。
<br>
如果accept这个值错误的话会抛出 Error during WebSocket handshake: Sec-WebSocket-Accept mismatch
<br>
最后关闭连接。。一定要注意哦。
<br>
</div>
<p>这样，握手才算完成！连接确认之后，客户端及服务器端就可以相互发数据了。注意哦，它是全双工的哦，意思就是不会阻塞。</p>
<p>如图</p>
<p><a href="http://imsiren.com/wp-content/uploads/2012/07/201112242121408903.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/201112242121408903.png"></a></p>
<p>概念性的问题都清楚了，来看看WEBSOCKET为我们提供了哪些API</p>
<p>1、构造函数   WebSocket(char *host);</p>
<p>创建一个websocket对象，接受一个参数以ws://靠头，就像发起一个HTTP请求一样用http://开头</p>
<p>var ws=new WebSocket(“ws://10.32.21.27:9880”);</p>
<p>2、ws.onopen</p>
<p>连接成功后触发</p>
<p>3、ws.onerror</p>
<p>连接出错触发</p>
<p>4、ws.onmessage</p>
<p>server端有数据返回时触发</p>
<p>5、ws.onclose</p>
<p>关闭连接时触发</p>
<p>还有一个最重要的</p>
<p>6、ws.send</p>
<p>用来发送消息到server</p>
<p>7、ws.close</p>
<p>关闭连接</p>
<p><a href="http://imsiren.com/wp-content/uploads/2012/07/22222222.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/22222222.png"></a></p>
<p>完整的client端的代码如下</p>
<pre>
&lt;!DOCTYPE HTML&gt;
&lt;html&gt;
&lt;head&gt;
	&lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html&quot; /&gt;
	&lt;meta name=&quot;author&quot; content=&quot;blog.anchen8.net&quot; /&gt;

	&lt;title&gt;Untitled 2&lt;/title&gt;
    &lt;script&gt;
var socket;
function connect(){
    try{
        socket=new WebSocket(&#39;ws://10.32.21.27:9880&#39;);
    }catch(e){
        alert(&#39;error&#39;);
        return;
    }
    socket.onopen = sOpen;
    socket.onerror=sError;
    socket.onmessage=sMessage;
    socket.onclose=sClose

}
function sOpen(){
    alert(&#39;connect success!&#39;)
}
function sError(){
    alert(&#39;connect error&#39;)
}
function sMessage(msg){
    alert(&#39;server says:&#39;+msg)
}

function sClose(){
    alert(&#39;connect close&#39;)
}
function send(){
    socket.send(&#39;hello ,i am siren!&#39;)
}
function close(){
    socket.close();
}
&lt;/script&gt;
&lt;/head&gt;

&lt;body&gt;
&lt;input id=&quot;msg&quot; type=&quot;text&quot;&gt;
&lt;button id=&quot;connect&quot; onclick=&quot;connect();&quot;&gt;Connect&lt;/button&gt;
&lt;button id=&quot;send&quot; onclick=&quot;send();&quot;&gt;Send&lt;/button&gt;
&lt;button id=&quot;close&quot; onclick=&quot;close();&quot;&gt;Close&lt;/button&gt;

&lt;/body&gt;

&lt;/html&gt;
</pre><p><a href="http://imsiren.com/wp-content/uploads/2012/07/223.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/223.png"></a></p>
<p>server端代码如下</p>
<pre>
/*
 * =====================================================================================
 *
 *       Filename:  socket.c
 *
 *    Description:
 *
 *        Version:  1.0
 *        Created:  07/09/2012 07:00:25 PM
 *       Revision:  none
 *       Compiler:  gcc
 *
 *         Author:  斯人,imsiren.com
 *   Organization:
 *
 * =====================================================================================
 */
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;arpa/inet.h&gt;
#include &lt;netinet/in.h&gt;
#include &lt;sys/types.h&gt;
#include &lt;sys/socket.h&gt;
#include &lt;string.h&gt;
#include &lt;openssl/sha.h&gt;
#define PORT 9880
#define MAXLENGTH 1024+1
void parsestr(char *request,char *data){
        int needle;
        strcat(request,&quot;HTTP/1.1 101 WebSocket Protocol Handshake\r\n&quot;);
        strcat(request,&quot;Upgrade:WebSocket\r\n&quot;);
        strcat(request,&quot;Connection:Upgrade\r\n&quot;);
//这个值等于 base64_encode(sha1(key+258EAFA5-E914-47DA-95CA-C5AB0DC85B11)) 由于我这里没有合适的base64算法和sha1算法，所以就不写了。
        strcat(request,&quot;Sec-WebSocket-Accept:ZmQ5OWUxMjgwMTViNTEyM2FmZTRlOGViODZkNTk3OTBjMWRiYjBiYg==\r\n&quot;);
}
int main(int argc,char ** argv){
        int sockfd,len,maxfd,ret,retval,newfd;
        int reuse=1;
        fd_set rwfd;
        struct sockaddr_in l_addr;
        struct sockaddr_in c_addr;
        struct timeval tv;
        char buf[MAXLENGTH];
        char request[MAXLENGTH];
  sockfd=socket(AF_INET,SOCK_STREAM,0);
        setsockopt(sockfd,SOL_SOCKET,SO_REUSEADDR,&amp;reuse,sizeof(int));
        if(sockfd&lt;0)
                perror(&quot;socket&quot;);
        l_addr.sin_port=htons(PORT);
        l_addr.sin_family=AF_INET;
        l_addr.sin_addr.s_addr=INADDR_ANY;
        if(bind(sockfd,(struct sockaddr*)&amp;l_addr,sizeof(struct sockaddr))&lt;0)
                perror(&quot;bind&quot;);
        listen(sockfd,5);
        len=sizeof(struct sockaddr);
        tv.tv_sec=5;
        tv.tv_usec=0;
        bzero(request,MAXLENGTH);
        while(1){
                newfd=accept(sockfd,(struct sockaddr*)&amp;c_addr,&amp;len);
                if(newfd==-1){
                        perror(&quot;accept&quot;);
                        exit(1);
                }else{
                        printf(&quot;%s is comming\n&quot;,inet_ntoa(c_addr.sin_addr));
                }
                while(1){
                        FD_ZERO(&amp;rwfd);
                        FD_SET(0,&amp;rwfd);
                        FD_SET(newfd,&amp;rwfd);
                        maxfd=0;
                        if(newfd&gt;maxfd)
                                maxfd=newfd;
                        retval=select(maxfd+1,&amp;rwfd,NULL,NULL,&amp;tv);
                        if(retval==-1){
                                perror(&quot;select&quot;);
                                break;
                        /*  }else if(retval==0){
                                printf(&quot;no data\n&quot;);
                                continue;
*/
                        }else{
                                if(FD_ISSET(0,&amp;rwfd)){
                                        bzero(buf,MAXLENGTH);
                                        fgets(buf,MAXLENGTH,stdin);
                                        parsestr(request,buf);
                                        len=send(newfd,request,MAXLENGTH,0);
                                        if(len&gt;0)
 printf(&quot;i sayed:%s\n&quot;,buf);
                                }
                                if(FD_ISSET(newfd,&amp;rwfd)){
                                        len=recv(newfd,buf,MAXLENGTH,0) ;
                                         if(len&gt;0){
                                                if(strncmp(buf,&quot;quit&quot;,4)==0){
                                                        close(newfd);
                                                }
                                        }else{
                                                printf(&quot;client says:%s\n&quot;,buf);
                                        }
                                }
                        }
                }
        }
        return 0;
</pre><p>server用select 做 非阻塞处理。。
<br>
没什么特别的，只是一个简单的收发server，client客户端连接之后，
<br>
需要在server输入字符确认连接，握手才能完成哦。
<br>
开启监听，客户端连接后 如下图
<br>
<a href="http://imsiren.com/wp-content/uploads/2012/07/socket.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/socket.png"></a>
<br>
我们可以看到 发过来的头内容。</p>
<p>最重要的就是 Sec-WebSocket-Key: m1ZF/Zd5hOP7Jh5d8mnDoA==了。</p>
<p><a href="http://imsiren.com/wp-content/uploads/2012/07/44.png"><img src="http://imsiren.com/wp-content/uploads/2012/07/44.png"></a>
<br>
这个是我们发回给client的头信息。。</p>
<p>这样客户端的onopen就会被触发。。。
<br>
一次握手之后就完成了。。。
<br>
虽然websocket非常好用，但是目前来看 很难应用到生产环境中，因为websocket是html5之后才有的，对浏览器的要求较高，上面虽然提到了浏览器支持的版本，但是都还不稳定，websocket目前只属于草案阶段，IE6/7/8甚至9都不支持。
<br>
参考文献：</p>
<p>http://www.websocket.org/</p>
<p>http://en.wikipedia.org/wiki/WebSocket</p>
<p>http://www.ibm.com/developerworks/cn/web/1112_huangxa_websocket/</p>

			<div style="margin-top:8px;padding:6px 0;border-top:1px solid #3cf">
				<div style="text-align:center;margin:16px 0;padding:6px;border:0px dashed #999;font-family:arial;font-size:26px;font-weight:bold">
	<a href="http://www.udpwork.com/item/7867.html#review_form" title="不喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_down24.gif" alt="">
		<span style="color:#f33">0</span>
	</a>
	   
	<a href="http://www.udpwork.com/item/7867.html#review_form" title="喜欢" style="text-decoration:none">
		<img src="http://www.udpwork.com//images/thumb_up24.gif" alt="">
		<span style="color:#3c3">0</span>
	</a>
</div>				<p>
					由 <a href="http://www.udpwork.com/">IT 牛人博客聚合网站(udpwork.com)</a> 聚合
					|
					<a href="http://www.udpwork.com/item/7867.html#reviews">评论: 0</a>
					|
					<a href="http://book.benegg.com/tag/%E7%BC%96%E7%A8%8B?from=udpwork-feed">10000+ 本编程/Linux PDF/CHM 电子书下载</a>
				</p>
			</div>