---
layout: post
title:  "Include vs. Extend"
date:   2010-09-17 06:00:00
author: 
categories: program
---

## Include vs. Extend
### by 
### at 2010-09-17 06:00:00
### original <http://feedproxy.google.com/~r/RubyQuicktips/~3/dqQqVfdqibk/1133877859>

<p>You can either use <code>include</code> or <code>extend</code> to mix in a module’s functionality into a class. The difference is this:</p>

<ul>
<li>
<code>include</code> makes the module’s methods available <strong>to the instance of a class</strong>, while</li>
<li>
<code>extend</code> makes these methods available <strong>to the class itself</strong>.</li>
</ul>
<p>Check out this example:</p>

<pre><code>module Greetings
  def say_hello
    puts &quot;Hello!&quot;
  end
end

class Human
  include Greetings
end

Human.new.say_hello # =&gt; &quot;Hello!&quot;
Human.say_hello     # NoMethodError

class Robot
  extend Greetings
end

Robot.new.say_hello # NoMethodError
Robot.say_hello     # =&gt; &quot;Hello!&quot;
</code></pre>

<p>If you want more information on <code>include</code> vs. <code>extend</code>, I recommend the following resources:</p>

<ul>
<li>
<a href="http://stackoverflow.com/questions/156362/what-is-the-difference-between-include-and-extend-in-ruby" title="What is the difference between include and extend in Ruby? - Stack Overflow">What is the difference between include and extend in Ruby?</a> on Stack Overflow.</li>
<li>
<a href="http://railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/" title="Include vs Extend in Ruby // RailsTips by John Nunemaker">Include vs Extend in Ruby</a> by John Nunemaker</li>
<li>
<a href="http://blog.jayfields.com/2006/05/ruby-extend-and-include.html" title="Jay Fields&#39; Thoughts: Ruby extend and include">Ruby extend and include</a> by Jay Fields</li>
</ul><img src="http://feeds.feedburner.com/~r/RubyQuicktips/~4/dqQqVfdqibk" height="1" width="1">