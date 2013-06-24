---
layout: post
title:  "Invalid gemspec because of the date format in specification"
date:   2011-11-07 02:14:46
author: 
categories: program
---

## Invalid gemspec because of the date format in specification
### by 
### at 2011-11-07 02:14:46
### original <http://hlee.iteye.com/blog/1236965>

又是这个date format的错误。
<br>
<br>上次出错忘了，记录下来，可好又得查一遍，赶快抄回来。
<br>
<br><pre name="code">
Invalid gemspec in [/home/ra/.rvm/gems/ruby-1.9.2-p180/specifications/json_pure-1.6.1.gemspec]: invalid date format in specification: "2011-09-18 00:00:00.000000000Z"

</pre>
<br>
<br>总之，问题就是这个格式的时间，认不出来
<br>
<br>那么理论上只要
<br>
<br>在给出的问题gem包里把
<br><pre name="code">
"s.date = %q{2011-05-21 00:00:00.000000000Z}" 
#改成
"s.date = %q{2011-05-21}"</pre>
<br>
<br>也有说可以gem update或者再装解决
<br>
<br>我比较推荐
<br>
<br><pre name="code">
#/home/user/.rvm/gems/ruby-1.9.2-p180/specifications/
grep -i *.gemspec -e '.*s\.date.*=.*%q{\(....-..-..\) \(.*Z\)}
</pre>
<br>
<br>或者
<br>
<br><pre name="code">
sed -i -e 's/\(.*\)s\.date.*=.*%q{\(....-..-..\) \(.*Z\)}/\1s.date = %q\{\2}/p' ./*.gemspec
</pre>
<br>shell脚本多文件替换，方便快捷
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1236965#comments" style="color:red">已有 <strong>1</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>