---
layout: post
title:  "四种 JavaScript 客户端 MVC 框架综述"
date:   2012-11-27 09:46:12
author: 
categories: program
---

## 四种 JavaScript 客户端 MVC 框架综述
### by 
### at 2012-11-27 09:46:12
### original <http://blog.jobbole.com/30719/?utm_source=rss&utm_medium=rss&utm_campaign=javascript-%25e5%25ae%25a2%25e6%2588%25b7%25e7%25ab%25af-mvc-%25e6%25a1%2586%25e6%259e%25b6%25e8%25b0%2583%25e6%259f%25a5>

<p>来源：<a href="http://www.ibm.com/developerworks/cn/web/wa-clientmvc/index.html" rel="nofollow">IBM DeveloperWorks</a></p>
<p><strong>简介</strong></p>
<p>15 年前，许多人都使用 Perl 和 ColdFusion 之类的工具构建网站。我们经常编写可以在页面顶部查询数据库的脚本，对数据应用必要的转换，以及在同一个脚本底部显示数据。这类架构适合于向网站添加简单的 “Contact us” 表单。然而，随着应用程序变得更加复杂，这种方法无法进行相应的扩展来处理更大的复杂问题。大部分 Web 应用程序现在已经对模型-视图-控制器 (MVC) 架构进行了标准化，使用单独的代码实现业务逻辑、显示逻辑和用户交互（路由）逻辑。涌现出从 Spring MVC 到 Rails 的各种框架可以帮助您快速实现基于 MVC 的 Web 应用程序。</p>
<p><a href="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" rel="lightbox[30719]" title="JavaScript 客户端 MVC 框架调查"><img title="JavaScript 客户端 MVC 框架调查" src="http://blog.jobbole.com/wp-content/uploads/2011/06/javascript-logo.png" alt="JavaScript 客户端 MVC 框架调查" width="200" height="200"></a></p>
<p>几年前，jQuery 是用于构建客户端 JavaScript 应用程序的主流库。然而，随着应用程序中的 JavaScript 的复杂性日益增加，jQuery 成为一项处理复杂性的必要不充分技术。例如，用于待办事项 (to-do) 列表的单页面应用程序可以包含一个紧急待办事项列表、一个完整的待办事项列表、一个当日待办事项列表，以及一个过期待办事项列表。在删除某个待办事项时会怎样？如果任务很紧急但已过期，您可能需要手动编写代码来从视图中的三个或四个不同位置中删除该事项。如果删除某个对象后需要您删除或更改屏幕上显示的其他相关对象，这样复杂性就会变得无法控制。</p>
<p>客户端 MVC 框架旨在解决此类问题，并且大多数框架都表现出色。但是您如何从许多 JavaScript 客户端 MVC 框架中选择合适的框架。本文将从较高的层面简要介绍其中一些最流行的框架。以及如何针对给定的用例选择合适的框架。</p>
<p> </p>
<p><strong>Backbone.js</strong></p>
<p>在使用率方面，Backbone 是目前为止最流行的客户端 MVC 框架。它被广泛应用于各个开发社区，Rails 开发人员对它的采用率一直较高，并出现了许多广受欢迎的资源，比如 thoughtbot（一家备受尊敬的 Rails 咨询公司）推出了 Backbone on Rails 。Backbone.js 的优势在于它与具象状态传输 Web 服务实现了良好的集成。如果您对后端数据使用 RESTful JavaScript Object Notation (JSON) 模型并遵循 Backbone 所期望的约定（与 Rails 中的约定匹配），那么您不需要编写任何代码就可以将 Backbone 连接到服务器，从而节省大量的时间。</p>
<p>在 Backbone 中，应用程序包含集合（用户或文章）、模型（单个用户或文章）、视图和路由器。Backbone.js 中的视图是非预定的 (nonprescriptive)，允许您使用自己喜欢的 JavaScript 模板或框架。路由器结合了 Rail 风格的路由器和一个传统的 MVC 控制器，负责获得给定的 URL 并通知框架要运行的代码。清单 1 中的 Backbone.js 路由器代码给出了一个示例。</p>
<p><strong>清单 1. 样例 Backbone.js 路由器代码</strong></p>
<pre>				
var Workspace = Backbone.Router.extend({

  routes: {
    &quot;help&quot;:                 &quot;help&quot;,    // #help
    &quot;search/:query&quot;:        &quot;search&quot;,  // #search/kiwis
    &quot;search/:query/p:page&quot;: &quot;search&quot;   // #search/kiwis/p7
  },

  help: function() {
    ...
  },

  search: function(query, page) {
    ...
  }
});</pre>
<p>Backbone.js 附带了一个 Underscore.js 副本。Underscore.js 是一组实用工具，可以通过更加功能化的方式简化 JavaScript 的编写，并支持一系列有用的基于集合的操作。它还包括 Backbone.history，后者可以帮助您巧妙地处理页面导航。</p>
<p>Backbone.js 主要优势在于它与服务器的自动集成。如果这样做适合您的用例，那么学习如何使用 Backbone.js 将是值得的。您可以通过一些框架在一两个小时之内初步掌握可能需要花一到两天学习的Backbone.js 基础知识。这非常适合比较大的项目，这类项目至少持续几周的时间。</p>
<p>Backbone.js 仍然算不上一种完好的解决方案。您可能需要编写相当数量的代码来处理潜在的内存泄露等问题。您还可能需要试验几种方法来查看呈现内容，之后才能找到真正满足需求的方法。</p>
<p> </p>
<p><strong>Spine.js</strong></p>
<p>Spine.js 通常与 Backbone.js 进行比较；它受到 Backbone.js 的影响，并在使用率方面与前者接近。Spine.js 包含类、模型、控制器和视图，这比 Backbone.js 引入的集合更加传统一些。</p>
<p>Spine.js 使用 CoffeeScript 编写，这使它更加简练且（依我看来）更易于读取源代码。要了解 Spine.js 如何工作，您需要熟悉 CoffeeScript。然而，您不必使用 CoffeeScript 构建 Spine.js 应用程序。但是，如果已使用 CoffeeScript 进行了构建，您可以访问 CoffeeScript 特性（如类）。CoffeeScript 使用原型继承而非经典继承，因而无法支持在本地 JavaScript 中的类。CoffeeScript 使用了一些非常标准的模式，为希望使用它们的开发人员提供类。如果使用纯 JavaScript 编写 Spine.js 应用程序，您只需使用 <code>Spine.Class: var Users = Spine.Class.sub();</code>，后者使您不需要编写 CoffeeScript 代码就可以访问类。</p>
<p>Spine.js 中的模型、控制器和视图都使用类实现，因此可以同时编写类和实例方法。模型负责处理业务逻辑，属于模块类，您可以扩展并包括其他模块，从而混合重用属性和功能。模型可以自动序列化到 JSON 中，通过仅使用本地存储实现持久化。或者可以使用 Asynchronous JavaScript + XML (Ajax) 将对象持久化到服务器中。和 Backbone.js 一样，Spine.js 现在提供了合理的默认设置，可以通过 Ajax 实现持久化，但是仍可以在必要时编写自己的特定实现，并且非常简单。清单 2 展示了来自一个 Spine.js 应用程序中的 CoffeeScript 代码的示例。</p>
<p><strong>清单 2. Spine.js 应用程序中的 CoffeeScript</strong></p>
<pre>				
class Contact extends Spine.Model
  @configure &quot;Contact&quot;, &quot;first_name&quot;, &quot;last_name&quot;

  @filter: (query) -&gt; 
    @select (c) -&gt; 
      c.first_name.indexOf(query) is not -1

  fullName: -&gt; [@first_name, @last_name].join(&#39; &#39;)</pre>
<p>Spine.js 和 Backbone.js 两者之间最主要区别是它们处理服务器交互的方式。Backbone.js 在显示响应之前将等待服务器响应。如果试图删除、插入或更新某个元素，用户界面 (UI) 直到操作成功完成才会刷新。而 Spine.js 侧重于即时更新 UI，而且在进行后台处理时处理 Ajax 服务器。这种更新是一种非常重要的实践，也是在这两种优化的拥有良好编档的流行框架之间选择时需要考虑的的主要因素。</p>
<p>如果您的目标是创建一种客户端体验，而对服务器状态的更新是次要的，那么 Spine.js 可能是一种更好的选择。如果仍然使用服务器来检查状态变化的有效性则 Backbone.js 可能更适合。Spine.js 提供了响应性更好的 UI。但是，如果显示成功删除某个元素，只是让服务器发送一个响应，不允许您删除该项，因为该项正在被其他人使用，那么会发生什么？针对这个问题存在一些应急方案，但是通常来讲 Spine.js 更加适合用户操作自有（而非共享）数据。Spine.js 的一个常见用例就是购物车，其中所有验证都可以在客户端处理。</p>
<p> </p>
<p><strong>Knockout</strong></p>
<p>人们可能会争论目前为止讨论的这些工具是否是原本意义上的真正的 MVC 框架。Knockout 明确实现了模型-视图-视图-模型 (MVVM)，而不是经典的 MVC。但是，不要因此而妨碍到您的决策制定。在选择框架时，更重要的是查看所提供的功能而非首字母缩略词或分类。</p>
<p>Knockout.js 在熟悉 MVVM 模型的 Microsoft .NET 开发人员之间特别受欢迎。对于主要问题是将模型状态通过声明的方式绑定到视图用例，Knockout.js 是非常好的选择。Knockout.js 对于前面提到的示例待办事项应用程序是非常理想的选择，该应用程序的主待办事项列表的子集都有自己的视图，在删除某个待办事项后需要更新所有的列表。</p>
<p>在 Knockout.js 中，您将创建模型、视图模型和视图。与在 Spine.js 和 Backbone.js 中一样，负责处理业务逻辑、验证和与远程服务器交互的 Ajax（假设您不仅仅是创建一个本地应用程序）。视图模型代码负责保留和操作模型数据。例如一个视图模型可能包含添加、编辑以及从列表中删除内容项的方法。视图模型非常贴近于传统的 MVC 架构中的控制器。视图就是一些模板，包含将信息呈现到屏幕的标记。在 Knockout.js 中，这些可以通过声明的方式绑定到视图模型（方便入门）。一些学员可以在一个小时内掌握并使用 Knockout，并可在三个小时内构建非凡 (non-trivial) 应用程序。</p>
<p>一般来讲，Knockout.js 比较适合较小、较简单的项目。人们往往将 Backbone.js 或 Spine.js 用于更大、更复杂的项目。也就是说，有经验的 Knockout.js 开发人员可以创建非常复杂、同时又易于维护的应用程序。如果考虑使用 Knockout.js，您也应当考虑 Angular.js 和 Sammy.js ，后两者是两种相对轻量级、易于启动的框架。</p>
<p> </p>
<p><strong>Batman.js</strong></p>
<p>Batman.js 是一种有趣的新框架，由 JSConf 在 2011 年推出，但是又经过了几个月的时间才能够通过下载获取。Batman.js 已经开始受到一些喜欢并得到开发 MVC 应用程序的<span><a href="http://blog.jobbole.com/821/" title="程序员的本质">程序员</a></span>的关注。表面上看，Batman 在易于入门、支持视图声明绑定方面与 Knockout.js 类似。Batman.js 提供了一些其他功能，包括可选的全栈 (full-stack) 框架，用于自动代码生成器、构建工具甚至后端 <span><a href="http://blog.jobbole.com/1254/" title="Node.js 究竟是什么？">Node.js</a></span> 服务器代码，可以实现您的服务器端 API。</p>
<p>和 Knockout.js 一样，Batman.js 也使用视图绑定。清单 3展示了一些样例视图代码。</p>
<p><strong>清单 3. Batman.js 中的视图代码示例</strong></p>
<pre>				
&lt;ul id=&quot;items&quot;&gt;
    &lt;li data-foreach-todo=&quot;Todo.all&quot; data-mixin=&quot;animation&quot;&gt;
        &lt;input type=&quot;checkbox&quot; data-bind=&quot;todo.isDone&quot; data-event-change=&quot;todo.save&quot; /&gt;
        &lt;label data-bind=&quot;todo.body&quot; data-addclass-done=&quot;todo.isDone&quot; 
            data-mixin=&quot;editable&quot;&gt;&lt;/label&gt;
        &lt;a data-event-click=&quot;todo.destroy&quot;&gt;delete&lt;/a&gt;
    &lt;/li&gt;
    &lt;li&gt;&lt;span data-bind=&quot;Todo.all.length&quot;&gt;&lt;/span&gt; 
       &lt;span data-bind=&quot;&#39;item&#39; | pluralize Todo.all.length&quot;&gt;&lt;/span&gt;&lt;/li&gt;
&lt;/ul&gt;</pre>
<p><strong>清单 3</strong>中的代码是有效的 HTML5，包含一些额外的属性，供 Batman 绑定数据和事件。在 Batman.js 中，您的应用程序包含模型、视图和控制器。模型支持验证功能，能够实现生命周期事件，包括一个内置的恒等映射 (identity map)，并且可以被告知（主动记录样式）如何坚持使用 <code>Batman.LocalStorage</code>、<code>Batman.RestStorage</code>、<code>Batman.RailsStorage</code> 或自定实现。视图为一些 JavaScript 类，呈现用纯 HTML 编写的模板，还有一些用 <code>data-*</code> 属性绑定模型数据并触发事件处理器的构件。控制器为一些永久对象，处理来自视图的事件，访问模型数据，并呈现相应的视图。</p>
<p> </p>
<p><strong>选择一种 JavaScript 框架</strong></p>
<p>如果您正在从事一个长期的大项目，那么了解 Backbone.js 或 Spine.js 很有必要，因为它们获得了广泛的采用，可以解决您可能遇到的问题。然而，即使有了这些项目，您要明白您不是有必要使用一个成熟的服务器端 MVC 框架，而是还需继续编写基础架构代码。</p>
<p>尝试使用在视图中用了声明式绑定的框架将非常有必要。此类框架具有与 Backbone.js 之类的项目不同的优缺点。如果考虑使用声明式视图绑定，那么花些时间研究一下更新的 Batman.js 框架提供的额外功能。虽然 Batman.js 不像其他框架那么流行，但它正在快速发展，而且提供了比普通客户端 MVC 框架更丰富的特性。</p>
<p>在不同框架中进行原型化，感受一下这些框架的用法，这样做非常有必要。特别是对于客户端 MVC 框架来说，原型化是从不同选项中进行选择的最快速、最有效的方法之一。一种方法是让每个团队成员花一到两天的时间，使用不同的框架进行原型化，然后进行回顾并讨论结果。最坏的情况是，如果您还有一对框架需要从中进行选择，那么再额外花一天左右的时间构建二者的概念，直到选出最适合您的用例的框架。</p>
<p>考虑灵活性。仔细考虑您可以做哪些工作来降低对框架的依赖性，这对于许多框架来说是一个艰巨的任务。为将来 12 到 18 个月内迁移到另一个框架制定一个备份方案，以防您发现需求和所选的框架没有按照预期进行。</p>
<p> </p>
<p><strong>结束语</strong></p>
<p>JavaScript 客户端 MVC 框架仍然不够成熟。这个领域正在发生快速改变，缺少一致认可的最佳实践。对于较大的项目 Backbone.js 和 Spine.js 都是非常流行、具有良好支持的。如果倾向于声明视图绑定，那么 Knockout.js 和 Batman.js 则都是不错的选择。</p>
<p> </p>

<h2>相关文章</h2><ul><li><a href="http://blog.jobbole.com/30854/">JavaScript开发规范要求</a></li><li><a href="http://blog.jobbole.com/30134/">Testacular：Google开源的JavaScript测试执行过程管理工具</a></li><li><a href="http://blog.jobbole.com/30468/">测试：你自认为理解了JavaScript？</a></li><li><a href="http://blog.jobbole.com/30445/">开始使用Web Workers</a></li><li><a href="http://blog.jobbole.com/30133/">跟开涛学Spring MVC：第三章 DispatcherServlet详解</a></li><li><a href="http://blog.jobbole.com/30046/">Javascript模块化编程（三）：require.js的用法</a></li><li><a href="http://blog.jobbole.com/29815/">构建web前端异常监控系统–FdSafe</a></li><li><a href="http://blog.jobbole.com/29747/">对 JavaScript 进行单元测试的工具</a></li><li><a href="http://blog.jobbole.com/29739/">Javascript模块化编程（二）：AMD规范</a></li><li><a href="http://blog.jobbole.com/29583/">用JSLint精炼提升JavaScript代码</a></li></ul>