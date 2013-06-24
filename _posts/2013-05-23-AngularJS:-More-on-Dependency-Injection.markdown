---
layout: post
title:  "AngularJS: More on Dependency Injection"
date:   2013-05-23 07:00:00
author: 
categories: program
---

## AngularJS: More on Dependency Injection
### by 
### at 2013-05-23 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/dEzcXA0X9fc/angularjs-injection>

<p>In the <a href="http://dailyjs.com/tags#angularfeeds">AngularJS tutorials</a> I’ve been writing, you might have noticed the use of dependency injection. In this article I’m going to explain how dependency injection works, and how it relates to the small tutorial project we’ve created.</p>

<p>Dependency injection is a software design pattern. The motivation for using it in Angular is to make it easier to transparently load mocked objects in tests. The <code>$http</code> module is a great example of this: when writing tests you don’t want to make real network calls, but defer the work to a fake object that responds with fixture data.</p>

<p>The earlier tutorials used dependency injection for this exact use case: in <a href="https://github.com/alexyoung/djsreader/blob/c9f9d06258f4973018a1cc48c226642bbb32938f/app/scripts/controllers/main.js#L3-L4">main controller</a>, the <code>MainCtrl</code> module is set up to load the <code>$http</code> module which can then be transparently replaced during testing.</p>
<div><pre><code><span>angular</span><span>.</span><span>module</span><span>(</span><span>&#39;djsreaderApp&#39;</span><span>)</span>
  <span>.</span><span>controller</span><span>(</span><span>&#39;MainCtrl&#39;</span><span>,</span> <span>function</span><span>(</span><span>$scope</span><span>,</span> <span>$http</span><span>,</span> <span>$timeout</span><span>)</span> <span>{</span>
</code></pre>
</div>
<p>Now forget everything I just said about dependency injection, and look at the callback that has been passed to <code>.controller</code> in the previous example. The <code>$http</code> and <code>$timeout</code> modules have been added by me because I want to use the <a href="http://docs.angularjs.org/api/ng.$http">$http service</a> and the <a href="http://docs.angularjs.org/api/ng.$timeout">$timeout service</a>. These are built-in “services” (an Angular term), but they’re not standard arguments. In fact, I could have specified these arguments in any order:</p>
<div><pre><code><span>angular</span><span>.</span><span>module</span><span>(</span><span>&#39;djsreaderApp&#39;</span><span>)</span>
  <span>.</span><span>controller</span><span>(</span><span>&#39;MainCtrl&#39;</span><span>,</span> <span>function</span><span>(</span><span>$scope</span><span>,</span> <span>$timeout</span><span>,</span> <span>$http</span><span>)</span> <span>{</span>
</code></pre>
</div>
<p>This is possible because Angular looks at the function argument <em>names</em> to load dependencies. Before you run away screaming about magic, it’s important to realise that this is just one way to load dependencies in Angular projects. For example, this is equivalent:</p>
<div><pre><code><span>angular</span><span>.</span><span>module</span><span>(</span><span>&#39;djsreaderApp&#39;</span><span>)</span>
  <span>.</span><span>controller</span><span>(</span><span>&#39;MainCtrl&#39;</span><span>,</span> <span>[</span><span>&#39;$scope&#39;</span><span>,</span> <span>&#39;$http&#39;</span><span>,</span> <span>&#39;$timeout&#39;</span><span>],</span> <span>function</span><span>(</span><span>$scope</span><span>,</span> <span>$http</span><span>,</span> <span>$timeout</span><span>)</span> <span>{</span>
</code></pre>
</div>
<p>The array-based style is more like <a href="https://github.com/amdjs/amdjs-api/wiki/AMD">AMD</a>, and requires a little bit of syntactical overhead. I call the first style “introspective dependency injection”. The array-based syntax allows us to use different names for the dependencies, which can be useful sometimes.</p>

<p>This raises the question: how does introspective dependency injection cope with minimisers, where variables are renamed to shorter values? Well, it doesn’t cope with it at all. In fact, minimisers need help to translate the first style to the second.</p>

<h3>Yeoman and ngmin</h3>

<p>One reason I built the tutorial series with <a href="http://yeoman.io/">Yeoman</a> was because the Angular generator includes <a href="https://github.com/btford/grunt-ngmin">grunt-ngmin</a>. This is a <a href="http://gruntjs.com/">Grunt</a> task that uses <a href="https://github.com/btford/ngmin">ngmin</a> – an Angular-aware “pre-minifier”. It allows you to use the shorter, introspective dependency injection syntax, while still generating valid minimised production builds.</p>

<p>Therefore, building a production version of <a href="https://github.com/alexyoung/djsreader">djsreader</a> with <code>grunt build</code> will correctly generate a deployable version of the project.</p>

<p>Why is it that almost all of Angular’s documentation and tutorials include the potentially dangerous introspective dependency injection syntax? I’m not sure, and I haven’t looked into it. I’d be happier if the only valid solution was the array-based approach, which looks more like AMD which most of us are already comfortable with anyway.</p>

<p>Just to prove I’m not making things up, here is the minimised source for djsreader:</p>
<div><pre><code><span>&quot;use strict&quot;</span><span>;</span><span>angular</span><span>.</span><span>module</span><span>(</span><span>&quot;djsreaderApp&quot;</span><span>,[]).</span><span>config</span><span>([</span><span>&quot;$routeProvider&quot;</span><span>,</span><span>function</span><span>(</span><span>e</span><span>){</span><span>e</span><span>.</span><span>when</span><span>(</span><span>&quot;/&quot;</span><span>,{</span><span>templateUrl</span><span>:</span><span>&quot;views/main.html&quot;</span><span>,</span><span>controller</span><span>:</span><span>&quot;MainCtrl&quot;</span><span>}).</span><span>otherwise</span><span>({</span><span>redirectTo</span><span>:</span><span>&quot;/&quot;</span><span>})}]),</span><span>angular</span><span>.</span><span>module</span><span>(</span><span>&quot;djsreaderApp&quot;</span><span>).</span><span>controller</span><span>(</span><span>&quot;MainCtrl&quot;</span><span>,[</span><span>&quot;$scope&quot;</span><span>,</span><span>&quot;$http&quot;</span><span>,</span><span>&quot;$timeout&quot;</span><span>,</span><span>function</span><span>(</span><span>e</span><span>,</span><span>r</span><span>,</span><span>t</span><span>){</span><span>e</span><span>.</span><span>refreshInterval</span><span>=</span><span>60</span><span>,</span><span>e</span><span>.</span><span>feeds</span><span>=</span><span>[{</span><span>url</span><span>:</span><span>&quot;http://dailyjs.com/atom.xml&quot;</span><span>}],</span><span>e</span><span>.</span><span>fetchFeed</span><span>=</span><span>function</span><span>(</span><span>n</span><span>){</span><span>n</span><span>.</span><span>items</span><span>=</span><span>[];</span><span>var</span> <span>o</span><span>=</span><span>&quot;http://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20xml%20where%20url%3D&#39;&quot;</span><span>;</span><span>o</span><span>+=</span><span>encodeURIComponent</span><span>(</span><span>n</span><span>.</span><span>url</span><span>),</span><span>o</span><span>+=</span><span>&quot;&#39;%20and%20itemPath%3D&#39;feed.entry&#39;&amp;format=json&amp;diagnostics=true&amp;callback=JSON_CALLBACK&quot;</span><span>,</span><span>r</span><span>.</span><span>jsonp</span><span>(</span><span>o</span><span>).</span><span>success</span><span>(</span><span>function</span><span>(</span><span>e</span><span>){</span><span>e</span><span>.</span><span>query</span><span>.</span><span>results</span><span>&amp;&amp;</span><span>(</span><span>n</span><span>.</span><span>items</span><span>=</span><span>e</span><span>.</span><span>query</span><span>.</span><span>results</span><span>.</span><span>entry</span><span>)}).</span><span>error</span><span>(</span><span>function</span><span>(</span><span>e</span><span>){</span><span>console</span><span>.</span><span>error</span><span>(</span><span>&quot;Error fetching feed:&quot;</span><span>,</span><span>e</span><span>)}),</span><span>t</span><span>(</span><span>function</span><span>(){</span><span>e</span><span>.</span><span>fetchFeed</span><span>(</span><span>n</span><span>)},</span><span>1</span><span>e3</span><span>*</span><span>e</span><span>.</span><span>refreshInterval</span><span>)},</span><span>e</span><span>.</span><span>addFeed</span><span>=</span><span>function</span><span>(</span><span>r</span><span>){</span><span>e</span><span>.</span><span>feeds</span><span>.</span><span>push</span><span>(</span><span>r</span><span>),</span><span>e</span><span>.</span><span>fetchFeed</span><span>(</span><span>r</span><span>),</span><span>e</span><span>.</span><span>newFeed</span><span>=</span><span>{}},</span><span>e</span><span>.</span><span>deleteFeed</span><span>=</span><span>function</span><span>(</span><span>r</span><span>){</span><span>e</span><span>.</span><span>feeds</span><span>.</span><span>splice</span><span>(</span><span>e</span><span>.</span><span>feeds</span><span>.</span><span>indexOf</span><span>(</span><span>r</span><span>),</span><span>1</span><span>)},</span><span>e</span><span>.</span><span>fetchFeed</span><span>(</span><span>e</span><span>.</span><span>feeds</span><span>[</span><span>0</span><span>])}]);</span>
</code></pre>
</div>
<p>The demangled version shows that we’re using the array-based syntax, thanks to ngmin:</p>
<div><pre><code><span>angular</span><span>.</span><span>module</span><span>(</span><span>&quot;djsreaderApp&quot;</span><span>).</span><span>controller</span><span>(</span><span>&quot;MainCtrl&quot;</span><span>,</span> <span>[</span><span>&quot;$scope&quot;</span><span>,</span> <span>&quot;$http&quot;</span><span>,</span> <span>&quot;$timeout&quot;</span><span>,</span>
</code></pre>
</div>
<h3>Internals</h3>

<p>In case you’re wondering how the introspective dependency injection style works, then look no further than <a href="https://github.com/angular/angular.js/blob/0272240400d7896224f34b9f10b492994e29c655/src/auto/injector.js#L45-L71">annotate(fn)</a>. This function uses <code>Function.prototype.toString</code> to extract the argument names from the JavaScript source code. The results are effectively cached, so even though this sounds horrible it doesn’t perform as badly as it could.</p>

<h3>Conclusion</h3>

<p>Nothing I’ve said here is new – while researching this post I found <a href="http://www.alexrothenberg.com/2013/02/11/the-magic-behind-angularjs-dependency-injection.html">The Magic Behind Dependency Injection</a> by Alex Rothenberg, which covers the same topic, and the <a href="http://docs.angularjs.org/guide/di">Angular Dependency Injection documentation</a> outlines the issues caused by the introspective approach and suggests that it should only be used for <a href="http://www.pretotyping.org/">pretotyping</a>.</p>

<p>However, I felt like it was worth writing an overview of the matter, because although Yeoman is great for a quick start to a project, you really need to understand what’s going on behind the scenes!</p>
   <img src="http://feeds.feedburner.com/~r/dailyjs/~4/dEzcXA0X9fc" height="1" width="1">