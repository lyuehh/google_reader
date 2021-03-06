---
layout: post
title:  "XMLHttpRequest Level 2 使用指南"
date:   2012-09-08 17:12:09
author: 
categories: program
---

## XMLHttpRequest Level 2 使用指南
### by 
### at 2012-09-08 17:12:09
### original <http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html>

<p><a href="http://en.wikipedia.org/wiki/XMLHttpRequest">XMLHttpRequest</a>是一个浏览器接口，使得Javascript可以进行HTTP(S)通信。</p><p>最早，微软在IE 5引进了这个接口。因为它太有用，其他浏览器也模仿部署了，ajax操作因此得以诞生。</p>

<p>但是，这个接口一直没有标准化，每家浏览器的实现或多或少有点不同。HTML 5的概念形成后，W3C开始考虑标准化这个接口。2008年2月，就提出了<a href="http://dev.w3.org/2006/webapi/XMLHttpRequest-2/">XMLHttpRequest Level 2</a> 草案。</p>

<p>这个XMLHttpRequest的新版本，提出了很多有用的新功能，将大大推动互联网革新。本文就对这个新版本进行详细介绍。</p>

<p><img src="http://image.beekka.com/blog/201209/bg2012090801.png"></p>

<p><strong>一、老版本的XMLHttpRequest对象</strong></p>

<p>在介绍新版本之前，我们先回顾一下老版本的用法。</p>

<p>首先，新建一个XMLHttpRequest的实例。</p>

<blockquote>

<p>　　var xhr = new XMLHttpRequest();</p>

</blockquote>

<p>然后，向远程主机发出一个HTTP请求。</p>

<blockquote>

<p>　　xhr.open('GET', 'example.php');</p>

<p>　　xhr.send();</p>

</blockquote>

<p>接着，就等待远程主机做出回应。这时需要监控XMLHttpRequest对象的状态变化，指定回调函数。</p>

<blockquote>

<p>　　xhr.onreadystatechange = function(){ </p>

<p>　　　　if ( xhr.readyState == 4 &amp;&amp; xhr.status == 200 ) {</p>

<p>　　　　　　alert( xhr.responseText );</p>

<p>　　　　} else {</p>

<p>　　　　　　alert( xhr.statusText );</p>

<p>　　　　}</p>

<p>　　}; </p>

</blockquote>

<p>上面的代码包含了老版本XMLHttpRequest对象的主要属性：</p>

<blockquote>

<p>　　* xhr.readyState：XMLHttpRequest对象的状态，等于4表示数据已经接收完毕。</p>

<p>　　* xhr.status：服务器返回的状态码，等于200表示一切正常。</p>

<p>　　* xhr.responseText：服务器返回的文本数据</p>

<p>　　* xhr.responseXML：服务器返回的XML格式的数据</p>

<p>　　* xhr.statusText：服务器返回的状态文本。</p>

</blockquote>

<p><strong>二、老版本的缺点</strong></p>

<p>老版本的XMLHttpRequest对象有以下几个缺点：</p>

<blockquote>

<p>　　* 只支持文本数据的传送，无法用来读取和上传二进制文件。</p>

<p>　　* 传送和接收数据时，没有进度信息，只能提示有没有完成。</p>

<p>　　* 受到<a href="http://www.w3.org/Security/wiki/Same_Origin_Policy">"同域限制"</a>（Same Origin Policy），只能向同一域名的服务器请求数据。</p>

</blockquote>

<p><strong>三、新版本的功能</strong></p>

<p>新版本的XMLHttpRequest对象，针对老版本的缺点，做出了大幅改进。</p>

<blockquote>

<p>　　* 可以设置HTTP请求的时限。</p>

<p>　　* 可以使用FormData对象管理表单数据。</p>

<p>　　* 可以上传文件。</p>

<p>　　* 可以请求不同域名下的数据（跨域请求）。</p>

<p>　　* 可以获取服务器端的二进制数据。</p>

<p>　　* 可以获得数据传输的进度信息。</p>

</blockquote>

<p>下面，我就一一介绍这些新功能。</p>

<p><strong>四、HTTP请求的时限</strong></p>

<p>有时，ajax操作很耗时，而且无法预知要花多少时间。如果网速很慢，用户可能要等很久。</p>

<p>新版本的XMLHttpRequest对象，增加了timeout属性，可以设置HTTP请求的时限。</p>

<blockquote>

<p>　　xhr.timeout = 3000;</p>

</blockquote>

<p>上面的语句，将最长等待时间设为3000毫秒。过了这个时限，就自动停止HTTP请求。与之配套的还有一个timeout事件，用来指定回调函数。</p>

<blockquote>

<p>　　xhr.ontimeout = function(event){</p>

<p>　　　　alert('请求超时！');</p>

<p>　　}</p>

</blockquote>

<p>目前，Opera、Firefox和IE 10支持该属性，IE 8和IE 9的这个属性属于XDomainRequest对象，而Chrome和Safari还不支持。</p>

<p><strong>五、FormData对象</strong></p>

<p>ajax操作往往用来传递表单数据。为了方便表单处理，HTML 5新增了一个FormData对象，可以模拟表单。</p>

<p>首先，新建一个FormData对象。</p>

<blockquote>

<p>　　var formData = new FormData();</p>

</blockquote>

<p>然后，为它添加表单项。</p>

<blockquote>

<p>　　formData.append('username', '张三');</p>

<p>　　formData.append('id', 123456);</p>

</blockquote>

<p>最后，直接传送这个FormData对象。这与提交网页表单的效果，完全一样。</p>

<blockquote>

<p>　　xhr.send(formData);</p>

</blockquote>

<p>FormData对象也可以用来获取网页表单的值。</p>

<blockquote>

<p>　　var form = document.getElementById('myform');</p>

<p>　　var formData = new FormData(form);</p>

<p>　　formData.append('secret', '123456'); // 添加一个表单项</p>

<p>　　xhr.open('POST', form.action);</p>

<p>　　xhr.send(formData);</p>

</blockquote>

<p><strong>六、上传文件</strong></p>

<p>新版XMLHttpRequest对象，不仅可以发送文本信息，还可以上传文件。</p>

<p>假定files是一个"选择文件"的表单元素（input[type="file"]），我们将它装入FormData对象。</p>

<blockquote>

<p>　　var formData = new FormData();</p>

<p>　　for (var i = 0; i &lt; files.length;i++) {</p>

<p>　　　　formData.append('files[]', files[i]);</p>

<p>　　}</p>

</blockquote>

<p>然后，发送这个FormData对象。</p>

<blockquote>

<p>　　xhr.send(formData); </p>

</blockquote>

<p><strong>七、跨域资源共享（CORS）</strong></p>

<p>新版本的XMLHttpRequest对象，可以向不同域名的服务器发出HTTP请求。这叫做<a href="http://en.wikipedia.org/wiki/Cross-Origin_Resource_Sharing">"跨域资源共享"</a>（Cross-origin resource sharing，简称CORS）。 </p>

<p>使用"跨域资源共享"的前提，是浏览器必须支持这个功能，而且服务器端必须同意这种"跨域"。如果能够满足上面的条件，则代码的写法与不跨域的请求完全一样。</p>

<blockquote>

<p>　　xhr.open('GET', 'http://other.server/and/path/to/script');</p>

</blockquote>

<p>目前，除了IE 8和IE 9，主流浏览器都支持CORS，IE 10也将支持这个功能。服务器端的设置，请参考<a href="https://developer.mozilla.org/en-US/docs/Server-Side_Access_Control">《Server-Side Access Control》</a>。</p>

<p><strong>八、接收二进制数据（方法A：改写MIMEType）</strong></p>

<p>老版本的XMLHttpRequest对象，只能从服务器取回文本数据（否则它的名字就不用XML起首了），新版则可以取回二进制数据。</p>

<p>这里又分成两种做法。较老的做法是改写数据的MIMEType，将服务器返回的二进制数据伪装成文本数据，并且告诉浏览器这是用户自定义的字符集。</p>

<blockquote>

<p>　　xhr.overrideMimeType("text/plain; charset=x-user-defined");</p>

</blockquote>

<p>然后，用responseText属性接收服务器返回的二进制数据。</p>

<blockquote>

<p>　　var binStr = xhr.responseText;</p>

</blockquote>

<p>由于这时，浏览器把它当做文本数据，所以还必须再一个个字节地还原成二进制数据。</p>

<blockquote>

<p>　　for (var i = 0, len = binStr.length; i &lt; len; ++i) {</p>

<p>　　　　var c = binStr.charCodeAt(i);</p>

<p>　　　　var byte = c &amp; 0xff; </p>

<p>　　}</p>

</blockquote>

<p>最后一行的位运算&quot;c &amp; 0xff&quot;，表示在每个字符的两个字节之中，只保留后一个字节，将前一个字节扔掉。原因是浏览器解读字符的时候，会把字符自动<a href="http://web.archive.org/web/20080821092906/http://mgran.blogspot.com/2006/08/downloading-binary-streams-with.html">解读</a>成Unicode的0xF700-0xF7ff区段。</p>

<p><strong>八、接收二进制数据（方法B：responseType属性）</strong></p>

<p>从服务器取回二进制数据，较新的方法是使用新增的responseType属性。如果服务器返回文本数据，这个属性的值是"TEXT"，这是默认值。较新的浏览器还支持其他值，也就是说，可以接收其他格式的数据。</p>

<p>你可以把responseType设为blob，表示服务器传回的是二进制对象。</p>

<blockquote>

<p>　　var xhr = new XMLHttpRequest();</p>

<p>　　xhr.open('GET', '/path/to/image.png');</p>

<p>　　xhr.responseType = 'blob';</p>

</blockquote>

<p>接收数据的时候，用浏览器自带的Blob对象即可。</p>

<blockquote>

<p>　　var blob = new Blob([xhr.response], {type: 'image/png'});</p>

</blockquote>

<p>注意，是读取xhr.response，而不是xhr.responseText。</p>

<p>你还可以将responseType设为arraybuffer，把二进制数据装在一个数组里。</p>

<blockquote>

<p>　　var xhr = new XMLHttpRequest();</p>

<p>　　xhr.open('GET', '/path/to/image.png');</p>

<p>　　xhr.responseType = "arraybuffer";</p>

</blockquote>

<p>接收数据的时候，需要遍历这个数组。</p>

<blockquote>

<p>　　var arrayBuffer = xhr.response;</p>

<p>　　if (arrayBuffer) {</p>

<p>　　　　var byteArray = new Uint8Array(arrayBuffer);</p>

<p>　　　　for (var i = 0; i &lt; byteArray.byteLength; i++) {</p>

<p>　　　　　　// do something</p>

<p>　　　　}<br>
　　}</p>

</blockquote>

<p>更详细的讨论，请看<a href="https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest/Sending_and_Receiving_Binary_Data">Sending and Receiving Binary Data</a>。</p>

<p><strong>九、进度信息</strong></p>

<p>新版本的XMLHttpRequest对象，传送数据的时候，有一个progress事件，用来返回进度信息。</p>

<p>它分成上传和下载两种情况。下载的progress事件属于XMLHttpRequest对象，上传的progress事件属于XMLHttpRequest.upload对象。</p>

<p>我们先定义progress事件的回调函数。</p>

<blockquote>

<p>　　xhr.onprogress = updateProgress;</p>

<p>　　xhr.upload.onprogress = updateProgress;</p>

</blockquote>

<p>然后，在回调函数里面，使用这个事件的一些属性。</p>

<blockquote>

<p>　　function updateProgress(event) {</p>

<p>　　　　if (event.lengthComputable) {</p>

<p>　　　　　　var percentComplete = event.loaded / event.total;</p>

<p>　　　　} </p>

<p>　　}</p>

</blockquote>

<p>上面的代码中，event.total是需要传输的总字节，event.loaded是已经传输的字节。如果event.lengthComputable不为真，则event.total等于0。</p>

<p>与progress事件相关的，还有其他五个事件，可以分别指定回调函数：</p>

<blockquote>

<p>　　* load事件：传输成功完成。</p>

<p>　　* abort事件：传输被用户取消。</p>

<p>　　* error事件：传输中出现错误。</p>

<p>　　* loadstart事件：传输开始。</p>

<p>　　* loadEnd事件：传输结束，但是不知道成功还是失败。</p>

</blockquote>

<p><strong>十、阅读材料</strong></p>

<p>　　1. <a href="http://dev.opera.com/articles/view/xhr2/">Introduction to XMLHttpRequest Level 2</a>： 新功能的综合介绍。</p>

<p>　　2. <a href="http://www.html5rocks.com/en/tutorials/file/xhr2/">New Tricks in XMLHttpRequest 2</a>：一些用法的介绍。</p>

<p>　　3. <a href="https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest/Using_XMLHttpRequest">Using XMLHttpRequest</a>：一些高级用法，主要针对Firefox浏览器。</p>

<p>　　4. <a href="https://developer.mozilla.org/en-US/docs/HTTP_access_control">HTTP Access Control</a>：CORS综述。</p>

<p>　　5. <a href="http://dev.opera.com/articles/view/dom-access-control-using-cross-origin-resource-sharing/">DOM access control using cross-origin resource sharing</a>：CORS的9种HTTP头信息</p>

<p>　　6. <a href="https://developer.mozilla.org/en-US/docs/Server-Side_Access_Control">Server-Side Access Control</a>：服务器端CORS设置。</p>

<p>　　7. <a href="http://enable-cors.org/">Enable CORS</a>：服务端CORS设置。</p>

<p>（完）</p><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;border:1px solid #d3d3d3;margin:1em;background-color:#aad2f0;border-radius:10px"><h3>文档信息</h3>
<ul>
<li>版权声明：自由转载-非商用-非衍生-保持署名 | <a href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh">Creative Commons BY-NC-ND 3.0</a></li>
<li>原文网址：<a href="http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html">http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html</a></li>
<li>最后修改时间：2012年10月 2日 15:54</li>
<li>付费支持：<a href="https://me.alipay.com/ruanyf"><img src="http://www.ruanyifeng.com/blog/images/rmb_32.png" title="人民币" alt="人民币 - 支付宝" style="border:none;vertical-align:middle"></a> | <a href="https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&amp;business=yifeng.ruan@gmail.com&amp;currency_code=USD&amp;amount=0.99&amp;return=http://www.ruanyifeng.com/thank.html&amp;item_name=Ruan%20YiFeng&#39;s%20Blog&amp;undefined_quantity=1&amp;no_note=0"><img src="http://www.ruanyifeng.com/blog/images/dollar_32.png" alt="美元 - paypal" title="美元" style="border:none;vertical-align:middle"></a> </li>
</ul></div><div style="color:#556677;line-height:160%;padding:0.3em 0.5em;margin:1em;border-radius:10px"></div>