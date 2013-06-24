---
layout: post
title:  "芝加哥老板[chicagoBoss]"
date:   2011-04-21 17:55:28
author: jackyz
categories: program
---

## 芝加哥老板[chicagoBoss]
### by jackyz
### at 2011-04-21 17:55:28
### original <http://erlang-china.org/2011/04/21/chicagoboss/>

<p><a href="http://www.chicagoboss.org/">Chicago Boss</a> 这个名字听起来，怎么也让人联想不到 Web 开发领域。反而总让人不由得想起黑帮、枪战、还有私酒泛滥的 1920 年代（估计是最近看《大西洋帝国》给闹的）。但是，的而且确，这个玩意明白无误地就是 Web 开发领域的事儿。</p>
<p>先上亮点：</p>
<blockquote><p>
High Productivity</p>
<p>Compact query syntax:</p>
<p><code>boss_db:find(person, [age &gt; 25])</code></p>
<p>Elegant controllers:</p>
<p><code>edit(&#39;GET&#39;, []) -&gt;<br>
   ok;<br>
edit(&#39;POST&#39;, []) -&gt;<br>
   {redirect, "/somewhere"}.<br>
</code></p>
<p>Simple data models:</p>
<p><code>-module(person, [Id, FirstName, LastName]).</code></p>
<p>full_name() -&gt;<br>
    FirstName ++ " " ++ LastName.<br>
</p>
<p>Rails-y records:</p>
<p><code>Person = person:new(id, "Joe", "Blow"),<br>
Person:save()<br>
</code></p>
<p>Django templates:</p>
<p><code>{% for person in people %}<br>
 - {{ person.full_name }}<br>
{% endfor %}<br>
</code></p>
<p>Easy Comet – New in 0.4.5</p>
<p>BossMQ introduces a simple API for sending and receiving messages. Here is a complete long-polling chat backend:</p>
<p><code>send_chat(&#39;POST&#39;, [Channel]) -&gt;<br>
  boss_mq:push(Channel, Req:post_param("msg")).</code></p>
<p>receive_chat(&#39;GET&#39;, [Channel]) -&gt;<br>
  {ok, Time, Messages} = boss_mq:pull(Channel),<br>
  {json, [{time, Time}, {messages, Messages}]}.<br>
</p>
<p>With BossMQ, any controller action is a potential Comet endpoint. Thus, Comet endpoints have complete access to your entire application — databases, templates, the whole stack.</p>
<p>High Reliability</p>
<p>100% asynchronous network I/O</p>
<ul>
<li>Under load, uses less RAM than synchronous apps (PHP, Rails, Django)</li>
<li>Comet long-polls won’t block the app server</li>
<li>Single-process model keeps CPU context switches to a minimum</li>
</ul>
<p>Supports both SQL and schema-less databases</p>
<ul>
<li>MySQL</li>
<li>PostgreSQL</li>
<li>MongoDB (NEW)</li>
<li>Tokyo Tyrant</li>
<li>Mnesia</li>
<li>Or write your own adapter — usually ~300 lines of code</li>
</ul>
<p>Operational simplicity</p>
<ul>
<li>Zero downtime code upgrades in production (NEW)</li>
<li>Standalone server does not require Apache, Nginx, etc.</li>
<li>Direct SMTP mail delivery does not require Postfix</li>
</ul>
<p>Unique testing framework</p>
<ul>
<li>Closure-based tests reduce test code size</li>
<li>Run functional tests in-memory — no network I/O</li>
<li>Integrated web and email testing</li>
</ul>
<p>Future plans (not yet implemented!)</p>
<ul>
<li>Full OTP compliance</li>
<li>Full test coverage</li>
<li>See the Roadmap</li>
</ul>
</blockquote>
<p>好吧我得承认，其中的一些亮点其实是来自 Erlang 语言本身的。</p>
<p>毫无疑问，这个项目提供了 Erlang 和 ruby on rails’ way 的跨界组合优势。比如说，在采用高生产力的 rails 方式来构建项目时，如果心存疑虑，担心某一天会被可能的性能问题跳出来反咬一口的话，那么现在又多了一个具有 FP 风格的选择了。</p>
<p>有必要表扬一句的是：chicago boss 的文档整理得相当靠谱，单就这一点而言，在众多的 Erlang Web 项目中，就很有走红潜质。此外，开发过程似乎也可圈可点，按照 roadmad 的特性持续升级，很让人看好。不过也有的地方让人感觉有点奇怪，比如说，源代码竟然是要下载的，而没有用“开源工业标准”的 github ，这多少有点让我不适应。</p>
<p>顺便 blah 一下，如今这世道， web 开发似乎已经渐渐地在更多转变为前端以 jquery 为基础的 ajax 开发了，换句话说，前端正在静态页面化。而所谓的后端系统也已经相对标准——更像是 restful 的 CRUD 接口再加上薄薄的一层业务逻辑。前端与后端之间的接口方式与约定已经相当之稳定。那么，比如说，有了 chicago boss 这么有生产力的架构，在后端性能开始吃紧的情况下，趁着系统升级的机会，简单地整一把，平滑地将后端系统“迁移”到更高效的平台上，未尝也不是一个不错的升级方向嘛。</p>