---
layout: post
title:  "读取远程图片或者文件并存储到本地的nodejs模块（readof)"
date:   2012-09-14 16:27:56
author: admin
categories: program
---

## 读取远程图片或者文件并存储到本地的nodejs模块（readof)
### by admin
### at 2012-09-14 16:27:56
### original <http://item.feedsky.com/~feedsky/helloJavaScript/~8514355/703064732/6618683/1/item.html>

<p></p>
<div>
<h1><a name="nodejs读取远程图片或者文件并存储到本地">nodejs读取远程图片或者文件并存储到本地</a></h1>
<div>
<p>这个模块有点蛋疼，原理很简单，给搞不清楚原理的同学直接用的。一句话读取远程文件到本地。
</p>
</div>
<h3><a name="summary">summary</a></h3>
<div>
<ul>
<li>
<div> 读取远程文件到指定路径并提供回调</div>
</li>
<li>
<div> @param picUrl string 要读取的图片的路径</div>
</li>
<li>
<div> @param targetPath string 目标存储路径，写完整，类型自行判断，无法根据content-type判断，可伪装</div>
</li>
<li>
<div> @param callback function 回调方法，写入完成或者出错的时候回调 callback(info,error)</div>
</li>
<li>
<div> 回调数据 出错时为空对象{}，成功时为</div>
</li>
</ul>
<pre>{
 targetPath:targetPath,
 picUrl:picUrl
}</pre>
<ul>
<li>
<div> @author yutou</div>
</li>
<li>
<div> @email xinyu198736@gmail.com</div>
</li>
<li>
<div> @blog <a href="http://www.html-js.com" title="http://www.html-js.com" rel="nofollow">http://www.html-js.com</a></div>
</li>
</ul>
</div>
<h3><a name="install">install</a></h3>
<div>
<pre>
npm install readof
</pre>
</div>
<h3><a name="clone">clone</a></h3>
<div>
<pre>
$ git clone https://bitbucket.org/xinyu198736/readonlinefile.git/wiki
</pre>
</div>
<h3><a name="code">code</a></h3>
<div>
<pre> <span>var</span> readOF=require<span>(</span><span>&quot;readof&quot;</span><span>)</span>;
 readOF.<span>read</span><span>(</span>pic,target_path,<span>function</span><span>(</span>data,error<span>)</span><span>{</span>
     <span>//do something</span>
 <span>}</span><span>)</span>;</pre>
</div>
</div>
<p></p><img src="http://www1.feedsky.com/t1/703064732/helloJavaScript/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/helloJavaScript/~8514355/703064732/6618683/1/item.html" border="0" height="0" width="0">