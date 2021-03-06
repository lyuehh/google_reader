---
layout: post
title:  "A Simple Blog with CouchDB, Bogart, and Node.js"
date:   2011-09-19 23:53:58
author: nrstott@gmail.com (Nathan Stott)
categories: program
---

## A Simple Blog with CouchDB, Bogart, and Node.js
### by nrstott@gmail.com (Nathan Stott)
### at 2011-09-19 23:53:58
### original <http://howtonode.org/bogart-couchdb>

<p><em>Update:</em> By request I have posted a <a href="https://gist.github.com/1232272">gist of the app.js</a>
using <em>MongoDB</em> instead of <em>CouchDB</em>.  This gist also serves as a beginning example
for how to use non-promise-based APIs with bogart.</p>

<p>In this article, you will learn how to use Bogart and CouchDB to create a minimal
blogging engine. The Express with MongoDB article was a huge hit. This article has
similar goals but shows a different way of using Node.JS.</p>

<h2>Pre-Requisites</h2>

<h3>npm</h3>

<p><a href="http://github.com/isaacs/npm">npm</a> is the most popular package manager for Node.js.
Installing npm is easy.</p>

<pre><code>curl http://npmjs.org/install.sh | sh
</code></pre>

<p>A note for windows users: npm does not currently work on windows. It will in the future.</p>

<h3>Bogart</h3>

<p><a href="http://github.com/nrstott/bogart">Bogart</a> is a Sinatra-like framework designed 
to make it easy to create JSGI compliant web applications for node.js.</p>

<p>Bogart is in the npm registry.</p>

<pre><code>npm install bogart
</code></pre>

<h3>CouchDB</h3>

<p><a href="http://couchdb.apache.org">CouchDB</a> is a document-oriented database with 
a RESTful interface. CouchDB works well with JavaScript since CouchDB speaks JSON.
Also, CouchDB is queried using 'views' that are, by default, written in JavaScript.
<a href="http://couchdb.apache.org/downloads.html">Download the latest release</a> from here.
CouchBase also maintains <a href="http://www.couchbase.com/downloads">debian and rpm packages</a> 
for the community.</p>

<h2>JSGI</h2>

<p>Bogart is a JSGI-based framework. JSGI is specified by the CommonJS mailing list. Knowledge
of JSGI is helpful when dealing with Bogart; however, it is not necessary. You can find
<a href="http://wiki.commonjs.org/wiki/JSGI">more information about JSGI</a> on the 
<a href="http://wiki.commonjs.org">CommonJS wiki</a>.</p>

<h2>Mustache</h2>

<p><a href="http://github.com/janl/mustache.js">Mustache</a> is a minimal templating engine 
with {{mustaches}}. Mustache is the default templating engine of Bogart. 
When you install Bogart, you will also be installing Mustache.</p>

<h2>CouchDB-CommonJS</h2>

<p><a href="http://github.com/nrstott/couchdb-commonjs">CouchDB-CommonJS</a> is a promise-based 
CouchDB library available in the npm registry. It can also be used in the browser 
or with Narhwal.</p>

<pre><code>npm install couchdb
</code></pre>

<h2>What will our application do?</h2>

<p>To keep things simple, we're going to only tackle basic functionality. Our blog
application will support the following methods:</p>

<ul>
<li>Create a new post (POST /posts)</li>
<li>Show a list of all the posts (GET /posts)</li>
<li>Show a single post (GET /posts/:id)</li>
<li>Comment on a post (POST /posts/:id/comments)</li>
</ul>

<h2>Lets get started!</h2>

<p>A Bogart application consists of a JSGI server with one or more pieces of middleware and 
one or more Bogart routers each containing any number of routes.</p>

<p>The canonical 'Hello World' application in Bogart can be written as follows:</p>

<div><a href="http://howtonode.org/bogart-couchdb/hello-world.js">hello-world.js</a><pre><code>var bogart = require('bogart');

var router = bogart.router();

router.get('/', function() {
  return bogart.html('Hello World');
});

bogart.start(app);</code></pre></div>

<p>This JavaScript program defines a single route that accepts <code>GET</code> requests to the root of the
site and returns a simple HTML greeting.</p>

<p>To run this program, first execute the following commands to setup your blog directory:</p>

<pre><code>mkdir bogart-couchdb-blog
cd bogart-couchdb-blog
npm install bogart
</code></pre>

<p>This will create a new directory named bogart-couchdb-blog and install bogart to the 
node_modules subdirectory of this directory. Next, copy the JavaScript into a file into
bogart-couchdb-blog and name it <code>hello-world.js</code> and then execute</p>

<pre><code>node hello-world.js
</code></pre>

<p>Visit <a href="http://localhost:8080">http://localhost:8080</a> in your browser.</p>

<h2>Creating the package.json file</h2>

<p>In order to manage dependencies, it is useful to create a <code>package.json</code> file. This file
provides details on the packages you depend on so that you can more easily use <code>npm</code> to manage
these dependencies.</p>

<p>Create a file named <code>package.json</code> in your <code>bogart-couchdb-blog</code> directory.</p>

<div><a href="http://howtonode.org/bogart-couchdb/package.json">package.json</a><pre><code>{
    &quot;name&quot;: &quot;blog&quot;,
    &quot;description&quot;: &quot;Simple Blogging Engine&quot;,
    &quot;version&quot;: &quot;0.1.0&quot;,
    &quot;author&quot;: &quot;Nathan Stott&quot;,
    &quot;email&quot;: &quot;nathan.stott@whiteboard-it.com&quot;,
    &quot;main&quot;: &quot;./app&quot;,
    &quot;directories&quot;: { &quot;lib&quot;: &quot;./lib&quot; },
    &quot;dependencies&quot;: {
      &quot;bogart&quot;: &quot;&gt;=0.2.0&quot;,
      &quot;mustache&quot;: &quot;0.3.1-dev&quot;,
      &quot;couchdb&quot;: &quot;&gt;=0.1.2&quot;,
      &quot;promised-io&quot;: &quot;=0.2.3&quot;
    }
}</code></pre></div>

<p>The most important field in this JSON file is the <code>dependencies</code> field. This field will allow
you to execute <code>npm install</code> to install the dependencies for your project.</p>

<h2>Creating a Post</h2>

<p>There are two routes that we will need in order to create a post.</p>

<ul>
<li>GET /posts/new -&gt; returns a form to create a new post</li>
<li>POST /posts -&gt; creates a new post from the form parameters provided</li>
</ul>

<p>The mustache template to create a new post is as follows:</p>

<div><a href="http://howtonode.org/bogart-couchdb/new-post.html">new-post.html</a><pre><code>&lt;form method=&#39;post&#39; action=&#39;/posts&#39;&gt;
  &lt;fieldset&gt;
    &lt;legend&gt;New Post&lt;/legend&gt;

    &lt;div&gt;
      &lt;label for=&#39;title&#39;&gt;Title&lt;/label&gt;
      &lt;input name=&#39;title&#39; /&gt;
    &lt;/div&gt;

    &lt;div&gt;
      &lt;label for=&#39;body&#39;&gt;Body&lt;/label&gt;
      &lt;textarea name=&#39;body&#39; rows=&#39;15&#39; columns=&#39;25&#39;&gt;&lt;/textarea&gt;
    &lt;/div&gt;

    &lt;div class=&#39;buttons&#39;&gt;
      &lt;input type=&#39;submit&#39; value=&#39;Save Post&#39; /&gt;
    &lt;/div&gt;
  &lt;/fieldset&gt;
&lt;/form&gt;</code></pre></div>

<p>This post will be rendered inside of a layout to keep the look of the site consistant.
By convention, Bogart's view engine uses a file called <code>layout.html</code> as the layout if
it exists. A Bogart layout is a template with a <code>{{{body}}}</code> tag to include the
view inside of the layout.</p>

<div><a href="http://howtonode.org/bogart-couchdb/layout.html">layout.html</a><pre><code>&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD XHTML 1.0 Strict//EN&quot; &quot;http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd&quot;&gt;

&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;{{title}}&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
  {{{body}}}
&lt;/body&gt;
&lt;/html&gt;</code></pre></div>

<p>The route to return the new post template makes use of the <code>bogart.respond</code> helper. 
Even though it is not strictly necesarry to understand JSGI in order to use Bogart, 
lets go over the basic concept of a JSGI response. Bogart routes expect a JSGI response 
or a promise that will resolve to a JSGI response to be returned. A JSGI response is an 
object that contains three attributes: status (required), body (required), and headers (optional).</p>

<p>A simple JSGI response:</p>

<pre><code>{
  status: 200,
  body: [ 'Hello World' ]
}
</code></pre>

<p>The Bogart route to render new-post.html is as follows:</p>

<pre><code>router.get('/posts/new', function(req) {
  return viewEngine.respond('new-post.html', {
    locals: {
      title: 'New Post'
    }
  })
});
</code></pre>

<p><code>viewEngine</code> should be defined at the beginning of the Bogart configuartion closure as</p>

<pre><code>viewEngine = bogart.viewEngine('mustache');
</code></pre>

<p>Bogart supports <code>mustache</code> out of the box. There is a <code>jade</code> view engine in the package
bogart-jade. If you want to use <code>jade</code> then <code>npm install bogart-jade</code> and 
<code>require('bogart-jade')</code>. After that, <code>bogart.viewEngine('jade')</code> will work. It is easy to add
support for more view engines as well.</p>

<p>Bogart includes useful middleware to make working with forms easy. Normally, <code>req.body</code> will
contain the raw body of a form post. It is more conveniant if this is automatically converted to
a JSON object for us. The Bogart middleware <code>Parted</code> accomplishes this.</p>

<p>Adding middleware is easiest using a Bogart Application object.
JSGI <code>Parted</code> middleware wraps the streaming multipart, json, and urlencoded parsing utility
<a href="https://github.com/chjj/parted">Parted</a>.</p>

<pre><code>var app = bogart.app();
app.use(bogart.middleware.Parted);
app.use(router);
app.start();
</code></pre>

<p>Lets take a side-step to discuss how we can work with CouchDB using the <code>couchdb</code> package
from the npm registry.</p>

<p>At the top of our <code>app.js</code> add the following:</p>

<pre><code>var couchdb = require('couchdb');
</code></pre>

<p>In the closure that configures Bogart routes, create a couchdb client and a database representation.</p>

<pre><code>var app = bogart.router(function(get, post, put, destroy) {

  var client     = couchdb.createClient(5984, '127.0.0.1', { user: 'myuser', password: 'mypass' })
    , db         = client.db('blog')
    , viewEngine = bogart.viewEngine('mustache');

  // configure routes...
});
</code></pre>

<p>This creates a couchdb client connecting to '127.0.0.1' and port 5984. If your CouchDB is in
Admin Party, you do not need to supply the user and password in an options hash. If you have a 
CouchDB users setup, please provide your username and password.</p>

<p>Now we will create a route to handle the <code>POST</code> of our form.</p>

<pre><code>router.post('/posts', function(req) {
  var post = req.params;
  post.type = 'post';

  return db.saveDoc.then(function(resp) {
    return bogart.redirect('/posts');
  });
});
</code></pre>

<p>We add the <code>type</code> attribute to the <code>post</code> so that as we add more document types in the future,
we can easily create CouchDB views to find only specific document types. This is not a built-in
CouchDB concept. It is a useful convention that makes creating views simpler.</p>

<h2>Adding a CouchDB view to retrieve posts</h2>

<p>CouchDB is queried using <a href="http://wiki.apache.org/couchdb/HTTP_view_API">map/reduce views</a> 
that are defined on design documents. This means that we need to create a design
document before we can query a list of the posts in our database.</p>

<p>Lets create a JavaScript file <code>syncDesignDoc.js</code> in the <code>lib</code> directory of our project.</p>

<div><a href="http://howtonode.org/bogart-couchdb/syncDesignDoc.js">syncDesignDoc.js</a><pre><code>var couchdb = require('couchdb')
  , settings = require('../settings');

var client  = couchdb.createClient(settings.port, settings.host, { user: settings.user, password: settings.password });
var db      = client.db(settings.db);

var designDoc = {
  _id: '_design/blog',

  language: 'javascript',

  views: {
    'posts_by_date': {
      map: function(doc) {
        if (doc.type === 'post') {
          emit(doc.postedAt, doc);
        }
      }.toString()
    }
  }
};

db.saveDoc(designDoc).then(function(resp) {
  console.log('updated design doc!');
}, function(err) {
  console.log('error updating design doc: '+require('util').inspect(err));
});</code></pre></div>

<p>Execute <code>node lib/syncDesignDoc.js</code> to update the database with the latest design document.</p>

<h2>Listing Posts</h2>

<p>Lets create a Mustache template to list the posts from our database.</p>

<div><a href="http://howtonode.org/bogart-couchdb/posts.html">posts.html</a><pre><code>&lt;h2&gt;Posts&lt;/h2&gt;

&lt;a href=&#39;/posts/new&#39;&gt;New Post&lt;/a&gt;

&lt;ul&gt;
  {{#posts}}
    &lt;li&gt;
      &lt;div class=&#39;post&#39;&gt;
        &lt;h2&gt;&lt;a href=&#39;/posts/{{_id}}&#39;&gt;{{title}}&lt;/a&gt;&lt;/h2&gt;
        &lt;p&gt;
          {{body}}
        &lt;/p&gt;
      &lt;/div&gt;
    &lt;/li&gt;
  {{/posts}}
&lt;/ul&gt;</code></pre></div>

<p>Next, lets create a Bogart route to render this template. We will query the database using
<code>db.view</code>, process the response from CouchDB, and respond with the rendered template. Bogart
makes this easy:</p>

<pre><code>  router.get('/posts', function(req) {

    return db.view('blog', 'posts_by_date').then(function(resp) {
      var posts = resp.rows.map(function(x) { return x.value; });

      return viewEngine.respond('posts.html', {
        locals: {
          posts: posts
        }
      });
    });
  });
</code></pre>

<h2>Show an Individaul Post</h2>

<p>It's time to create a route to show an individual post. This page will also contain
a form for adding comments.</p>

<p>The template will be as follows:</p>

<div><a href="http://howtonode.org/bogart-couchdb/post.html">post.html</a><pre><code>&lt;a href=&#39;/posts&#39;&gt;Home&lt;/a&gt;

&lt;h1&gt;{{title}}&lt;/h1&gt;

&lt;div class=&#39;post-content&#39;&gt;
  {{body}}
&lt;/div&gt;

&lt;div&gt;
  &lt;h2&gt;Comments&lt;/h2&gt;
  &lt;ul id=&#39;comments&#39;&gt;
    {{#comments}}
      &lt;li&gt;
        &lt;div&gt;
          Author: &lt;span class=&#39;author&#39;&gt;{{author}}&lt;/span&gt;
        &lt;/div&gt;
        &lt;div&gt;
          {{body}}
        &lt;/div&gt;
      &lt;/li&gt;
    {{/comments}}

    {{^comments}}
      No Comments Yet
    {{/comments}}
  &lt;/ul&gt;
&lt;/div&gt;

&lt;form method=&#39;post&#39; action=&#39;/posts/{{_id}}/comments&#39;&gt;
  &lt;fieldset&gt;
    &lt;legend&gt;Leave a Comment&lt;/legend&gt;

    &lt;div&gt;
      &lt;label for=&#39;author&#39;&gt;Your Name&lt;/label&gt;
      &lt;input name=&#39;author&#39; /&gt;
    &lt;/div&gt;

    &lt;div&gt;
      &lt;label for=&#39;body&#39;&gt;Your Comment&lt;/label&gt;
      &lt;input name=&#39;body&#39; /&gt;
    &lt;/div&gt;

    &lt;div class=&#39;buttons&#39;&gt;
      &lt;input type=&#39;submit&#39; value=&#39;Post Comment&#39; /&gt;
    &lt;/div&gt;
  &lt;/fieldset&gt;
&lt;/form&gt;</code></pre></div>

<p>The Bogart route to display this is as simple as the route to display the form
for creating new posts.</p>

<pre><code>router.get('/posts/:id', function(req) {
  return db.openDoc(req.params.id).then(function(post) {
    return viewEngine.respond('post.html', { locals: post });
  });
});
</code></pre>

<p>The route to accept the <code>POST</code> from the comments form is similar to the route to accept
a new blog post:</p>

<pre><code>router.post('/posts/:id/comments', function(req) {
  var comment = req.params;

  return db.openDoc(req.params.id).then(function(post) {
    post.comments = post.comments || [];
    post.comments.push(comment);

    return db.saveDoc(post).then(function(resp) {
      return bogart.redirect('/posts/'+req.params.id);
    });
  });
});
</code></pre>

<h2>Summing it up</h2>

<p>As you can see, getting started with Bogart and CouchDB is simple. We are a long way from 
having a full-featured blog, but hopefully this will inspire some out there to try
working with Node.JS, Bogart, and CouchDB!</p>

<p>The full source code of the finished <code>app.js</code> is below:</p>

<div><a href="http://howtonode.org/bogart-couchdb/app.js">app.js</a><pre><code>var bogart  = require(&#39;bogart&#39;)
  , couchdb = require(&#39;couchdb&#39;)
  , settings = require(&#39;./settings&#39;);

var client     = couchdb.createClient(settings.port, settings.host, { user: settings.user, password: settings.password })
  , db         = client.db(settings.db)
  , viewEngine = bogart.viewEngine(&#39;mustache&#39;)
  , router     = bogart.router();

router.get(&#39;/&#39;, function(req) {
  return bogart.redirect(&#39;/posts&#39;);
});

router.get(&#39;/posts&#39;, function(req) {

  return db.view(&#39;blog&#39;, &#39;posts_by_date&#39;).then(function(resp) {
    var posts = resp.rows.map(function(x) { return x.value; });

    return viewEngine.respond(&#39;posts.html&#39;, {
      locals: {
        posts: posts,
        title: &#39;Blog Home&#39;
      }
    });
  }, function(err) {
    if (err.error &amp;&amp; err.error === &#39;not_found&#39;) {
      return bogart.error(&#39;execute syncDesignDoc before trying to use the blog&#39;);
    }
    throw err;
  });
});

router.get(&#39;/posts/new&#39;, function(req) {
  return viewEngine.respond(&#39;new-post.html&#39;, {
    locals: {
      title: &#39;New Post&#39;
    }
  });
});

router.post(&#39;/posts&#39;, function(req) {
  var post = req.params;
  post.type = &#39;post&#39;;
  post.postedAt = new Date();

  return db.saveDoc(post).then(function(resp) {
    return bogart.redirect(&#39;/posts&#39;);
  });
});

router.get(&#39;/posts/:id&#39;, function(req) {
  return db.openDoc(req.params.id).then(function(post) {
    return viewEngine.respond(&#39;post.html&#39;, { locals: post });
  });
});

router.post(&#39;/posts/:id/comments&#39;, function(req) {
  var comment = req.params;

  return db.openDoc(req.params.id).then(function(post) {
    post.comments = post.comments || [];
    post.comments.push(comment);

    return db.saveDoc(post).then(function(resp) {
      return bogart.redirect(&#39;/posts/&#39;+req.params.id);
    });
  });
});

var app = bogart.app();

// Include batteries, a default JSGI stack.
app.use(bogart.batteries);

// Include our router, it is significant that this is included after batteries.
app.use(router);

app.start();</code></pre></div>