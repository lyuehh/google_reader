---
layout: post
title:  "Getting Hired: fog Edition"
date:   2010-12-01 03:34:32
author: geemus (Wesley Beary)
categories: program
---

## Getting Hired: fog Edition
### by geemus (Wesley Beary)
### at 2010-12-01 03:34:32
### original <http://www.engineyard.com/blog/2010/getting-hired-fog-edition/>

Having Ruby experience makes you hirable; but how can you stand out? You need to demonstrate your abilities. What better way than using Ruby and "the cloud" to store and serve your resume!

In this blog post you will learn to use <a href="http://github.com/geemus/fog">fog</a> - the cloud computing library - to upload your resume to Amazon's <a href="http://aws.amazon.com/s3/">Simple Storage Service</a> (S3), Rackspace's <a href="http://www.rackspacecloud.com/cloud_hosting_products/files">CloudFiles</a> or Google's <a href="http://code.google.com/apis/storage/">Storage for Developers</a>.

Here's my out of date resume stored on <a href="http://geemus.s3.amazonaws.com/resume.html">S3</a>, <a href="http://c0023559.cdn2.cloudfiles.rackspacecloud.com/resume.html">CloudFiles</a> and <a href="https://geemus.commondatastorage.googleapis.com/resume.html">Google Storage</a>; programmatically stored in the cloud using this tutorial. NOTE: my boss would like me to add that I'm not currently looking for a new gig ;)

Check out those cloud-specific URLs! You could put all three in your job application, add the Ruby source for how you did it, and have your choice of Ruby jobs for being so awesome!

How? The all-clouds-in-one library of choice is <a href="https://github.com/geemus/fog">fog</a>.
<h3>Installing fog</h3>

<a href="https://github.com/geemus/fog">fog</a> is distributed as a RubyGem:

<pre>gem install fog</pre>

Or add it in your application's Gemfile:

<pre lang="ruby">gem "fog"</pre>

<h3>Using Amazon S3 and fog</h3>
Sign up for an account <a href="http://aws-portal.amazon.com/gp/aws/developer/subscription/index.html?productCode=AmazonS3">here</a> and copy down your secret access key and access key id from <a href="http://aws-portal.amazon.com/gp/aws/developer/account/index.html?action=access-key">here</a>. We are about to get into the code samples, so be sure to fill in anything in ALL_CAPS with your own values!

First, create a connection with your new account:
<pre lang="ruby">require &#39;rubygems&#39;
require &#39;fog&#39;

# create a connection
connection = Fog::Storage.new(
  :provider                 =&gt; &#39;AWS&#39;,
  :aws_secret_access_key    =&gt; YOUR_SECRET_ACCESS_KEY,
  :aws_access_key_id =&gt; YOUR_SECRET_ACCESS_KEY_ID
)

# First, a place to contain the glorious details
directory = connection.directories.create(
  :key    =&gt; &quot;fog-demo-#{Time.now.to_i}&quot;, # globally unique name
  :public =&gt; true
)

# list directories
p connection.directories

# upload that resume
file = directory.files.create(
  :key    =&gt; &#39;resume.html&#39;,
  :body   =&gt; File.open(&quot;/path/to/my/resume.html&quot;),
  :public =&gt; true
)
</pre>
If you are anything like me, you will continually tweak your resume. Pushing updates is easy:
<pre lang="ruby">
file.body = File.open("/path/to/my/resume.html")
file.save
</pre>
As you can see, cloud storage files in fog are a lot like an ActiveRecord model. Attributes that can be changed and a `#save` method that creates or updates the stored file in the cloud.

But if it took you longer to realize the mistake you might not still have file around, but you've got options.
<pre lang="ruby">directory = connection.directories.get(&quot;proclamations1234567890&quot;)

# get the resume file
file = directory.files.get(&#39;resume.html&#39;)
file.body = File.open(&quot;/path/to/my/resume.html&quot;)
file.save

# also, create(attributes) is just new(attributes).save, so you can also do:
file = directory.files.new(
  :key =&gt; &#39;resume.html&#39;,
  :body =&gt; &#39;improvements&#39;,
  :public =&gt; true
)
file.save
</pre>
Alright, so you (eventually) become satisfied enough to send it off, what is the URL endpoint to your resume?
<pre lang="ruby">puts file.public_url</pre>
Pop that link in an email and you should be ready to cruise job ads and send your resume far and wide (Engine Yard is <a href="http://www.engineyard.com/company/careers/wanted-head-in-the-clouds-engineer">hiring</a>, so check us out!). Now you are set, unless you are interviewing for Google, or Rackspace... Both of these companies have their own cloud storage services, so using Amazon S3 might not be the foot in the door you hoped for.

More clouds? How much extra stuff will you have to do for these services!?! Hardly anything needs to change, you just have to pass slightly different credentials in, but I'm getting ahead of myself.
<h3>Google Storage for Developers</h3>
Sign up <a href="http://gs-signup-redirect.appspot.com/">here</a> and get your credentials <a href="https://sandbox.google.com/storage/m/">here</a>.
<pre lang="ruby">
connection = Fog::Storage.new(
  :provider =&gt; &#39;Google&#39;,
  :google_storage_secret_access_key =&gt; YOUR_SECRET_ACCESS_KEY,
  :google_storage_access_key_id =&gt; YOUR_SECRET_ACCESS_KEY_ID
)
</pre>
<h3>Rackspace CloudFiles</h3>
Rackspace has <a href="http://www.rackspacecloud.com/cloud_hosting_products/files">Cloud Files</a> and you can sign up <a href="https://www.rackspacecloud.com/signup">here</a> and get your credentials <a href="https://manage.rackspacecloud.com/APIAccess.do">here</a>.
<pre lang="ruby">
connection = Fog::Storage.new(
  :provider =&gt; &#39;Rackspace&#39;,
  :rackspace_username =&gt; RACKSPACE_USERNAME,
  :rackspace_api_key =&gt; RACKSPACE_API_KEY
)
</pre>
Then create, save, destroy as per fog-for-AWS. The `:public =&gt; true` option when creating directories (see above) is important for Rackspace; your folder and files won&#39;t be shared to Rackspace&#39;s CDN and hence your users without it.  Similarly the `:public =&gt; true` on files is important for AWS and Google or they will be private.
<h3>Local Storage</h3>
While you are working out the kinks you might not want to do everything live though, ditto for while you are running tests, so you have a couple options to try before you buy.  First, you can use a local provider to store things in a directory on your machine.
<pre lang="ruby">
connection = Fog::Storage.new(
  :provider =&gt; &#39;Local&#39;,
  :local_root =&gt; &#39;~/fog&#39;
)
</pre>
<h3>Mocking out Cloud Storage</h3>
Of course when you are testing or developing you can always just use the mocks (at least for AWS and Google, Rackspace still needs mocks implemented if you are looking for somewhere to contribute).  They emulate the behavior of the external systems without actually using them.  It is as simple as:
<pre lang="ruby">
Fog.mock!
connection = Fog::Storage.new(config_hash)
</pre>
<h3>Cleaning up</h3>
Fog takes care of the rest so you can focus on your cover letter. And with the awesome cover letter and cloud delivered resume you are probably a shoe-in. So all that is left is to cleanup that leftover job hunt residue.
<pre lang="ruby">
file.destroy
directory.destroy
</pre>
<h3>Summary</h3>
All done. Try out all the different options and let me know if you have any bugs or issues.  I also wrote up a more <a href="https://gist.github.com/710869">consolidated example as a script</a> that you can use for reference.

Bonus, note the `Fog.mock!` command. In your tests you can easily mock out calls to cloud providers.

Please let me know in the comments if you got a new Ruby job because you hosted your CV on 3 different Cloud Stores without getting your hands dirty.

Have questions or comments?  Hop into <a href="javascript:void(0);">#ruby-fog</a> on freenode, ping <a href="http://twitter.com/fog">@fog</a> or <a href="http://twitter.com/geemus">@geemus</a>.

And please always remember that I accept high fives and contributions!<p><a href="http://www.engineyard.com/blog"><img height="98" width="61" title="logo-engineyard" alt="" src="http://www.engineyard.com/blog/?getfile=4050"></a></p>