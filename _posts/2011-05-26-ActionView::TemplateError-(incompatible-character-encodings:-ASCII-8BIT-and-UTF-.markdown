---
layout: post
title:  "ActionView::TemplateError (incompatible character encodings: ASCII-8BIT and UTF-"
date:   2011-05-26 03:07:27
author: 
categories: program
---

## ActionView::TemplateError (incompatible character encodings: ASCII-8BIT and UTF-
### by 
### at 2011-05-26 03:07:27
### original <http://hlee.iteye.com/blog/1058790>

ruby 1.9.2和mysql 2.8.1的encode不匹配问题
<br>
<br>mysql 2.8.1有时候明明返回的是utf8但是却告诉ruby是ascii所以
<br>
<br><img src="http://dl.iteye.com/upload/attachment/487829/481cada8-cfb6-3339-97bd-be375ab24dfc.png">
<br>
<br>所以，要么呢装一个好点版本的msyql，要么呢，如下？
<br>
<br><pre name="code">
#lib/mysql_utf8.rb
require &#39;mysql&#39;

class Mysql::Result
  def encode(value, encoding = &quot;utf-8&quot;)
    String === value ? value.force_encoding(encoding) : value
  end
  
  def each_utf8(&amp;block)
    each_orig do |row|
      yield row.map {|col| encode(col) }
    end
  end
  alias each_orig each
  alias each each_utf8

  def each_hash_utf8(&amp;block)
    each_hash_orig do |row|
      row.each {|k, v| row[k] = encode(v) }
      yield(row)
    end
  end
  alias each_hash_orig each_hash
  alias each_hash each_hash_utf8
end
</pre>
<br>
<br>
<br>
<br>在environments相关环境里load一下，就可以了
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1058790#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>