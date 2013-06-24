---
layout: post
title:  "ruby的to_proc"
date:   2010-10-21 00:41:43
author: 
categories: program
---

## ruby的to_proc
### by 
### at 2010-10-21 00:41:43
### original <http://www.javaeye.com/topic/790051>

1,先看api
<br><div>引用</div><div>Method#proc
<br>meth.to_proc =&gt; prc
<br>Returns a Proc object corresponding to this method.
<br>
<br>prc.to_proc → prc
<br>Part of the protocol for converting objects to Proc objects. Instances of class Proc simply return themselves.</div>
<br>
<br>2,to_proc经常和&amp;一起使用.
<br>可以把block传递给带block的方法.block的to_proc返回的就是本身.
<br>如果对象自带to_proc方法的话，就可以把它当作带&quot;&amp;&quot;的参数传给带block方法(默认状态下，Proc、Method对象都有to_proc方法)。方法调用时会执行to_proc，它将返回Proc对象.
<br><pre name="code">pobj = lambda {|v| p v }
[1,2,3].each(&amp;pobj)
=&gt; 1
   2
   3</pre>
<br>
<br><pre name="code">class Foo
  def to_proc
    lambda  {|v| p v}
  end
end
[1,2,3].each(&amp;Foo.new)

=&gt; 1
   2
   3</pre>
<br>
<br><pre name="code">class Symbol
  def to_proc
    proc { |obj, *args| obj.send(self, *args) }
  end
end
projects.collect(&amp;:name)
[1, 2, 3].map(&amp;:to_s.to_proc) #=&gt; [&quot;1&quot;, &quot;2&quot;, &quot;3&quot;]
</pre>
<br>
<br><pre name="code">
class Person &lt; ActiveRecord::Base; end;
[{:name =&gt; &#39;Dan&#39;}, {:name =&gt; &#39;Josh&#39;}].map(&amp;Person.method(:new))
class Class
  def to_proc
    proc(&amp;method(:new))
  end
end
[{:name =&gt; &#39;Dan&#39;}, {:name =&gt; &#39;Josh&#39;}].map(&amp;Person)
</pre>
<br>
<br>3,to_proc和method_missing
<br><pre name="code">
module Kernel   
  protected   
  def it() It.new end  
  alias its it   
end  
  
class It   
  
  undef_method(*(instance_methods - %w*__id__ __send__*))   
  
  def initialize   
    @methods = []   
  end  
  
  def method_missing(*args, &amp;block)   
    @methods &lt;&lt; [args, block] unless args == [:respond_to?, :to_proc]   
    self  
  end  
  
  def to_proc   
    lambda do |obj|   
      @methods.inject(obj) do |current,(args,block)|   
        current.send(*args, &amp;block)   
      end  
    end  
  end  
end 

File.read(&quot;/etc/passwd&quot;).split.sort_by &amp;it.split(&quot;:&quot;)[2]   
User.find(:all).map &amp;its.contacts.map(&amp;its.last_name.capitalize)  
</pre>
<br>
<br>4,Going crazy
<br>What I always regretted though was not being to pass any arguments, so I hacked and monkeypatched a bit, and got:
<br><pre name="code">

class Symbol

  def with(*args, &amp;block)
    @proc_arguments = { :args =&gt; args, :block =&gt; block }
    self
  end

  def to_proc
    @proc_arguments ||= {}
    args = @proc_arguments[:args] || []
    block = @proc_arguments[:block]
    @proc_arguments = nil
    Proc.new { |obj, *other| obj.send(self, *(other + args), &amp;block) }
  end

end

#So you can now write:
some_dates.map(&amp;:strftime.with(&quot;%d-%M-%Y&quot;))
</pre>
<br>Not that this is any shorter than just creating the darn block in the first place. But hey, it’s a good exercise in metaprogramming and show of more of Ruby’s awesome flexibility.
<br>
<br>After this I remembered something similar that annoyed me before. It’s that Rails helper methods are just a bag of methods available to, because they are mixed in your template. So if you have an array of numbers that you want to format as currency, you’d have to do:
<br><pre name="code">&lt;%= @prices.map { |price| number_to_currency(price) }.to_sentence %&gt;</pre>
<br>What if I could apply some to_proc-love to that too? All these helper methods cannot be added to strings, fixnums, and the likes; that would clutter way to much. Rather, it might by a nice idea to use procs that understands helper methods. Here is what I created:
<br><pre name="code">
module ProcProxyHelper

  def it(position = 1)
    ProcProxy.new(self, position)
  end

  class ProcProxy

    instance_methods.each { |m| undef_method(m) unless m.to_s =~ /^__|respond_to\?|method_missing/ }

    def initialize(object, position = 1)
      @object, @position = object, position
    end

    def to_proc
      raise &quot;Please specify a method to be called on the object&quot; unless @delegation
      Proc.new { |*values| @object.__send__(*@delegation[:args].dup.insert(@position, *values), &amp;@delegation[:block]) }
    end

    def method_missing(*args, &amp;block)
      @delegation = { :args =&gt; args, :block =&gt; block }
      self
    end

  end

end
</pre>
<br>I used a clean blank class (in Ruby 1.9, you’d want to inherit it from BasicObject), in which I will provide the proper proc-object. I play around with the argument list a bit, handling multiple arguments and blocks too. You can now use this syntax:
<br><pre name="code">
&lt;%= @prices.map(&amp;it.number_to_currency).to_sentence %&gt;</pre>
<br>That is a lot sexier if you as me. And you can use it in any object, not just inside views. And lets add some extra arguments and some Enumerator-love too:
<br><pre name="code">
class SomeClass
  include ProcProxyHelper

  def initialize(name, list)
    @name, @list = name, list
  end

  def apply(value, index, seperator)
    &quot;#{@name}, #{index} #{separator} #{value}&quot;
  end

  def applied_list
    @list.map.with_index(&amp;it.apply(&quot;:&quot;))
  end

end
</pre>
<br>In case you are wondering, the position you can specify is to tell where the arguments need to go. Position 0 is the method name, so you shouldn’t use that, but any other value is okay. An example might be that you cant to wrap an array of texts into span-tags:
<br><pre name="code">&lt;%= some_texts.map(&amp;it(2).content_tag(:span, :class =&gt; &quot;foo&quot;)).to_sentence %&gt;</pre>
<br>So there you have it. I’m probably solving a problem that doesn’t exist. It is however a nice example of the awesome power of Ruby. I hope you’ve enjoyed this little demonstration of the possible uses of to_proc.
<br>
<br>最后的一部分来自于http://iain.nl/2010/02/going-crazy-with-to_proc/
          
          <br><br>
          作者: <a href="http://orcl-zhang.javaeye.com">orcl_zhang</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/790051" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/439"><span style="color:blue;font-weight:bold">限时报名参加Oracle技术大会</span></a></li><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>