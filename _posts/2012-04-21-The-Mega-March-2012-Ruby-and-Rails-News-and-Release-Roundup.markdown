---
layout: post
title:  "The Mega March 2012 Ruby and Rails News and Release Roundup"
date:   2012-04-21 06:13:28
author: Peter Cooper
categories: program
---

## The Mega March 2012 Ruby and Rails News and Release Roundup
### by Peter Cooper
### at 2012-04-21 06:13:28
### original <http://feedproxy.google.com/~r/RubyInside/~3/gqpGiGNYKIE/march-2012-ruby-news-5841.html>

<p><img src="http://www.rubyinside.com/wp-content/uploads/2012/03/mega.png" alt="" title="mega" width="140" height="111" style="float:right;margin-left:12px;margin-bottom:12px">And again, a mixture of travel, illness, and exhaustion have prevented me from my weekly updates on here (although <a href="http://rubyweekly.com/">Ruby Weekly</a> is still going out on a weekly basis!) so here's a bumper update for all of the top Ruby and Rails news from March 2012.</p>
<p>Highlights include: Matz wins a prize, Ruby is approved by the ISO, some awesome <a href="http://www.rubyinside.com/march-2012-ruby-news-5841.html#jobs">jobs</a>, Bundler 1.1, Vagrant 1.0, Rails 3.2.3, Avdi Grimm's Object on Rails book, the Pragmatic Programmers release some more awesome books and, of course, a lot more.</p>
<h3>Headlines</h3>
<div style="margin:16px 0px">
  <a href="https://www.fsf.org/news/2011-free-software-awards-announced"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/3034/thumb_matzward.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="https://www.fsf.org/news/2011-free-software-awards-announced" style="font-weight:bold">Matz Wins FSF's 2011 Award for the Advancement of Free Software</a><br>
Free Software Foundation president Richard M. Stallman announced the winners of the FSF's annual free software awards recently with Ruby's own Yukihiro 'Matz' Matsumoto picking up the Award for the Advancement of Free Software. He joins a short list of open source heroes including Alan Cox, Larry Wall, and Guido van Rossum.
</p>
</div>
<div style="margin:16px 0px">
  <a href="http://pragprog.com/book/warv/the-rails-view"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/2979/thumb_rview.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="http://pragprog.com/book/warv/the-rails-view" style="font-weight:bold">Released: 'The Rails View' by John Athayde and Bruce Williams</a><br>
Working in Rails' view layer can be tricky with brittle, complex views all too easy to rustle up. This book digs into strategies and approaches for upping your Rails view game and breaking free from tangles of logic and markup in your views. Two thumbs up.
</p>
</div>
<div style="margin:16px 0px">
  <a href="http://objectsonrails.com/"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/2718/thumb_oor.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="http://objectsonrails.com/" style="font-weight:bold">Objects on Rails: How to Apply Classic OO Ideas to Rails Apps</a><br>
For a while now, Avdi Grimm has been slaving over a delicious 'developer's notebook' documenting guidelines, techniques, and ideas for applying classic object-oriented thought to Rails apps. He's now released it free to read on the Web. I recommend reading this, it's good.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://dallarosa.tumblr.com/post/20338743300/ruby-is-accepted-as-an-international-standard" style="font-weight:bold">Ruby Language Accepted As An International Standard by ISO</a><br>
Dalla Rosa noticed a press release from Japan's IT Promotion Agency which notes that the long awaited Ruby specification (not to be confused with RubySpec) has been approved by the International Standards Organization as the ISO/IEC 30170 standard.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://weblog.rubyonrails.org/2012/3/30/ann-rails-3-2-3-has-been-released/" style="font-weight:bold">Rails 3.2.3 Released</a><br>
The usual laundry list of minor changes but the biggest deal here is the default value of 'config.active_record.whitelist_attributes' becomes 'true', inspired by the recent GitHub mass assignment issue. Note that the change only affects newly generated apps but you can learn more in this post.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://www.kickstarter.com/projects/1397300529/railsapp" style="font-weight:bold">Yehuda Katz Raises $40k for 'rails.app', an OS X Rails Environment Installer</a><br>
Recently Yehuda Katz (well known as the lead architect behind Rails 3) launched a campaign to raise $25k to build a Ruby and Rails environment installer for OS X. It has now gone on to raise over $40k but left some in the community wondering quite what was really needed.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://vagrantup.com/?v1" style="font-weight:bold">Vagrant 1.0: Virtualized Development for the Masses</a><br>
Vagrant is a popular VirtualBox-driven Ruby tool for quickly building and deploying virtual machines for development and testing purposes. After years of development, it has reached the all important 1.0 release. Congrats!
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://gembundler.com/v1.1/index.html" style="font-weight:bold">Bundler 1.1 is Out!</a><br>No official blog post yet, but Bundler 1.1 is out and you can grab it with a gem install bundler. The big win on this release is significantly improved performance when fetching gemsets with complex dependencies.</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation" style="font-weight:bold">GitHub Public Key Security Vulnerability and Mitigation</a><br>GitHub experienced a security issue involving mass assignments in Rails. They've fixed it up now but you might want to get up to speed with what happened.</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://news.railstutorial.org/ruby-on-rails-tutorial-now-with-twitters-boot" style="font-weight:bold">The Ruby on Rails Tutorial, Now With Twitter's Bootstrap</a><br>
Michael Hartl has updated the new Rails 3.2 version of his popular 'Rails Tutorial' to use Twitter's increasingly popular Bootstrap framework.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/rails/rails/pull/572" style="font-weight:bold">ActiveResource Removed From Edge Rails (and Rails 4.0)</a><br>Just as it says in the title, but take care to scroll down to see the full story, since the proposal was initially deferred but has now been implemented. Active Resouce is now available as a separate project.</p>
</div>
<div style="margin:16px 0px">
  <a href="http://pragprog.com/book/sidruby/the-druby-book"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/2871/thumb_druby.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="http://pragprog.com/book/sidruby/the-druby-book" style="font-weight:bold">The Prags Release 'The dRuby Book'</a><br>
Learn from legendary Japanese Ruby hacker Masatoshi Seki in this first English-language book on his own Distributed Ruby library (DRb). Pick up distributed programming ideas straight from the source here. Available in print and e-book formats.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://blog.davidchelimsky.net/2012/03/17/rspec-290-is-released/">RSpec 2.9 Released</a></p>
</div>
<h3>Reading</h3>
<div style="margin:16px 0px">
<p><a href="http://patshaughnessy.net/2012/4/3/exploring-rubys-regular-expression-algorithm" style="font-weight:bold">Exploring Ruby's Regular Expression Algorithm</a><br>
Pat Shaughnessy, Ruby implementation spelunker extraordinaire, digs into Oniguruma, the regular expression engine used by MRI Ruby 1.9. What does it do and how does it process your regexes?
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://patshaughnessy.net/2012/3/23/why-you-should-be-excited-about-garbage-collection-in-ruby-2-0" style="font-weight:bold">Why You Should Be Excited About Garbage Collection in Ruby 2.0</a><br>
Could it be Pat Shaughnessy again? Yes, sirree. Here he digs into the 'bitmap marking' garbage collection algorithm that promises to reduce Ruby's memory consumption in Ruby 2.0.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://metaskills.net/2011/03/26/using-minitest-spec-with-rails/" style="font-weight:bold">Using MiniTest::Spec With Rails</a><br>
Ken Collins has been working on minitest-spec-rails, a gem that makes it reasonably trivial to use MiniTest::Spec (part of the Ruby 1.9 stdlib) with Rails. Learn how here. Ignore the date on this article, it was just updated!
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://addyosmani.com/blog/building-backbone-js-apps-with-ruby-sinatra-mongodb-and-haml/" style="font-weight:bold">Building Backbone.js Apps With Ruby, Sinatra, MongoDB and Haml</a><br>
An extensive tutorial by Addy Osmani on building a Backbone.js at both the front and back ends. The server side part is powered by Ruby's light but powerful webapp DSL Sinatra.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://carlhoerberg.github.com/blog/2012/03/29/run-jruby-on-heroku/">Running JRuby on Heroku</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rubysource.com/zero-to-jekyll-in-20-minutes/" style="font-weight:bold">Zero to Jekyll in 20 Minutes</a><br>
Jonathan Jackson explains how to use the popular Jekyll blog-focused static Web site generator from scratch.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://merbist.com/2012/04/04/building-and-implementing-a-single-sign-on-solution/">Building and Implementing a Single Sign-On Solution</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://robots.thoughtbot.com/post/20222801236/what-would-happen-if-you-ran-bundle-update-right-now" style="font-weight:bold">What Would Happen If You Ran 'bundle update' Right Now?</a><br>
Is there a bundle command to tell you what would be updated with bundle update, without actually making those updates? With Bundler 1.1.. yes there is!
</p>
</div>
<div style="margin:16px 0px">
  <a href="https://github.com/styleguide/ruby"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/2880/thumb_github.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="https://github.com/styleguide/ruby" style="font-weight:bold">GitHub's Ruby Style Guide</a><br>
The folks at GitHub have put together a document outlining the Ruby styles and conventions they use for their internal apps. Plenty of good practice in here, along with a little opinion.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rubysource.com/learning-more-about-jruby-from-charles-nutter/" style="font-weight:bold">Learning More About JRuby from Charles Nutter</a><br>
Pat Shaughnessy has interviewed Charles Nutter of the JRuby core team and digs into the meaning behind JRuby, what JRuby is well suited for, how its internals work, and where JRuby is headed in the future.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://blog.segment7.net/2012/03/16/a-use-of-enumerable-chunk" style="font-weight:bold">A Use of Enumerable#chunk</a><br>
Enumerable#chunk is a not particularly well known method in the Enumerable module (and therefore available to your arrays and hashes by default) and in this post, Eric Hodel shows off a use for it.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rakeroutes.com/blog/how-to-use-bundler-instead-of-rvm-gemsets/" style="font-weight:bold">How to Use Bundler Instead of RVM Gemsets</a><br>
Stephen Ball recently heard Bundler maintainer Andre Arko say that Bundler can obviate the need to use RVM gemsets. In this post, he investigates the idea.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://blog.alexmaccaw.com/rails-is-just-and-api-and-that-s-ok" style="font-weight:bold">Rails Is Just An API</a><br>
Alex MacCaw says there's nothing wrong with relegating Rails to the API layer.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://timelessrepo.com/tailin-ruby" style="font-weight:bold">Tailin' Ruby: 'Faking' Tail Call Optimization</a><br>
By default, Ruby doesn't implement tail call optimization (although it can be enabled in various ways in 1.9) so Magnus Holm set out to try and 'fake' it. An interesting experiment.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://blog.carbonfive.com/2012/03/18/how-to-test-external-apis/" style="font-weight:bold">How to Test External APIs</a><br>
It's common to integrate with an external API and in order to effectively test the integration, you might want to stub it out. Jared Carroll of Carbon Five shares a testing strategy using stubs for an external API.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rakeroutes.com/blog/write-a-gem-for-the-rails-asset-pipeline/">How to Write (and Test) a Gem to Serve Static Files on the Rails Asset Pipeline</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://blog.railsware.com/2012/03/13/ruby-2-0-enumerablelazy/" style="font-weight:bold">Ruby 2.0's Enumerable::Lazy</a><br>
Innokenty Mihailov's Enumerable::Lazy patch was accepted into Ruby trunk this week which gives us some ActiveRecord 3-style 'lazy evaluation' features on enumerations in Ruby. Worth checking out as a key new feature to come along in Ruby 2.0.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://stackoverflow.com/a/9627796/3951" style="font-weight:bold">API Versions in Rails Routes: A Mind Blowing Answer?</a><br>
Ryan Bigg promises to 'blow your mind' in his answer on Stack Overflow that shows some Rails 3 routing magic (in the context of versioning an API through the URL).
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rubysource.com/sinatra-heroku-super-fast-deployment/" style="font-weight:bold">Sinatra + Heroku = Super Fast Deployment</a><br>
Darren Jones demonstrates creating a very simple Sinatra app and deploying it on Heroku.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://gist.github.com/2032303">How to Write MiniTest::Spec Expectations</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://iain.nl/getting-the-most-out-of-bundler-groups" style="font-weight:bold">Getting the Most out of Bundler Groups</a><br>
Bundler lets you create different 'groups' in your Gemfile so different environments can have different dependencies. Iain Hecker shows off some uses for this feature.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://isotope11.com/blog/use-different-user-agents-per-cucumber-scenario">Use Different User Agents Per Cucumber Scenario</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://futuresimple.github.com/posts/2012-02-24-graceful-exiting-from-console-programs-in-ruby/">Gracefully Exiting from Console Programs in Ruby</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://blog.flavorjon.es/2012/03/y-u-no-gemspec.html">Why doesn't Nokogiri have a .gemspec file in its repository?</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://gist.github.com/1974187" style="font-weight:bold">Yehuda Katz's Proposal for Improving Mass Assignment (in Rails)</a><br>After the GitHub issue (above), Rails 3 guru Yehuda Katz came up with a proposal for improving how mass assignment works in Rails. I don't agree with the approach but it sparked an interesting discussion.</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://pragprog.com/magazines/2012-03/the-nor-machine" style="font-weight:bold">The NOR Machine: Building a CPU With Only One Instruction</a><br>
Have you ever developed in an assembly language? Have you developed an assembly language? Ever developed a CPU running your own assembly language? Alexander Demin shows you how in this fun Ruby-oriented walkthrough.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rubysource.com/flexible-searching-with-solr-and-sunspot/" style="font-weight:bold">Flexible Searching with Solr and Sunspot</a><br>
Mike Pack outlines how the Solr full text search server can benefit your project's indexing capabilities and shows how Solr can be used within a Rails app using Sunspot.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://gist.github.com/1912050" style="font-weight:bold">A Common .ruby-version File For Ruby Projects</a><br>
Got .rvmrc and .rbenv-version (and possibly more) floating around your projects? What if we had an ecosystem of fabulous Ruby managers that all understood the semantics of a generic dotfile like '.ruby-version'? Here's a proposal to weigh in on.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://schneems.com/post/18437886598/wizard-ify-your-rails-controllers-with-wicked" style="font-weight:bold">Wizard-ify Your Rails Controllers with Wicked</a><br>
Multi-page 'wizards' are popular in both desktop software and webapps and Wicked by Richard Schneeman brings a way to make them easier to produce in a Rails app.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://spin.atomicobject.com/2012/02/28/load-balancing-and-reverse-proxying-with-nginx/" style="font-weight:bold">Load Balancing and Reverse Proxying with Nginx (for Rails Apps)</a><br>
Nginx is a modern, open-source, high-performance web server that's well known in the Rails hosting world. In this post, Justin Kulesza demonstrates how to do a load balanced, reverse proxying setup with Nginx for a Rails app.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://www.unlimitednovelty.com/2012/03/why-critics-of-rails-have-it-all-wrong.html">Why Critics of Rails Have It All Wrong (and Ruby's Bright Multicore Future)</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://weblog.rubyonrails.org/2012/3/7/what-is-docrails" style="font-weight:bold">What is docrails?</a><br>docrails is a branch of Ruby on Rails with public write access where anyone can push documentation fixes. Xavier Noria explains how it works.</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://mitchellhashimoto.com/post/18870472135/hardest-and-most-rewarding-job" style="font-weight:bold">The Hardest, Most Rewarding Job I've Ever Had</a><br>Mitchell Hashimoto of the Vagrant project (which recently hit 1.0 - above) shares his take on the history of Vagrant and the experiences it brought him. Always nice to see a post like this.</p>
</div>
<h3>Watching and Listening</h3>
<div style="margin:16px 0px">
  <a href="http://railscasts.com/episodes/336-copycopter"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/3037/thumb_336-copycopter.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="http://railscasts.com/episodes/336-copycopter" style="font-weight:bold">RailsCasts on Copycopter</a><br>
Copycopter provides an interface that clients can use to edit the text in a Rails application. Learn how to deploy a Copycopter server using Heroku and integrate it in a Rails application through I18n.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://peepcode.com/products/play-by-play-jimweirich-ruby" style="font-weight:bold">PeepCode Play by Play: Jim Weirich</a><br>
PeepCode continues its Play by Play screencast series with Jim Weirich, the author of the ubiquitous Rake build tool for Ruby and chief scientist at EdgeCase. Want a view over an experienced Rubyist's shoulder? This is a good place to go.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://www.naildrivin5.com/blog/2012/03/23/slides-from-my-talk-on-jruby-and-threads.html" style="font-weight:bold">Don't Fear The Threads: Simplify Your Life with JRuby</a><br>
An epic 161 slide slide-deck by David Copeland, focused on threading and JRuby.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://vimeo.com/38531248" style="font-weight:bold">Inside Ruby: Concurrency and Garbage Collection Explained</a><br>
Matt Aimonetti's presentation from Ruby Argentina has finally been released. Skip a few minutes in unless you want to enjoy Matt's Spanish skills.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://arrrrcamp.be/videos/2011/" style="font-weight:bold">14 Presentation Videos from ArrrrCamp 2011</a><br>
Videos from last year's ArrrrCamp have been released, including presentations by Corey Haines, Elise Huard, Jim Gay, Anthony Eden and John Long.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://vimeo.com/album/1870460" style="font-weight:bold">RubyConf Argentina 2011 - Day 1</a><br>
It must be the week for releasing conference videos. RubyConf Argentina (back in November 2011) has released theirs too.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://railscasts.com/episodes/332-refinery-cms-basics" style="font-weight:bold">Refinery CMS Basics with RailsCasts</a><br>
Ryan Bates shows off how to quickly build a Rails app with out of the box content management using the Rails-based CMS, Refinery CMS.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://bostonrb.org/presentations/love-your-lib-directory" style="font-weight:bold">Love Your Lib Directory</a><br>
Brian Cardarella shares some conventions over the use of the 'lib' directory within Ruby projects and libraries. A 20 minute talk given at Boston.rb.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://rubyrogues.com/045-rr-bundler-with-andre-arko/" style="font-weight:bold">Ruby Rogues on Bundler with Andre Arko</a><br>The lovable rogues are back for another hour long podcast, this time discussing the Bundler project with its maintainer Andre Arko.
</p>
</div>
<h3>Libraries and Code</h3>
<div style="margin:16px 0px">
  <a href="https://github.com/dcadenas/rubydeps"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/2597/thumb_dep.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="https://github.com/dcadenas/rubydeps" style="font-weight:bold">Rubydeps: Create Dependency Graphs from Test Suites</a><br>
Rubydeps is a tool to create class dependency graphs from test suites. It runs your suite, records the call graph between the classes, and uses this info to create a Graphviz dot graph.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/mwunsch/weary" style="font-weight:bold">Weary: Framework and DSL for Building RESTful Web Service Clients</a><br>
Mark Wunsch's Weary is a suite of tools built around the Rack ecosystem that makes it both easy to build elegant clients for (ideally RESTful) Web services.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/cldwalker/debugger">debugger: Port of ruby-debug That Works on 1.9.2 and 1.9.3</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/metaskills/store_configurable" style="font-weight:bold">store_configurable: A Hash for Config Options on ActiveRecord Objects</a><br>
A zero-configuration recursive hash for storing a tree of options in a serialized ActiveRecord column.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/hudge/proffer">Proffer: Hide Instance Variables from Rails Views by Default</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/mattmatt/s3itch" style="font-weight:bold">S3itch: Amazon S3 WebDAV Proxy for Skitch</a><br>
Popular OS X screenshot tool Skitch is dropping its native file sharing component so Mathias Meyer has built a Sinatra-based proxy that can accept files over WebDAV and then upload them to S3.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/cubox/rankable_graph">rankable_graph: A Ruby PageRank-Like Implementation</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/wojtekmach/minitest-metadata" style="font-weight:bold">minitest-metadata: Metadata for your MiniTest Test Cases</a><br>
minitest-metadata allows you to set metadata (key-value) for your test cases so that before and after hooks can use them.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://gist.github.com/2143990">Non-Rails Rackup with Sprockets, Compass, and Twitter Bootstrap</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://gist.github.com/2050539" style="font-weight:bold">RSpec is Not The Reason Your Rails Test Suite is Slow</a><br>
In a simple gist, RSpec maintainer David Chelimsky dispels the myth that RSpec drags slowness around with it, wherever it goes.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/ohler55/oj">Oj: Optimized JSON Parser and Marshaller</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/jayferd/ry" style="font-weight:bold">Ry: The Simplest Ruby Version Manager</a><br>
We have RVM, we have rbenv.. we now have 'ry' too! It bills itself as the 'simplest Ruby virtual environment' and its major design goal is to explicit, unobtrusive, and easy to query. It can also lean on ruby-build to install new versions.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/livingsocial/rake-pipeline" style="font-weight:bold">Rake-Pipeline: Rake-Powered Asset Packaging</a><br>
A system for packaging assets for deployment to the web built as an extension to Rake. Developed by the masterminds over at Living Social.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/laserlemon/capital" style="font-weight:bold">Capital: 'Top Off' Your ActiveRecord Columns</a><br>
Capital extends what's returned via your model's columns, converting values to and from 'rich objects.' It's inspired by MongoMapper's serialization.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://ruby-docs.com/" style="font-weight:bold">Ruby-Doc.com: Ruby and Rails API Docs Hosted on S3</a><br>
Rob Cameron was getting tired of slow or inaccessible Ruby and Rails docs so has rendered and put up an entire set of Ruby and Rails API docs (various versions) on Amazon S3.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://myronmars.to/n/dev-blog/2012/03/vcr-2-0-0-released" style="font-weight:bold">VCR 2.0.0 Released: Recording Your Tests' HTTP Interactions</a><br>
VCR is a library for recording a test suite's HTTP interactions to replay during future runs. Version 2 is now out and brings a lot more flexibility, custom request matchers and serializers, request hooks, and more.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/rails/rails/commit/b83965785db1eec019edf1fc272b1aa393e6dc57" style="font-weight:bold">'wow how come I commit in master? O_o'</a><br>
The Rails commit that started the drama around the GitHub / Rails mass assignment issue. Linked for posterity but also because the comments turned into the typical meme-fest.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/highgroove/zonebie" style="font-weight:bold">Zonebie: Timezone Randomization for Robust Time Tests</a><br>
Zonebie helps you hunt down bugs in code that deals with timezones by randomly assigning a different timezone on each run.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://bitmonkey.net/post/18854033582/introducing-metriks" style="font-weight:bold">Metriks: A Simple, Lightweight Ruby Metrics Experiment</a><br>
An experiment in making a thread-safe, low impact library to measure performance metrics in your Ruby apps. One of the most interesting uses is to have it update your process's title so the metrics info appears live in ps or top!
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/sunaku/md2man#readme" style="font-weight:bold">md2man: Markdown to Man Page</a><br>
A Ruby library and command-line program that converts Markdown documents into UNIX manual pages.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/railsbp/rails_best_practices">Rails Best Practices: Code Metric Tool to Check Rails Code Quality</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/filtersquad/rocket_pants" style="font-weight:bold">Rocket Pants: Tools for Building Well Designed Web APIs</a><br>
A humorously named and highly opinionated toolkit for building well-designed Web APIs, with a focus on Rails.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="https://github.com/change/method_profiler" style="font-weight:bold">MethodProfiler: Get Performance Info about Methods on Your Objects</a><br>
MethodProfiler collects performance information about the methods in your objects and creates reports to help you identify slow methods. The collected data can be sorted in various ways, converted into an array, or pretty printed as a table.
</p>
</div>
<h3>Jobs</h3>
<div style="margin:16px 0px">
<p><a href="http://www.onthebeach.co.uk/jobs/rails-developer" style="font-weight:bold">Rails Developer at On The Beach [Manchester, UK]</a><br>One of the better designed and more creative job ad pages I've seen!</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://apply.hubbub.co.uk/" style="font-weight:bold">Ruby and Web Developer at Hubbub [Highbury, London, UK]</a><br>Pass a simple API challenge to apply..</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/658386">Ruby Developers for an Awesome Social Media Co. (Wildfire Interactive) [Bay Area, California]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/659986">Web Engineer at Sports Technologies, LLC [Canton, Connecticut]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/661391">There's more to life than CRUD: Solve hard problems with Ruby at MegaPhone Labs [New York City]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/661856">Rails Developer at Infusionsoft [Phoenix, Arizona]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/662521">Ruby Agent Engineer at New Relic [Portland, Oregon]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/661932">Ruby on Rails Developer at ManageIQ [Mahwah, New Jersey]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/664486">Ruby Developer at Decisiv, Inc. [Remote working OK]</a></p>
</div>
<div style="margin:16px 0px">
<p><a href="http://jobs.rubyinside.com/a/jbb/job-details/668885">Mid-Senior Ruby Software Engineers at FreeAgent [Edinburgh, United Kingdom (Remote poss)]</a></p>
</div>
<h3>Last but not least..</h3>
<div style="margin:16px 0px">
  <a href="http://zfer.us/5iJ5Z"><img src="http://s3.amazonaws.com/nlga/uploads/item/image/1807/thumb_smallcover-cropped.png" width="133" height="100" style="float:right;margin-left:14px;margin-bottom:16px;border:1px solid #1173c7"></a>
<p><a href="http://zfer.us/5iJ5Z" style="font-weight:bold">'Working With Unix Processes' by Jesse Storimer</a><br>
Jesse Storimer doesn't think you should need to learn C to pick up some intricacies of Unix and Unix-style systems. In this (pay for) e-book he takes a Ruby based approach at explaining file descriptors, processes and forking, signals, and more.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://cloudinary.com/" style="font-weight:bold">Cloudinary: Image Manipulation in the Cloud</a><br>
A lot of webapp developers seem sick of installing things like ImageMagic to do image cropping and scaling. Cloudinary adds another solution. It's a commercial service but makes it easy to do image manipulations via URLs.
</p>
</div>
<div style="margin:16px 0px">
<p><a href="http://addons-catalog.herokuapp.com/" style="font-weight:bold">Heroku Add-ons Catalog</a><br>
There are lots of add-ons for the Heroku cloud hosting service nowadays but Ivan Schneider thought they were hard to scan through, merely being in an alphabetical list, so he built a different way to browse them.
</p>
</div>
<div>
<a href="http://feeds.feedburner.com/~ff/RubyInside?a=gqpGiGNYKIE:zBw266f8Uoc:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/RubyInside?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/RubyInside?a=gqpGiGNYKIE:zBw266f8Uoc:3H-1DwQop_U"><img src="http://feeds.feedburner.com/~ff/RubyInside?i=gqpGiGNYKIE:zBw266f8Uoc:3H-1DwQop_U" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/RubyInside/~4/gqpGiGNYKIE" height="1" width="1">