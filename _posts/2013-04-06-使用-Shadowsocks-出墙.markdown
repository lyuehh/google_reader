---
layout: post
title:  "使用 Shadowsocks 出墙"
date:   2013-04-06 00:58:55
author: sofish
categories: program
---

## 使用 Shadowsocks 出墙
### by sofish
### at 2013-04-06 00:58:55
### original <http://sofish.de/2170>

<p>Firefox 似乎不知道从那个版本升级后， SSH + AutoProxy 工作起来就不是那么顺畅。Twitter 有时候连得上，有时候又不行，Facebook 就直接断开了。晚上又运作不正常，局域网太受不了，所以… 细节就不说了，弃 SSH + AutoProxy 的模式，试用了 Shadowsocks。</p>
<p>有 Python 版本，不过由于自己比较习惯 Node，所以用的是 <a href="https://github.com/clowwindy/shadowsocks-nodejs">Shadowsocks for Node</a>。要它工作非常方便，在远程服务器和本地各 <code>git-clone</code> 下来一份，然后做下面两步：</p>
<h3>一、在远程服务器上</h3>
<h4>1. 修改 <code>config.json</code>:</h4>
<pre>{
    &quot;server&quot;: &quot;127.0.0.1&quot;,      // 推荐改成 0.0.0.0
    &quot;server_port&quot;: 8388,
    &quot;local_port&quot;: 1080,
    &quot;password&quot;: &quot;barfoo!&quot;,      // 修改成自己的吧
    &quot;timeout&quot;: 600,
    &quot;method&quot;: null              // 可以使用 &quot;rc4&quot;             
}</pre>
<p><span></span></p>
<p>推荐把 server IP 改成 <code>0.0.0.0</code>，因为刚才遇到了 <code>connect ECONNREFUSED</code> 错误，推荐看下 Stackoverflow 的这个<a href="http://stackoverflow.com/questions/7949277/getting-econnrefused-error-when-socket-connection-is-established-on-different">答案</a>：</p>
<blockquote>
<p>Set the server to bind to 0.0.0.0 and set the client to connect to the correct IP address of the server. If the server is listening on 127.0.0.1, it will only accept connections from its local host.</p>
</blockquote>
<h4>2. 后台运行 <code>server.js</code></h4>
<pre>$ nohup node server.js &gt; log &amp;</pre>
<h3>二、在本地</h3>
<p>修改一下 <code>config.json</code>，保持和服务器一样，唯一不一样的是，把 server IP 变成你远程服务器的 IP。</p>
<pre>$ nohup node local.js &gt; log &amp;</pre>
<p>然后把电脑的代理设置成：</p>
<ul>
<li>协议：socks5</li>
<li>地址：127.0.0.1</li>
<li>端口：1080</li>
</ul>
<p>然后，开始玩吧。速度还挺不错的。</p>
<p>BTW. 如果用用 Firefox 的话，推荐用 FoxyProxy 代替 AutoProxy。配置先「使用系统代理」就可以了。</p>