---
layout: post
title:  "Rails integrate with Google analytics Api"
date:   2013-02-21 22:57:00
author: 
categories: program
---

## Rails integrate with Google analytics Api
### by 
### at 2013-02-21 22:57:00
### original <http://hlee.iteye.com/blog/1812201>

<a href="https://github.com/chrisle/gattica">https://github.com/chrisle/gattica</a>
<br>
<br><a href="https://github.com/robmckinnon/rugalytics">https://github.com/robmckinnon/rugalytics</a>
<br>
<br><a href="https://www.ruby-toolbox.com/categories/Web_Analytics">https://www.ruby-toolbox.com/categories/Web_Analytics</a>
<br>
<br>Google api Doc
<br>
<br><a href="https://developers.google.com/analytics/devguides/reporting/core/v3/">https://developers.google.com/analytics/devguides/reporting/core/v3/</a>
<br>
<br>
<br>
<br><pre name="code">
# Include the gem
require &#39;gattica&#39;

# Login
ga = Gattica.new({ 
    :email =&gt; &#39;email@gmail.com&#39;, 
    :password =&gt; &#39;password&#39;
})

# Get a list of accounts
accounts = ga.accounts

# Choose the first account
ga.profile_id = accounts.first.profile_id

# Get the data
data = ga.get({ 
    :start_date   =&gt; &#39;2011-01-01&#39;,
    :end_date     =&gt; &#39;2011-04-01&#39;,
    :dimensions   =&gt; [&#39;month&#39;, &#39;year&#39;],
    :metrics      =&gt; [&#39;visits&#39;, &#39;bounces&#39;],
})

# Show the data
puts data.inspect


# Sorting by number of visits in descending order (most visits at the top)
data = ga.get({ 
    :start_date   =&gt; &#39;2011-01-01&#39;,
    :end_date     =&gt; &#39;2011-04-01&#39;,
    :dimensions   =&gt; [&#39;month&#39;, &#39;year&#39;],
    :metrics      =&gt; [&#39;visits&#39;],
    :sort         =&gt; [&#39;-visits&#39;]
})


# Return visits and bounces for mobile traffic 
# (Google&#39;s default user segment gaid::-11)

mobile_traffic = ga.get({ 
  :start_date   =&gt; &#39;2011-01-01&#39;, 
  :end_date     =&gt; &#39;2011-02-01&#39;, 
  :dimensions   =&gt; [&#39;month&#39;, &#39;year&#39;],
  :metrics      =&gt; [&#39;visits&#39;, &#39;bounces&#39;],
  :segment      =&gt; &#39;gaid::-11&#39;
})


# Filter by Firefox users
firefox_users = ga.get({
  :start_date   =&gt; &#39;2010-01-01&#39;, 
  :end_date     =&gt; &#39;2011-01-01&#39;,
  :dimensions   =&gt; [&#39;month&#39;, &#39;year&#39;],
  :metrics      =&gt; [&#39;visits&#39;, &#39;bounces&#39;],
  :filters      =&gt; [&#39;browser == Firefox&#39;]
})

# Filter where visits is &gt;= 10000
lots_of_visits = ga.get({
  :start_date   =&gt; &#39;2010-01-01&#39;, 
  :end_date     =&gt; &#39;2011-02-01&#39;,
  :dimensions   =&gt; [&#39;month&#39;, &#39;year&#39;],
  :metrics      =&gt; [&#39;visits&#39;, &#39;bounces&#39;],
  :filters      =&gt; [&#39;visits &gt;= 10000&#39;]
})


# Get the top 25 keywords that drove traffic
data = ga.get({ 
  :start_date =&gt; &#39;2011-01-01&#39;,
  :end_date =&gt; &#39;2011-04-01&#39;,
  :dimensions =&gt; [&#39;keyword&#39;],
  :metrics =&gt; [&#39;visits&#39;],
  :sort =&gt; [&#39;-visits&#39;],
  :max_results =&gt; 25 
})

# Output our results
data.points.each do |data_point|
  kw = data_point.dimensions.detect { |dim| dim.key == :keyword }.value
  visits = data_point.metrics.detect { |metric| metric.key == :visits }.value
  puts &quot;#{visits} visits =&gt; &#39;#{kw}&#39;&quot;
end

# =&gt;
#   19667 visits =&gt; &#39;(not set)&#39;
#   1677 visits =&gt; &#39;keyword 1&#39;
#   178 visits =&gt; &#39;keyword 2&#39;
#   165 visits =&gt; &#39;keyword 3&#39;
#   161 visits =&gt; &#39;keyword 4&#39;
#   112 visits =&gt; &#39;keyword 5&#39;
#   105 visits =&gt; &#39;seo company reviews&#39;
#   ...
</pre>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1812201#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>