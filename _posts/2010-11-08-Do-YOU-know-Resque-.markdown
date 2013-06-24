---
layout: post
title:  "Do YOU know Resque?"
date:   2010-11-08 08:15:29
author: Dave Hoover
categories: program
---

## Do YOU know Resque?
### by Dave Hoover
### at 2010-11-08 08:15:29
### original <http://feedproxy.google.com/~r/LearningRubyBlog/~3/Qk-7Ihss6cY/>

<p></p>
<div>
<h3>Do YOU know Resque?</h3>
<p>This guest post is by <strong><a href="http://redsquirrel.com/dave">Dave Hoover</a></strong>, who authored the book <a href="http://oreilly.com/catalog/9780596518387">Apprenticeship Patterns: Guidance for the Aspiring Software Craftsman</a> for O’Reilly, instigated the <a href="http://scna.softwarecraftsmanship.org/">Software Craftsmanship North America conference</a>, and is the Chief Craftsman at <a href="http://obtiva.com">Obtiva</a>. Dave began teaching himself to program in 2000, back when he was a family therapist. Dave lives near Chicago with his wife and three children. In his spare time, Dave competes in endurance sports.</p>
<p><img style="float:right;padding:4px;margin:0 0 2px 7px" src="http://rubylearning.com/images/davehoover.jpg" alt="Dave Hoover"> <span>W</span>eb developers can sometimes forget the importance of doing as little work as possible during the HTTP request-response life-cycle. When we’re developing new features, the simplest thing to do is just handle all the work that has been requested before responding, making the user wait, patiently watching their browser spin. This is nearly always a bad idea, and for reasons beyond user experience, most notably, it ties up a web process, making your site more likely to experience outages as traffic spikes. While it often makes sense to develop features with slow responses for your initial implementation, it’s usually unwise to deploy that version of the feature to your production environment. Thankfully, Ruby developers can choose from a number of “background job” libraries. I’m going to introduce you to <a href="http://github.com/defunkt/resque">Resque</a>, developed at <a href="http://github.com/">Github</a>, built on top of <a href="http://code.google.com/p/redis/">Redis</a>, an advanced key-value store which Resque uses for queuing.</p>
<p>One nice thing about Resque is that it’s not dependant on Rails or any web framework. This is great, because today, I’m not interested in writing a web application. I want to write a fast-running Ruby program that figures out what work needs to be done, tells someone else to do it, and then exits. (Similar to a web request, but simpler.) I’ll start with this:</p>
<pre>idea = ARGV
puts "Analyzing your idea: #{idea.join(" ")}"
idea.each do |word|
  puts "Asking for a job to analyze: #{word}"
  # This is where we would enqueue something
end</pre>
<p>If I named this program <tt>idea_analyzer.rb</tt> and it was in my current working directory, I could run it like this:</p>
<pre>$ ruby idea_analyzer.rb I will learn ruby
Analyzing your idea: I will learn ruby
Asking for a job to analyze: I
Asking for a job to analyze: will
Asking for a job to analyze: learn
Asking for a job to analyze: ruby</pre>
<p>As you can see, this program takes an “idea” from the command line and claims to ask for a “job” to analyze each word in the idea. Obviously, the next step is to actually ask for that job, instead of just talking about it. First, I’ll write the code that tells Resque to enqueue a job, and then we’ll get Resque in place. That might seem backward to some of you, since I’m writing code I know will fail, but “fast-failure” is a technique I use all the time, whether I’m practicing <a href="http://en.wikipedia.org/wiki/Test-driven_development">test-driven development</a>, or learning a new technology with a toy problem like this:</p>
<pre>idea = ARGV
puts "Analyzing your idea: #{idea.join(" ")}"
idea.each do |word|
  puts "Asking for a job to analyze: #{word}"
  Resque.enqueue(WordAnalyzer, word)
end</pre>
<p>Nice and simple. We’re calling a method on the Resque class. We’re passing in the word, but we’re also passing in the class <tt>WordAnalayzer</tt>. This is the only code that interacts directly with Resque. The <tt>enqueue</tt> method takes the name of the class responsible for doing the background work and the data required to accomplish the work, in this case the <tt>word</tt> variable. It will attempt to place a job in the appropriate queue. If we run the current version of this program, it fails like this:</p>
<pre>$ ruby idea_analyzer.rb I will learn ruby
Analyzing your idea: I will learn ruby
Asking for a job to analyze: I
idea_analyzer.rb:5: uninitialized constant Resque (NameError)
	from /tmp/stuff.rb:3:in `each'
	from /tmp/stuff.rb:3</pre>
<p>The <tt>uninitialized constant Resque</tt> error is telling me that Ruby doesn’t know about Resque yet. I can fix that by installing the Resque gem.</p>
<pre>$ gem install resque
Successfully installed resque-1.10.0
1 gem installed
Installing ri documentation for resque-1.10.0...
Installing RDoc documentation for resque-1.10.0...</pre>
<p>You’ll likely see other gems being installed as well, these are the gems that Resque depends on. Now I’ll just tell our program about Resque:</p>
<pre>require "resque"

idea = ARGV
puts "Analyzing your idea: #{idea.join(" ")}"
idea.each do |word|
  puts "Asking for a job to analyze: #{word}"
  Resque.enqueue(WordAnalyzer, word)
end</pre>
<p>When we run this, we’ll get a different error. Excellent! We’re making progress.</p>
<pre>$ ruby idea_analyzer.rb I will learn ruby
Analyzing your idea: I will learn ruby
Asking for a job to analyze: I
idea_analyzer.rb:7: uninitialized constant WordAnalyzer (NameError)
	from idea_analyzer.rb:5:in `each'
	from idea_analyzer.rb:5</pre>
<p>If you see an error like <tt>no such file to load -- resque</tt>, then you need to add <tt>require "rubygems"</tt> at the top of your program. You should eventually see the error about a missing <tt>WordAnalyzer</tt>. I’ll take care of that next by creating a <tt>word_analyzer.rb</tt> file, defining the class…</p>
<pre>class WordAnalyzer

end</pre>
<p>…and then require it.</p>
<pre>require "resque"
require "word_analyzer"

idea = ARGV
puts "Analyzing your idea: #{idea.join(" ")}"
idea.each do |word|
  puts "Asking for a job to analyze: #{word}"
  Resque.enqueue(WordAnalyzer, word)
end</pre>
<p>And this fails with a different error, we’re almost there!</p>
<pre>$ ruby idea_analyzer.rb I will learn ruby
Analyzing your idea: I will learn ruby
Asking for a job to analyze: I
/my/gems/resque-1.10.0/lib/resque/job.rb:44:in `create': Jobs must be placed onto a queue. (Resque::NoQueueError)
	from /my/gems/resque-1.10.0/lib/resque.rb:206:in `enqueue'
	from idea_analyzer.rb:8
	from idea_analyzer.rb:6:in `each'
	from idea_analyzer.rb:6</pre>
<p>Now our problem is that we haven’t specified a queue for the <tt>WordAnalyzer</tt> class. As its name suggests, Resque is all about queues. Each Resque class, such as <tt>WordAnalyzer</tt>, can specify its default queue, like this:</p>
<pre>class WordAnalyzer
  @queue = "word_analysis"
end</pre>
<p>Re-running this results in:</p>
<pre>$ ruby idea_analyzer.rb I will learn ruby
Analyzing your idea: I will learn ruby
Asking for a job to analyze: I
/my/gems/redis-2.0.10/lib/redis/client.rb:226:in `connect_to': Connection refused - Unable to connect to Redis on localhost:6379 (Errno::ECONNREFUSED)</pre>
<p>Resque is trying to enqueue a <tt>WordAnalyzer</tt> job for “I” on the <tt>word_analysis</tt> queue, and is using the default host (localhost) and port (6379). I’ll start Redis and our program should be much happier. I recommend installing Redis via <a href="https://github.com/antirez/redis/archives/master">https://github.com/antirez/redis/archives/master</a> with <tt>antirez-redis-v2.0.3-stable-0-gb766149.zip</tt>. Then starting it in a new console with <tt>redis-server</tt>. With that running, you can rerun your program and it should look like the output of the first version:</p>
<pre>$ ruby idea_analyzer.rb I will learn ruby
Analyzing your idea: I will learn ruby
Asking for a job to analyze: I
Asking for a job to analyze: will
Asking for a job to analyze: learn
Asking for a job to analyze: ruby</pre>
<p>But this time, after its quick run, it has left some work behind, sitting in Redis. You can see it if you type <tt>resque-web</tt> in your console. This will launch a browser and bring up a little <a href="http://www.sinatrarb.com/">Sinatra</a> app that ships with Resque, allowing you to watch the activity between Resque’s queues and workers. Now that we can see 4 jobs waiting patiently in the <tt>word_analysis</tt> queue, let’s get a worker started. The customary way to start Resque workers is via Rake, so I’ll create a <tt>Rakefile</tt> beside my other 2 files and just put this in the <tt>Rakefile</tt>:</p>
<pre>require "word_analyzer"
require "resque/tasks"</pre>
<p>Then, from the command line, I can start the worker with:</p>
<pre>rake resque:work QUEUE=*</pre>
<p>This will start a worker listening on all of Resque’s queues, and will never exit. If you want to stop it, just hit CTRL-C. Once it has run for a few seconds, refresh the browser you had pointing at resque-web, click-through to the failure queue, and you’ll see all the jobs failed with <tt>undefined method 'perform' for WordAnalyzer:Class</tt>. That’s a nice way of telling us it’s time to write the <tt>perform</tt> method for our Resque class:</p>
<pre>class WordAnalyzer
  @queue = "word_analysis"

  def self.perform(word)
    puts "About to do heavy duty analysis on #{word}"
    sleep 3 # fake analysis here
    # this would be something impressive
    puts "Finished with analysis on #{word}"
  end
end</pre>
<p>If your worker is still running, stop it with a CTRL-C. Then restart it via Rake so it loads up our new <tt>perform</tt> method. As you’ve probably guessed, Resque simply calls a method named <tt>perform</tt> on the class you enqueue. Be aware that any arguments you pass into <tt>Resque.enqueue</tt> are going to be serialized as <a href="http://www.json.org/">JSON</a>, which means Ruby Symbols will turn into Strings, and complex objects like instances of ActiveRecord will not work. When I need to work with ActiveRecords in Resque, I just pass their ids across and re-query them from the database.</p>
<p>Now that the worker is restarted and <tt>WordAnalyzer</tt> knows what to perform, our background processing system is ready. Start a new console and execute <tt>ruby idea_analyzer.rb I will learn ruby</tt>. Your Resque worker should perform some “successful” analysis over the course of about 12 seconds:</p>
<pre>$ rake resque:work QUEUE=*
(in /Users/redsquirrel/Desktop)
About to do heavy duty analysis on I
Finished with analysis on I
About to do heavy duty analysis on will
Finished with analysis on will
About to do heavy duty analysis on learn
Finished with analysis on learn
About to do heavy duty analysis on ruby
Finished with analysis on ruby</pre>
<p>That’s all there is to it. You can keep running your <tt>idea_analyzer.rb</tt> and the worker will keep analyzing words. Here is a visual workflow of this little system that may help clarify the different roles:</p>
<p style="text-align:center"><img src="http://rubylearning.com/images/resque_workflow.jpg" alt="resque_workflow"></p>
<p>We have a fast running program that queues work for later. We have a simple class that performs a time-consuming job, managed by a long-running Resque worker. We also have a web interface to monitor our queues and workers. These are the building blocks used by large-scale web sites like <a href="http://github.com/">Github</a>, <a href="http://madmimi.com/">Mad Mimi</a>, and <a href="http://groupon.com/">Groupon</a>, who leverage Resque for their mission critical background processing.</p>
<p><em>I hope you found this article valuable. Feel free to ask questions and give feedback in the comments section of this post. Thanks!</em></p>
<p><strong><em>Do also read</em> these awesome Guest Posts:</strong></p>
<ul>
<li><a href="http://rubylearning.com/blog/2010/11/03/do-you-understand-rubys-objects-messages-and-blocks/">Do You Understand Ruby’s Objects, Messages and Blocks?</a></li>
<li><a href="http://rubylearning.com/blog/2010/11/02/how-does-one-use-design-patterns-in-ruby/">How Does One Use Design Patterns In Ruby?</a></li>
<li><a href="http://rubylearning.com/blog/2010/10/26/do-you-know-whats-new-in-ruby-1-9/">Do you know what’s new in Ruby 1.9?</a></li>
<li><a href="http://rubylearning.com/blog/2010/10/25/the-value-of-a-personal-bug-log/">The value of a personal bug log</a></li>
<li><a href="http://rubylearning.com/blog/2010/10/18/do-you-enjoy-your-code-quality/">Do You Enjoy Your Code Quality?</a></li>
</ul>
</div>
<p>Technorati Tags: <a href="http://technorati.com/tag/Resque" rel="tag">Resque</a>, <a href="http://technorati.com/tag/Programming" rel="tag">Programming</a>, <a href="http://technorati.com/tag/Ruby+programming" rel="tag">Ruby programming</a></p>

<div>
<a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=Qk-7Ihss6cY:tF-Gne-8A3g:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=Qk-7Ihss6cY:tF-Gne-8A3g:MclgosYkiZA"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?i=Qk-7Ihss6cY:tF-Gne-8A3g:MclgosYkiZA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=Qk-7Ihss6cY:tF-Gne-8A3g:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?i=Qk-7Ihss6cY:tF-Gne-8A3g:F7zBnMyn0Lo" border="0"></a> <a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=Qk-7Ihss6cY:tF-Gne-8A3g:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=Qk-7Ihss6cY:tF-Gne-8A3g:gIN9vFwOqvQ"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?i=Qk-7Ihss6cY:tF-Gne-8A3g:gIN9vFwOqvQ" border="0"></a> <a href="http://feeds.feedburner.com/~ff/LearningRubyBlog?a=Qk-7Ihss6cY:tF-Gne-8A3g:-BTjWOF_DHI"><img src="http://feeds.feedburner.com/~ff/LearningRubyBlog?i=Qk-7Ihss6cY:tF-Gne-8A3g:-BTjWOF_DHI" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/LearningRubyBlog/~4/Qk-7Ihss6cY" height="1" width="1">