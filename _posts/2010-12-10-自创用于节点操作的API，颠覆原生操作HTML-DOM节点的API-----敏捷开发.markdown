---
layout: post
title:  "自创用于节点操作的API，颠覆原生操作HTML DOM节点的API --- 敏捷开发"
date:   2010-12-10 20:09:00
author: jkit
categories: program
---

## 自创用于节点操作的API，颠覆原生操作HTML DOM节点的API --- 敏捷开发
### by jkit
### at 2010-12-10 20:09:00
### original <http://www.cnblogs.com/jkit/archive/2010/12/10/1902624.html>

<p><div style="font:12px Verdana,Arial,Helvetica,sans-serif;line-height:17px;color:#2d2d2d">
<p>敏捷开发是一种以人为核心、迭代、循序渐进的开发方法。在敏捷开发中，软件项目的构建被切分成多个子项目，各个子项目的成果都经过测试，具备集成和可运行的特征。第一次看到敏捷开发的定义，我就被敏捷开发迷住了。通俗来说，敏捷开发可以让我们用过的代码可以再次重用，因为是再次重用，所以相对安全，再次调试也没有第一次那么费心，省时省力。不断重用代码的过程中把存在的bug不断的修复，也因为不断的去重用， 这个模板变得起越来越独立，适用的情况越来越广范，最后在安全方面达到铜墙铁壁，在开发方面达到随心所欲，在维护方面达到从容面对。<br><br>敏捷开发的确是利害，但如何练就这种深奥的武功呢？就我自身的情况靠人传授武功是不可能了，因为公司就我一个做开发的，苦思幂想之后，决定从开源的优秀框架入手，把它一行一行代码看懂，然后再为我所用。因为是一个人开发，前台和后台都得包办，哪从那一面做起呢？ 之前有过一二个月的开发经验，觉得前台的JS很费时，而且老觉得在做重复的事情，比如发ajax请求，接收结果之后操作节点（有时候遇到不兼容的情况，如select和table在IE下不支持使用innerHTML，style在IE不会自动转化为字符串，要用cssText代替，一旦这些情况遇上，真得是无比打击程序员的积极性，因为你要为此花时间去找替代方案，去调试），还有节点轮换，弹出层，表单验证等一系列烦琐工作。所以我就坚决从前台的JS做起。为了练就怎么把JS的重用性提高，我选择向Jquery取经。花了几个月看了一大半，略有所得。我其中之一的JS模块“无限深度操作节点”（文采不好，名字不妥别见怪）出来了。有了它，我在操作节点方面变得容易，代码变得精简，而且不用写额外的代码去兼容浏览器，谈笑中，功能就完成了。</p>
<div>
<h6 style="font-size:12px;color:#096598;font-weight:normal;margin:0px">首先我谈谈是什么让操作节点给我们带来烦恼：</h6>
<ul style="list-style-position:outside;list-style-type:decimal;padding-left:18px;margin-left:12px">
<li style="padding-bottom:12px">
			    编写ajax程序时，动态增删改页面元素几乎是不可避免的，使用属性innerHTML就是我们经常利用的途径，但在IE中table,thead,tfoot,tbody,tr,col,colgroup,html,title,style,frameset的innerHTML属性是只读的，也就是我们不能使用innerHTML更新这些节点。（这里没有提到select，其实select也是不会成功的，估计是IE的bug）。例子如下：（下面的$id代表document.getElementById）
				
<table style="background:none repeat scroll 0 0 #f5f5f5;border:1px solid #666666;padding:5px;margin:8px 0px;width:93%">
<tbody>
<tr>
<td width="238px">
&lt;select id=&quot;jkit&quot;&gt;<br>    &lt;option value=&quot;1&quot;&gt;jquery&lt;/option&gt;<br>&lt;/select&gt;
                        </td>
<td width="428px">
						    <span style="color:#818181">//执行代码:</span><br>
							$id(&#39;jkit&#39;).innerHTML = &#39;&lt;option&gt;jquery&lt;/option&gt;&#39;;<br>
							<span style="color:#818181">//IE下并没有报任何错误，但select一个option节点都没有了。如果你对table使用innerHTML，IE会报unknown runtime error</span>
						</td>
<td> </td>
</tr>
</tbody>
</table>
对于这种情况比较常用的兼容方法是外加一个外层元素，例子如下：
				
<table style="background:none repeat scroll 0 0 #f5f5f5;border:1px solid #666666;padding:5px;margin-top:8px;width:93%">
<tbody>
<tr>
<td width="238px">
&lt;div id=&quot;jkit&quot;&gt;<br>    &lt;select&gt;<br>        &lt;option value=&quot;1&quot;&gt;jquery&lt;/option&gt;<br>    &lt;/select&gt;<br>&lt;/div&gt;
                        </td>
<td width="428px">
						    <span style="color:#818181">//执行代码:</span><br>
							<span style="color:#030303">$id(&#39;jkit&#39;).innerHTML = &#39;&lt;select&gt;&lt;option value=&quot;1&quot;&gt;jkit&lt;/option&gt;&lt;/select&gt;&#39;;</span><br>
							<span style="color:#818181">//这样IE也成功改变select，但这种做法有个缺点，如果你对select注册过事件，这些事件会全部丢失，你要外加代码来重新注册事件。</span>
						</td>
<td> </td>
</tr>
</tbody>
</table>
</li>
<li style="padding-bottom:12px">
			    在指定的节点前后新增节点，这就涉及于节点定位，节点创建以及给节点属性设置。使用innerHTML通常只用于覆盖DOM元素的所有节点，如果只想改变元素的某个子节点，或者只想在某个子节点前后增加节点，仍然使用innerHTML就适得其反，实现起来很吃力了，而且使用了innerHTML之后，对子节点注册过的事件肯定全部丢失掉。不使用innerHTML，那只好使用原生的DOM方法了，但这种代替方案也不好使，看下面例子：
				
<table style="background:none repeat scroll 0 0 #f5f5f5;border:1px solid #666666;padding:5px;margin:8px 0px;width:93%">
<tbody>
<tr>
<td style="vertical-align:top" width="238px">
&lt;select&gt;<br>    &lt;option value=&quot;1&quot;&gt;jquery&lt;/option&gt;<br>    &lt;option value=&quot;2&quot;&gt;jkit&lt;/option&gt;<br>    &lt;option value=&quot;3&quot;&gt;mars&lt;/option&gt;<br>&lt;/select&gt;
                        </td>
<td width="428px">
						    <span style="color:#818181">//现在我想在jkit之前多加一个option，用原生的DOM方法实现:</span><br>
							<span style="color:#030303">var newNode = document.createElement('option')</span>,<span style="color:#818181">//新建一个节点</span><br>
							<span style="color:#030303">selector = document.getElementById('jkit3')</span>,<br>
							<span style="color:#818181">/* 也可以用selector.options，但getElementsByTagName更通用。</span><br>
							<span style="color:#818181">那用childNodes怎么?最好也不要，对于空白节点，IE和FF的处理方式不一样，</span><br>
							<span style="color:#818181">就这例子，在FF中，select的firstChild是空白文本节点，</span><br>
							<span style="color:#818181">因为select和第一个option之间有换行以及空白字符，</span><br>
							<span style="color:#818181">FF会为其创建节点，而IE会忽略 */</span><br>
							<span style="color:#030303">options = document.getElementsByTagName('option');</span><br>
							<span style="color:#030303">newNode.setAttribute('value','new');</span><br>
							<span style="color:#818181">//newNode.setAttribute('text','NewNode');text不支持这样设置</span><br>
							<span style="color:#818181">//newNode.text = 'NewNode';ie不支持这种方式</span><br>
							<span style="color:#030303">newNode.innerHTML = 'NewNode';</span><br>
							<span style="color:#030303">selector.insertBefore(newNode,options[1]);</span><span style="color:#818181">//在kit之前插入</span><br>
						</td>
<td> </td>
</tr>
</tbody>
</table>
执行了上面的代码之后，select多了一个option:<br>
				&lt;select&gt;<br>    &lt;option value=&quot;1&quot;&gt;jquery&lt;/option&gt;<br>    &lt;option value=&quot;new&quot;&gt;newNode&lt;/option&gt;<br>    &lt;option value=&quot;2&quot;&gt;jkit&lt;/option&gt;<br>    &lt;option value=&quot;3&quot;&gt;mars&lt;/option&gt;<br>&lt;/select&gt;<br>
				新增一个节点这要用到6句代码，现在只是新增一个节点，那对于批量操作怎么办啊？而且原生的DOM方法不怎么好用，名字长而且参数顺序不好记住，用的时候还得翻文档（除非你的开发的天天在操作节点，天天用这些方法）。有人说，也不怎么麻烦啊，我可以把这6句代码写成一个方法，然后通过传参来完成批量操作。某种程度上是可以减轻工作量，但看远一点，这也是不怎么现实的。原因有三：其一，每次新增的节点的属性不一样，加的位置不一样，你还得在方法外面确定新节点的位置以及在方法内部判断这个节点要加那些属些，下个节点要加哪个属性，把代码写在法方里面只能让你稍为轻松了些；其二，上面的代码可以看到，新增一个节点，你要排除很多不兼容的实现方法，由其在设置属性方面，每种元素有不一样的属性，在兼容方案上就有差异，你针对不同元素写这样的方法时，你必须要记住每种元素的兼容方案，可谁敢说能记住所有兼容方案，而且程序员想在技术上有所作为，靠的是思维逻辑的提升，而不是记忆，没必需在这方面下苦功；其三，一个项目是实现功能了，下一个项目再来，有相似的功能，但在细节上有差异，按上面的做法，可以想像的到你的代码重用性非常得低，因为要改变某些细节，你得重新理解曾经写下的逻辑，重新为你细节上的不同重构代码，这样一来，效率太低了，完全谈不上敏捷开发。<br><br>
				看到这样，估计读者都会问，如果不是这样做，难道开发真的可以做到复制粘贴吗？可以肯定的说，是不可能的。再全面的代码也不可能满足实现了不同包罗所有逻辑。但可以做得复用性很高，关键在于找出不同逻辑的共性并很好的独立出来。上面的操作节点做法我之所以说复用性低，是因为把操作节点跟特定的逻辑混合一起了，就是说特定逻辑的代码跟操作节点的代码关系太紧密了，到下一个逻辑又遇上节点操作时，再想重用上次的代码，就必须温习上次写下的逻辑，然后把操作节点的代码分离出来后再派上用场。下面介绍的由我开发的无限层次节点操作，就很好的从逻辑上独立出来，与逻辑无关。
			</li>
<li style="padding-bottom:12px">
			    上面的以select为例子，option是select的孩子节点，我们操作的是两层的树型结构，如果再深入一些，操作元素的子孙节点呢？如何定位元素后代节点呢？下面还是以原生的dom方法来实现：
				
<table style="background:none repeat scroll 0 0 #f5f5f5;border:1px solid #666666;padding:5px;margin:8px 0px;width:93%">
<tbody>
<tr>
<td style="vertical-align:top" width="238px">
&lt;table&gt;<br>    &lt;tr&gt;<br>        &lt;td&gt;<br>            &lt;ul&gt;<br>                &lt;li&gt;&lt;/li&gt;<br>                &lt;li&gt;&lt;/li&gt;<br>            &lt;/ul&gt;<br>        &lt;/td&gt;<br>        &lt;td&gt;<br>            &lt;ul&gt;<br>                &lt;li&gt;&lt;/li&gt;<br>            &lt;/ul&gt;<br>        &lt;/td&gt;<br>    &lt;/tr&gt;<br>&lt;/table&gt;
                        </td>
<td style="vertical-align:top" width="428px">
						    <span style="color:#818181">//现在我想在第一个li之前多加一个option，用原生的DOM方法实现:</span><br>
							<span style="color:#030303">newNode = document.createElement('li'),<br></span>
							<span style="color:#030303">table = document.getElementById('jkit4'),<br></span>
							<span style="color:#818181">//取li的父节点:</span><br>	
							<span style="color:#030303">uls = table.getElementsByTagName('ul'),<br></span>
							<span style="color:#818181">/* getElementsByTagName虽然通用，但如果li标签里面嵌套了li，li父节点的兄弟节点也有li的话，那么getElementsByTagName都会取到这些节点，如果你的html结构真有哪么复杂，取出来结果后你也很难定位到你想找的li节点。遇到这情况，你只以通过childNodes一层层往下找，但之前提过childNodes在IE和FF中行为是不致的，所以你还要做兼容处理。*/</span><br>	
							<span style="color:#030303">lis = table.getElementsByTagName('li');<br></span>
							<span style="color:#030303">newNode.innerHTML = 'NewNode';<br></span>
							<span style="color:#818181">//在指定位置插入</span><br>
							<span style="color:#030303">uls[0].insertBefore(newNode,lis[0]);<br></span>
						</td>
<td> </td>
</tr>
</tbody>
</table>
万变不离其中，跟上面的实现差不多，遇到复杂的html结构，可能就是定位比较麻烦，有人说定位不难，我可以在想要找的li上给定一个id属性，这样再怎么复杂也可以一步定位，但我想问如果要批量操作呢？ 如果table有很多行，每一行第都涉及到这样的操作，那是不是每个li给定一人id ？ 是可以，通过循环指也不怎么难，但我是反对这样的做法。如果有好的代替方案，我更赞成的是保持干净的HTML，在我开发的项目中，用于定位无素属性我是尽可能少给，因为我有足够灵活的方案寻节点。说到干净的HTML，这里说点题外话。怎么保持HTML的干净呢？在我开发中，HTML是不会混杂任何 逻辑性的javascript代码（只有接收后台数据的一些变量，而且这些变量统一放在HTML的最后面，变量太多的话，以组的形式组织），给元素注册事件也不会把onclick,onblur等事件代码嵌进html中，查看源码你不会在HTML中看到任何onclick,onblur等事件代码。回到这篇文章的主题吧！下面就这三点谈谈怎么让我们的工作变得简单！
			</li>
</ul>
</div>
<div>
<h6 style="font-size:12px;color:#096598;font-weight:normal;margin:0px">接下来要谈的是，怎么编写代码让我们的事件变得简单。先不要理会技术层面的事件，假如现在有这么一个JS类，里面有若干方法，这些方法足以让我们完成上面以及比上面更加复杂的事件。这个假设存在的类，因为只有方法名，没有具体实现，我们把它称为接口。那些下面要讲的是定义接口，当我们觉得接口中的方法能满足需求，我们再去实现它。（这是比较好的开发流程，先规划后行动）</h6>
<ul style="list-style-position:outside;list-style-type:decimal;padding-left:18px;margin-left:12px">
<li style="padding-bottom:12px">
			实例化一个对象<br><br>
			开发项目中，因为不止无限层次节点操作这个插件，还有很多，囊括了b2c和b2b常用的插件，比如轮换，分页，表单验证，表单元素取设值，批量上传图片，事件的重新包装，事件的批量处理，搜索的自动补全，购物车的ajax操作，以及公共方法的类。所以整个js会有很多类，每个类对应一个js变量，这样一来就有很多公共的js变量，如果开发的时候不小心在别的地方声明了一个同名的变量，那么这个类被消失。为了避免这种情况，我把所有类封装到一个名为$jkit类中，这样公共的变量就只有一个了。插件的类变成了局部，我怎么样访问呢？我另外定义一个$CL的类，里面定义了一些方法，这些方法用于访问$jkit的类，这样一来，就算有再多插件，都只会有两个公共变量$CL和$jkit。$CL负责调用$jkit。比如$CL中的newObj就用于实例化插件对象的方法。newObj有两个参数，第一个指定要实例化哪个插件，第二个参数用于实例化这个插件时的初始化参数，以数组的形式传参。
			
<table style="background:none repeat scroll 0 0 #f5f5f5;border:1px solid #666666;padding:5px;margin:8px 0px;width:93%">
<tbody>
<tr>
<td style="vertical-align:top" width="328px">
&lt;table id=&quot;jkit&quot;&gt;<br>    &lt;tbody&gt;<br>        &lt;tr&gt;<br>            &lt;th class=&quot;align&quot;&gt;Option&lt;/th&gt;<br>            &lt;th&gt;<br>                &lt;a href=&quot;javascript:void(0);&quot;&gt;Status&lt;/a&gt;<br>            &lt;/th&gt;<br>            &lt;th&gt;<br>                &lt;a href=&quot;javascript:void(0);&quot;&gt;Attribute&lt;/a&gt;<br>            &lt;/th&gt;<br>        &lt;/tr&gt;<br>        &lt;tr&gt;<br>            &lt;td&gt;&lt;/td&gt;<br>            &lt;td&gt;<br>                &lt;select size=&quot;3&quot; name=&quot;status&quot;&gt;<br>                    &lt;option value=&quot;all&quot;&gt;所有&lt;/option&gt;<br>                    &lt;option value=&quot;0&quot;&gt;下架&lt;/option&gt;<br>                &lt;/select&gt;<br>            &lt;/td&gt;<br>            &lt;td&gt;<br>                &lt;ul&gt;<br>                    &lt;li id=&quot;lior&quot;&gt;L&lt;/li&gt;<br>                    &lt;li&gt;XL&lt;/li&gt;<br>                &lt;/ul&gt;<br>            &lt;/td&gt;<br>        &lt;/tr&gt;<br>        &lt;tr&gt;<br>            &lt;td&gt;&lt;/td&gt;<br>            &lt;td&gt;&lt;/td&gt;<br>            &lt;td&gt;&lt;/td&gt;<br>        &lt;/tr&gt;<br>    &lt;/tbody&gt;<br>&lt;/table&gt;
                        </td>
<td style="vertical-align:top" width="368px">
						    <span style="color:#818181">/* 下面代码实例化了一个无限层次节点操作插件的对象，相当于重新构造了一棵新的对象树，新的对象树带有新的成员方法。就下面的例子，新对象树引用为table，table也是新树的根。它的后代对象由第二个参数childs决定 。新树的根引用是操作其后代对象的入口。重点讲解一下第二个参数。childs是一个数组结构，数组的第一个元素为tr，表示为dom根节点root的孩子节点tr重新构造新对象。如果原dom根节点root的中没有tr，将不会构造对象；如果tr内嵌有tr，不会为内嵌的tr构造节点，也就是说只会为孩子节点构造对象，孩子以下不会理会。第二个元素是td th，为什么有两个呢？因为tr下的孩子节点可以是th也可以是td，如果想同时为th td构造新的对象，就要同时写进去，用空格分开，检签的顺序不限制。第三个元素为select ul，为什么这两个可以写在一起？因为他们位于同一层次的，相对于根节点，它们都在第三层。只要同一层次的都可以写在一起。后面的以此类推，数量不限，就是无限层次了。新对象树的层次结构和原dom树的层次结构是一一对应的。 */</span><br>
							<span style="color:#030303">root = document.getElementById('category'), <br><br></span>
							<span style="color:#030303">childs = ['tr', 'td th', 'select ul', 'li option'],<br><br></span>
							<span style="color:#030303">table = $CL.newObj('maNode', [root, childs]);						</span>
						</td>
<td> </td>
</tr>
</tbody>
</table>
</li>
<li style="padding-bottom:12px">
			    新树的成员方法，<span style="color:red">阅读下面的API时，请心里紧记两点，其一，根对象以及后代对象的所有方法都是针对原DOM对象的，比如对新对象调用del，其实质是删除了对应的原DOM对象；其二，每次调用对象的增删改时，都会重新构造相应的分枝</span><br><br>
				<dl>
				    <dt>根对象独有方法</dt>
				    <dd style="margin-left:28px;margin-bottom:5px">
					    <span style="color:#8f5b00;font-weight:bold">function map(index1,index2,,,indexN){}</span><br>
						该方法用于寻找后代节点，table.map(1,1,0)会找到第二行的第二个单元格的第一个对象，也就是select对应的对象。当map只有一个参数，并且该参数为DOM原生对象，那么该方法返回对应的新对象。
					</dd>
					<dd style="margin-left:28px;margin-bottom:5px">
					    <span style="color:#8f5b00;font-weight:bold">function index(DOMElement){}</span><br>
						该方法返回原生DOMElement对象对应的素引，table.index(document.getElementById('lior'))，则返回[1,2,0,0]，结果是数组形式
					</dd>
					<dt style="margin-top:28px">后代对象独有的方法</dt>
				    <dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function add(index, html){}</span><br>
						该方法用于增加兄弟节点，index是相对于调用该方法的对象所处位置的位移，html就是要插要的节点，html可以是符合W3c标淮的任意html字任串<br>
						table.map(2).add(-1,&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，在第三行前面新增一行（可以同时插入多行）<br>
						table.map(2).add(-2,&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，在第三行前一行前面新增一行<br>
						table.map(0).add(2,&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，在第一行后一行前面新增一行<br>
						table.map(1).add(0,&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，index为0表示在当前行前面新增一行，并把当前行删除<br>
						table.map(1).add(&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，省略第一个参数，这是比较特别的用法，不用理会由哪个对象用，会在最后一行新增一行<br>
						table.map(1,1).add(1,&#39;&lt;td class=&quot;one&quot;&gt;新增单元格&lt;/td&gt;&#39;)，在第二行的第二个单元格之后新增一个单元格，再深入的节点如些类推，只需由map方法来确定节点就可以了<br>
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function del(index){}</span><br>
						该方法用于删除兄弟节点，index是相对于调用该方法的对象所处位置的位移<br>
						table.map(1).del(),index省略表示删除自身，这里删除第二行等同于table.map(1).del(0)<br>
						table.map(0).del(2),这里删除相对于当前调用对象后面第二行，这里就是删除第三行<br>
						table.map(2).del(-2),这里删除相对于当前调用对象前面第二行，这里就是删除第一行<br>
						table.map(0,1).del([0,-1,1]),如果index是一个数组，那么就是删除指定索引的兄弟节点，这时不用理会由哪个对象调用，索引为负数表示从最后计起，-1表示最后一个，这里删除第一个，第二个以及最后一个th<br>
						table.map(0,1).del(0,-1)，如果有两个参数，表示删除指定区间的兄弟节点，这时不用理会由哪个对象调用，索引为负数表示从最后计起，-1表示最后一个，这里删除第一个至后一个元素，参数大的可以作为第一个参数，大小顺序没有限制<br>
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function getParent(){}</span><br>
						获取调用对象父对象对应的原生DOM对象节点，table.map(0,1).getParent().tagName为tr
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function getHigher(){}</span><br>
						获取调用对象的父对象，table.map(0,1).getHigher.getNode().tagName为tr
					</dd>
					<dt style="margin-top:18px">根对象以及后代对象都拥有的方法</dt>
				    <dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function getNode(){}</span><br>
						获取调用对象对应的原生DOM对象节点，table.getNode().tagName为table，table.map(0,1).getNode()为th
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function sizeOf(){}</span><br>
						获取调用对象子对象的个数，table.sizeOf()为3，表示有三行
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function pos(){}</span><br>
						获取调用对象在其所有兄弟节点的所处的位置，table.map(1).pos()为1
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function html(html){}</span><br>
						获取调用对象对应原生dom对象的innerHTML，如果有传参，为其innerHTML属性赋值（请不要对innerHTML为只读的对象赋值）
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function attr(html){}</span><br>
						获取调用对象对应原生dom对象的innerHTML，如果有传参，为其对应的属性赋值（还未实现）
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function before(index,html){}</span><br>
						往调用对象的指定子对象前面添加节点，index是相对位移<br>
						table.before(1,&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，第二行前面添加一个行<br>
                        table.map(1,2,0).before(-1,&#39;&lt;li style=&quot;color:red;&quot;&gt;新增li节点&lt;/li&gt;&#39;)，在最后一个li前面新增一个li（索引为负数表示从最后计起，-1表示最后一个）<br>
						table.before(&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，省略第一个参数表示在第一个子对象前面新增节点
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function append(index,html){}</span><br>
						往调用对象的指定子对象后面添加节点，index是相对位移<br>
						table.append(1,&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，第二行后面添加一个行<br>
                        table.map(1,2,0).append(-1,&#39;&lt;li style=&quot;color:red;&quot;&gt;新增li节点&lt;/li&gt;&#39;)，在最后一个li后面新增一个li（索引为负数表示从最后计起，-1表示最后一个）<br>
						table.append(&#39;&lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，省略第一个参数表示在第一个子对象后面新增节点
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function replace(index,html){}</span><br>
						用html生成的节点替代调用对象的指定子对象对应的原生DOM节点，index是相对位移<br>
						table.replace(2,&#39;&lt;tr&gt;&lt;td&gt;新增行&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，新增一行，并用其替换掉第二行<br>
						table.replace(-1,&#39;&lt;tr&gt;&lt;td&gt;新增行&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;/tr&gt;&#39;)，新增一行，并用其替换掉最后一行（索引为负数表示从最后计起，-1表示最后一个）
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#8f5b00;font-weight:bold">function clean(index){}</span><br>
						该方法用于删除兄弟节点，index是相对于调用该方法的对象所处位置的位移<br>
						table.clean(),index省略表示删除第一个子对象，这里删除第一行等同于table.map(1).del(0)<br>
						table.clean(2),这里表示删除第三行<br>
						table.clean(-2),这里表示删除最后一行<br>
						table.map(0).clean([0,-1,1]),如果index是一个数组，那么就是删除指定索引的子对象，索引为负数表示从最后计起，-1表示最后一个，这里删除第一个，第二个以及最后一个th<br>
						table.map(0).clean(0,-1)，如果有两个参数，表示删除指定区间的子对象，索引为负数表示从最后计起，-1表示最后一个，这里删除第一个至后一个元素，参数大的可以作为第一个参数，大小顺序没有限制<br>
					</dd>
					<dd style="margin-left:28px;margin-bottom:8px">
					    <span style="color:#105cb6;font-weight:bold">如果新树的根节点是table，而其子节点是tbody/thead/tfoot，由于我们更多时候不会操作这些节点，而是直接操作tr，所以我做了一个允许略过这些节点的处理。当然你如果想操作tbody也是可以的，你大可以这样传参['tbody thead tfoot','tr','td']；只想取其中一个的话可以['tbody','tr','td']；直接取tr的话，可以['tr','td']，这种情况会对tbody/thead/tfoot中的所有tr生成新对象</span>
					</dd>
				</dl>
			</li>
</ul>
</div>
<div>
<h6 style="font-size:12px;color:#096598;font-weight:normal;margin:0px">结束语：回过头想想，有了这么一个插件之后，节点操作不就是小菜一碟，之前提到的操作节点的三大烦恼就迎刃而解了吧。而且这个插件与任何逻辑无关，不需要二次加工，拿起来就可以用了，遇到无法满足需求的时候还可以对其进行扩展。想象有一天开发流程好像是拼图游戏，开发好的插件经过组合，一个项目就出来，那是多么美妙的事啊。可能结果并没有预期的美好，但是可以预料得到，向着这个方向走，事件变得越来越简单是必然的。因为篇幅太长了，源码部分会在下次发表文章的时候进行详细的解讲。</h6>
</div>
</div><img src="http://www.cnblogs.com/jkit/aggbug/1902624.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/jkit/">jkit</a> 发表于 2010-12-10 20:09 <a href="http://www.cnblogs.com/jkit/archive/2010/12/10/1902624.html">原文链接</a></p><p>评论: 6　<a href="http://www.cnblogs.com/jkit/archive/2010/12/10/1902624.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/jkit/archive/2010/12/10/1902624.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/84306/">PHP 5.3.4 发布</a><span style="color:gray">(2010-12-11 18:31)</span><br>· <a href="http://news.cnblogs.com/n/84305/">蔡文胜购得的t.cn域名已经转至新浪网名下</a><span style="color:gray">(2010-12-11 18:23)</span><br>· <a href="http://news.cnblogs.com/n/84304/">分析称2011年WP7与Android相争 iPhone受益</a><span style="color:gray">(2010-12-11 17:38)</span><br>· <a href="http://news.cnblogs.com/n/84303/">黑莓在Verizon智能手机销售中所占比例降至20%</a><span style="color:gray">(2010-12-11 17:35)</span><br>· <a href="http://news.cnblogs.com/n/84302/">Groupon创始人Andrew Mason接受访谈</a><span style="color:gray">(2010-12-11 17:14)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/wsky/archive/2010/12/11/1902969.html">Velocity China 2010大会回顾</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>