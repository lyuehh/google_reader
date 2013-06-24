---
layout: post
title:  "Managing module dependencies"
date:   2011-12-06 00:30:20
author: kishore@kishorelive.com (Kishore Nallan)
categories: program
---

## Managing module dependencies
### by kishore@kishorelive.com (Kishore Nallan)
### at 2011-12-06 00:30:20
### original <http://howtonode.org/managing-module-dependencies>

<p>Following <a href="http://groups.google.com/group/nodejs/browse_thread/thread/9aa563f1fe3b3ff5">this discussion</a> on the node.js mailing list about managing module dependencies, I thought it's worth sharing some pointers on that here.</p>

<h2>Using NPM to bundle your module dependencies</h2>

<p>If you're building an application that is dependent on a number of NPM modules, you can specify them in your <code>package.json</code> file this way: </p>

<pre><code>&quot;dependencies&quot;: {
  &quot;express&quot;: &quot;2.3.12&quot;,
  &quot;jade&quot;: &quot;&gt;= 0.0.1&quot;,
  &quot;redis&quot;:   &quot;0.6.0&quot;
}
</code></pre>

<p>By doing so, every time you checkout your project fresh, all you need to do to get your dependencies sorted out is:</p>

<pre><code>$ npm install
</code></pre>

<p>Note that you can either require a specific version of a module, or a minimum version by prefixing the version number with <code>&gt;=</code>.</p>

<h2>Managing development dependencies</h2>

<p>If you have development related dependencies (e.g. testing framework) which you do not wish to install in production, specify them using the <code>devDependencies</code> property:</p>

<pre><code>&quot;devDependencies&quot;: {
  &quot;vows&quot;: &quot;&gt;= 0.4.x&quot;
}
</code></pre>

<p>On production, using <code>npm install --production</code> will ensure that the development dependencies are not installed. </p>

<h2>Managing private NPM modules</h2>

<p>If you're working on a private module, you can also add <code>"private": true</code> to the <code>package.json</code> file to prevent yourself from accidentally publishing your module to the NPM registry. </p>

<h2>Specifying a git repo as a dependency</h2>

<p>Finally, if you want to host your module in a private git repository, but still want to bundle it as a dependency to a project, you can do that too:</p>

<pre><code>"dependencies": {
    "secret-module": "git+ssh://git@github.com:username/secret-repo.git#v2.3"
}
</code></pre>

<p>The last part of the URL (<code>v2.3</code>) specifies which tag should be used. You can also specify a commit hash or a branch name. </p>

<p>This feature was added to the NPM only around mid August, so you will need a recent version of NPM for this to work. There are a lot more such <a href="https://github.com/isaacs/npm/issues?labels=nice+to+have&amp;sort=created&amp;direction=desc&amp;state=open&amp;page=1">nifty feature requests</a> in the pipeline, so keep a watch out for them on NPM's Github repo.</p>