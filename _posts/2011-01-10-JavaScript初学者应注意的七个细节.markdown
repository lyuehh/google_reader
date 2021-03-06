---
layout: post
title:  "JavaScript初学者应注意的七个细节"
date:   2011-01-10 19:20:00
author: 梦想
categories: program
---

## JavaScript初学者应注意的七个细节
### by 梦想
### at 2011-01-10 19:20:00
### original <http://www.cnblogs.com/lhb25/archive/2011/01/10/1932284.html>

<p><p><span style="font-size:13px">　　每种语言都有它特别的地方，对于JavaScript来说，使用var就可以声明任意类型的变量，这门脚本语言看起来很简单，然而想要写出优雅的代码却是需要不断积累经验的。本文利列举了JavaScript初学者应该注意的七个细节，与大家分享。 </span></p>
<p> </p>
<p><span style="font-size:14pt"><strong><span face="宋体" style="font-family:宋体">（1）</span>简化代码</strong></span></p>
<hr>
<p><span style="font-family:宋体"> </span></p>
<p><span style="font-size:13px"><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JavaScript</span></span><span style="font-family:宋体">定义对象和数组非常简单，我们想要创建一个对象，一般是这样写的：</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span><span style="font-family:宋体"> </span><span style="font-family:宋体"> </span></span></p>
<p>
<div>
<pre>var car = new Object();
car.colour = 'red';
car.wheels = 4;
car.hubcaps = 'spinning';
car.age = 4;</pre>
</div>
</p>
<p><span style="font-family:宋体">下面的写法可以达到同样的效果：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var car = {
	colour:'red',
	wheels:4,
　　hubcaps:'spinning',
　　age:4
}</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">后面的写法要短得多，而且你不需要重复写对象名称。</span></p>
<p><span style="font-family:宋体">另外对于数组同样有简洁的写法，过去我们声明数组是这样写的：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var moviesThatNeedBetterWriters = new Array(
  'Transformers','Transformers2','Avatar','Indiana Jones 4'
);
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">更简洁的写法是：</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var moviesThatNeedBetterWriters = [
  'Transformers','Transformers2','Avatar','Indiana Jones 4'
];
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">对于数组，还有关联数组这样一个特别的东西。</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span><span style="font-family:宋体">你会发现很多代码是这样定义对象的：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var car = new Array();
car['colour'] = 'red';
car['wheels'] = 4;
car['hubcaps'] = 'spinning';
car['age'] = 4;
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">这太疯狂了，不要觉得困惑，“</span><span style="font-family:宋体">关联数组”只</span><span style="font-family:宋体">是对象的一个别名而已。</span></p>
<p><span style="font-family:宋体">另外一个简化代码的方法是使用三元运算符，举个例子：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var direction;
if(x &lt; 200){
  direction = 1;
} else {
  direction = -1;
}
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">我们可以使用如下的代码替换这种写法：</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span></p>
<p><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">
<div>
<pre>var direction = x &lt; 200 ? 1 : -1;
</pre>
</div>
</span></span></p>
<p>
<p><strong><span style="font-size:14pt"><span style="font-family:宋体"> </span></span></strong></p>
<p><strong><span style="font-size:14pt"><span style="font-family:宋体">（2）使用</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JSON</span></span><span style="font-family:宋体">作为数据格式</span></span></strong> <span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span></p>
<p>
<hr>
</p>
<p> </p>
</p>
<div>
<p><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">伟大的Douglas Crockford</span></span><span style="font-family:宋体">发明了</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JSON</span></span><span style="font-family:宋体">数据格式来存储数据，你可以使用原生的</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">javascript</span></span><span style="font-family:宋体">方法来存储复杂的数据而不需要进行任何额外的转换，例如：</span><span style="font-family:宋体"> </span></p>
<div>
<pre>var band = {
  "name":"The Red Hot Chili Peppers",
  "members":[
    {
      "name":"Anthony Kiedis",
      "role":"lead vocals"
    },
    {
      "name":"Michael 'Flea' Balzary",
      "role":"bass guitar, trumpet, backing vocals"
    },
    {
      "name":"Chad Smith",
      "role":"drums,percussion"
    },
    {
      "name":"John Frusciante",
      "role":"Lead Guitar"
    }
  ],
  "year":"2009"
}
</pre>
</div>
<p><span style="font-family:宋体">你可以使用在</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JavaScript</span></span><span style="font-family:宋体">中直接使用</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JSON</span></span><span style="font-family:宋体">，甚至作为</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">API</span></span><span style="font-family:宋体">返回的一种格式，这就是所谓的</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JSON – P</span></span><span style="font-family:宋体">，在许多的</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">API中被应用</span></span><span style="font-family:宋体">，例如：</span></p>
<span style="font-family:宋体">
<p>
<pre>&lt;div id=&quot;delicious&quot;&gt;&lt;/div&gt;&lt;script&gt;
function delicious(o){
  var out = &#39;&lt;ul&gt;&#39;;
  for(var i=0;i&lt;o.length;i++){
    out += &#39;&lt;li&gt;&lt;a href=&quot;&#39; + o[i].u + &#39;&quot;&gt;&#39; +
           o[i].d + &#39;&lt;/a&gt;&lt;/li&gt;&#39;;
  }
  out += &#39;&lt;/ul&gt;&#39;;
  document.getElementById(&#39;delicious&#39;).innerHTML = out;
}
&lt;/script&gt;
&lt;script src=&quot;http://feeds.delicious.com/v2/json/codepo8/javascript?count=15&amp;callback=delicious&quot;&gt;&lt;/script&gt;</pre>
</p>
</span></div>
<p><span style="font-size:13px"><span style="font-family:宋体">这里调用</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">delicious </span></span><span style="font-family:宋体">的</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">Web</span></span><span style="font-family:宋体">服务获取最新书签，以</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JSON</span></span><span style="font-family:宋体">格式返回，然后将它们显示成无序列表的形式。</span></span></p>
<p><span style="font-size:13px"><span style="font-family:宋体">从本质上讲，</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JSON</span></span><span style="font-family:宋体">是用于描述复杂的数据最轻量级的方式，而且直接它运行在浏览器中。</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span><span style="font-family:宋体">你甚至可以在</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">PHP</span></span><span style="font-family:宋体">中调用 </span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">json_decode</span></span><span style="font-family:宋体">（）函数来使用它。</span></span></p>
<p> </p>
<p><strong><span style="font-size:14pt">（</span></strong><span style="font-size:14pt">3</span><strong><span style="font-size:14pt">）尽量使用JavaScript原生函数 </span></strong></p>
<p>
<hr>
</p>
<p><span style="font-size:13px"> </span></p>
<p><span style="font-size:13px">要找一组数字中的最大数，我们可能会写一个循环，例如： </span></p>
<p>
<pre>var numbers = [3,342,23,22,124];
var max = 0;
for(var i=0;i&lt;numbers.length;i++){
  if(numbers[i] &gt; max){
    max = numbers[i];
  }
}
alert(max);
</pre>
</p>
<p><span style="font-family:宋体">其实，不用循环可以实现同样的功能：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var numbers = [3,342,23,22,124];
numbers.sort(function(a,b){return b - a});
alert(numbers[0]);
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">而最简洁的写法是：</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span></p>
<p><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">
<div>
<pre>Math.max(12,123,3,2,433,4); // returns 433
</pre>
</div>
</span></span></p>
<p><span style="font-family:宋体">你甚至可以使用</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">Math.max</span></span><span style="font-family:宋体">来检测浏览器支持哪个属性：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var scrollTop= Math.max(
 doc.documentElement.scrollTop,
 doc.body.scrollTop
);
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">如果你想给一个元素增加</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">class</span></span><span style="font-family:宋体">样式，可能原始的写法是这样的：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>function addclass(elm,newclass){
  var c = elm.className;
  elm.className = (c === '') ? newclass : c+' '+newclass;</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">而更优雅的写法是：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>function addclass(elm,newclass){
  var classes = elm.className.split(' ');
  classes.push(newclass);
  elm.className = classes.join(' ');
}
</pre>
</div>
</span></p>
<div></div>
<p><strong><span style="font-family:宋体;font-size:14pt">  </span></strong></p>
<p><strong><span style="font-family:宋体;font-size:14pt">（4）事件委托</span></strong></p>
<p><span style="font-family:宋体">
<hr>
<p>  </p>
</span><span style="font-size:13px"><span style="font-family:宋体">事件是J</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">avaScript</span></span><span style="font-family:宋体">非常重要的一部分。我们想给一个列表中的链接绑定点击事件，一般的做法是写一个循环，给每个链接对象绑定事件，</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">HTML</span></span><span style="font-family:宋体">代码如下：</span></span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>&lt;h2&gt;Great Web resources&lt;/h2&gt;
&lt;ul id=&quot;resources&quot;&gt;
  &lt;li&gt;&lt;a href=&quot;http://opera.com/wsc&quot;&gt;Opera Web Standards Curriculum&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;http://sitepoint.com&quot;&gt;Sitepoint&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;http://alistapart.com&quot;&gt;A List Apart&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;http://yuiblog.com&quot;&gt;YUI Blog&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;http://blameitonthevoices.com&quot;&gt;Blame it on the voices&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;http://oddlyspecific.com&quot;&gt;Oddly specific&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">脚本如下：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>// Classic event handling example
(function(){
  var resources = document.getElementById(&#39;resources&#39;);
  var links = resources.getElementsByTagName(&#39;a&#39;);
  var all = links.length;
  for(var i=0;i&lt;all;i++){
    // Attach a listener to each link
    links[i].addEventListener(&#39;click&#39;,handler,false);
  };
  function handler(e){
    var x = e.target; // Get the link that was clicked
    alert(x);
    e.preventDefault();
  };
})();
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">更合理的写法是只给列表的父对象绑定事件，这样可行的原理在于事件是支持冒泡的，代码如下：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>(function(){
  var resources = document.getElementById('resources');
  resources.addEventListener('click',handler,false);
  function handler(e){
    var x = e.target; // get the link tha
    if(x.nodeName.toLowerCase() === 'a'){
      alert('Event delegation:' + x);
      e.preventDefault();
    }
  };
})();
</pre>
</div>
</span></p>
<div></div>
<p><strong><span style="font-family:宋体;font-size:14pt">  </span></strong></p>
<p><strong><span style="font-family:宋体;font-size:14pt">（5）匿名函数</span></strong></p>
<p><strong><span style="font-family:宋体;font-size:14pt">
<hr>
<p>  </p>
</span></strong><span style="font-size:13px"><span style="font-family:宋体">关于</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">JavaScript</span></span><span style="font-family:宋体">的最头疼的事情之一是，它的变量没有特定的作用范围。</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> 一般情况下，</span></span><span style="font-family:宋体">任何变量，函数，数组或对象都是全局性，这意味着在同一页上的其他脚本可以访问并覆盖它们。解决方法是把变量封装在一个匿名函数中。</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman"> </span></span><span style="font-family:宋体">例如，下面的定义将产生三个全局变量和和两个全局函数：</span></span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var name = 'Chris';
var age = '34';
var status = 'single';
function createMember(){
  // [...]
}
function getMemberDetails(){
  // [...]
}
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">封装后如下：</span><span style="font-family:宋体"> </span></p>
<p><span style="font-family:宋体">
<div>
<pre>var myApplication = function(){
  var name = 'Chris';
  var age = '34';
  var status = 'single';
  return{
    createMember:function(){
      // [...]
    },
    getMemberDetails:function(){
      // [...]
    }
  }
}();
// myApplication.createMember() and
// myApplication.getMemberDetails() now works.
</pre>
</div>
</span></p>
<div></div>
<p><span style="font-family:宋体">这被称为单体模式，是J</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">avaScript</span></span><span style="font-family:宋体">设计模式的一种，这种模式在</span><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">YUI</span></span><span style="font-family:宋体">中用得非常多，</span><span style="font-family:宋体">改进的写法是：</span><span lang="EN-US"> <span face="Times New Roman" style="font-family:Times New Roman"> </span></span></p>
<p><span lang="EN-US"><span face="Times New Roman" style="font-family:Times New Roman">
<div>
<pre>var myApplication = function(){
  var name = 'Chris';
  var age = '34';
  var status = 'single';
  function createMember(){
    // [...]
  }
  function getMemberDetails(){
    // [...]
  }
  return{
    create:createMember,
    get:getMemberDetails
  }
}();
//myApplication.get() and myApplication.create() now work.
</pre>
</div>
</span></span></p>
<p><span style="font-size:12px">   </span></p>
<p><strong><span style="font-size:14pt">（</span></strong><span style="font-size:14pt">6</span><strong><span style="font-size:14pt">）代码可配置</span></strong></p>
<p><span style="font-size:12px">
<hr>
<p> </p>
<p><span style="font-size:12px">你写的代码如果想让别人更容易进行使用或者修改，则需要可配置，解决方案是在你写的脚本中增加一个配置对象。要点如下： <br> <br>1、在你的脚本中新增一个叫configuration的对象。</span></p>
<p><span style="font-size:12px">2、在配置对象中存放所有其它人可能想要去改变的东西，例如CSS的ID、class名称、语言等等。</span></p>
<p><span style="font-size:12px">3、返回这个对象，作为公共属性以便其它人可以进行重写。 </span></p>
</span><span style="font-size:12px"> </span></p>
<p><span style="font-size:12px"> </span></p>
<p><strong><span style="font-size:14pt">（</span></strong><span style="font-size:14pt">7</span><strong><span style="font-size:14pt">）代码兼容性</span></strong></p>
<hr>
<p> </p>
<p><span style="font-size:12px">　　兼容性是初学者容易忽略的部分，通常学习Javascript的时候都是在某个固定的浏览器中进行测试，而这个浏览器很有可能就是IE，这是非常致命的，因为目前几大主流浏览器中偏偏IE对标准的支持是最差的。最终用户看到的结果也许就是，你写的代码在某个浏览器无法正确运行。你应该把你的代码在主流的浏览器中都测试一下，这也许很费时间，但是应该这样做。 </span><span style="font-size:12px"> </span></p>
<p><span style="font-size:12px"> </span><span style="font-size:12px"> </span></p>
<p>
<p><span style="font-size:12px">
<p>（编译来源：<a href="http://www.cnblogs.com/lhb25/"><span color="#6466b3" style="color:#6466b3">梦想天空</span></a>  原文来自：<a href="http://www.smashingmagazine.com/2010/04/20/seven-javascript-things-i-wish-i-knew-much-earlier-in-my-career/">Seven JavaScript Things I Wish I Knew Much Earlier In My Career</a>）</p>
</span></p>
</p><img src="http://www.cnblogs.com/lhb25/aggbug/1932284.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/lhb25/">梦想</a> 发表于 2011-01-10 19:20 <a href="http://www.cnblogs.com/lhb25/archive/2011/01/10/1932284.html">原文链接</a></p><p>评论: 39　<a href="http://www.cnblogs.com/lhb25/archive/2011/01/10/1932284.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/lhb25/archive/2011/01/10/1932284.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/87975/">欧盟委员会想把 Google 的数字化版权从 15 年砍到 7 年</a><span style="color:gray">(2011-01-11 15:21)</span><br>· <a href="http://news.cnblogs.com/n/87974/">淘宝，以及淘宝商城</a><span style="color:gray">(2011-01-11 15:17)</span><br>· <a href="http://news.cnblogs.com/n/87973/">问答型社交网络Quora六问</a><span style="color:gray">(2011-01-11 15:14)</span><br>· <a href="http://news.cnblogs.com/n/87972/">Skype单日在线用户突破2700万创纪录</a><span style="color:gray">(2011-01-11 15:07)</span><br>· <a href="http://news.cnblogs.com/n/87971/">Windows 7成主流操作系统</a><span style="color:gray">(2011-01-11 14:53)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/87913/">Groupon前传：从10个月的失败作品修改，1个月找到成功</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>