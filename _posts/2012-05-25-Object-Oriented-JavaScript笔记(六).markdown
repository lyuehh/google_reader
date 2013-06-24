---
layout: post
title:  "Object-Oriented JavaScript笔记(六)"
date:   2012-05-25 13:08:30
author: 枯藤昏鸦
categories: program
---

## Object-Oriented JavaScript笔记(六)
### by 枯藤昏鸦
### at 2012-05-25 13:08:30
### original <http://ioio.name/object-oriented-javascript-note-6.html>

<p>Object-Oriented JavaScript 笔记<br>
<a href="http://ioio.name/object-oriented-javascript-note.html">Object-Oriented JavaScript笔记(一)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-2.html">Object-Oriented JavaScript笔记(二)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-3.html">Object-Oriented JavaScript笔记(三)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-4.html">Object-Oriented JavaScript笔记(四)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-5.html">Object-Oriented JavaScript笔记(五)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-6.html">Object-Oriented JavaScript笔记(六)</a><br>
<a href="http://ioio.name/object-oriented-javascript-note-7.html">Object-Oriented JavaScript笔记(七)</a></p>
<p><strong>第七章 浏览器环境</strong></p>
<p>文档对象模型(处理当前页面内容的)与浏览器对象模型(处理当前页面之外的)<br>
参考资料：</p>
<p>	* <a href="http://developer.mozilla.org/en/docs/Gecko_DOM_Reference">The Mozilla DOM reference for Firefox information</a><br>
	* <a href="http://msdn2.microsoft.om/en-us/library/ms533050(vs.85).aspx">Microsoft’s documentation for Internet Explorer</a><br>
	* <a href="http://www.w3.org/DOM/DOMTR">W3C’ DOM specs</a></p>
<p>BOM<br>
提供一组对象访问浏览器(window)及计算机屏幕信息(window.screen)</p>
<p><strong>window对象相关属性</strong><br>
window.navigator 包含浏览器及其功能信息<br>
navigator.userAgent 返回浏览器标识信息，开发者常用它区分浏览器以提供不同的实现代码</p>

<div><div><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>navigator.<span style="color:#660066">userAgent</span>.<span style="color:#660066">indexOf</span><span style="color:#009900">(</span><span style="color:#3366cc">'MSIE'</span><span style="color:#009900">)</span> <span style="color:#339933">!==</span> <span style="color:#339933">-</span><span style="color:#cc0000">1</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#006600;font-style:italic">// this is IE</span>
   <span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#009900">{</span>
     <span style="color:#006600;font-style:italic">// not IE</span>
<span style="color:#009900">}</span></pre></div></div>

<p>这并不能完整的细分到每一个浏览器，更好的办法是使用特性嗅探法</p>

<div><div><pre style="font-family:monospace"><span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">typeof</span> window.<span style="color:#660066">addEventListener</span> <span style="color:#339933">===</span> <span style="color:#3366cc">'function'</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#006600;font-style:italic">// feature is supported, let's use it</span>
   <span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#009900">{</span>
     <span style="color:#006600;font-style:italic">// hmm, this feature is not supported, will have to</span>
     <span style="color:#006600;font-style:italic">// think of another way</span>
<span style="color:#009900">}</span></pre></div></div>

<p>window.location 提供与当前页面url有关的信息，同时提供了三个方法reload(), assign() 和 replace().<br>
assign和replace功能很相似，区别是replace不在window.history中产生信息。</p>
<p>window.history允许你访问当前会话窗口的访问历史</p>
<p>window.frames保存一组当前页面的frames信息,无论是否存在frames，window.frames总是指向window对象（这说明它也是一个伪数组）<br>
每个frame中都包含一个独立的window对象，可以通过下面几种形式访问它<br>
&gt;&gt;&gt; window.frames[0]<br>
&gt;&gt;&gt; window.frames[0].window<br>
&gt;&gt;&gt; frames[0].window<br>
&gt;&gt;&gt; frames[0]</p>
<p>父页面可以访问子页面，子页面同样可以访问父页面，每个页面都有一个window.parent指向其父页面(不存在时指向自身)<br>
如果要访问顶层页面，window.top (不存在时指向自身)</p>
<p>如果一个frame有name属性，也可以通过name访问它window.frames['myframe']</p>
<p>window.screen提供屏幕与颜色深度信息</p>
<p>window.open()/window.close()/window.moveTo()/window.resizeTo()/window.alert()/window.prompt()/window.confirm()这些别用了。</p>
<p>注意：alert()并不是ECMAScript中定义的方法。</p>
<p>window.setTimeout(), window.setInterval()延时执行和重复执行<br>
虽然第一个参数可以传字符串setTimeout(‘foo’,2000)但是还是别传字符串，传函数引用更妥当setTimeout(foo,2000)</p>
<p>window.document提供当前页面文档对象模型相关的信息(DOM)</p>
<p>nodeType为1的节点(element nodes)，nodeName和tagName都是节点的标签名称。</p>
<p>innerHTML并不是标准中存在的，但所有的浏览器都支持它。</p>
<p>document.getElementsByTagName(‘*’)<br>
早期的IE浏览器并不支持星号作为tag名称传入，不过可以使用document.all获得所有元素。<br>
IE7开始支持*号作为标签名，但它会返回所有节点，而不仅仅只是元素类节点。</p>
<p><strong>遍历dom</strong></p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> walkDOM<span style="color:#009900">(</span>n<span style="color:#009900">)</span> <span style="color:#009900">{</span>
 <span style="color:#000066;font-weight:bold">do</span> <span style="color:#009900">{</span>
   console.<span style="color:#660066">log</span><span style="color:#009900">(</span>n<span style="color:#009900">)</span><span style="color:#339933">;</span>
   <span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>n.<span style="color:#660066">hasChildNodes</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">)</span> <span style="color:#009900">{</span>
     walkDOM<span style="color:#009900">(</span>n.<span style="color:#660066">firstChild</span><span style="color:#009900">)</span>
   <span style="color:#009900">}</span>
 <span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">while</span> <span style="color:#009900">(</span>n <span style="color:#339933">=</span> n.<span style="color:#660066">nextSibling</span><span style="color:#009900">)</span>
<span style="color:#009900">}</span>
walkDOM<span style="color:#009900">(</span>document.<span style="color:#660066">body</span><span style="color:#009900">)</span></pre></div></div>

<p>CSS中的短横线在JavaScript并不是合法的名字，所以margin-left在在js中是以el.style.marginLeft的形式访问。<br>
可以直接修改节点的属性,如<br>
el.id = ‘sidebar’<br>
由于class在JavaScript中是保留字，修改class属性则使用el.className</p>
<p><strong>HTML 私有对象</strong><br>
document.images 等效于 <code>document.getElementsByTagName('img')</code></p>
<p>document.applets 等效于 <code>document.getElementsByTagName ('applets')</code></p>
<p>document.links 所有的</p>

<div><div><pre style="font-family:monospace"><span style="color:#339933">&lt;</span>a href<span style="color:#339933">=</span><span style="color:#3366cc">&quot;…&quot;</span><span style="color:#339933">&gt;&lt;/</span>a<span style="color:#339933">&gt;</span></pre></div></div>

<p>document.anchors 所有的</p>

<div><div><pre style="font-family:monospace"><span style="color:#339933">&lt;</span>a <span style="color:#000066">name</span><span style="color:#339933">=</span><span style="color:#3366cc">&quot;…&quot;</span><span style="color:#339933">&gt;&lt;/</span>a<span style="color:#339933">&gt;</span></pre></div></div>

<p>document.forms</p>
<p><strong>非核心对象</strong><br>
document.cookie<br>
document.title<br>
document.referrer<br>
document.domain<br>
document.location</p>
<p>注意，改变document.title并不等效于访问<code>document.getElementsByTagName(' title')[0]</code>，它只是更改浏览器窗口上的显示。</p>
<p>document.referrer的值等同于http头里的Referer(由于历史原因，这是个拼写错的词)</p>
<p>document.domain可以从小域改称大域(www.yahoo.com -&gt; yahoo.com)，但不能从大域改称小域(yahoo.com -&gt; www.yahoo.com)，常用于解决跨域问题。</p>
<p>window.location === document.location</p>
<p><strong>事件</strong><br>
在2级DOM事件模型中，事件传播分三个阶段进行，即捕获阶段（capturing）、目标阶段和冒泡阶段（bubbling）。</p>
<p>IE 只支持冒泡<br>
Netscape 只支持捕获</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">var</span> mypara <span style="color:#339933">=</span> document.<span style="color:#660066">getElementById</span><span style="color:#009900">(</span><span style="color:#3366cc">'my-div'</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
mypara.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'click'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span> <span style="color:#009900">{</span><span style="color:#000066">alert</span><span style="color:#009900">(</span><span style="color:#3366cc">'Boo!'</span><span style="color:#009900">)</span><span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span></pre></div></div>

<p>第三个参数决定是否使用捕获，由于兼容性原因，建议传false，在实际的使用中只使用冒泡。</p>
<p>可以使用e.stopPropagation();放弃冒泡。<br>
事件代理(event delegation)的应用</p>
<p>IE中还是可能使用事件捕获的 setCapture() / releaseCapture()，但它只支持鼠标事件，其它事件（如键盘事件）则不支持。</p>

<div><div><pre style="font-family:monospace">window.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'click'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#000066">alert</span> <span style="color:#009900">(</span><span style="color:#3366cc">'clicked window'</span><span style="color:#009900">)</span><span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
window.<span style="color:#660066">attachEvent</span><span style="color:#009900">(</span><span style="color:#3366cc">'click'</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">function</span><span style="color:#009900">(</span><span style="color:#009900">)</span><span style="color:#009900">{</span><span style="color:#000066">alert</span> <span style="color:#009900">(</span><span style="color:#3366cc">'clicked window'</span><span style="color:#009900">)</span><span style="color:#009900">}</span><span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span></pre></div></div>

<p>DOM 规范中并没有说事件绑在window上会怎么样，在IE中(注：低版本)并不会触发，在Firefox中会出发。</p>
<p>移除一个事件，需要传入原事件指针，故绑的匿名函数不能被移除掉。</p>

<div><div><pre style="font-family:monospace">para.<span style="color:#660066">removeEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'click'</span><span style="color:#339933">,</span> paraHandler<span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span></pre></div></div>

<p>终止默认事件<br>
e.preventDefault();</p>
<p>并不是所有事件的默认行为都能被终止，可以检查事件对象的cancellable字段来确认。</p>
<p>IE中事件不同的部分：</p>
<p>1.（低版本的）IE没有addEventListener() 但IE5开始它有个等价的 attachEvent()方法<br>
2.attachEvent()中click要写作onclick<br>
3.使用老式方法（onclick）时，回调函数并不会传入event参数，但会有个全局的window.event供使用(注：现实场景中它需要先cache下来)<br>
4.IE的事件没有target属性用于指示目标函数，但有个等价的srcElement<br>
5.并不是素偶有的事件都有捕获截断，所以只能使用冒泡<br>
6.没有stopPropagation()方法，但是可以设置cancelBubble 属性值为true<br>
7.没有preventDefault()方法，但是可以设置returnValue值为false<br>
8.removeEventListener()的等效方法detachEvent().</p>
<p>兼容性示例：</p>

<div><div><pre style="font-family:monospace"><span style="color:#003366;font-weight:bold">function</span> callback<span style="color:#009900">(</span>evt<span style="color:#009900">)</span> <span style="color:#009900">{</span>
     <span style="color:#006600;font-style:italic">// prep work</span>
     evt <span style="color:#339933">=</span> evt <span style="color:#339933">||</span> window.<span style="color:#660066">event</span><span style="color:#339933">;</span>
     <span style="color:#003366;font-weight:bold">var</span> target <span style="color:#339933">=</span> <span style="color:#009900">(</span><span style="color:#000066;font-weight:bold">typeof</span> evt.<span style="color:#660066">target</span> <span style="color:#339933">!==</span> <span style="color:#3366cc">'undefined'</span><span style="color:#009900">)</span> <span style="color:#339933">?</span> evt.<span style="color:#660066">target</span> <span style="color:#339933">:</span> evt.<span style="color:#660066">srcElement</span><span style="color:#339933">;</span>
     <span style="color:#006600;font-style:italic">// actual callback work</span>
     console.<span style="color:#660066">log</span><span style="color:#009900">(</span>target.<span style="color:#660066">nodeName</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span>
<span style="color:#006600;font-style:italic">// start listening for click events</span>
<span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>document.<span style="color:#660066">addEventListener</span><span style="color:#009900">)</span><span style="color:#009900">{</span> <span style="color:#006600;font-style:italic">// FF</span>
     document.<span style="color:#660066">addEventListener</span><span style="color:#009900">(</span><span style="color:#3366cc">'click'</span><span style="color:#339933">,</span> callback<span style="color:#339933">,</span> <span style="color:#003366;font-weight:bold">false</span><span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#000066;font-weight:bold">if</span> <span style="color:#009900">(</span>document.<span style="color:#660066">attachEvent</span><span style="color:#009900">)</span><span style="color:#009900">{</span> <span style="color:#006600;font-style:italic">// IE</span>
     document.<span style="color:#660066">attachEvent</span><span style="color:#009900">(</span><span style="color:#3366cc">'onclick'</span><span style="color:#339933">,</span> callback<span style="color:#009900">)</span><span style="color:#339933">;</span>
<span style="color:#009900">}</span> <span style="color:#000066;font-weight:bold">else</span> <span style="color:#009900">{</span>
     document.<span style="color:#660066">onclick</span> <span style="color:#339933">=</span> callback<span style="color:#339933">;</span>
<span style="color:#009900">}</span></pre></div></div>

<p><strong>事件类型</strong></p>
<p>鼠标事件：<br>
mouseup, mousedown, click (the sequence is mousedown-up-click), dblclick</p>
<p>mouseover (mouse is over an element), mouseout (mouse was over an element but left it), mousemove</p>
<p>键盘事件：<br>
keydown, keypress, keyup (occur in this sequence)</p>
<p>载入\window事件：<br>
load (an image or a page and all of its components are done loading), unload (user leaves the page), beforeunload (the script can provide the user with an option to stop the unload)<br>
abort (user stops loading the page in Firefox or an image in IE), error (a JavaScript error in Firefox and IE, also when an image cannot be loaded in IE)<br>
resize (browser window is resized), scroll (the page is scrolled), contextmenu (the right-click menu appears)</p>
<p>表单事件：<br>
focus (enter a form field), blur (leave form field)<br>
change (leave a field after the value has changed), select (select text in a text field)<br>
reset, submit</p>
<p><strong>AJAX</strong>（XMLHttpRequest以及IE7之前使用ActiveXObject）<br>
-EOF-</p>
<hr><h2>Related posts:</h2><ul><li><a href="http://ioio.name/object-oriented-javascript-note-4.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(四)">Object-Oriented JavaScript笔记(四)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-2.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(二)">Object-Oriented JavaScript笔记(二)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-7.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(七)">Object-Oriented JavaScript笔记(七)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(一)">Object-Oriented JavaScript笔记(一)</a></li><li><a href="http://ioio.name/object-oriented-javascript-note-5.html" rel="bookmark" title="Permanent Link: Object-Oriented JavaScript笔记(五)">Object-Oriented JavaScript笔记(五)</a></li></ul><hr><small>Copyright © 2005~2011 | <a href="http://ioio.name/object-oriented-javascript-note-6.html" title="Permalink">Permalink</a> | <a href="http://ioio.name/object-oriented-javascript-note-6.html#comments">3 Comments</a> | <a href="http://closetou.com" title="Close To U">Close To U</a> <br>
<a href="http://feeds.feedburner.com/miss">订阅</a> <a href="https://twitter.com/tearnon">Twitter</a> <a href="http://ioio.name/godaddy">域名优惠码</a> <a href="http://ioio.name/mt">Media Temple空间</a>
<a href="http://mdiatemple.com/" title="Domain Sale! $6.89 .com at GoDaddy">主机/域名优惠码</a>
</small> )