---
layout: post
title:  "SpringMVC学习笔记"
date:   2010-08-01 20:36:21
author: 
categories: program
---

## SpringMVC学习笔记
### by 
### at 2010-08-01 20:36:21
### original <http://www.javaeye.com/topic/726610>

这是一个SpringMVC的学习笔记
<br>
<br>基本上是一个精简版的SpringMVC参考，很简单，因为大部分的web框架的概念都是通用的。
<br>而本文更关心的是SpringMVC中的概念性问题，至于细节，不在本学习笔记之内。
<br>
<br>该文档基于Spring2.5.2
<br>
<br><strong>概述</strong>
<br>Spring的web框架围绕DispatcherServlet设计。 DispatcherServlet的作用是将请求分发到不同的处理器。 Spring的web框架包括可配置的处理器（handler）映射、视图（view）解析、本地化（local）解析、 主题（theme）解析以及对文件上传的支持。Spring的Web框架中缺省的处理器是Controller 接口，这是一个非常简单的接口，仅包含ModelAndView handleRequest(request, response) 方法。可以通过实现这个接口来创建自己的控制器（也可以称之为处理器），但是更推荐继承Spring提供的一系列控制器， 比如AbstractController、AbstractCommandController 和SimpleFormController。
<br>
<br><strong>Spring Web MVC框架的特点</strong>
<br>
<br>清晰的角色划分：控制器（controller）、验证器（validator）、 命令对象（command object）、表单对象（form object）、模型对象（model object）、 Servlet分发器（DispatcherServlet）、 处理器映射（handler mapping）、视图解析器（view resolver）等等。 每一个角色都可以由一个专门的对象来实现。 
<br>
<br>强大而直接的配置方式：将框架类和应用程序类都能作为JavaBean配置，支持跨多个context的引用，例如，在web控制器中对业务对象和验证器（validator）的引用。 
<br>
<br>可适配、非侵入：可以根据不同的应用场景，选择合适的控制器子类 （simple型、command型、form型、wizard型、multi-action型或者自定义），而不是从单一控制器 （比如Action/ActionForm）继承。
<br>
<br>可重用的业务代码：可以使用现有的业务对象作为命令或表单对象，而不需要去扩展某个特定框架的基类。
<br>
<br>可定制的绑定（binding） 和验证（validation）：比如将类型不匹配作为应用级的验证错误， 这可以保存错误的值。再比如本地化的日期和数字绑定等等。
<br>
<br>可定制的handler mapping和view resolution：Spring提供从最简单的URL映射， 到复杂的、专用的定制策略。
<br>
<br>灵活的model转换：在Springweb框架中，使用基于Map的 键/值对来达到轻易地与各种视图技术的集成。 
<br>
<br>可定制的本地化和主题（theme）解析：支持在JSP中可选择地使用Spring标签库、支持JSTL、支持Velocity（不需要额外的中间层）等等。
<br>
<br>简单而强大的JSP标签库（Spring Tag Library）：支持包括诸如数据绑定和主题（theme） 之类的许多功能。它提供在标记方面的最大灵活性。
<br>
<br>JSP表单标签库：在Spring2.0中引入的表单标签库，使得在JSP中编写 表单更加容易。
<br>
<br>Spring Bean的生命周期可以被限制在当前的HTTP Request或者HTTP Session。 准确的说，这并非Spring MVC框架本身特性，而应归属于Sping MVC使用的WebApplicationContext容器。
<br>
<br>这个图是很通用的一个mvc的图，基本上主流的web框架都是这么做。
<br><img src="http://www.iteye.com/upload/attachment/286512/60f8f517-1107-3e85-a9af-efcd8bffebeb.jpg">
<br><img src="http://www.iteye.com/upload/attachment/286514/0a988738-44df-388e-b987-8ce5c9871c86.jpg">
<br>
<br><strong>控制器</strong>
<br>
<br>控制器的概念是MVC设计模式的一部分（确切地说，是MVC中的C）。应用程序的行为通常被定义为服务接口， 而控制器使得用户可以访问应用所提供的服务。控制器解析用户输入，并将其转换成合理的模型数据，从而可以进一步由视图展示给用户。 Spring以一种抽象的方式实现了控制器概念，这样可以支持不同类型的控制器。
<br>
<br>控制器还是推荐一个request一个controller的方式，向导型除外。
<br>
<br>为提供一套基础设施，所有的Spring控制器都继承了 AbstractController ，AbstractController 提供了诸如缓存支持和mimetype设置这样的功能。 
<br>
<br>Spring提供了MultiActionController来将多个请求处理方法合并在一个控制器里，这样可以把相关功能组合在一起。
<br>
<br>Spring的command controllers是Spring MVC的重要部分。 命令控制器提供了一种和数据对象交互的方式，并动态地将来自HttpServletRequest的参数绑定到指定的数据对象上。
<br>
<br><strong>处理器映射</strong>
<br>
<br>HandlerMapping的基本功能是将请求传递到HandlerExecutionChain上。 首先，这个HandlerExecutionChain必须包含一个能处理该请求的处理器。 其次，这个链也可以包含一系列可以拦截请求的拦截器。 当收到请求时，DispatcherServlet将请求交给处理器映射，让它检查请求并找到一个适当的HandlerExecutionChain。 然后，DispatcherServlet执行定义在链中的处理器和拦截器（interceptor）。 
<br>
<br>在处理器映射中通过配置拦截器（包括处理器执行前、执行后、或者执行前后运行拦截器）将使其功能更强大。 同时也可以通过自定义HandlerMapping来支持更多的功能。 比如，一个自定义的处理器映射不仅可以根据请求的URL，而且还可以根据和请求相关的特定session状态来选择处理器。 
<br>
<br><strong>拦截器</strong>
<br>Spring的处理器映射支持拦截器。当你想要为某些请求提供特殊功能时，例如对用户进行身份认证，这就非常有用。
<br>打log，测性能，做授权与认证就靠它了。
<br>
<br>处理器映射中的拦截器必须实现org.springframework.web.servlet包中的HandlerInterceptor接口。 这个接口定义了三个方法，一个在处理器执行前被调用，一个在处理器执行后被调用，另一个在整个请求处理完后调用。 这三个方法提供你足够的灵活度做任何处理前后的操作。
<br>
<br><strong>视图与视图解析</strong>
<br>ViewResolver和View是Spring的视图处理方式中特别重要的两个接口。 ViewResolver提供了从视图名称到实际视图的映射。 View处理请求的准备工作，并将该请求提交给某种具体的视图技术。
<br>
<br><strong>视图解析链</strong>
<br>Spring支持多个视图解析器一起使用。可以把它们当作一个解析链。 这样有很多好处，比如在特定情况下重新定义某些视图。 定义视图解析链很容易，只要在应用上下文中定义多个解析器就可以了。 必要时，也可以通过order属性来声明每个解析器的序列。
<br>
<br>如果某个解析器没有找到合适的视图，Spring会在上下文中寻找是否配置了其它的解析器。 如果有，它会继续进行解析，否则，Srping会抛出一个Exception。 
<br>
<br>要记住，当一个视图解析器找不到合适的视图时，它可能 返回null值。 但是，不是每个解析器都这么做。这是因为，在某些情况下，解析器可能无法侦测出符合要求的视图是否存在。 比如，InternalResourceViewResolver在内部调用了RequestDispatcher。 请求分发是检查一个JSP文件是否存在的唯一方法，不幸的是，这个方法只能用一次。 同样的问题在VelocityViewResolver和其它解析器中也有。 当使用这些解析器时，最好仔细阅读它们的Javadoc，看看需要的解析器是否无法发现不存在的视图。 这个问题产生的副作用是，如果InternalResourceViewResolver解析器没有放在链的末端， InternalResourceViewResolver后面的那些解析器根本得不到使用， 因为InternalResourceViewResolver总是返回一个视图！
<br>
<br><strong>重定向（Rediret）到另一个视图</strong>
<br>在控制器中强制重定向的方法之一是让控制器创建并返回一个Spring的RedirectView的实例。 在这种情况下，DispatcherServlet不会使用通常的视图解析机制， 既然它已经拿到了一个（重定向）视图，它就让这个视图去完成余下的工作。
<br>
<br>尽管使用RedirectView帮我们达到了目的，但是如果控制器生成RedirectView的话， 控制器不可避免地要知道某个请求的结果是让用户重定向到另一个页面。这不是最佳的实现，因为这使得系统不同模块之间结合得过于紧密。 其实控制器不应该过问返回结果是如何生成的，通常情况下，它应该只关心注入给它的视图名称。 
<br>
<br>解决上述问题的方法是依靠redirect:前缀。 如果返回的视图名包含redirect:前缀，UrlBasedViewResolver （以及它的子类） 会知道系统要生成一个HTTP redirect。 视图名其余的部分会被当作重定向URL。
<br>
<br>即时是web编程也要考虑松耦合的问题。
<br>
<br><strong>本地化解析器</strong>
<br>
<br>Spring架构的绝大部分都支持国际化，Spring的web MVC框架也不例外。 DispatcherServlet 允许使用客户端本地化信息自动解析消息。 这个工作由LocaleResolver对象完成。 
<br>
<br>当收到请求时，DispatcherServlet查找一个本地化解析器，如果找到，就使用它设置本地化信息。 通过RequestContext.getLocale()方法，总可以获取由本地化解析器解析的客户端的本地化信息。 
<br>
<br><strong>使用主题</strong>
<br>Sping的web MVC框架允许你通过主题（theme）来控制网页的风格，这将进一步改善用户的体验。 简单来说，一个主题就是一组静态的资源（比如样式表和图片），它们可以影响应用程序的视觉效果。
<br>这个东西挺好的，不知道有没有人用起来。
<br>
<br><strong>Spring对分段文件上传（multipart file upload）的支持</strong>
<br>
<br><strong>Spring的表单标签库</strong>
<br>
<br><strong>处理异常</strong>
<br>当与请求匹配的控制器处理请求时，可能会发生意料之外的异常。
<br>
<br><strong>惯例优先原则</strong>
<br>对于很多项目来说，严格遵从已有惯例和使用合理的缺省选项大概是这些项目需要的……现在Spring Web MVC明确的支持了这种惯例优先原则的主旨。 这意味着，如果建立了一套命名规范，诸如此类，就可以显著地减少系统所需配置项目的数量， 来建立处理器映射、视图解析器、ModelAndView实例，等等。 这为快速原型开发提供了很大方便。同时提供了一定程度的（通常是好事情）代码库的一致性，进而可以从中选择并发展为成型产品。
<br>
<br><strong>基于注解的控制器配置</strong>
<br>使用@Controller定义一个控制器
<br>使用@RequestMapping映射请求
<br>使用@RequestParam绑定请求参数到方法参数
<br>使用@ModelAttribute提供一个从模型到数据的链接
<br>使用@SessionAttributes指定存储在会话中的属性
          
          <br><br>
          作者: <a href="http://zhang-xzhi-xjtu.javaeye.com">zhang_xzhi_xjtu</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/726610" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>