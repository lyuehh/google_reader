---
layout: post
title:  "现实世界的LISP：Clojure语言初探"
date:   2013-02-03 14:34:29
author: baiyuzhong
categories: program
---

## 现实世界的LISP：Clojure语言初探
### by baiyuzhong
### at 2013-02-03 14:34:29
### original <http://www.programmer.com.cn/14719/>

<p><span style="color:#888888"><strong>文/<a href="http://weibo.com/lazyseq?from=profile&amp;wvr=5&amp;loc=infdomain">江宏</a></strong></span></p>
<p>Clojure由于兼具LISP高效、可扩展的特性，同时又能利用Java的生态圈，在短时间内得到广泛传播，并在一些互联网公司得到应用。</p>
<div>
<p>我在学生时代最喜欢两门程序设计语言：Scheme和Haskell。Scheme的简洁灵活和Haskell的纯函数世界都给我留下了 深刻印象，因此一直希望能用这样的语言做一些实际工作。了解到Clojure之后，欣喜地发现它结合了LISP和函数式语言的优点，同时又拥有JVM成熟 的生态圈，虽然年轻，但可以立即在实际项目中应用。<span></span></p>
<p><strong>开始使用Clojure</strong></p>
<p>使用Clojure最简便的方法是安装Leiningen。它是Clojure的项目管理工具，作用类似于Java的Maven。只需要下载名为lein的 Shell脚本，并在命令行中输入lein repl，Leiningen就会下载Clojure及它所依赖的.jar库文件，之后就会看到类似下面的提示：</p>
<p><img title="代码1" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%811.jpg" alt="" width="595" height="257"></p>
<p>接着输入Clojure表达式，便能看到结果：</p>
<p><img title="代码2" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%812.jpg" alt="" width="157" height="42"></p>
<p><strong>Clojure语法</strong></p>
<p>Clojure是LISP家族的一门语言，这意味着在语法上，Clojure和其他的LISP语言非常相似。例如(+ 1 2)这个Clojure表达式， 在其他编程语言里一般写为1 + 2。</p>
<p>LISP 最著名的特性是它的代码形式和数据表示形式一致，并有一个专用词homoiconic描述这种特性。例如上面的(+ 1 2)，如果看作数据，它就是一个普通的列表。在求值时，Clojure的运行环境会把列表的第一个元素作为操作或函数，而把后面的元素作为参数。如果在运 行时不希望把一个列表作为表达式对待，可以在前面加’，例如’(+ 1 2)。这意味着Clojure程序可以像操作数据一样修改和生成其他Clojure程序。这样改变程序结构的程序在LISP语言中称为宏。宏系统使得 LISP家族的语言具有高度可扩展性。使用者可以用宏来定义更高层的领域特定语言，使得用来描述和解决问题的语言更加接近问题本身的领域。</p>
<p>与其他LISP语言相比，Clojure提供了更为丰富的原生数据结构，例如下面这几种。</p>
<p>【vector】</p>
<p><img title="代码3" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%813.jpg" alt="" width="115" height="53"></p>
<p>这里的a、b、c是Clojure的符号（Symbol）。每个符号代表运行环境里的某个对象，与其他语言里的标识符（Identifier）概念类似。</p>
<p>【map】</p>
<p><img title="代码4" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%814.jpg" alt="" width="127" height="26"></p>
<p>这里的:a和:b是键，而1和2是它们对应的值。:a和:b这样以:开头的表达式被称为关键字（Keyword）。它们和符号有些类似，但只代表它们自己， 而不代表任何其他对象。Ruby中也有类似概念（叫做Symbol，注意不要和Clojure的Symbol混淆）。关键字通常被用作map的键，但也可 以用其他类型做键。上面的map也可以写作{:a 1, :b 2}。逗号“,”没有实际意义，一般只为提高可读性。</p>
<p>【set】</p>
<p><img title="代码5" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%815.jpg" alt="" width="103" height="25"></p>
<p>当然，数据结构是可以嵌套的。Clojure并不要求一个vector、map或set里的元素是同一类型，也就是说下面的数据都是合法的：</p>
<p><img title="代码6" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%816.jpg" alt="" width="340" height="56"></p>
<p>与其他语言类似，不同的数据结构有不同的性能保证和特性。Clojure为数据结构提供了丰富和一致的操作，例如取元素：</p>
<p><img title="代码7" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%817.jpg" alt="" width="304" height="41"></p>
<p>分号;是Clojure的行注释符，后面的内容会被忽略，这里我用注释来说明对每一行求值的结果。很容易注意到，从容器中取元素在语法上和函数调用是一致的，因此也可以把一个容器看作一个函数，它能接受一个参数，返回所对应的值。 第二行也可以写成下面这样。</p>
<p><img title="代码8" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%818.jpg" alt="" width="228" height="36"></p>
<p>这是在用Keyword做map的key时为了方便而做的特殊处理，不适用于其他情况。</p>
<p>再例如添加元素：</p>
<p><img title="代码9" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%819.jpg" alt="" width="332" height="45"></p>
<p>注意用conj向数组和列表添加元素时，得到的结果不一样。因为conj会以最适合具体数据类型的方式操作——对数组来说向末尾添加元素最高效，对列表来说向头部添加元素最高效。使用这样的通用函数可以让我们定义其他高效通用函数。</p>
<p>前面说到符号可以代表运行环境中的对象。Clojure可以通过下面的语句把一个符号绑定到特定对象上：</p>
<p><img title="代码10" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8110.jpg" alt="" width="317" height="26"></p>
<p>与其他数据一样，函数也可以绑定到符号上。</p>
<p><img title="代码11" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8111.jpg" alt="" width="318" height="64"></p>
<p>这里(fn [name] …)定义了一个函数，这个函数有一个名为name的参数。这个函数被绑定到say-hello这个名称上。如果运行(say-hello “James”)，则会输出Hello James。</p>
<p>Clojure还有一个定义函数的简便方法。</p>
<p><img title="代码12" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8112.jpg" alt="" width="308" height="50"></p>
<p>除此之外，Clojure还提供了let用于局部绑定，在且仅在(let …)的范围内a为1，b为2。</p>
<p><img title="代码13" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8113.jpg" alt="" width="147" height="71"></p>
<p><strong>函数式程序设计</strong></p>
<p>什么样的语言可以算是函数式语言有很大争议。一方面，很多人认为LISP家族的语言都是函数式语言，仅是因为在LISP中函数可以像数据一样被作为参数和返 回值；另一方面，也有不少人认为函数式语言必须把输入输出等副作用排除在主程序之外，让程序的输出只依赖于输入。例如Haskell语言通过Monad抽 象机制在理论上达到了这样的效果。另外Haskell的延迟求值特性对于实现纯函数式语言也是必须的。我的一位老师Paul Hudak是Haskell的主要设计者之一，他曾多次说过LISP不是函数式语言。有些人更愿意把LISP称为符号语言（Symbolic Language），而把LISP代表的编程范型称为符号式程序设计（Symbolic Programming）。</p>
<p>Clojure采取了中间路线。既提供了不可变数据结构，鼓励函数式程序设计，也并不排斥副作用和非函数式的风格，这与Java实现方便的互操作，充分利用Java的强大生态圈非常重要。另外Clojure语言本身是默认立即求值的，但它也支持延迟求值的数据结构。</p>
<p>提供不可变的核心数据结构使得Clojure与传统LISP比更加偏向于函数式语言，函数的参数在逻辑上完全可以被作为值（而不是引用）对待，不会被函数改 变，而结果在逻辑上也是一个新的值。Clojure并不像Haskell一样对I/O做限制。函数体中的I/O不会反映到函数的类型上。</p>
<p><strong>高效的不可变数据结构</strong></p>
<p>前面说过Clojure的核心数据结构是不可变的，它通过提供不可变的数据结构来鼓励函数式编程。很多习惯于用传统过程式语言或面向对象语言的朋友不是很理解这一点。举个简单的例子，当你向数组的末尾加入一个元素时：</p>
<p>在逻辑上，一个包含这个新元素和所有旧元素的新数组被创建；</p>
<p>原来的数组（这不包含这个新元素）仍然可以被访问；</p>
<p>这个操作在时间复杂度上的保证和对应的可变数据结构是一致的（对于在数组末尾增加元素来说，平均复杂度应该接近于O(1)）。</p>
<p>要同时满足以上三个条件 ，显然Clojure不可能用类似于C/C++的方法实现数组。事实上，Clojure提供的大部分数据结构都是用树结构实现的。在原有对象基础上构造新 对象时，只需要复制必须改变的节点。数据可以被作为不可变的值对待，而同时各种操作又能相对比较高效地实现。</p>
<p><strong>动态语言</strong></p>
<p>LISP语言是动态语言的先驱。Clojure自然也是一门动态语言。所谓动态，体现在很多方面。首先Clojure使用动态类型系统，每个Symbol所指代的 值的类型是在运行时确定的。对一个函数来说，它只关心参数能接受某些操作，并不对具体类型做限制。例如(fn[a b](+ a b))这个函数，它只要求a和b必须能作为+的参数，而并不对其类型做限制。这样的动态类型系统称为Duck Typing，来源于美国诗人James Whitcomb Riley的话：</p>
<p>当我看到一只像鸭子一样游泳、像鸭子一样叫的鸟时，我就认为它是一只鸭子。</p>
<p>动态也体现在Symbol的绑定上。假设有一个函数some-op是这样的：</p>
<p><img title="代码14" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8114.jpg" alt="" width="200" height="66"></p>
<p>其中send-email是一个用来发送邮件的函数。它的单元测试中可能有下面的代码：</p>
<p><img title="代码15" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8115.jpg" alt="" width="322" height="52"></p>
<p>很显然，我们不能每运行一次测试都真的发送邮件，因此用binding把send-email动态绑定到一个空函数上来避免实际的效果。在测试中，动态绑定还可以用来验证特定函数的参数和结果。这样的特性使得Clojure的程序对测试非常友好，对开发大型系统很有帮助。</p>
<p><strong>与平台的结合</strong></p>
<p>目前Clojure存在多个平台的实现，包括JVM、CLR和JavaScript。为了充分利用各宿主平台的优势，Clojure的不同实现除了基本语法 一致之外，并不特别注意可移植性。与其他追求平台独立性的语言不同，Clojure更强调和宿主平台的无缝互操作，因此在Clojure里可以非常容易地 使用第三方Java、CLR或JavaScript库。这个特点使得Clojure可以充分地利用宿主平台的成熟生态圈，让它在发布不久后就显示出强大的 生命力。</p>
<p>以JVM上的实现为例，在Clojure里调用Java代码并不需要类似于FLI（Foreign Language Interface）之类的机制。例如下面的例子：</p>
<p><img title="代码16" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8116.jpg" alt="" width="481" height="93"></p>
<p>简单几行就展示了从Clojure调用Java代码的几种情况：对象方法、类（静态）方法/属性以及创建对象的两种方式。</p>
<p><strong>一个简单实例</strong></p>
<p>这里展示一个用Clojure做Web开发的简单例子，希望能让读者熟悉用Clojure做现实中的开发所需的知识。在开始一个新项目前，可以用Leiningen生成一个基本的项目框架：</p>
<p><img title="代码17" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8117.jpg" alt="" width="177" height="38"></p>
<p>进入hello目录后可以看到目录结构：</p>
<p style="text-align:center"><img src="http://ipad-cms.csdn.net/cms/attachment/201212/50bc08ece88dd.jpg" alt="" width="502" height="208"></p>
<p>project.clj是Leiningen的项目定义文件，主要包含项目所依赖的库以及插件的版本信息：</p>
<p><img title="代码19" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8119.jpg" alt="" width="629" height="123"></p>
<p>src目录是源码目录，test目录是测试目录。源码目录中只有一个文件src/hello/core.clj：</p>
<p><img title="代码20" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8120.jpg" alt="" width="509" height="93"></p>
<p>第一行(ns hello.core)声明了这个文件是模块hello.core。与Java、Python类似，Clojure的模块名称和文件名是存在对应关系的。 (defn foo后面有一个字符串”I don’t do a whole lot.”，这是一个可选的doc string，对foo起说明作用。</p>
<p>接下来，我们把Noir加到项目的依赖列表里。Noir是一个用来开发Web应用的轻量框架。</p>
<p><img title="代码21" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/%E4%BB%A3%E7%A0%8121.jpg" alt="" width="514" height="97"></p>
<p>然后把src/hello/core.clj的内容改为：</p>
<p><img title="22" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/22.jpg" alt="" width="402" height="169"></p>
<p>开头(ns …)里的(:use)和(:require)把我们用到的库引入到当前模块。defpage是Noir定义网页的方法，它非常像一个函数：输入是用户 的请求和参数，返回的是页面的内容。短短几行代码就完成一个简单的Web应用。下面用Leiningen运行：</p>
<p><img title="23" src="http://www.programmer.com.cn/wp-content/uploads/2013/02/23.jpg" alt="" width="253" height="47"></p>
<p>如果你用浏览器访问<a href="http://localhost:9999/World">http://localhost:9999/World</a>，就可以看到Hello World!。</p>
<p><strong>结语</strong></p>
<p>受篇幅所限，本文没有介绍Clojure的两个重要特色：丰富的并行运算机制，以及LISP语言最重要的特色之一“宏”。</p>
<p>有人说Clojure是为并行而设计的，然而它最吸引我的还是LISP语言的简洁、强大和可扩展性，以及Clojure对函数式程序设计的鼓励。一门程序设计语言可以通过提供更高层及更丰富的抽象来帮助用户更方便地描述问题和过程。然而这些抽象是基于人本身对问题和过程的理解，不能超越人的认知。例如 Clojure提供了STM（Software Transactional Memory，软件事务内存）来管理多个线程对共享资源的访问，它与其他语言中常见的基于锁的方案相比有很多优点。但如果一个程序员对STM的机制没有深 入了解就在Clojure程序中随意使用，很容易造成问题。STM会带来便利，但没有降低对使用者在对问题理解方面的要求。</p>
<p>Fred Brooks在《No Silver Bullet》中说，没有任何单一技术进步会使得软件开发效率大幅度提升。真正困难的问题并不是语言造成的，也不会因为某个语言提供了新的抽象而简单很 多。语言是一个工具，用户应该对它有合理的期望。好的工程师可以用任何语言实现高质量的软件，而一个不好的工程师也不会因为使用一门特定的语言在产出上有 很大提高。Clojure不是Silver Bullet，但它能在一定范围内提高程序设计的效率。你仍然需要自己分析和寻找问题的答案，但它可以让你在实现解决方案时更高效。学习和熟悉 Clojure采用的各项技术及倡导的编程风格，可以使用户成为更好的软件工程师，同样的技术和方法也可以应用到其他语言的实践中去。</p>
<p><span style="color:#888888"><strong>作者江宏，耶鲁大学计算机科学博士，2007–2010年在Google总部担任研发工程师，从事搜索系统基础架构的开发。之后回国创业，目前担任AVOS中国子公司负责人。</strong></span></p>
<p><span style="color:#888888"><strong>作者博客：<a href="http://www.hjiang.net/">http://www.hjiang.net/</a></strong></span></p>
<p><a href="http://www.programmer.com.cn/14159/"><strong>本文选自《程序员》杂志2012年12期，未经允许不得转载。如需转载请联系 market@csdn.net</strong></a></p>
<p><strong><a href="http://dingyue.programmer.com.cn/">《程序员》2012年杂志订阅送好礼活动火热进行中</a></strong></p>
</div>