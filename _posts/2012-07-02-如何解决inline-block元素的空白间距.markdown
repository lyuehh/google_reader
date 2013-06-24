---
layout: post
title:  "如何解决inline-block元素的空白间距"
date:   2012-07-02 23:30:31
author: Airen
categories: program
---

## 如何解决inline-block元素的空白间距
### by Airen
### at 2012-07-02 23:30:31
### original <http://www.w3cplus.com/css/fighting-the-space-between-inline-block-elements>

<div><div><div><p>有关于使用inline-block来代替float的讨论也蛮多的，最常说的就是使用inline-block来代替float进行布局，或者使用inline-block来实现元素的居中效果。前面《<a href="http://www.w3cplus.com/css3/creat-pager-list-with-css3">CSS3制作的分页导航</a>》一文中就是使用的inline-block制作的居中效果，不过留下了一上问题，就是使用inline-block的元素之间会存在“4px”的空白间距。那么今天我们就一起来说说这个“4px”的问题。</p>
<p>大家首先来看一个<a href="http://jsfiddle.net/w3cplus/HLM9h/">DEMO</a>：</p>
<p><strong>HTML Markup</strong></p>
<pre>&lt;ul&gt;
  &lt;li&gt;item1&lt;/li&gt;
  &lt;li&gt;item2&lt;/li&gt;
  &lt;li&gt;item3&lt;/li&gt;
  &lt;li&gt;item4&lt;/li&gt;
  &lt;li&gt;item5&lt;/li&gt;
&lt;/ul&gt;
</pre><p><strong>CSS Code</strong></p>
<pre>*{
  margin: 0;
  padding: 0;
}
ul {
  list-style: none outside none;
  padding: 10px;
  background: green;
  text-align: center;
}
ul li {
  display: inline-block;
  *display: inline;
  zoom: 1;
  background: orange;
  padding: 5px;
}
</pre><p>上面的demo效果，明显的可以看出，在inline-block的元素之间存在“4px”的空白：</p>
<p><a href="http://jsfiddle.net/w3cplus/HLM9h/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/inline-block-1.jpg"></a></p>
<p>上面截图是：IE8-9、Firefox、Safari等浏览器下的效果，换句话说，这种现像只有在这几种浏览器中才会出现。下面我们就来说说解决这个“4px”(Chrome下是8px)的几种方法：</p>
<h4>方法一：改变HTML结构</h4>
<p>简单一点的方法就是就是改变HTML的结构，你可以使用下面几种方法的任何一种都可以达到效果：</p>
<p><strong>结构一：</strong></p>
<pre>&lt;ul&gt;
  &lt;li&gt;
  item1&lt;/li&gt;&lt;li&gt;
  item2&lt;/li&gt;&lt;li&gt;
  item3&lt;/li&gt;&lt;li&gt;
  item4&lt;/li&gt;&lt;li&gt;
  item5&lt;/li&gt;  
&lt;/ul&gt;
</pre><p>这种方法接近标签换行格式的写法，也更趋近阅读。</p>
<p><strong>结构二：</strong></p>
<pre>&lt;ul&gt;
  &lt;li&gt;item1&lt;/li
  &gt;&lt;li&gt;item2&lt;/li
  &gt;&lt;li&gt;item3&lt;/li
  &gt;&lt;li&gt;item4&lt;/li
  &gt;&lt;li&gt;item5&lt;/li&gt;  
&lt;/ul&gt;
</pre><p>结构二和结构一极呼是一样，结束标签的“&gt;”成了另一行的起始标签。</p>
<p><strong>结构三：</strong></p>
<pre>&lt;ul&gt;
  &lt;li&gt;item1&lt;/li&gt;&lt;!--
  --&gt;&lt;li&gt;item2&lt;/li&gt;&lt;!--
  --&gt;&lt;li&gt;item3&lt;/li&gt;&lt;!--
  --&gt;&lt;li&gt;item4&lt;/li&gt;&lt;!--
  --&gt;&lt;li&gt;item5&lt;/li&gt;  
&lt;/ul&gt;
</pre><p>结构三的方法采用的是html的注释的方法，这种方法我想大家不太常见，不过同样能解决我们需要解决的问题。</p>
<p><strong>结构四：</strong></p>
<pre>&lt;ul&gt;&lt;li&gt;item1&lt;/li&gt;&lt;li&gt;item2&lt;/li&gt;&lt;li&gt;item3&lt;/li&gt;&lt;li&gt;item4&lt;/li&gt;&lt;li&gt;item5&lt;/li&gt;&lt;/ul&gt;
</pre><p>结构四，我想是大家常用来解决这样的问题的方法吧，下面我们来看看按上述几种方法写的效果：</p>
<p><a href="http://jsfiddle.net/w3cplus/ddmRX/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/inline-block-2.jpg"></a></p>
<p>方法一所说的是通过标签来解决，虽然问题是解决了，但可以说不能称作是技巧。而且上面的方法只适合于写静态页面的时候，一旦你的HTML不是自己写，而是后台生成，就比如CMS来说，标签后台生成，此时，我想大家又要骂街了，这可怎么办？其实我们除了上面的方法，还可以使用CSS来解决的。</p>
<h4>方法二：负的margin</h4>
<p>很多地方讨论使用负的margin来解决，比如说：</p>
<pre>	ul {
		font-size: 12px;
	}
	ul li {
		margin-right: -4px;
		*margin-right: 0;
	}
</pre><p>这种解决方法并不完美，如果你的父元素设置的字号不一样，可能你的“-4px”就不能解决问题。况且在Chrome中你需要另外设置一个负的margin值才能实现同等的效果。</p>
<p><a href="http://jsfiddle.net/w3cplus/kW7F3/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/inline-block-3.jpg"></a></p>
<p>当然有些文章介绍使用"-0.25em"来解决，这也是跟元素的字号有极大的关系。<strong style="color:red">所以我个人建议不使用负的margin来解决这样的问题。</strong></p>
<h4>方法三：设置父元素字体为0</h4>
<p>第三种方法设置父元素的字体为“0”，然后在“inline-block”元素上重置字体需要的大小。</p>
<pre>ul {
  list-style: none outside none;
  padding: 10px;
  background: green;
  text-align: center;
	<strong style="color:green">font-size: 0px;</strong>
    }
ul li {
  display: inline-block;
  *display: inline;
  zoom: 1;
  background: orange;
  padding: 5px;
  <strong style="color:green">font-size: 12px;</strong>
}
</pre><p>这样处理在Firexfox,chrome等浏览器下是达到了效果，可是在Safari下可问题依然存在：</p>
<p><a href="http://jsfiddle.net/w3cplus/JP9Q2/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/inline-block-4.jpg"></a></p>
<p>按此来说，方法三也不是绝佳的好方法，用不用大家自己考虑。</p>
<h4>方法四：丢失结束标签</h4>
<p>说实在的，这种方法又回到了方法一，在html标签上动手脚。就是让“inline-block”元素丢失关闭标签</p>
<pre>&lt;ul&gt;
  &lt;li&gt;item1
  &lt;li&gt;item2
  &lt;li&gt;item3
  &lt;li&gt;item4
  &lt;li&gt;item5
&lt;/ul&gt;
</pre><p>样式基本不变，我们来看看效果：</p>
<p><a href="http://jsfiddle.net/w3cplus/TRRVq/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/inline-block-5.jpg"></a></p>
<p>这种方法虽然能达到各浏览器的兼容，但还是有一个前提，那就是“DOCTYPE”要选择对，在“XHTML”下可就问题又出来了。</p>
<h4>方法五：jquery方法</h4>
<p>最后在给大家提供一种jquery的方法：</p>
<p><strong>HTML Markup</strong></p>
<pre>&lt;ul class=&quot;removeTextNodes&quot;&gt;
  &lt;li&gt;item1&lt;/li&gt;
  &lt;li&gt;item2&lt;/li&gt;
  &lt;li&gt;item3&lt;/li&gt;
  &lt;li&gt;item4&lt;/li&gt;
  &lt;li&gt;item5&lt;/li&gt;
&lt;/ul&gt;
</pre><p><strong>CSS Code</strong></p>
<pre>ul {
  list-style: none outside none;
  padding: 10px;
  background: green;
  text-align: center;
  font-size: 12px;
}
ul li {
  display: inline-block;
  *display: inline;
  zoom: 1;
  background: orange;
  padding: 5px;
}
</pre><p><strong>jQuery Code</strong></p>
<pre>	$('.removeTextNodes').contents().filter(function() {
    return this.nodeType === 3;
	}).remove();
</pre><p>最后一种方法是通过jQuery来改变“nodeType”的值，从而实现我们需要的效果。</p>
<p><a href="http://jsfiddle.net/w3cplus/GSPYA/"><img alt="" src="http://www.w3cplus.com/sites/default/files/201202/inline-block-5.jpg"></a></p>
<p>上面讲述了多种方法，但要兼容多个浏览器版本，并不每种方法实用，以前常用的方法，这次测试并不兼容所有浏览器。要做到兼容所有浏览器，个人认为还是在html的标签上做一定的处理，或者采用最后一种方法，通过“jQuery”来改变“nodeType”值达到效果。针对这个“inline-block”之间的间距有几篇文章做过介绍，但里面的方法，和上面介绍的测试的基本一样，具体如何运用，大家还是根据自己的需求进行选择或处理。</p>
<h4>扩展阅读：</h4>
<ol><li><a href="http://css-tricks.com/fighting-the-space-between-inline-block-elements/">Fighting the Space Between Inline Block Elements</a></li>
<li><a href="http://www.alsacreations.com/astuce/lire/1432-display-inline-block-espaces-indesirables.html">display: inline-block et les espaces indésirables</a></li>
<li><a href="http://www.zhangxinxu.com/wordpress/?p=2357">去除inline-block元素间间距的N种方法</a></li>
</ol><p>那么有关于“inline-block”的间距如何去除，今天就扯到这了，希望这篇文章对喜欢用inline-block的童鞋有所帮助。如果您有更好的方法，记得与我们一起分享，欢迎在下面的评论中留下您的经验，或者指正上面的不对之处。</p>
<h4>更新：全兼容的样式解决方法</h4>
<p>经过高人指点，使用纯CSS还是找到了兼容的方法，就是在父元素中设置font-size:0,用来兼容chrome，而使用letter-space:-N px来兼容safari:</p>
<pre>.finally-solve {
  letter-spacing: -4px;/*根据不同字体字号或许需要做一定的调整*/
  word-spacing: -4px;
  font-size: 0;
}
.finally-solve li {
  font-size: 16px;
  letter-spacing: normal;
  word-spacing: normal;
  display:inline-block;
  *display: inline;
  zoom:1;
}
</pre><p>如需转载烦请注明出处：<strong><a href="http://www.w3cplus.com">W3CPLUS</a></strong></p>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blogclassify/8.html">css</a></li></ul></div><div><div><div><div>
      <div>2</div>
                  <a href="http://www.w3cplus.com/vote/node/294/1/vote/alternate/ooxDa8rbKsrjNgGQtmUiZsVTZIsaRjGX3goB7D6Bq0k/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><img src="http://www1.feedsky.com/t1/654894284/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/css/fighting-the-space-between-inline-block-elements" border="0" height="0" width="0">