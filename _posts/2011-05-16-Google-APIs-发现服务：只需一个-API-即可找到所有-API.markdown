---
layout: post
title:  "Google APIs 发现服务：只需一个 API 即可找到所有 API"
date:   2011-05-16 09:22:49
author: xslidian
categories: program
---

## Google APIs 发现服务：只需一个 API 即可找到所有 API
### by xslidian
### at 2011-05-16 09:22:49
### original <http://www.guao.hk/posts/google-apis-discovery-service-one-api-to-find-them-all.html>

<p>如 <a title="Google I/O 2010 - How Google builds APIs" href="http://www.youtube.com/watch?v=nyu5ZxGUfgs">Google I/O  2010 所宣布的</a>，新系列的 Google APIs 客户端库运行在一个全新的 API 架构之上，允许 Google 降低发布与维护客户端库所需的工作量。该架构是通过一个简单的 API 实现的，它提供了机器可读的 Google APIs 描述，客户端库可以利用这些信息与所有 API 交互。</p>
<p>今天发布的 <a href="https://code.google.com/apis/discovery/">Google APIs 发现服务</a>就是新版客户端库背后的秘密武器。该服务提供关于 Google APIs 的机器可读的元数据，包括：</p>
<ul>
<li>支持的 API 目录。</li>
<li>各支持 API 的“发现文档”，包括：
<ul>
<li>基于 <a href="http://json-schema.org/">JSON Schema</a> 的 API 资源纲要列表。</li>
<li>API 方法列表，及各方法可用的参数。</li>
<li>可用的 <a href="http://code.google.com/apis/accounts/docs/OAuth2.html">OAuth 2.0</a> 权限（scope）列表。</li>
</ul>
</li>
<li>行内的方法、参数以及可用参数值文档。</li>
</ul>
<p>该服务可通过轻量级的基于 JSON 的 API 访问。请转至 <a href="https://www.googleapis.com/discovery/v1/apis">https://www.googleapis.com/discovery/v1/apis</a> 看看有哪些数据可用。</p>
<p>您可以使用 APIs 发现服务创建用于与 Google APIs 交互的工具，如 IDE 插件和客户端库。Google 自己就采用该服务创建了一些这类工具：</p>
<ul>
<li><a href="http://code.google.com/apis/discovery/libraries.html">Google APIs 客户端库</a>——如上所述</li>
<li><a href="https://code.google.com/apis/explorer/">Google APIs Explorer</a>——互动性基于 web 的 Google APIs 探索工具</li>
<li><a href="http://code.google.com/eclipse/docs/googleapis.html">Google Plugin for Eclipse 中的 Google API 支持</a>——一项 IDE 整合，允许直接将 Google APIs 支持导入到 Eclipse 项目中</li>
</ul>
<p>在发布该服务的同时，Google 同时<a href="http://code.google.com/p/google-apis-explorer/">开放了 APIs Explorer 的源代码</a>，可作为如何使用这些服务的很好的例子。</p>
<p>关于 Google APIs 发现服务的详情可参阅<a href="https://code.google.com/apis/discovery/">文档</a>，或者在 <a href="http://code.google.com/apis/explorer/#_s=discovery&amp;_v=v1">APIs Explorer</a> 中探索其 API。本届 Google I/O 上还有“<a href="http://www.google.com/events/io/2011/sessions.html#building-custom-client-libraries-for-google-apis">构建自定义 Google APIs  客户端库</a>”的议程，感兴趣的同学可前往 YouTube 观看<a title="Google I/O 2011: Building Custom Client Libraries for Google APIs" href="http://www.youtube.com/watch?v=lQbT1NrxpUo">录像</a>。</p>
<p>via <a title="Google APIs Discovery Service: one API to find them all" href="http://googlecode.blogspot.com/2011/05/google-apis-discovery-service-one-api.html">Google Code Blog</a></p>
<hr>
<p><small>© xslidian 发表于 <a href="http://www.guao.hk">谷奥——探寻谷歌的奥秘 ( http://www.guao.hk )</a>, 2011.  |
<a href="http://www.guao.hk/posts/google-apis-discovery-service-one-api-to-find-them-all.html#comments">2 条评论</a> |
<a href="http://www.guao.hk/posts/google-apis-discovery-service-one-api-to-find-them-all.html">永久链接</a> |
<a href="http://google.org.cn/about/">关于谷奥</a> |
<a href="http://google.org.cn/submit/">投稿/爆料</a><br>
Post tags: <a href="http://www.guao.hk/tag/api" rel="tag">API</a>, <a href="http://www.guao.hk/tag/google-apis-explorer" rel="tag">Google APIs Explorer</a>
</small></p>
<img src="http://img.tongji.linezing.com/1105192/tongji.php" border="0" width="0" height="0">