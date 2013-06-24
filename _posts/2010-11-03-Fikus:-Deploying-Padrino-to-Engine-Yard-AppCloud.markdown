---
layout: post
title:  "Fikus: Deploying Padrino to Engine Yard AppCloud"
date:   2010-11-03 00:22:22
author: Tim Gourley
categories: program
---

## Fikus: Deploying Padrino to Engine Yard AppCloud
### by Tim Gourley
### at 2010-11-03 00:22:22
### original <http://www.engineyard.com/blog/2010/fikus-deploying-padrino-to-engine-yard-appcloud/>

<h1>Padrino and Engine Yard AppCloud</h1>
Engine Yard AppCloud is a great platform for deploying Ruby on Rails applications quickly and easily. It's not only good for Rails, but it also makes it just as easy to deploy any application using <a rel="nofollow" href="http://rack.rubyforge.org/">Rack</a> or Rack-based frameworks like <a rel="nofollow" href="http://www.sinatrarb.com/">Sinatra</a>.

I'm a big fan of Sinatra and use it for both internal and personal projects. I even spoke about creating "stupid simple" Sinatra micro-apps at <a rel="nofollow" href="http://reddirtrubyconf.com/">Red Dirt Ruby Conf</a> this past year. As a web application developer, it's a wonderful tool to have under your belt for shipping quick, small, and targeted applications or middleware.

However, with increasingcomplexity in a project, I want the ease of Sinatra with a little more "oomf". Sinatra is a great web DSL and is perfect for small, isolated applications. But when crafting complex applications, one quickly longs for improved organization, such as the logical separation of an MVC architecture.

I found myself writing boilerplate code for each project, setting up the same code for common components: haml, mongo_mapper, rspec, and whatever component the project required. The layout of the boilerplate Sinatra setup I used looked a lot like an ugly parody of Rails and quickly overwhelmed me with too much bloat for a "micro" Sinatra application.
<h2>The Padrino Framework</h2>
<a rel="nofollow" href="http://wynnnetherland.com/">Wynn Netherland</a> asked me at Red Dirt Ruby Conf if I had heard of a framework called <a rel="nofollow" href="http://www.padrinorb.com">Padrino</a>, which solves a lot of these problems. I hadn't heard of it but promised him I'd take a look. Later that day I dove in and instantly fell in love.

Padrino provides a well-organized MVC architecture on top of Sinatra. You can easily create complex web applications while harnessing the power of Sinatra and Rack with lots of additional goodies thrown in. You get helpers, forms, mailers, and a built-in admin interface, similar to what you get with <a rel="nofollow" href="http://www.djangoproject.com/">Django</a> and easier than implementing something like <a rel="nofollow" href="http://github.com/fesplugas/typus">Typus</a> in your application. You can mount multiple applications with ease and share code between applications. Routing is accomplished with Sinatra's wonderful HTTP verb declaration system instead of a router like in Rails.

The feature I enjoy the most with Padrino involves its component agnosticism and generator abilities. Need mongo_mapper instead of ActiveRecord? Done. Want a project with less for CSS generation, haml for your views, jquery, and shoulda? It's easy. I encourage you all to check out Padrino and try it for yourself. If you're a fan of Sinatra, you'll likely enjoy what Padrino has to offer.
<h2>Deploying a Padrino Application</h2>
Deploying Padrino to Engine Yard AppCloud doesn't require any major changes to your deployment workflow. For example, I have a very simple CMS project called <a rel="nofollow" href="http://github.com/bratta/fikus">Fikus</a>, which is a Padrino application using <a rel="nofollow" href="http://www.mongodb.org/">MongoDB</a> for document storage. To deploy this to Engine Yard AppCloud, fork the main repository and make any local edits for configuration.

You're going to need access to a MongoDB database. You can either use the <a rel="nofollow" href="http://github.com/engineyard/ey-cloud-recipes/tree/master/cookbooks/mongodb/">unofficial chef recipe</a> in the Engine Yard <a rel="nofollow" href="http://github.com/engineyard/ey-cloud-recipes">ey-cloud-recipes</a> repository to spin up MongoDB on a utility instance, or you can use a third-party service such as <a rel="nofollow" href="http://mongomachine.com/">Mongo Machine</a>.

The first step is to create your environment. On your Engine Yard AppCloud Dashboard, choose "Create New Environment", give it a name (I called mine fikus), and choose "Passenger" from the Web Server Stack list. You can choose "Show Advanced Options" if you wish to choose specific SSH keys to install, change your availability zone, or the name of the deployment user.

Add a new application for the environment, and fill out the name and the git repository of your fork of Fikus. For the application type, choose Sinatra. Make sure to copy down the deploy key given to you and add it to your application's deploy keys on Github. This is necessary for your application to be deployed to your instances.

Next you will be asked to specify the gems and UNIX packages for your application. By default it adds the sinatra gem, but you'll want to remove that and add bundler 1.0.0. You don't need to add the padrino gem as it is included in the project's Gemfile. There are no special UNIX packages to add for this application, so once you are done with the gems you can go to the Dashboard.

The environment and application are in place, so now the only other piece is setting up your chef recipes for the application. Follow our <a rel="nofollow" href="http://docs.engineyard.com/appcloud/howtos/customizations/custom-chef-recipes">HOWTO guide for using Chef</a> to install the engineyard gem, fork the base recipe repository, and add your custom recipes. Fikus comes with a custom chef recipe already prepared in the <strong>fikus/chef</strong> directory. Copy that to your chef repository's cookbooks directory and customize as you see fit. Open up <strong>cookbooks/main/recipes/default.rb</strong> and make sure the following lines are in place:

<pre>require_recipe "mongodb"
require_recipe "fikus"</pre>

If you are using a third-party service for MongoDB such as Mongo Machine, you won't need the MongoDB recipe. Instead you can update <strong>cookbooks/fikus/templates/default/database.erb</strong> with the proper credentials.

Commit your changes and push them to Github, then upload them using the <code>ey</code> command:

<pre>$ ey recipes upload -e fikus</pre>

Now you are ready to boot your instances! You can boot a single instance if you use a third-party MongoDB service, but if you choose to use the unofficial MongoDB recipes in the ey-cloud-recipes repository, you will need to boot up a utility instance named mongodb_master (I strongly discourage having MongoDB on the same instance as your application and/or MySQL). I set it up like this, using the "Custom" cluster option:

<img src="http://img.skitch.com/20101017-e4rfqppxtnsd1ea4a3cwfks726.jpg" alt="Fikus Environment Setup">

After a while the instances will boot, and then you can deploy the application, again using the <code>ey</code> command:

<pre>$ ey deploy -e fikus -r master -a fikus --no-migrate</pre>

Pass in the <code>--no-migrate</code> since we're using MongoDB and don't have the concept of migrations. They're not needed. However, if you are using ActiveRecord instead, you would need to use the command <code>bundle exec padrino rake ar:migrate</code> to migrate the database.

The final step involved is to ssh into your instance and run the rake task to seed the database with the basic data needed for Fikus:

<pre>$ cd /data/fikus/current
$ bundle exec padrino rake seed</pre>
That sets up an initial site, admin user, and page for you to play with. Now you can hit your application at the /admin URI, log in with the credentials provided in the "rake seed" step above, and begin adding content to your site.

Fikus currently allows you to serve up content for multiple sites, but other than that it is fairly basic. Pages are authored using <a rel="nofollow" href="http://daringfireball.net/projects/markdown/">Markdown</a>, and there are a few options for page caching which are still works in progress. This is a fairly new project, so if you want to contribute, contact me <a rel="nofollow" href="http://github.com/bratta">on Github</a> or just send a pull request.

I hope this shows you how flexible the Engine Yard AppCloud can be for deploying non-Ruby on Rails applications.<p><a href="http://www.engineyard.com/blog"><img height="98" width="61" title="logo-engineyard" alt="" src="http://www.engineyard.com/blog/?getfile=4050"></a></p>