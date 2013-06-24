---
layout: post
title:  "MongoMapper 0.8: Goodies Galore"
date:   2010-06-17 04:03:07
author: John Nunemaker
categories: program
---

## MongoMapper 0.8: Goodies Galore
### by John Nunemaker
### at 2010-06-17 04:03:07
### original <http://feedproxy.google.com/~r/railstips/~3/colOUOXk9i0/>

<p>Let me tell you, this release has been a tough one. It is made up of <a href="http://github.com/jnunemaker/plucky/compare/0bdc4998df86febc1fcd...master">43 commits</a> to Plucky and <a href="http://github.com/jnunemaker/mongomapper/compare/69fe5a0654b0eba8b880bfe86ade599d5e9c8bf9...master">92 commits</a> to MongoMapper. Features added include a sexy query language, scopes, attr_accessible, a fancy cache key helper, a :typecast option for array/set keys, and a bajillion little improvements. Let’s run through each of them just for fun.</p>
<h2>Sexy Query Language</h2>
<p>This right here is all thanks to <a href="http://github.com/jnunemaker/plucky">plucky</a>. The goal for plucky is a fancy query language on top of MongoDB. It has been created in a such a way that other MongoDB projects (Mongoid, Candy, MongoDoc, etc.) can benefit from it if they wish. It still has a long way to go in covering edge cases and deeply nested queries, but the majority of queries one will do are covered quite nicely.</p>
<pre><code>User.where(:age.gt =&gt; 27).sort(:age).all
User.where(:age.gt =&gt; 27).sort(:age.desc).all
User.where(:age.gt =&gt; 27).sort(:age).limit(1).all
User.where(:age.gt =&gt; 27).sort(:age).skip(1).limit(1).all</code></pre>
<p>All of the above are supported out of the box. Each query method (limit, reverse, update, skip, fields, sort, where) returns a Plucky::Query object so they can be changed together until a kicker is hit, such as all, first, last, paginate, count, size, each, etc. It is <strong>fashioned in a similar form as ARel</strong> in this manner, but more simple as ARel has to handle a lot more than just simple queries (joins, etc).</p>
<h2>Scopes</h2>
<p>The main thing I was waiting for to do scopes was to get plucky to a point where scopes would be just a sprinkling of code to merge plucky queries. Thankfully that day has finally arrived and with this latest release, you can now scope away. The code is so compact, that I figured I would drop it in here for those that are curious:</p>
<pre><code>module MongoMapper
  module Plugins
    module Scopes
      module ClassMethods
        def scope(name, scope_options={})
          scopes[name] = lambda do |*args|
            result = scope_options.is_a?(Proc) ? scope_options.call(*args) : scope_options
            result = self.query(result) if result.is_a?(Hash)
            self.query.merge(result)
          end
          singleton_class.send :define_method, name, &amp;scopes[name]
        end

        def scopes
          read_inheritable_attribute(:scopes) || write_inheritable_attribute(:scopes, {})
        end
      end
    end
  end
end</code></pre>
<p>Yep, that is it. With that bit of code, you can now do stuff like this:</p>
<pre><code>class User
  include MongoMapper::Document

  # plain old vanilla scopes with fancy queries
  scope :johns,   where(:name =&gt; &#39;John&#39;)

  # plain old vanilla scopes with hashes
  scope :bills, :name =&gt; &#39;Bill&#39;

  # dynamic scopes with parameters
  scope :by_name,  lambda { |name| where(:name =&gt; name) }
  scope :by_ages,  lambda { |low, high| where(:age.gte =&gt; low, :age.lte =&gt; high) }

  # Yep, even plain old methods work as long as they return a query
  def self.by_tag(tag)
    where(:tags =&gt; tag)
  end

  # You can even make a method that returns a scope
  def self.twenties; by_ages(20, 29) end

  key :name, String
  key :tags, Array
end

# simple scopes
pp User.johns.first
pp User.bills.first

# scope with arg
pp User.by_name(&#39;Frank&#39;).first

# scope with two args
pp User.by_ages(20, 29).all

# chaining class methods on scopes
pp User.by_ages(20, 40).by_tag(&#39;ruby&#39;).all

# scope made using method that returns scope
pp User.twenties.all</code></pre>
<p>I am sure there are some edge cases, but I cannot wait to start swapping some of the code I have out for scopes. This is definitely <strong>one of the features I missed most from ActiveRecord</strong>.</p>
<h2>attr_accessible</h2>
<p>Previously, MongoMapper only supported attr_protected. The main reason was that I am lazy and someone from the community contributed the beginnings of the code. I spent some time today adding attr_accessible, so now you can really <strong>lock down your models</strong> if you want to.</p>
<pre><code>class User
  include MongoMapper::Document

  attr_accessible :first_name, :last_name, :email

  key :first_name, String
  key :last_name, String
  key :email, String
  key :admin, Boolean, :default =&gt; false
end</code></pre>
<p>Based on the example above, only first_name, last_name and email can be assigned when using mass assignment, such as in <code>.new</code> or <code>#update_attributes</code>.</p>
<h2>Cache Key</h2>
<p>On a recent MongoMapper project, I had to some caching. This led me to create <a href="http://github.com/jnunemaker/bin">bin</a>, a MongoDB ActiveSupport cache store. The first thing you notice when you start to cache stuff is that you need a key to name the cached object or fragment. I dug around in AR and discovered the cache_key method. MongoMapper’s cache_key works the same with a little twist. You can <strong>pass arguments</strong> to it and they will <strong>become suffixes</strong> on the cache key. Lets look at an example:</p>
<pre><code>class User
  include MongoMapper::Document
end

User.new.cache_key # =&gt; &quot;User/new&quot;
User.create.cache_key # =&gt; &quot;User/:id&quot;
User.create.cache_key(:foo, :bar) # =&gt; &quot;User/:id/foo/bar&quot;</code></pre>
<p>It should also be noted that if the User model <strong>has an updated_at key</strong>, that will be appended after the id like so User/:id-:timestamp. This addition is definitely going to clean up some code on a project of mine.</p>
<h2>Typecasting Array/Set values</h2>
<p>A common idiom in MongoDB modeling is to use Array keys for many to many type relationships. You have a User model and a Site model. Sites can have many Users and Users can have many Sites. Typically, I make a key :user_ids, Array and store the ids of each user that has access to the site.</p>
<p>When this is done from web forms, everything comes in as a string, so you have to typecast those strings to object ids. The new :typecast option wraps this up in a single key/value.</p>
<pre><code>class Site
  include MongoMapper::Document
  key :user_ids, Array, :typecast =&gt; &#39;ObjectId&#39;
end</code></pre>
<p>Now, whenever user_ids is assigned, each value gets typecast to an ObjectId. This will work with <strong>any class that defines the to_mongo class method</strong>, which means you can use it with custom types as well.</p>
<h2>Conclusion</h2>
<p>I learned <strong>more about Ruby</strong> while working on this release of MongoMapper than probably any other period in my brief history. I really feel like this release brings MongoMapper to the forefront of MongoDB/Ruby object mappers.</p>
<p>All the typical dressings are now in place and with a few more tweaks, I can smell 1.0. Hope you all find this stuff useful and as always, if you don’t, that is ok because I am enjoying the heck out of working on this stuff. :)</p>
<p>Oh, and if all of this above did not excite you, know that the <strong>new MongoMapper site</strong>, including full documentation, is <a href="http://frst.in/~mLh">well underway</a> and should be ready for consumption soon. Hooray!</p><div>
<a href="http://feeds.feedburner.com/~ff/railstips?a=colOUOXk9i0:MLvWl7V2RII:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/railstips?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/railstips?a=colOUOXk9i0:MLvWl7V2RII:dnMXMwOfBR0"><img src="http://feeds.feedburner.com/~ff/railstips?d=dnMXMwOfBR0" border="0"></a> <a href="http://feeds.feedburner.com/~ff/railstips?a=colOUOXk9i0:MLvWl7V2RII:7Q72WNTAKBA"><img src="http://feeds.feedburner.com/~ff/railstips?d=7Q72WNTAKBA" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/railstips/~4/colOUOXk9i0" height="1" width="1">