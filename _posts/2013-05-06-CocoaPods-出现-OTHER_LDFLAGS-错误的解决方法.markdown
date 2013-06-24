---
layout: post
title:  "CocoaPods 出现 OTHER_LDFLAGS 错误的解决方法"
date:   2013-05-06 17:52:36
author: ytzong
categories: program
---

## CocoaPods 出现 OTHER_LDFLAGS 错误的解决方法
### by ytzong
### at 2013-05-06 17:52:36
### original <http://www.99css.com/archives/1346>

<p>在一些项目中运行</p>
<pre><code>pod install</code></pre>
<p>后经常会出现如下错误</p>
<pre><code>[!] The target `项目名 [Debug]` overrides the `OTHER_LDFLAGS` build setting defined in `Pods/Pods.xcconfig'.
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.

[!] The target `项目名 [Debug - Release]` overrides the `OTHER_LDFLAGS` build setting defined in `Pods/Pods.xcconfig'.
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.</code></pre>
<p>对于我等菜鸟看得莫名奇妙，无从下手，那么如何解决呢？</p>
<ol>
<li>在 Finder 中找到 <strong>项目名.xcodeproj</strong>，右键选择<strong>显示包内容</strong></li>
<li>用文本编辑器打开 <strong>project.pbxproj</strong>，搜索
<pre><code>OTHER_LDFLAGS</code></pre>
</li>
<li>若项目未运行
<pre><code>pod install</code></pre>
<p>则会看到2处类似格式的语句</p>
<pre><code>OTHER_LDFLAGS = "";</code></pre>
<p>将其删除后运行</p>
<pre><code>pod install</code></pre>
<p>即可</p>
<p>若已运行</p>
<pre><code>pod install</code></pre>
<p>则删除2处如下格式的语句</p>
<pre><code>&lt;key&gt;OTHER_LDFLAGS&lt;/key&gt;
&lt;string&gt;&lt;/string&gt;</code></pre>
</li>
</ol>