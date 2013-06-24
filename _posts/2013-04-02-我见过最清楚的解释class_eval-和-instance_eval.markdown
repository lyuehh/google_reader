---
layout: post
title:  "我见过最清楚的解释class_eval 和 instance_eval"
date:   2013-04-02 07:06:59
author: 
categories: program
---

## 我见过最清楚的解释class_eval 和 instance_eval
### by 
### at 2013-04-02 07:06:59
### original <http://hlee.iteye.com/blog/1840489>

忘了，看一次就能回忆起来
<br><pre name="code">
class A
  # defs here go to A
  puts self  # =&gt; A
  class &lt;&lt; self
     #defs here go to A&#39;s eigenclass
  end
end

A.class_eval do
  #defs here go to A
end

A.instance_eval do
  #defs here go to A&#39;s eigenclass     
end

s = &quot;Hello World&quot;

class &lt;&lt; s
  #defs here go to s&#39;s eigenclass
end
</pre>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1840489#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>