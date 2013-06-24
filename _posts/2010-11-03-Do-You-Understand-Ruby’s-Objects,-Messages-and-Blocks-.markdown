---
layout: post
title:  "Do You Understand Ruby’s Objects, Messages and Blocks?"
date:   2010-11-03 13:06:25
author: Ed Howland
categories: program
---

## Do You Understand Ruby’s Objects, Messages and Blocks?
### by Ed Howland
### at 2010-11-03 13:06:25
### original <http://feedproxy.google.com/~r/LearningRubyBlog/~3/lMs70vWJPgs/>

<p></p>
<div>
<h3>Do You Understand Ruby’s Objects, Messages and Blocks?</h3>
<p>This guest post is by <strong><a href="http://greenprogrammer.wordpress.com/">Ed Howland</a></strong>, an independent consultant who has worked in Ruby and RoR for more than 5 years, since the 0.13 days of Rails. He has over 22 years in the software development industry working on mainly O-O projects in C/C++, Java, Perl and PHP. Ed is a frequent speaker at regional conferences and local Ruby/Linux user groups.</p>
<h3>Introduction</h3>
<p><img style="float:right;padding:4px;margin:0 0 2px 7px" src="http://rubylearning.com/images/ed_howland.jpg" alt="Ed Howland" title="Ed Howland" width="125" height="125"> <span>R</span>uby is a very elegant and expressive language. Consider the following complete Ruby program (one of the first examples I was exposed to, reconstructed from memory):</p>
<pre>#!/usr/bin/env ruby
Dir[ARGV.first+'/*'].each {|f| File.rename f, f.downcase}
</pre>
<p>Running <code>./rename.rb .</code> will rename all files in the current directory into their lowercase equivalents. Useful for importing a DOS/Windows directory to Linux. When I showed this to my friend Mark, an IT pro, he grokked it immediately.</p>
<p>But how does it work?</p>
<p>Actually, all that is going on here is a bunch of Ruby objects that are talking to each other. By deconstructing Ruby’s object-message protocol, we can analyze almost any Ruby code and discover how it works internally.</p>
<h3>Objects</h3>
<p>In Ruby, nearly everything is an object. Classically object-oriented languages define an object as something that maintains state and exhibits behavior. While this is true in Ruby, I think it is more precise to say that Ruby objects contain other objects and respond to messages. You might say that Ruby is a ‘message oriented’ language.</p>
<h3>Messages</h3>
<pre>:message [payload1, ... payloadN]
</pre>
<p>A Ruby message consists of a name, usually represented by a symbol such as :message, and an optional payload. The payload is often called the list of arguments or parameters. The payload, if present, is a list of other objects.</p>
<p>When the object receives the message, it handles it and returns some other object. Each object has a number of message handlers corresponding to the messages it responds to. In the Ruby program above, the fragment <code>f.downcase</code>, the ‘f’ variable, which is a string containing the current filename, is sent the message <code>:downcase</code>, and it returns a new string that is converted to lowercase.</p>
<p>Some other messages:</p>
<pre>1.send :+, 2
1.+ 2       # same thing
1 + 2       # same thing
</pre>
<p>You can use the <code>send</code> method on any object to invoke a message handler. Ruby gives us a lot of syntax sugar to make it easy to express messages. We don’t need to use the <code>send</code> method, we can just use the shortcut ‘.’ (dot) operator.</p>
<p>Ruby also provides some other syntactic sugar. In some cases, we can just use the message name inline, as in <code>1 + 2</code>. In another case, the :[] message can have its payload embedded between the braces. <code>Dir['*']</code> is the same as <code>Dir.[]('*')</code>. There is also the ‘=’ message, which can be appended to another message name, such as an instance variable setter and then used inline. <code>a.iv=(13)</code> is the same as <code>a.iv = 13</code>. (This is what happens when you use the <code>attr_writer</code> to declare an instance variable set method.)</p>
<p>The payload of a message can be defined as having (in this sequence) required arguments, optional (or default) arguments, a variable number of arguments and an implicit or explicit block. Default arguments are specified with <code>arg=value</code>. The variable arguments are collected in ‘‘splatted’ (‘*’) parameter. In the message handler, every actual argument is collected in a list.</p>
<p>The explicit block argument is declared with a preceding ‘&amp;’, as in <code>&amp;blk</code>. Default arguments are often used to emulate named parameters to a message handler. Setting a parameter to the empty hash: <code>{}</code> lets us pass in a (bare) hash as the last parameter: <code>object.method :find=&gt;true, :query=&gt;&#39;fred&#39;</code>. You see that in a lot of Rails code.</p>
<p>Here is a complete example:</p>
<pre>def message required, default={}, *variable, &amp;block</pre>
<p>We can query an object to see if it understands a certain message, with the :responds_to? message.</p>
<pre>&quot;AAAAA&quot;.respond_to? :downcase
 =&gt; true
</pre>
<p>How does an object handle a message? We first need to look at blocks, a way to encapsulate Ruby code in an object. We will then expand that concept to methods, another type of object that contains code.</p>
<h3>Blocks</h3>
<p>A block is a special kind of object that contains executable code. In fact, all Ruby code is evaluated in the context of a block. Many other blocks can be declared and since they are just objects they can be used like any object. Particularly, they can be passed in the payload of a message.</p>
<p>There are a variety of synonyms for blocks: ‘lambdas’, ‘procs’ and ‘closures’. In most cases <sup><a href="http://rubylearning.com/blog#fn:1" rel="footnote">1</a></sup>, <sup><a href="http://rubylearning.com/blog#fn:2" rel="footnote">2</a></sup>, these refer to the same thing, an instance of the <code>Proc</code> class. <code>Proc</code> objects understand a variety of messages, the most significant of which is :call. The :call message invokes the code in the block.</p>
<p>We can create a proc with any of the block keywords ‘proc’, ‘lambda’ and ‘-&gt;’ (the later form. ‘-&gt;’ is new to Ruby 1.9 and above). Another way is to send the :new message to the <code>Proc</code> class itself. The code block needs to be surrounded by curly braces ‘{}’ or by <code>do ... end</code>.</p>
<pre>p=proc {puts &quot;in block&quot;}
 =&gt; #&lt;Proc:0x0000010095ca30@(irb):5&gt;
p.call
in block
</pre>
<p>The :call message can have a payload like any other message. Inside the block, these payload objects are bound to variables defined between ‘||’ in the front of the declaration. Collection iterators apply this approach calling the passed in block once for every element in the collection passing the element to the block. Let’s re-implement the :join message with the :inject message.</p>
<pre>[&quot;aa&quot;,&quot;bb&quot;, &quot;cc&quot;].inject([&#39;&#39;,&#39;&#39;]) {|accum,v| [&#39;, &#39;, accum[1]+accum[0]+v]}[1]
 =&gt; &quot;aa, bb, cc&quot;
</pre>
<p>The result of the last expression evaluated in the block is returned from the :call invocation. You can also explicitly return an expression with the <code>return</code> keyword at any point</p>
<p>Procs or lambdas are also closures. I admit, it took me a long time to understand what that means. The most understandable closures I’ve seen involve code that creates code based on passed in parameters. Assume we want to make some micro-blogging formatters. We can combine some functions to carry out a simple setup. (Remember that blocks passed in are part of the saved context too.)</p>
<pre>mbmakr = proc do |&amp;blk|
  proc {|*args| blk.call(*args)[0..139]}
end

tweetr = mbmakr.call {|msg| msg}
reply = mbmakr.call {|user, msg| &quot;@&quot;+user+&quot;: &quot;+msg}
retweetr = mbmakr.call {|user, msg| &#39;RT &#39;+reply.call(user, msg)}
dmesgr= mbmakr.call {|user, msg| &#39;D &#39;+user+&#39; &#39;+msg}

retweetr.call &quot;john_x&quot;, &quot;French tomato soup is tasty! #protip&quot;
 =&gt; &quot;RT @john_x: French tomato soup is tasty! #protip&quot;
</pre>
<p>Wow, code making code! C-3PO would be amazed.</p>
<p>The context captured by a block can be retrieved via the :binding message. We can pass the binding object the :eval message and a string containing some Ruby code. The code will be evaluated in the context of the binding. This is very useful in templating systems, for example. Passing the current binding to ERb, local variables will be interpreted in the context of the template. Remember that the even the outermost code is running in a block.</p>
<pre>require &quot;erb&quot;

template=ERB.new &quot;~ &lt;%= a %&gt; ~&quot;
a=&quot;hi there&quot;
template.result binding
 =&gt; &quot;~ hi there ~&quot;
</pre>
<p>Procs also can be queried as to their number of arguments with :arity message. A bit out of our scope, so I refer you to the Proc class documentation. We also will look at this a bit more when we discuss Methods.</p>
<p>The last step on our journey to the actual implementation of a message handler are ‘methods’.</p>
<h3>Methods</h3>
<p>If we combine a block with a name and bind it to an owner (the <em>module</em> to which it belongs) and a recipient (the <em>object</em> to which it belongs), we get a message handler. These are called ‘methods’ which are objects too. Methods objects can only exist in the context of a module. Modules and classes are outside the scope of this article, but fortunately we can get access to the current class with the combined message <code>self.class</code>.</p>
<p>One way to create a method is to send the module object the :define_method message which takes a symbol for the method name and a block containing the code to be executed when objects get that message.</p>
<pre>self.class.send(:define_method, :greet) {|user| puts &quot;hello #{user}&quot;}
 =&gt; #&lt;Proc:0x000001008e4b48@(irb):78 (lambda)&gt;
greet &quot;Fred&quot;
hello Fred
</pre>
<p>We can access the actual method object by sending the class the :method message. Then we can query its attributes.</p>
<pre>m=self.class.method :greet
=&gt; #&lt;Method: Class(Object)#greet&gt;
puts m.name, m.owner, m.receiver
greet
Object
Object
</pre>
<p>And, just like a block, we can invoke the method’s code block via its :call message.</p>
<p>There is an easier way to define a method within a module: the <code>def method args ... end</code> pattern:</p>
<pre>def who_am_i user
    "I am #{user}"
  end
</pre>
<p>Let’s combine all we have learned so far. Imagine we’d like a very idiomatic Ruby way to create HTML text. </p>
<pre>def opt_to_s opt={}
  opt.empty? ? &#39;&#39; : &#39; &#39; + opt.map {|e,v| &quot;#{e}=\&quot;#{v}\&quot;&quot;}.join(&#39;, &#39;)
end

[:html, :body, :h1].each do |el|
  start=&quot;&lt;#{el}&quot;
  fin=&quot;&lt;/#{el}&gt;&quot;
  self.class.send(:define_method, el) {|options={}, &amp;blk| start + opt_to_s(options) + &#39;&gt;&#39; + blk.call + fin}
end

# Now, we can neatly nest tags and content
html do
  body do
    h1 :class=&gt;&quot;bold-h1&quot;, :id=&gt;&quot;h1_99&quot; do
      &quot;header&quot;
    end
  end
end
 =&gt; &quot;&lt;html&gt;&lt;body&gt;&lt;h1 class=\&quot;bold-h1\&quot;, id=\&quot;h1_99\&quot;&gt;header&lt;/h1&gt;&lt;/body&gt;&lt;/html&gt;&quot;
</pre>
<p>Inside the array’s <code>each</code> iterator. methods are created using a closure containing the contracted start and fin tag fragments.</p>
<p>Ruby 1.9.2 gives you a nice way to query method object parameters. </p>
<pre>def x(a, b, c=3, *d, &amp;e); end
self.class.method(:x).parameters
 =&gt; [[:req, :a], [:req, :b], [:opt, :c], [:rest, :d], [:block, :e]]
</pre>
<p>This can also be accomplished with the ‘methopara’ gem in 1.9.1.</p>
<p>What happens when an object gets a message that it doesn’t understand? Ruby will invoke the <code>:method_missing</code> message on that object. That handler is defined in Object, but you can override it in your own classes. Custom method_missing handlers can lead to very dynamic code that self adjusts to runtime conditions.</p>
<h3>Summary</h3>
<p>The message-object protocol is fundamental to the way Ruby works. Messages are sent to objects to perform some task. Within the object, a message handler responds to the message and executes some code. Code blocks or procs can be created and executed at any time. A method is an enhancement of a proc  that is bound to an object and named for the message it handles. Examining this interaction between Ruby objects at the message and block level leads to a deeper understanding of the way Ruby works and should help you write more expressive code.</p>
<p>I urge you to explore the Proc, Module and Method class documentation and experiment with creating your own blocks.</p>
<div>
<ol>
<li>
<p>As we will see when we discuss the ‘yield’ operator, inline blocks passed implicitly in a message call, is not really an instance of Proc. This is presumably a performance optimization.<a href="http://rubylearning.com/blog#fnref:1">↩</a></p>
</li>
<li>
<p>There is also a subtle difference between the <code>proc</code> and <code>lambda</code> declarations. A lambda will throw an ArgumentError if the number of supplied arguments to :call do not match the number of required arguments, whereas proc will just set these to nil, or just drop any extra ones. Inline blocks behave more like procs than lambdas.<a href="http://rubylearning.com/blog#fnref:2">↩</a></p>
</li>
</ol>
</div>
<p><em>I hope you found this article valuable. Feel free to ask questions and give feedback in the comments section of this post. Thanks!</em></p>
<p><strong><em>Do also read</em> these awesome Guest Posts:</strong></p>
<ul>
<li><a href="http://rubylearning.com/blog/2010/11/02/how-does-one-use-design-patterns-in-ruby/">How Does One Use Design Patterns In Ruby?</a></li>
<li><a href="http://rubylearning.com/blog/2010/10/26/do-you-know-whats-new-in-ruby-1-9/">Do you know what’s new in Ruby 1.9?</a></li>
<li><a href="http://rubylearning.com/blog/2010/10/25/the-value-of-a-personal-bug-log/">The value of a personal bug log</a></li>
<li><a href="http://rubylearning.com/blog/2010/10/18/do-you-enjoy-your-code-quality/">Do You Enjoy Your Code Quality?</a></li>
</ul>
</div>
<p>Technorati Tags: <a href="http://technorati.com/tag/Ruby+Objects" rel="tag">Ruby Objects</a>, <a href="http://technorati.com/tag/Ruby+Messages" rel="tag"> Ruby Messages</a>, <a href="http://technorati.com/tag/Ruby+Blocks" rel="tag">Ruby Blocks</a>, <a href="http://technorati.com/tag/Programming" rel="tag">Programming</a>, <a href="http://technorati.com/tag/Ruby+programming" rel="tag">Ruby programming</a></p>

<div>
<a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=lMs70vWJPgs:AG1ZEzn_n44:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=lMs70vWJPgs:AG1ZEzn_n44:MclgosYkiZA"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?i=lMs70vWJPgs:AG1ZEzn_n44:MclgosYkiZA" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/LearningRubyBlog/~4/lMs70vWJPgs" height="1" width="1">