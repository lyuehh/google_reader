---
layout: post
title:  "newLISP你也行 --- newLISP简介"
date:   2012-04-05 16:09:48
author: address-withheld@my.opera.com.invalid (F0)
categories: program
---

## newLISP你也行 --- newLISP简介
### by address-withheld@my.opera.com.invalid (F0)
### at 2012-04-05 16:09:48
### original <http://my.opera.com/freewinger/blog/show.dml/44438562>

<span>  <span>#############################################################################</span><br>
  <span># </span><span>Name:newLISP你也行</span><span> </span><span>---</span><span> </span><span>newLISP简介</span><br>
  <span># </span><span>Author:黄登</span><span>(</span><span>winger</span><span>)</span><br>
  <span># </span><span>Project:http://code.google.com/p/newlisp-you-can-do</span><br>
  <span># </span><span>Gtalk:free.winger@gmail.com</span><br>
  <span># </span><span>Gtalk-Group:zen0code@appspot.com</span><br>
  <span># </span><span>Blog:http://my.opera.com/freewinger/blog/</span><br>
  <span># </span><span>QQ-Group:31138659</span><br>
  <span># 大道至简 </span><span>--</span><span> </span><span>newLISP</span><br>
  <span>#</span><br>
  <span># </span><span>Copyright</span><span> </span><span>2012</span><span> 黄登</span><span>(</span><span>winger</span><span>)</span><span> </span><span>All</span><span> </span><span>rights</span><span> </span><span>reserved.</span><br>
  <span># </span><span>Permission</span><span> </span><span>is</span><span> </span><span>granted</span><span> </span><span>to</span><span> </span><span>copy,</span><span> </span><span>distribute</span><span> </span><span>and/or</span><br>
  <span># </span><span>modify</span><span> </span><span>this</span><span> </span><span>document</span><span> </span><span>under</span><span> </span><span>the</span><span> </span><span>terms</span><span> </span><span>of</span><span> </span><span>the</span><span> </span><span>GNU</span><span> </span><span>Free</span><span> </span><span>Documentation</span><span> </span><span>License,</span><br>
  <span># </span><span>Version</span><span> </span><span>1.2</span><span> </span><span>or</span><span> </span><span>any</span><span> </span><span>later</span><span> </span><span>version</span><span> </span><span>published</span><span> </span><span>by</span><span> </span><span>the</span><span> </span><span>Free</span><span> </span><span>Software</span><span> </span><span>Foundation</span><span>;</span><br>
  <span># </span><span>with</span><span> </span><span>no</span><span> </span><span>Invariant</span><span> </span><span>Sections,</span><span> </span><span>no</span><span> </span><span>Front-Cover</span><span> </span><span>Texts,and</span><span> </span><span>no</span><span> </span><span>Back-Cover</span><span> </span><span>Texts.</span><br>
  <span>#############################################################################</span><br>
  <br>
  <span>一 生命短暂</span><br>
  <span>    </span><span>---我用newLISP.</span><br>
  <br>
  <br>
  <span>    本系列文章以官方</span><span>introduction-to-newlisp.pdf为大纲,请相互结合学习.</span><br>
  <span>    如果你是以下几种人请尝试</span><span>newLISP.</span><br>
  <br>
  <span>    </span><span>1:希望体验编程快感的人.</span><br>
  <span>    </span><span>2:喜欢简洁的人.</span><br>
  <span>    </span><span>3:喜欢高效的人.</span><br>
  <span>    </span><span>4:喜欢自由的人.</span><br>
  <span>    </span><span>5:喜欢突破的人.</span><br>
  <span>    </span><span>6:实干主义者.</span><br>
  <span>    </span><span>7:编码狂人,键盘破坏者.</span><br>
  <span>    </span><span>8:希望找一门终身使用的语言.</span><br>
  <span>    </span><span>9:珍惜时间的人.</span><br>
  <span>    </span><span>10:珍惜生命的人.</span><br>
  <br>
  <span>    如果你是以下几种人请速度尝试</span><span>newLISP.</span><br>
  <br>
  <span>    </span><span>1:没有任何编程经验,却想学习编程的人.</span><br>
  <span>    </span><span>2:讨厌繁杂呆板语法和高深教条的人.</span><br>
  <span>    </span><span>3:对编程绝望的人.</span><br>
  <span>    </span><span>4:想学lisp却始终无法入门的人.</span><br>
  <span>    </span><span>5:初学完lisp,却不知道lisp能做什么的人.</span><br>
  <span>    </span><span>6:想无痛体验lisp思想的人.</span><br>
  <span>    </span><span>7:想使用lisp工作的人.</span><br>
  <span>    </span><span>8:至今都找不到适合自己的语言的人.</span><br>
  <span>    </span><span>9:想找一门简单强大语言的人.</span><br>
  <span>    </span><span>10:不堪忍受复杂细节的人.</span><br>
  <br>
  <br>
  <br>
  <span>    </span><span>newLISP</span><span> 将和你以前看到的别的</span><span>LISP会有很大的差别,所以请将他看成一门全新的语</span><br>
  <span>言</span><span>.newLISP的最大特点就是自然简洁为应用而生.你不会看到太多的</span><span> </span><span>&quot;高级&quot;</span><span> 语法</span><span>,当然更</span><br>
  <span>不会局限于各种晦涩难懂的教条</span><span>^_^.我发誓当你真正使用newLISP开始工作的时候,你会发</span><br>
  <span>现这是你学过的最简单的语言</span><span>!</span><br>
  <br>
  <span>    如果你有过编程或者脚本编写的经验</span><span>,你就会发现newLISP是一个简单易学,功能强大</span><br>
  <span>的脚本语言</span><span>.既具有传统LISP的优雅,又具有强大实用性:比如正则表达式,网络通信,</span><br>
  <span>Unicode支持,多任务,等等.</span><br>
  <br>
  <span>    如果你没有过编程经验</span><span>,那就更好了.现在就开始newLISP之旅吧,你一定会不虚此行.</span><br>
  <br>
  <br>
  <span>二</span><span>.序</span><br>
  <br>
  <span>    经过半个世纪的发展</span><span>,LISP已经成为了一个语系.无论这些LISP方言之间有多少差别,</span><br>
  <span>他们都遵循了一个至理</span><span>---</span><span>&quot;</span><span>All</span><span> </span><span>is</span><span> </span><span>Data</span><span>&quot;</span><span>.</span><br>
  <br>
  <span>    当然</span><span>newLISP不仅实现了LISP的核心lists,symbols,和lambda表达式.还加入了数组</span><br>
  <span>(</span><span>arrays</span><span>)</span><span>,隐式索引</span><span>(</span><span>implicit</span><span> </span><span>indexing</span><span> </span><span>on</span><span> </span><span>lists</span><span> </span><span>and</span><span> </span><span>arrays</span><span>)</span><span>,动态作用域和词法作用</span><br>
  <span>域</span><span>(</span><span>dynamic</span><span>  </span><span>and</span><span> </span><span>lexical</span><span> </span><span>scoping</span><span>)</span><span>.</span><br>
  <br>
  <span>    毫无疑问</span><span>newLISP是如今最简洁的LISP,同样也是最易学的LISP.他比Scheme实现起来</span><br>
  <span>都小</span><span>.拥有350个内建函数,不超过260k的大小.内部实现使用大多数UNIX系统中常用的C库</span><br>
  <span>函数</span><span>.加载速度快,内存消耗小</span><span>(</span><span>60k</span><span>)</span><span>.newLISP比别的流行脚本语言</span><span>(</span><span>python...</span><span>)</span><span>更快</span><span>,消耗</span><br>
  <span>更小</span><span>.</span><br>
  <br>
  <span>    </span><span>newLISP使用dynamically</span><span> </span><span>scope</span><span>(</span><span>动态作用域</span><span>)</span><span>,同时用context</span><span>(</span><span>命名空间</span><span>)</span><span>完成词法作</span><br>
<span><span>- </span><span>用域</span><span>.context的作用很多.</span><span>(</span><span>1</span><span>)</span><span> 模块话编程</span><span>,</span><span>(</span><span>2</span><span>)</span><span> </span><span>FOOP</span><span>(</span><span>Functional</span><span> </span><span>Object</span><span> </span><span>Oriented</span><span> </span><span>Prog</span></span><br>
<span>  <span>ramming</span><span>)</span><span>,</span><span>(</span><span>3</span><span>)</span><span> 定义保留状态的函数 </span><span>(</span><span>4</span><span>)</span><span>为关联键值创建</span><span>hash树.</span><br></span>
  <br>
  <span>    更多的功能细节请参看手册中的</span><span>introduction章节:强大的N级嵌套列表操作,方便的</span><br>
<span><span>- </span><span>网络函数</span><span>(</span><span>IPV6,IPV4</span><span>)</span><span>,64系统位支持,PCRE</span><span>(</span><span>Perl兼容正则表达式</span><span>)</span><span>,import</span><span>(</span><span>轻松导入任何</span></span><br>
<span>  <span>共享库</span><span>)</span><span>,原生的分布式支持,GUI-SERVER跨平台界面库</span><span>(</span><span>也可以使用</span><span>GTK</span><span> </span><span>Tcl/Tk</span><span> 和</span><span>OpenGL</span><span>)</span><br></span>
  <span>.</span><br>
  <br>
  <br>
  <span>三</span><span>.资源</span><br>
  <br>
  <span>    官网 </span><span><a href="http://www.newlisp.org">http://www.newlisp.org</a></span><span> 东西非常全</span><span>,如果要老版本去SourceForge上,从v6到</span><br>
  <span>现在的都有</span><span>.</span><br>
  <br>
  <span>    论坛 </span><span><a href="http://newlispfanclub.alh.net/">http://newlispfanclub.alh.net/</a></span><span> 大部分讨论</span><span>,更新,问题,都在这里发布.</span><br>
  <br>
  <span>    </span><span>21分钟教程</span><span> </span><span><a href="http://newlispfanclub.alh.net/org/newlisp-in-21-minutes.html">http://newlispfanclub.alh.net/org/newlisp-in-21-minutes.html</a></span><br>
  <br>
  <span>    中文版的</span><span>&lt;&lt;newLISP代码模式&gt;&gt;</span><br>
  <span>    </span><span><a href="http://www.newlisp.org/CodePatterns-cn.html">http://www.newlisp.org/CodePatterns-cn.html</a></span><span> </span><br>
  <br>
  <br>
  <span>    作者邮箱 </span><span>cormullion</span><span> </span><span>-</span><span> </span><span>at</span><span> </span><span>-</span><span> </span><span>mac.com.</span><br>
  <br>
  <span>    项目 </span><span>github.com</span><span> 和 </span><span>googlecode.com</span><span> </span><span>(</span><span>因为家里无网更新</span><span>,所以只能用google了555</span><span>)</span><br>
  <br>
  <span>    我的</span><span>BLOG</span><span> </span><span><a href="http://my.opera.com/freewinger/blog/">http://my.opera.com/freewinger/blog/</a></span><span> 有时间就更新点教程或者更新下</span><br>
  <span>项目</span><span>,毕竟不能天天上.</span><br>
  <br>
  <span>    </span><span>QQ群:31138659</span><br>
  <br>
  <span>    </span><span>Gtalk群:zen0code@appspot.com</span><br>
  <br>
  <span>四</span><span>.开发工具</span><br>
  <br>
  <span>    </span><span>newLISP-Edit</span><br>
  <span>    </span><span>newLISP自带的,使用gui-server编写</span><span>(</span><span>如果你是</span><span>WIN32用户,要先安装jre</span><span>)</span><span>.在安装完</span><br>
  <span>newlisp以后,在桌面上会看到一个蜻蜓的图标名字叫newLISP-GS.然后右键点击图标-属性</span><br>
  <span>把目标改成下面的内容</span><span>:</span><br>
  <br>
  <span>java</span><span> </span><span>-jar</span><span> </span><span>&quot;C:\Program Files\newlisp\guiserver.jar&quot;</span><span> </span><span>47011</span><span> </span><span>newlisp-edit.lsp</span><br>
  <br>
  <span>然后点击图标就能看到</span><span>IDE了.</span><br>
  <br>
  <span>    </span><span>ScitefornewLISP</span><br>
  <span>    </span><span><a href="http://code.google.com/p/scite-for-newlisp">http://code.google.com/p/scite-for-newlisp</a></span><br>
  <span>    这个是我自己用的</span><span>scite组装的,具备了关键字高亮,自动提示,自动完成,括号和双引</span><br>
<span><span>- </span><span>号自动完成</span><span>,还有就是可以很方便的使用gui-server</span><span>(</span><span>解决了路径问题</span><span>,具体打开一个lsp文</span></span><br>
<span>  <span>件看工具菜单</span><span>)</span><span>.如果不喜欢语法提示的可以找到newlisp.properties文件,把下面这行用#</span><br></span>
  <span>注释掉</span><span>.</span><br>
  <br>
  <span>api.*.lsp=$</span><span>(</span><span>SciteDefaultHome</span><span>)</span><span>/api/newlisp.api</span><br>
  <span>改成</span><br>
  <span>#a</span><span>pi.*.lsp=$</span><span>(</span><span>SciteDefaultHome</span><span>)</span><span>/api/newlisp.api</span><br>
  <br>
  <br>
  <span>    其他任意的工具都可以</span><span>,官方也提供了别的很多IDE的配置文件</span><span>(</span><span>VIM</span><span> </span><span>EMACS</span><span> </span><span>JOE..</span><span>)</span><span>.如</span><br>
<span><span>- </span><span>果有你习惯的编辑器就可以直接用了</span><span>,没有的话就用我提供的scite</span><span>(</span><span>跨平台</span><span>,免费,小巧,方</span></span><br>
<span>  <span>便扩展</span><span>)</span><span>.<a href="http://newlisp.org/index.cgi?Code_Contributions">http://newlisp.org/index.cgi?Code_Contributions</a>.</span><br></span>
  <br>
  <span>    本系列教程的配色都使用</span><span>scitefornewlisp的配置,txt版本请使用scitefornewlisp.</span><br>
  <br>
  <span>    </span><span>2012-04-01</span><span> </span><span>22:44:58</span></span>

html 彩色版本请看 <a href="http://code.google.com/p/newlisp-you-can-do">http://code.google.com/p/newlisp-you-can-do</a>