---
layout: post
title:  "Spring Web 架构剖析（三）- Spring Web MVC工作流"
date:   2010-09-01 21:32:57
author: 
categories: program
---

## Spring Web 架构剖析（三）- Spring Web MVC工作流
### by 
### at 2010-09-01 21:32:57
### original <http://www.javaeye.com/topic/753034>

<h1>3. Spring Web MVC<span><span><span style="font-size:x-large">工作流</span>
</span>
</span>
 </h1>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>这一章，我们将描述</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>的各个组件，组件接口，以及各个组件之间如何协调通信，进而理解</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>框架的基本工作原理。</span>
</span>
<span style="font-size:small"> <span style="font-size:x-small"> </span>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
</span>
</p>
<h2>
<span>3.1 组件以及组件的接口</span>
</h2>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>是由若干组件组成的，这些组件相互独立又相互协调工作共同完成</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>工作流。每个组件都有清晰的接口定义，接口后面都有一个设计良好的类实现体系结构，清晰的抽象出公用的逻辑并且实现在通用的抽象类里，同时提供常用的具体实现类。</span>
<span style="font-family:Times New Roman"> </span>
<span>进而实现一个清晰的，高可扩展的，可插拔的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Web MVC</span>
</span>
<span>体系结构。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span><span style="font-size:small">下面我们介绍这些典型的组件，组件的功能以及组件所定义的接口。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h3>
</h3>
<h6>
<span style="font-family:Wingdings"><span><span style="font-size:small">l</span>
<span style="font-family:&#39;Times New Roman&#39;">  </span>
</span>
</span>
<span style="font-size:small"><span>派遣器处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
</span>
</h6>
<h3>
<span style="font-size:small"><span lang="EN-US">
</span>
</span>
</h3>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>派遣器处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
<span>是</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>中最核心组件，它是一个实现类而并不是一个接口。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>从类的继承角度来看，派遣器处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
<span>最终继承自</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServlet</span>
</span>
<span>对象。通过若干个抽象类的划分，是派遣器处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
<span>的类体系接口清晰明了，任务分明，容易扩展，我们将在后续章节详细分析派遣器处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
<span>的类体系接口。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>初始化时，</span>
<span style="font-family:Times New Roman"> </span>
<span>它通过内部的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web</span>
</span>
<span>应用程序环境，找到相应的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>的各个组件，如果这些组件没有显式配置，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>将会根据默认加载策略初始化各个模块的默认实现。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>服务时，它通过一套注册的处理器映射对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerMapping)</span>
</span>
<span>找到一个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>，然后，从一套注册的处理器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）中找到一个支持这个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>的处理器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>），并且通过它把控制流转发给这个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Hanlder)</span>
</span>
<span>，处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>结束业务逻辑的调用后把模型数据和逻辑视图传回派遣器处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
<span>。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>派遣器处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Dispatcher Servlet)</span>
</span>
<span>再通过视图解析器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(ViewResolver)</span>
</span>
<span>得到真正的视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>），把控制权交给视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>），同事传入模型数据，视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>）会按照一定的视图层定义展现这些数据到用户响应对象里。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>下一小节中我们将对这个流程进行完整的描述，并且在下一章中详细的分析</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlets</span>
</span>
<span>的类体系结构。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h3>
</h3>
<h6>
</h6>
<h6>
<span style="font-family:Wingdings"><span><span style="font-size:small">2</span>
 <span style="font-family:&#39;Times New Roman&#39;">  </span>
</span>
</span>
<span style="font-size:small"><span>处理器映射对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerMapping)</span>
</span>
</span>
</h6>
<h3>
<span style="font-size:small"><span lang="EN-US">
</span>
</span>
</h3>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>处理器映射对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerMapping)</span>
</span>
<span>是用于映射一个请求对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Request)</span>
</span>
<span>到一个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>。一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>实例可能会包含多个处理器映射对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerMapping)</span>
</span>
<span>。按照处理器映射对象所在的顺序，第一个返回非空处理器执行链对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerExecutionChain)</span>
</span>
<span>的处理器映射，会被当作为有效的处理器映射对象。处理器执行链对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerExecutionChain)</span>
</span>
<span>包含一个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Hanlder)</span>
</span>
<span>和一组能够应用在处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>上的拦截器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Interceptor).</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>它的接口定义如下</span>
<span lang="EN-US"><span style="font-family:Times New Roman">,</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt;text-align:center" align="center"><span lang="EN-US"><br><img src="http://dl.javaeye.com/upload/attachment/302036/12e8aa56-3621-30d9-8108-8aa66835388c.jpg" alt=""><br>
 </span>
</p>
<p style="margin:0cm 0cm 0pt;text-indent:21pt;text-align:center" align="center"><span style="font-size:x-small"><span>图表</span>
<span style="font-family:Arial"> <span lang="EN-US"><span>3</span>
</span>
<span lang="EN-US">‑</span>
<span lang="EN-US"><span>1</span>
</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>在这个接口中，通过输入一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletRequest)</span>
</span>
<span>给方法</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(getHandler)</span>
</span>
<span>，这个方法就会输出一个处理器执行链对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerExecutionChain)</span>
</span>
<span>。进而我们能够从处理器执行链对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerExecutionChain)</span>
</span>
<span>得到一个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h6>
</h6>
<h6>
<span style="font-family:Wingdings"><span><span style="font-size:small">3</span>
 <span style="font-family:&#39;Times New Roman&#39;">  </span>
</span>
</span>
<span style="font-size:small"><span>处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
</span>
</h6>
<p>
<span style="font-size:small"><span lang="EN-US">
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>处理器适配器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>是用来转接一个控制流到一个指定类型的处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）。一个类型的处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）通常会对应一个处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>的一个实现。处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>能够判断自己是否支持某个处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Hanlder</span>
</span>
<span>）。如果一个处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>支持这种类型的处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>，那么这个处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>就可以使用这个处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">handler</span>
</span>
<span>）处理当前的这个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>然而，处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Hanlder)</span>
</span>
<span>是一个通用的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Java</span>
</span>
<span>对象类型</span>
<span lang="EN-US"><span style="font-family:Times New Roman">, </span>
</span>
<span>这样做是允许</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>可以任意的去集成其他框架的处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(handler), </span>
</span>
<span>只需要为这个需要支持的处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>提供一个客户化的处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HanlderAdaptor), </span>
</span>
<span>就可以在不改变任何上层派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(DispatcherServlet)</span>
</span>
<span>代码以及下层控制器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Controller)</span>
</span>
<span>代码的前提下，实现任意集成。后来证明，基于注释的控制器，以及</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web Flow</span>
</span>
<span>都是通过这样的适配器的实现来完成和</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>工作流集成的，我们将在后面章节详细讨论。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>它的接口定义如下</span>
<span lang="EN-US"><span style="font-family:Times New Roman">,</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt;text-align:center" align="center"><span lang="EN-US"><br><img src="http://dl.javaeye.com/upload/attachment/302038/6e7a7202-01cb-34c2-903d-cff294906252.jpg" alt=""><br>
 </span>
</p>
<p style="margin:0cm 0cm 0pt;text-align:center" align="center"><span style="font-size:x-small"><span>图表</span>
<span style="font-family:Arial"> <span lang="EN-US"><span>3</span>
</span>
<span lang="EN-US">‑</span>
<span lang="EN-US"><span>2</span>
</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>在这个接口中，通过输入一个处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）给</span>
<span lang="EN-US"><span style="font-family:Times New Roman">supports()</span>
</span>
<span>方法，这个方法就会返回是否支持这个处理器。通过传入一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletRequese</span>
</span>
<span>，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletResponse</span>
</span>
<span>和一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Hanlder</span>
</span>
<span>对象给</span>
<span lang="EN-US"><span style="font-family:Times New Roman">handle()</span>
</span>
<span>方法，这个方法就会就会传递控制权给处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>当处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）处理这个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求后返回数据模型和逻辑视图名称的组合对象，处理器适配器会把这个返回结果进而返回给派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet(DispatcherServlet)</span>
</span>
<span>。</span>
<span lang="EN-US"><span style="font-family:Times New Roman">getLastModified</span>
</span>
<span>方法是用来处理一个带有</span>
<span lang="EN-US"><span style="font-family:Times New Roman">lastModified</span>
</span>
<span>头信息的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求的。不是所有的处理器适配器（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）都需要支持这个方法的。我们将在下一章详细讨论它的实现。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h6>
</h6>
<h6>
<span style="font-family:Wingdings"><span><span style="font-size:small">4</span>
 <span style="font-family:&#39;Times New Roman&#39;">  </span>
</span>
</span>
<span style="font-size:small"></span>
 <span style="font-size:small"><span>处理器对象</span>
<span style="font-family:Times New Roman"> <span lang="EN-US">- </span>
</span>
<span>控制器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler - Controller)</span>
</span>
</span>
</h6>
<p>
<span style="font-size:small"><span lang="EN-US">
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）是用于处理业务逻辑的一个基本单元。它通过传入的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletRequese)</span>
</span>
<span>来决定如何处理业务逻辑和执行哪些服务，执行服务后返回相应的模型数据和逻辑视图名。处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>是一个通用的对象类型。控制流是由处理器适配器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>传递给处理器的。所以，一个类型的处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Hanlder)</span>
</span>
<span>一定会对应一个支持的处理器适配器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdaptor)</span>
</span>
<span>。这样可以实现，处理器适配器（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdaptor</span>
</span>
<span>）和处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>的任意插拔。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>控制器是</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>中最简单的一个处理器。这个处理器有清晰的接口定义，通常这个类型的处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>是通过一个简单控制器处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(SimpleControllerHandlerAdapter)</span>
</span>
<span>来传递控制的。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>它的接口定义如下</span>
<span lang="EN-US"><span style="font-family:Times New Roman">,</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt;text-align:center" align="center"><span lang="EN-US"><br><img src="http://dl.javaeye.com/upload/attachment/302040/f5191a87-6148-3350-81b7-d0db5034792e.jpg" alt=""><br>
 </span>
</p>
<p style="margin:0cm 0cm 0pt;text-align:center" align="center"><span style="font-size:x-small"><span>图表</span>
<span style="font-family:Arial"> <span lang="EN-US"><span>3</span>
</span>
<span lang="EN-US">‑</span>
<span lang="EN-US"><span>3</span>
</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>在这个接口中，通过输入一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletRequese</span>
</span>
<span>）和</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletResponse)</span>
</span>
<span>给方法</span>
<span lang="EN-US"><span style="font-family:Times New Roman">handleRequest()</span>
</span>
<span>方法，这个方法就会返回一个处理后的模型数据和逻辑视图名的组合对象。这个对象将通过处理器适配器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdapter)</span>
</span>
<span>进而返回给派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet(DispatcherServlet)</span>
</span>
<span>。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>之所以具有高可扩展性，在于处理器适配器（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）和处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>的设计。这样可以让任意的一个处理器对象插入到</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>的体系结构中，是它具有无限的扩展性。一个例子就是自从</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring 2.5</span>
</span>
<span>新引进的注释方法处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(AnnotationMethodHandlerAdapter)</span>
</span>
<span>和注释处理器。</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web Flow</span>
</span>
<span>也是器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>扩展的一个典型例子，我们讲在下面章节中详细讨论他们的实现。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h6>
</h6>
<h6>
<span style="font-family:Wingdings"><span>5 <span style="font-family:&#39;Times New Roman&#39;">  </span>
</span>
</span>
<span style="font-size:small"><span>视图解析器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(ViewResolver)</span>
</span>
</span>
</h6>
<p>
<span style="font-size:small"><span lang="EN-US">
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>视图解析器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(ViewResolver)</span>
</span>
<span>是用于映射一个逻辑视图名称到一个真正的视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>）。当控制器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Controller)</span>
</span>
<span>处理业务逻辑之后，通常会返回需要显示的数据模型对象和视图的逻辑名称，这样就需要一个支持的视图解析器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(ViewResolver)</span>
</span>
<span>解析出一个真正的视图对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(View)</span>
</span>
<span>，然后传递控制流给这个视图对象。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>它的接口定义如下</span>
<span lang="EN-US"><span style="font-family:Times New Roman">,</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt;text-align:center" align="center"><span lang="EN-US"><br><img src="http://dl.javaeye.com/upload/attachment/302042/37543e41-2a80-32b7-b43b-6c24baa4a877.jpg" alt=""><br>
 </span>
</p>
<p style="margin:0cm 0cm 0pt;text-align:center" align="center"><span style="font-size:x-small"><span>图表</span>
<span style="font-family:Arial"> <span lang="EN-US"><span>3</span>
</span>
<span lang="EN-US">‑</span>
<span lang="EN-US"><span>4</span>
</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>在这个接口中，通过输入一个逻辑视图名称和本地对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Locale)</span>
</span>
<span>给</span>
<span lang="EN-US"><span style="font-family:Times New Roman">resolveViewName</span>
</span>
<span>方法，这个方法就会返回一个事实上的物理视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>）。本地对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Locale)</span>
</span>
<span>可以用来查找本地化的资源或者资源包。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h6>
</h6>
<h6>
<span style="font-family:Wingdings"><span><span style="font-size:small">6</span>
 <span style="font-family:&#39;Times New Roman&#39;">  </span>
</span>
</span>
<span style="font-size:small"><span>视图对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(View)</span>
</span>
</span>
</h6>
<p>
<span style="font-size:small"><span lang="EN-US">
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>是用来把模型数据通过某种显示方式反馈给客户，通常是通过执行</span>
<span lang="EN-US"><span style="font-family:Times New Roman">JSP</span>
</span>
<span>页面来完成的。也可以通过其他的更复杂的显示技术来完整这个展示过程，例如，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">JSTL</span>
</span>
<span>视图，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Tiles</span>
</span>
<span>视图，报表视图和二进制文件视图等等。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>它的接口定义如下</span>
<span lang="EN-US"><span style="font-family:Times New Roman">,</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt;text-align:center" align="center"><span lang="EN-US"><br><img src="http://dl.javaeye.com/upload/attachment/302044/f24de4ab-2e97-37ed-8f8b-efbfe7451b11.jpg" alt=""><br>
 </span>
</p>
<p style="margin:0cm 0cm 0pt;text-align:center" align="center"><span style="font-size:x-small"><span>图表</span>
<span style="font-family:Arial"> <span lang="EN-US"><span>3</span>
</span>
<span lang="EN-US">‑</span>
<span lang="EN-US"><span>5</span>
</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt 21pt"><span style="font-size:small"><span>在这个接口中，通过输入</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletRequese</span>
</span>
<span>）</span>
<span lang="EN-US"><span style="font-family:Times New Roman">, HTTP</span>
</span>
<span>响应对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletResponse)</span>
</span>
<span>和一个模型</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Map</span>
</span>
<span>对象，这个组件就会把模型</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Map</span>
</span>
<span>通过一定的显示方式包含到客户的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletResponse)</span>
</span>
<span>，这就是提供给用户请求的最终响应。</span>
<span lang="EN-US"><span style="font-family:Times New Roman">getContentType()</span>
</span>
<span>能够返回这个视图所支持的内容类型，例如，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">XML, JSON, text/html</span>
</span>
<p style="margin:0cm 0cm 0pt 21pt"><span>等等。<span style="font-size:x-small"> </span>
</span>
</p>
</span>
</p>
<p><span style="font-size:small"><span style="font-size:small"><span>
</span>
</span>
</span>
</p>
<p> </p>
<h2>
<span>3.2 <span>组件间的协调通信</span>
</span>
</h2>
<p style="margin:0cm 0cm 0pt;text-align:center" align="center"><span lang="EN-US"><br><img src="http://dl.javaeye.com/upload/attachment/302046/9cd8b04d-42c1-385f-862c-326439e54a6b.jpg" alt=""><br>
 </span>
</p>
<p style="margin:0cm 0cm 0pt;text-align:center" align="center"><span style="font-size:x-small"><span>图表</span>
<span style="font-family:Arial"> <span lang="EN-US"><span>3</span>
</span>
<span lang="EN-US">‑</span>
<span lang="EN-US"><span>6</span>
</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>众所周知，一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求发送到</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Web</span>
</span>
<span>容器，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Web</span>
</span>
<span>容器就会封装一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletRequest)</span>
</span>
<span>，这个对象包含所有的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求信息，例如，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>参数以及参数值，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求头的各种元数据。同时，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Web</span>
</span>
<span>容器会创建一个</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletResponse</span>
</span>
<span>），用以发送</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应给客户端用户。然后，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Web</span>
</span>
<span>容器传递</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletRequest)</span>
</span>
<span>和</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletResponse</span>
</span>
<span>）给</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">service()</span>
</span>
<span>方法。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>实际上，</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>的入口就是一个客户化的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>，称为派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(DispatcherServlet)</span>
</span>
<span>。这个派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）得到</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HttpServletRequest)</span>
</span>
<span>和</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletResponse</span>
</span>
<span>）后，一个典型的</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>工作流就开始了。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）首先查找所有注册的处理器映射器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerMapping</span>
</span>
<span>），然后，遍历所有的处理器映射器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerMapping</span>
</span>
<span>），直到一个处理器映射器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerMapping</span>
</span>
<span>）返回一个非空的处理器执行链对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerExecutionChain)</span>
</span>
<span>。那么，处理器执行链对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerExecutionChain)</span>
</span>
<span>就包含着一个需要处理当前</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求的一个处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）。如图表</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3-6</span>
</span>
<span>第</span>
<span lang="EN-US"><span style="font-family:Times New Roman">1</span>
</span>
<span>步。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>这里，一个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>被设计成了一个通用的对象类型，所以，这里需要一个处理器适配器（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdaptor</span>
</span>
<span>）去派遣这个控制流到一个处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>，因为只有支持这种类型的处理器（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）的处理器适配器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdapter)</span>
</span>
<span>才知道如何去传递控制流给这个类型的处理器（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Hanlder</span>
</span>
<span>）。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>拿到了处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Hanlder)</span>
</span>
<span>以后，派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）则查找所有注册的处理器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>），然后，遍历所有的处理器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）查询是否有一个处理器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）支持这个处理器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Handler</span>
</span>
<span>）。如图表</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3-6</span>
</span>
<span>第</span>
<span lang="EN-US"><span style="font-family:Times New Roman">2</span>
</span>
<span>步。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>如果有这样的一个处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdapter)</span>
</span>
<span>，则派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）将控制权转交给这个派遣器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdapter)</span>
</span>
<span>。如图表</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3-6</span>
</span>
<span>第</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3</span>
</span>
<span>步。派遣器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）和真正的处理器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>是成对出现的，所以，这个支持的处理器适配器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HandlerAdapter</span>
</span>
<span>）则知道如何去使用这个处理器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(Handler)</span>
</span>
<span>去处理这个请求。最简单的一个处理器则是控制器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Controller</span>
</span>
<span>）。处理器适配器对象</span>
<span lang="EN-US"><span style="font-family:Times New Roman">(HandlerAdapter)</span>
</span>
<span>将传递</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>请求对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletRequest</span>
</span>
<span>）和</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletResponse</span>
</span>
<span>）给控制器，并且期待控制器返回模型和视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">ModelAndView</span>
</span>
<span>）。如图表</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3-6</span>
</span>
<span>第</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3.1</span>
</span>
<span>步。这个模型和视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">ModelAndView</span>
</span>
<span>）对象包含着一组模型数据和视图逻辑名称，并且最终返回给派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）</span>
<span lang="EN-US"><span style="font-family:Times New Roman">.</span>
</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）然后查找所有注册的视图解析器对象，并且遍历所有的视图解析器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">ViewResolver</span>
</span>
<span>），直到一个视图解析器对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">ViewResolver</span>
</span>
<span>）返回一个物理的视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>）。如图表</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3-6</span>
</span>
<span>第</span>
<span lang="EN-US"><span style="font-family:Times New Roman">4</span>
</span>
<span>步。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>最后，派遣器</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Servlet</span>
</span>
<span>（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">DispatcherServlet</span>
</span>
<span>）把得到的一组模型数据传递给得到的物理视图对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">View</span>
</span>
<span>）。如图表</span>
<span lang="EN-US"><span style="font-family:Times New Roman">3-6</span>
</span>
<span>第</span>
<span lang="EN-US"><span style="font-family:Times New Roman">5</span>
</span>
<span>步。然后，视图对象则会使用响应的表现层技术，把模型数据展现成</span>
<span lang="EN-US"><span style="font-family:Times New Roman">UI</span>
</span>
<span>界面，并且通过</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>响应对象（</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HttpServletResponse</span>
</span>
<span>）发送给</span>
<span lang="EN-US"><span style="font-family:Times New Roman">HTTP</span>
</span>
<span>客户。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<h2>3.3 本章小结</h2>
<p style="margin:0cm 0cm 0pt"> </p>
<p style="margin:0cm 0cm 0pt"><span style="font-size:small"><span>这一章，我们介绍了</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Web MVC</span>
</span>
<span>在</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring</span>
</span>
<span>中是如何实现的。具体分析了</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>架构中的各个组件，组件的功能，以及组件的接口。最后，大体的分析了</span>
<span lang="EN-US"><span style="font-family:Times New Roman">Spring Web MVC</span>
</span>
<span>的工作流是如何传递和工作的。</span>
</span>
</p>
<p style="margin:0cm 0cm 0pt"><span lang="EN-US"><span style="font-family:Times New Roman;font-size:small"> </span>
</span>
</p>
<p><span>下一章，我们将深入抛析</span>
<span>Spring Web MVC</span>
<span>的架构的具体实现。</span>
</p>
          
          <br><br>
          作者: <a href="http://robertleepeak.javaeye.com">robertleepeak</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/753034" style="color:red">已有 <strong>5</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>