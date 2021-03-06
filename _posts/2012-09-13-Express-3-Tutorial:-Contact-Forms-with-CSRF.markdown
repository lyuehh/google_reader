---
layout: post
title:  "Express 3 Tutorial: Contact Forms with CSRF"
date:   2012-09-13 07:00:00
author: 
categories: program
---

## Express 3 Tutorial: Contact Forms with CSRF
### by 
### at 2012-09-13 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/krIOIRJqY34/express-3-csrf-tutorial>

<div><div><div></div></div></div><br>
     <p><img src="http://dailyjs.com/images/posts/sayhello.png" alt="The contact form"></p>

<p>This tutorial is a hands on, practical introduction to writing Express 3 applications complete with CSRF protection. As a bonus, it should be fairly easy to install on Heroku.</p>

<h3>Prerequisites</h3>

<p>A working Node installation is assumed, and basic knowledge of Node and the command-line.</p>

<h3>Getting Started</h3>

<p>Create a new directory, then create a new file called <code>package.json</code> that looks like this:</p>
<div><pre><code><span>{</span>
  <span>&quot;author&quot;</span><span>:</span> <span>&quot;Alex R. Young&quot;</span>
<span>,</span> <span>&quot;name&quot;</span><span>:</span> <span>&quot;dailyjs-contact-example&quot;</span>
<span>,</span> <span>&quot;version&quot;</span><span>:</span> <span>&quot;0.0.1&quot;</span>
<span>,</span> <span>&quot;private&quot;</span><span>:</span> <span>true</span>
<span>,</span> <span>&quot;dependencies&quot;</span><span>:</span> <span>{</span>
    <span>&quot;express&quot;</span><span>:</span> <span>&quot;3.0&quot;</span>
  <span>,</span> <span>&quot;jade&quot;</span><span>:</span> <span>&quot;0.27.2&quot;</span>
  <span>,</span> <span>&quot;validator&quot;</span><span>:</span> <span>&quot;0.4.11&quot;</span>
  <span>,</span> <span>&quot;sendgrid&quot;</span><span>:</span> <span>&quot;latest&quot;</span>
  <span>}</span>
<span>,</span> <span>&quot;devDependencies&quot;</span><span>:</span> <span>{</span>
    <span>&quot;mocha&quot;</span><span>:</span> <span>&quot;latest&quot;</span>
  <span>},</span>
  <span>&quot;engines&quot;</span><span>:</span> <span>{</span>
    <span>&quot;node&quot;</span><span>:</span> <span>&quot;0.8.9&quot;</span>
  <span>}</span>
<span>}</span>
</code></pre>
</div>
<p>Express has a built-in app generator, but I want to explain all the gory details. If you want to try it out, try typing <code>express myapp</code> in the terminal.</p>

<p>Back to the <code>package.json</code> file. The author and name can be changed as required. The <code>private</code> flag is set so we don’t accidentally publish this module to <a href="https://npmjs.org/">npmjs.org</a>. The dependencies are as follows:</p>

<ul>
<li><code>express</code>: The web framework we’re using, version 3 has been specified</li>

<li><code>jade</code>: The template language, you could convert this project to <a href="https://npmjs.org/package/ejs">ejs</a> or something else if desired</li>

<li><code>validator</code>: The <a href="https://github.com/chriso/node-validator">validator</a> library will be used to validate user input</li>

<li><code>sendgrid</code>: <a href="http://sendgrid.com/">SendGrid</a> is a commercial email provider that’s easy to use with Heroku</li>
</ul>

<p>The <code>engines</code> section has been included because it’s a good idea to be specific about Node versions when deploying to Heroku.</p>

<h3>Configuration</h3>

<p>Although I typically encourage breaking up Express projects into multiple files, this project will use a single JavaScript file for brevity.</p>

<p>First, the modules are loaded, and an Express app is instantiated. Users of Express 2.x will notice that there is no longer a <code>createServer()</code> method call:</p>
<div><pre><code><span>var</span> <span>express</span> <span>=</span> <span>require</span><span>(</span><span>&#39;express&#39;</span><span>)</span>
  <span>,</span> <span>app</span> <span>=</span> <span>express</span><span>()</span>
  <span>,</span> <span>SendGrid</span> <span>=</span> <span>require</span><span>(</span><span>&#39;sendgrid&#39;</span><span>).</span><span>SendGrid</span>
  <span>,</span> <span>Validator</span> <span>=</span> <span>require</span><span>(</span><span>&#39;validator&#39;</span><span>).</span><span>Validator</span>
  <span>;</span>
</code></pre>
</div>
<p>The <code>Validator</code> object is just one way to work with the node-validator module. The author has also provided <a href="https://github.com/ctavan/express-validator">Express middleware</a> for directly validating data in requests. I didn’t use it here because I was concerned it might not work with Express 3, and I’m writing to a deadline, but it’s worth taking a look at it. In general, I like to avoid tying too much code into Express in case I want to migrate to another framework, so that’s worth considering as well.</p>

<p>The next few lines are application configuration:</p>
<div><pre><code><span>app</span><span>.</span><span>configure</span><span>(</span><span>function</span><span>()</span> <span>{</span>
  <span>app</span><span>.</span><span>set</span><span>(</span><span>&#39;views&#39;</span><span>,</span> <span>__dirname</span> <span>+</span> <span>&#39;/views&#39;</span><span>);</span>
  <span>app</span><span>.</span><span>set</span><span>(</span><span>&#39;view engine&#39;</span><span>,</span> <span>&#39;jade&#39;</span><span>);</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>cookieParser</span><span>());</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>session</span><span>({</span> <span>secret</span><span>:</span> <span>&#39;secret goes here&#39;</span> <span>}));</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>bodyParser</span><span>());</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>app</span><span>.</span><span>router</span><span>);</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>csrf</span><span>());</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>static</span><span>(</span><span>__dirname</span> <span>+</span> <span>&#39;/public&#39;</span><span>));</span>
<span>});</span>
</code></pre>
</div>
<p>When you’re writing Express configuration, avoid copying and pasting lines from examples without fully understanding what each line does – it will get you into trouble later! You should understand what every single line does here, because changing the order of <code>app.use</code> lines can impact the way requests are processed and result in frustrating errors.</p>

<p>With that in mind, here’s what each line does:</p>

<ul>
<li><code>app.set(&#39;views&#39;, __dirname + &#39;/views&#39;)</code>: Use <code>./views</code> as the default path for the client-side templates</li>

<li><code>app.set(&#39;view engine&#39;, &#39;jade&#39;)</code>: Automatically load <code>index.jade</code> files just by passing <code>index</code></li>

<li><code>app.use(express.cookieParser())</code>: Parse the HTTP <code>Cookie</code> header and create an object in <code>req.cookies</code> with properties for each cookie</li>

<li><code>app.use(express.session...</code>: Use a session store – this is needed for the CSRF middleware</li>

<li><code>app.use(express.bodyParser())</code>: Parse the request body when forms are submitted with <code>application/x-www-form-urlencoded</code> (it also supports <code>application/json</code> and <code>multipart/form-data</code>)</li>

<li><code>app.use(app.router)</code>: Use the actual router provided by Express</li>

<li><code>app.use(express.csrf())</code>: The <a href="http://www.senchalabs.org/connect/csrf.html">CSRF</a> protection middleware</li>

<li><code>app.use(express.static(__dirname + &#39;/public&#39;))</code>: Serve static files in the <code>./public</code> directory</li>
</ul>

<p>Next follows configuration for development and production environments:</p>
<div><pre><code><span>app</span><span>.</span><span>configure</span><span>(</span><span>&#39;development&#39;</span><span>,</span> <span>function</span><span>()</span> <span>{</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>errorHandler</span><span>({</span> <span>dumpExceptions</span><span>:</span> <span>true</span><span>,</span> <span>showStack</span><span>:</span> <span>true</span> <span>}));</span>
  <span>app</span><span>.</span><span>locals</span><span>.</span><span>pretty</span> <span>=</span> <span>true</span><span>;</span>
  <span>sendgrid</span> <span>=</span> <span>{</span>
    <span>send</span><span>:</span> <span>function</span><span>(</span><span>opts</span><span>,</span> <span>cb</span><span>)</span> <span>{</span>
      <span>console</span><span>.</span><span>log</span><span>(</span><span>&#39;Email:&#39;</span><span>,</span> <span>opts</span><span>);</span>
      <span>cb</span><span>(</span><span>true</span><span>,</span> <span>opts</span><span>);</span>
    <span>}</span>
  <span>};</span>
<span>});</span>

<span>app</span><span>.</span><span>configure</span><span>(</span><span>&#39;production&#39;</span><span>,</span> <span>function</span><span>()</span> <span>{</span>
  <span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>errorHandler</span><span>());</span>
  <span>sendgrid</span> <span>=</span> <span>new</span> <span>SendGrid</span><span>(</span><span>process</span><span>.</span><span>env</span><span>.</span><span>SENDGRID_USERNAME</span><span>,</span> <span>process</span><span>.</span><span>env</span><span>.</span><span>SENDGRID_PASSWORD</span><span>);</span>
<span>});</span>
</code></pre>
</div>
<p>The <code>app.locals.pretty = true</code> line causes Jade to render templates with indentation and newlines; otherwise it spits out a single line of HTML. Notice that <code>app.use</code> is being called outside of <code>app.configure</code> – this is perfectly fine, and <code>app.use</code> can actually be called anywhere. There was some discussion about removing <code>app.configure</code> from Express 3.x, and it isn’t technically required.</p>

<p>I’ve made a mock <code>sendgrid</code> object for development mode that just prints out the email and then runs a callback. The production configuration block uses environmental variables (<code>process.env.SENDGRID_USERNAME</code>) to set the SendGrid username and password. It’s a good idea to use environmental variables for passwords, because it means you can keep them out of your source code repository. Since only specific developers should have access to the deployment environment, then it’s potentially safer to store variables there. Heroku allows such variables to be set with <code>heroku config:add SENDGRID_USERNAME=example</code>.</p>

<h3>Helpers</h3>

<p>The next few lines are new to Express 3:</p>
<div><pre><code><span>app</span><span>.</span><span>locals</span><span>.</span><span>errors</span> <span>=</span> <span>{};</span>
<span>app</span><span>.</span><span>locals</span><span>.</span><span>message</span> <span>=</span> <span>{};</span>
</code></pre>
</div>
<p>The <code>app.locals</code> object is passed to all templates, and it’s how helpers are defined in Express 3 applications. I’ve used these properties so I can write templates without first checking if these objects exist, else a <code>ReferenceError</code> would be raised.</p>

<h3>Middleware Callbacks: CSRF Protection</h3>

<p>I’ve mentioned CSRF but haven’t fully explained it yet. It stands for “Cross-Site Request Forgery”, and is a class of exploits in web applications where an attacker forces another user to execute unwanted actions on a web site. In this case it’s not particularly useful, but it’s good practice to guard against CSRF attacks in production web apps. The <a href="https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF">Open Web Application Security Project has a good article on CSRF</a>), which includes example attacks.</p>
<div><pre><code><span>function</span> <span>csrf</span><span>(</span><span>req</span><span>,</span> <span>res</span><span>,</span> <span>next</span><span>)</span> <span>{</span>
  <span>res</span><span>.</span><span>locals</span><span>.</span><span>token</span> <span>=</span> <span>req</span><span>.</span><span>session</span><span>.</span><span>_csrf</span><span>;</span>
  <span>next</span><span>();</span>
<span>}</span>
</code></pre>
</div>
<p>The <a href="http://www.senchalabs.org/connect/csrf.html">Connect CSRF middleware</a> automatically generates the <code>req.session._csrf</code> token, and this function maps it to <code>res.locals.token</code> so it will be available to templates. Any route that needs CSRF protection now just needs to include the middleware callback:</p>
<div><pre><code><span>app</span><span>.</span><span>get</span><span>(</span><span>&#39;/&#39;</span><span>,</span> <span>csrf</span><span>,</span> <span>function</span><span>(</span><span>req</span><span>,</span> <span>res</span><span>)</span> <span>{</span>
  <span>res</span><span>.</span><span>render</span><span>(</span><span>&#39;index&#39;</span><span>);</span>
<span>});</span>
</code></pre>
</div>
<p>The form in <code>views/index.jade</code> has a hidden input:</p>
<div><pre><code>form(action=&#39;/contact&#39;, method=&#39;post&#39;)
  input(type=&#39;hidden&#39;, name=&#39;_csrf&#39;, value=token)
</code></pre>
</div>
<p>The <code>token</code> variable is the one set by the middleware callback in <code>res.locals.token</code>.</p>

<h3>Validating Data</h3>

<p>The contact form must be validated before an email is sent. Seeing as database storage isn’t necessary for this project, we can use the <a href="https://github.com/chriso/node-validator">node-validator</a> module to verify user input. I’ve put this in a function to abstract it from the corresponding route:</p>
<div><pre><code><span>function</span> <span>validate</span><span>(</span><span>message</span><span>)</span> <span>{</span>
  <span>var</span> <span>v</span> <span>=</span> <span>new</span> <span>Validator</span><span>()</span>
    <span>,</span> <span>errors</span> <span>=</span> <span>[]</span>
    <span>;</span>

  <span>v</span><span>.</span><span>error</span> <span>=</span> <span>function</span><span>(</span><span>msg</span><span>)</span> <span>{</span>
    <span>errors</span><span>.</span><span>push</span><span>(</span><span>msg</span><span>);</span>
  <span>};</span>

  <span>v</span><span>.</span><span>check</span><span>(</span><span>message</span><span>.</span><span>name</span><span>,</span> <span>&#39;Please enter your name&#39;</span><span>).</span><span>len</span><span>(</span><span>1</span><span>,</span> <span>100</span><span>);</span>
  <span>v</span><span>.</span><span>check</span><span>(</span><span>message</span><span>.</span><span>email</span><span>,</span> <span>&#39;Please enter a valid email address&#39;</span><span>).</span><span>isEmail</span><span>();</span>
  <span>v</span><span>.</span><span>check</span><span>(</span><span>message</span><span>.</span><span>message</span><span>,</span> <span>&#39;Please enter a valid message&#39;</span><span>).</span><span>len</span><span>(</span><span>1</span><span>,</span> <span>1000</span><span>);</span>

  <span>return</span> <span>errors</span><span>;</span>
<span>}</span>
</code></pre>
</div>
<p>An instance of a <code>Validator</code> is created, and I’ve set a custom error handling function. This error handling function collects the errors into an array, but there are many other solutions supported by node-validator’s API.</p>

<p>Each message property is checked against a single validation, but several could be chained together.</p>

<p>The <code>validate</code> function itself expects a <code>message</code> object which will come from the posted form later.</p>

<h3>Sending Email</h3>

<p>Emails are sent with SendGrid. Again, I’ve made a function for this to keep it out of the corresponding Express routes:</p>
<div><pre><code><span>function</span> <span>sendEmail</span><span>(</span><span>message</span><span>,</span> <span>fn</span><span>)</span> <span>{</span>
  <span>sendgrid</span><span>.</span><span>send</span><span>({</span>
    <span>to</span><span>:</span> <span>process</span><span>.</span><span>env</span><span>.</span><span>EMAIL_RECIPIENT</span>
  <span>,</span> <span>from</span><span>:</span> <span>message</span><span>.</span><span>email</span>
  <span>,</span> <span>subject</span><span>:</span> <span>&#39;Contact Message&#39;</span>
  <span>,</span> <span>text</span><span>:</span> <span>message</span><span>.</span><span>message</span>
  <span>},</span> <span>fn</span><span>);</span>
<span>}</span>
</code></pre>
</div>
<p>I’ve made it accept a callback so the Express route can handle cases where sending the mail fails.</p>

<h3>Posting the Form</h3>

<p>Here is the Express route that handles the form post:</p>
<div><pre><code><span>app</span><span>.</span><span>post</span><span>(</span><span>&#39;/contact&#39;</span><span>,</span> <span>csrf</span><span>,</span> <span>function</span><span>(</span><span>req</span><span>,</span> <span>res</span><span>)</span> <span>{</span>
  <span>var</span> <span>message</span> <span>=</span> <span>req</span><span>.</span><span>body</span><span>.</span><span>message</span>
    <span>,</span> <span>errors</span> <span>=</span> <span>validate</span><span>(</span><span>message</span><span>)</span>
    <span>,</span> <span>locals</span> <span>=</span> <span>{}</span>
    <span>;</span>

  <span>function</span> <span>render</span><span>()</span> <span>{</span>
    <span>res</span><span>.</span><span>render</span><span>(</span><span>&#39;index&#39;</span><span>,</span> <span>locals</span><span>);</span>
  <span>}</span>

  <span>if</span> <span>(</span><span>errors</span><span>.</span><span>length</span> <span>===</span> <span>0</span><span>)</span> <span>{</span>
    <span>sendEmail</span><span>(</span><span>message</span><span>,</span> <span>function</span><span>(</span><span>success</span><span>)</span> <span>{</span>
      <span>if</span> <span>(</span><span>!</span><span>success</span><span>)</span> <span>{</span>
        <span>locals</span><span>.</span><span>error</span> <span>=</span> <span>&#39;Error sending message&#39;</span><span>;</span>
        <span>locals</span><span>.</span><span>message</span> <span>=</span> <span>message</span><span>;</span>
      <span>}</span> <span>else</span> <span>{</span>
        <span>locals</span><span>.</span><span>notice</span> <span>=</span> <span>&#39;Your message has been sent.&#39;</span><span>;</span>
      <span>}</span>
      <span>render</span><span>();</span>
    <span>});</span>
  <span>}</span> <span>else</span> <span>{</span>
    <span>locals</span><span>.</span><span>error</span> <span>=</span> <span>&#39;Your message has errors:&#39;</span><span>;</span>
    <span>locals</span><span>.</span><span>errors</span> <span>=</span> <span>errors</span><span>;</span>
    <span>locals</span><span>.</span><span>message</span> <span>=</span> <span>message</span><span>;</span>
    <span>render</span><span>();</span>
  <span>}</span>
<span>});</span>
</code></pre>
</div>
<p>It uses the <code>csrf</code> middleware callback to generate another token. This is required because the contact form will always be rerendered. The form data can be found in <code>req.body.message</code> – I’ve used form variables like <code>message[email]</code>, so these will get translated into a JavaScript object with corresponding properties.</p>

<p>When there are invalid fields, or sending the email fails, the contact form will be rendered again with the original message. To make the form retain the values, the <code>value</code> property of each field must be set:</p>
<div><pre><code>form(action=&#39;/contact&#39;, method=&#39;post&#39;)
  input(type=&#39;hidden&#39;, name=&#39;_csrf&#39;, value=token)
  .control-group
    label.control-label(for=&#39;message_name&#39;) Your Name
    .controls
      input#message_name.input-xxlarge(type=&#39;text&#39;, placeholder=&#39;Name&#39;, name=&#39;message[name]&#39;, value=message.name)
  .control-group
    label.control-label(for=&#39;message_email&#39;) Email
    .controls
      input#message_email.input-xxlarge(type=&#39;text&#39;, placeholder=&#39;Email&#39;, name=&#39;message[email]&#39;, value=message.email)
  .control-group
    label.control-label(for=&#39;message_message&#39;) Message
    .controls
      textarea#message_message.input-xxlarge(placeholder=&#39;Enter message&#39;, rows=&#39;6&#39;, name=&#39;message[message]&#39;)=message.message
  button.btn(type=&#39;submit&#39;) Send Message
</code></pre>
</div>
<p>This is quite a chunk of Jade, but the extra markup is there because I’ve used <a href="http://twitter.github.com/bootstrap/">Bootstrap</a> to style the project.</p>

<p>The <code>locals</code> object I’ve used gets passed to the <code>res.render</code> message and contains the form data when required.</p>

<h3>Download</h3>

<p>The full source is available here: <a href="https://github.com/alexyoung/dailyjs-contact-form-tutorial">alexyoung / dailyjs-contact-form-tutorial</a>.</p>
   <img src="http://feeds.feedburner.com/~r/dailyjs/~4/krIOIRJqY34" height="1" width="1">