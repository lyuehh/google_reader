---
layout: post
title:  "认识Dojo中的界面控件：Dijit"
date:   2010-09-16 13:20:09
author: 
categories: program
---

## 认识Dojo中的界面控件：Dijit
### by 
### at 2010-09-16 13:20:09
### original <http://www.javaeye.com/topic/764798>

看来JavaEye的群众不喜争论，更加倾向于潜心研究技术<img src="http://www.iteye.com/images/smiles/icon_idea.gif"> 
<br>翻译一篇，希望能帮助大家了解Dojo！同时欢迎讨论主题相关内容。
<br>
<br>本文翻译自：
<br>http://www.sitepen.com/blog/2010/07/12/dive-into-dijit/
<br>
<br>Dojo框架区别于其他Ajax框架的一个显著特征就是它的界面控件系统：Dijit。Dijit是Dojo中界面控件的总称，他们使用灵活而且易于理解。每个Dijit控件都是由Dojo类以及控件中使用的图片，CSS等资源文件共同组成。通过Dijit架构，可以方便的创建灵活、可扩展、可定制外观的控件。下面将逐步介绍如何创建、配置Dijit控件，从而将其用在自己的web应用中。
<br>
<br><img src="http://www.sitepen.com/blog/wp-content/uploads/2010/07/DijitBlogPost2.jpg">
<br>
<br><strong>开始使用Dijit：引入需要的类和资源</strong>
<br>前面提到Dijit的组成中除了Dojo类，还有图片和CSS。这些外观是通过Dojo框架的主题来提供的，Dojo包含了4个主题，分别是：nihilo, soria, tundra和claro，其中claro是最新的Dojo1.5新增加的一个外观主题。每个主题中都包含了用于定义控件外观的css和图片文件。因此，为了使用某一个主题，我们首先需要将其导入到Web页面中：
<br>
<br><pre name="code">&lt;style type=&quot;text/css&quot;&gt;&lt;!--  
        /* use the tundra theme */  
        @import &quot;http://ajax.googleapis.com/ajax/libs/dojo/1.5/dijit/themes/tundra/tundra.css&quot;;  
        /* Note that if you don’t specify a minor revision, e.g. 1.5.0 or 1.4.3, the CDN will deliver the latest version */  
&lt;/style&gt;</pre>  
<br>
<br>当然，这个地址你也可以用你自己的： @import "dojoroot/dijit/themes/tundra/tundra.css";
<br>
<br>你可以用Dijit的主题查看器 来查看每个主题的外观，本地也可以访问到这个主题查看器：&lt;dojoroot&gt;/dijit/themes/themeTester.html，你也可以定义自己的主题。
<br>
<br>除了引入样式文件，当然还要引入Dojo核心本身，和其他类库一样，需要引入一个js文件。但Dojo中可以在引用js文件的同时对其进行参数配置。这里有一个比较重要的就是parseOnLoad参数。这个参数使得在页面载入完成之后自动解析页面上的所有Dijit控件。因此，引入Dojo核心库的代码如下：
<br>
<br><pre name="code">&lt;script type=&quot;text/javascript&quot; src=&quot;http://ajax.googleapis.com/ajax/libs/dojo/1.4/dojo/dojo.xd.js.uncompressed.js&quot; djConfig=&quot;parseOnLoad:true&quot;&gt;&lt;/script&gt;  </pre>
<br>
<br>同理，这里的dojo.js文件你也可以直接使用本地文件。
<br>
<br>引入Dojo核心之后，你还需要引入你所用到的控件的代码文件，这是通过dojo.require语句完成的。例如，你需要在页面上使用一个按钮控件：dijit.form.Button，那么你就需要在页面载入的最开始加入：dojo.require('dijit.form.Button')。这个和java的import语句或者.net中using语句的作用是类似的。
<br>
<br>准备工作完成之后，下面我们来看如何使用dojo控件。
<br>
<br><strong>创建Dijit的两种方式</strong>
<br>在Dojo中创建Dijit可以有两种方式，这也是Dijit系统最显著的特性之一：
<br>
<br>通过HTML标记以声明方式创建
<br>通过javascript程序动态创建
<br>
<br>例如，一个基本的下拉框通过如下HTML表示：
<br>
<br><pre name="code">&lt;select name=&quot;players&quot; id=&quot;players&quot;&gt;  
        &lt;option value=&quot;&quot;&gt;Select an Arsenal Player&lt;/option&gt;  
        &lt;option value=&quot;Arshavin&quot; selected=&quot;selected&quot;&gt;Andrey Arshavin&lt;/option&gt;  
        &lt;option value=&quot;Vermaelen&quot;&gt;Thomas Vermaelen&lt;/option&gt;  
        &lt;!– more OPTION elements here –&gt;  
&lt;/select&gt; </pre> 
<br>
<br>
<br>这是一个包括多个选项的静态下拉框，那么现在我们想让其支持实时过滤：即下拉框选项会根据你输入的关键字被过滤，缩小选项范围。在Dijit中有相应的控件来实现这个特性：dijit.form.FilteringSelect. 因此，我们首先需要引入这个控件：
<br>
<br><pre name="code">dojo.require(‘dijit.form.FilteringSelect’);  </pre>
<br>
<br>
<br>引入控件之后，我们很容易将一个静态的下拉框转变为Dijit控件。
<br>
<br>通过声明方式创建Dijit
<br>这种方式直接在原来的Html标记里加上Dijit相关的属性：
<br>
<br><pre name="code">&lt;select name=&quot;players&quot; id=&quot;players&quot; dojoType=&quot;dijit.form.FilteringSelect&quot; autoComplete=&quot;true&quot; pageSize=&quot;10&quot;&gt;  
        &lt;option value=&quot;&quot;&gt;Select an Arsenal Player&lt;/option&gt;  
        &lt;option value=&quot;Arshavin&quot; selected=&quot;selected&quot;&gt;Andrey Arshavin&lt;/option&gt;  
        &lt;option value=&quot;Vermaelen&quot;&gt;Thomas Vermaelen&lt;/option&gt;  
        &lt;!– more OPTION elements here –&gt;  
&lt;/select&gt; </pre> 
<br>
<br>
<br>这是用声明方式来创建dijit的一个简单例子，dojoType属性告诉dojo一个给定的元素需要转换为哪个dijit。前面我们已经提到在引入Dojo核心库时可以对其进行一些参数设置，parseOnLoad就是这样一个配置属性，设为true之后，Dojo就会在页面载入完成之后立刻寻找所有带有dojoType属性的节点，并将其转换为相应的Dijit控件。
<br>
<br>在下拉框这个例子中，所有静态声明出来的选项（option）都会被新的FilteringSelect识别，除此之外，还可以在声明的时候对dijit的一些参数进行配置，例如：autoComplete = "true"就设置了自动完成功能，pageSize = "10"设置了每页最多显示10个结果。
<br>
<br>由此可见，FilteringSelect功能是在原有的select功能基础上增加了新的功能。但没有改变它是一个下拉框的本质。
<br>
<br>通过编程方式创建Dijit
<br>编程的方式就是通过javascript代码来创建Dijit控件。下面的代码创建了dojo中自带的一些常用表单元素：
<br>
<br><pre name="code">//require the class  
dojo.require(‘dijit.form.FilteringSelect’);  
//when the class has been loaded…  
dojo.ready(function() {  
        //dijitize my SELECT!  
        var enhancedSelect = new dijit.Form.FilteringSelect({  
                autoComplete: true,  
                pageSize: 10  
        },‘players’);  
        //dijitize any form field individually  
        var inputDijit = new dijit.form.Textbox({/*options*/},‘myInput’);  
        var textareaDijit = new dijit.form.Textarea({/*options*/},‘myTextarea’);  
        var mySelectDijit = new dijit.form.FilteringSelect({/*options*/},‘mySelect’);  
        var dateDijit = new dijit.form.DateTextBox({/*options*/},‘myDate’);  
        var timeDijit = new dijit.form.TimeTextBox({/*options*/},‘myTime’);  
        var checkboxDijit = new dijit.form.CheckBox({/*options*/},‘myCheckbox’);  
        var radioDijit1 = new dijit.form.RadioButton({/*options*/},‘myRadio1′);  
        var radioDijit2 = new dijit.form.RadioButton({/*options*/},‘myRadio2′);  
});</pre>  
<br>
<br>可以看到，创建一个dijit需要两个参数：
<br>第一个参数是一个javascript对象，这个对象的每一个属性都将成为要创建的dijit的属性。
<br>第二个参数指定了dom节点，可以是节点id也可以是实际的节点。这就指定了要创建的dijit在页面的位置。如果不指定此参数，那么会自动创建一个独立dom节点来对应这个dijit，由开发者决定何时将其加入到页面的什么位置。
<br>
<br>注意，这里编程的方式创建就和parseOnLoad属性没关系了，无论设为true或者false，都会按照程序逻辑创建需要的Dijit控件。
<br>
<br>有时你需要创建一个包含许多元素的大表单，但又不想一个个创建，那么下面的方式可以让你用css3选择器指定节点，并将这些节点统一转换为Dijit控件，这是另外一种对标准Html进行Dijit化的方法，同时结合了静态声明和动态创建。这是通过dojo.behavior工具类来完成的：
<br>
<br><pre name="code">//require the class  
dojo.require(‘dojo.behavior’);  
//when it has loaded…  
dojo.ready(function(){  
        //add two behaviors:  
        dojo.behavior.add({  
                //find SELECT elements, make them a FilteringSelect Dijit  
                ’select’: {  
                        found: function(item) {  
                                new dijit.form.FilteringSelect({/*options*/},item);  
                        }  
                },  
                //find all radio buttons, make them dijit.form.RadioButton Dijits  
                ‘input[type=&quot;radio&quot;]‘: {  
                        found: function(item) {  
                                new dijit.form.RadioButton({  },item);  
                        }  
                }/* , 
                        more selector =&gt; Dijit creation…. 
                */  
        });  
});  
</pre>
<br>
<br>当然，你也可以直接通过dojo.query查找到需要的元素，再手动创建这些节点，这个和上面的代码可以实现同样的功能，但dojo.behavior提供了更简洁的语法。
<br>
<br>下图显示了创建的表单：
<br><img src="http://www.sitepen.com/blog/wp-content/uploads/2010/07/DijitBlogPost4.jpg">
<br>
<br><strong>访问Dijit控件和Dijit控件的属性</strong>
<br>要获取某一个特定dom节点，可以方便的用dojo.byId(id)方法获取。Dijit也提供了类似的方法，dijit.byId(id)可以获取指定id的节点对应的dijit控件：如果创建Dijit控件时使用的Dom节点有id属性，那么这个id就是Dijit控件的id；如果Dom节点没有指定id，那么将会自动为Dijit控件产生一个唯一id。
<br>例如，如果我们要应用通过声明方式创建的一个Dijit控件，而且这个Dijit控件对应的Dom节点具有id='players'这个属性，那么可以用如下代码获得这个Dijit控件的引用：
<br>
<br><pre name="code">//grab the players dijit.form.Filtering widget  
var enhancedSelect = dijit.byId(‘players’);  </pre>
<br>
<br>
<br>可以通过Firebug查看Dijit控件的所有属性：
<br><img src="http://www.sitepen.com/blog/wp-content/uploads/2010/07/DijitBlogPost1.jpg">
<br>
<br>如果想要获取Dijit控件的某个属性，可以用如下代码：
<br><pre name="code">
var pageSize = dijit.byId(‘players’).attr(‘pageSize’); // returns 10  
</pre>
<br>
<br>如果要设置Dijit控件的某个属性，可以用如下代码：
<br>
<br><pre name="code">dijit.byId(‘players’).attr(‘pageSize’,20); //now pageSize is 20</pre>  
<br>
<br>
<br>可见，attr是所有Dijit控件都有的方法，当只有一个参数时，返回参数指定属性的值，当有2个参数时，将第二个参数赋值给参数一指定的参数。
<br>需要注意的是：dojo 1.5使用了一种新的方式来使用dijit控件的属性：get和set，这使得对dijit控件属性的访问更加直观。功能上和attr方法完全相同，分别对应单参数和双参数功能
<br>
<br>Dijit控件的事件绑定
<br>Dijit控件使用Dojo的connect方法来实现事件的绑定，例如：
<br>
<br><pre name="code">dojo.connect(dijit.byId(‘player’),‘onChange’,function() {  
        //the user has selected a new item  
        //do something  
}); </pre> 
<br>
<br>
<br>每个Dijit控件都有多个事件，但因为javascript的特殊性，即使绑定了一个不存在的事件，也不会报错。因此，我们需要在使用事件之前查找API文档，以确保他们的存在。
<br>
<br><strong>Dijit控件集</strong>
<br>Dijit是一个非常优秀的UI控件库，熟练的使用它们可以快速开发出功能强大的Ajax网站。在Dijit库中有大量的功能稳定的控件：
<br>
<br>上下文菜单，弹出菜单，按钮式菜单
<br>改进的基本表单元素：按钮，下拉框，复选款，单选按钮，文本框，等等。
<br>日期和时间选择控件
<br>所见即所得富文本编辑器
<br>水平和垂直滑动块
<br>进度条
<br>选项卡、手风琴控件
<br>支持拖放功能的树结构
<br>对话框，提示框
<br>支持调整大小的布局控件
<br>如果在基本的dijit库中没有你需要的控件，dojox（dojo扩展）是另外一个提供Dijit控件的地方，其中有更大量的控件可供选择。
<br>如果想要查看Dijit控件的功能和样式，可以使用Dojo的主题查看器来浏览。
<br>
<br><strong>Dijit资源</strong>
<br>下面的链接可以帮助你快速的学习和使用Dijit：
<br>
<br>Dijit API Documentation: <a href="http://www.dojotoolkit.org/api/dijit.html">http://www.dojotoolkit.org/api/dijit.html</a>
<br>Dijit Reference Guide: <a href="http://www.dojotoolkit.org/reference-guide/dijit/index.html#dijit-index">http://www.dojotoolkit.org/reference-guide/dijit/index.html#dijit-index</a>
<br>Dojo Nightlies (test files): <a href="http://archive.dojotoolkit.org/nightly/dojotoolkit/dijit/tests/">http://archive.dojotoolkit.org/nightly/dojotoolkit/dijit/tests/</a>
<br>Dijit Theme Tester: <a href="http://archive.dojotoolkit.org/nightly/dojotoolkit/dijit/themes/themeTester.html">http://archive.dojotoolkit.org/nightly/dojotoolkit/dijit/themes/themeTester.html</a>
<br>
<br>
<br>
<br><strong>小结</strong>
<br>本文简要介绍了Dojo中的界面系统：Dijit。将来我们会提供更深入介绍Dijit控件的文章，欢迎大家保持关注！
<br>
<br>
<br>
          
          <br><br>
          作者: <a href="http://dojotoolkit.javaeye.com">dojotoolkit</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/764798" style="color:red">已有 <strong>16</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>