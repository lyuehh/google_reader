---
layout: post
title:  "AngularJS: Iterators and Filters"
date:   2013-06-13 07:00:00
author: 
categories: program
---

## AngularJS: Iterators and Filters
### by 
### at 2013-06-13 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/9A6Te4aeg8Y/angularjs-8>

<ul>
  <li><a href="http://dailyjs.com/2013/04/11/angularjs-1/">Part 1: Google, Twitter, and AngularJS</a></li>
  <li><a href="http://dailyjs.com/2013/04/18/angularjs-2/">Part 2: Let's Make a Feed Reader</a></li>
  <li><a href="http://dailyjs.com/2013/04/25/angularjs-3/">Part 3: Rendering Feeds</a></li>
  <li><a href="http://dailyjs.com/2013/05/09/angularjs-4/">Part 4: Managing Feeds</a></li>
  <li><a href="http://dailyjs.com/2013/05/16/angularjs-5/">Part 5: Tests</a></li>
  <li><a href="http://dailyjs.com/2013/05/30/angularjs-6/">Part 6: Adding Dependencies</a></li>
  <li><a href="http://dailyjs.com/2013/06/06/angularjs-7/">Part 7: Form Validation</a></li>
  <li><a href="http://dailyjs.com/2013/06/13/angularjs-8/"><strong>Part 8: Iterators and Data</strong></a></li>
</ul>
<p>AngularJS has a rich expression-based system for filtering and ordering data based on predicates. The <a href="http://docs.angularjs.org/api/ng.filter:orderBy">orderBy filter</a> can be used with the <code>ng-repeat</code> directive:</p>
<div><pre><code><span>&lt;ul&gt;</span>
  <span>&lt;li</span> <span>ng-repeat=</span><span>&quot;item in stories | orderBy:predicate:date&quot;</span><span>&gt;&lt;a</span> <span>href=</span><span>&quot;&quot;</span><span>&gt;&lt;/a&gt;&lt;/li&gt;</span>
<span>&lt;/ul&gt;</span>
</code></pre>
</div>
<p>Today we’re going to use <code>orderBy</code> inside a controller using dependency injection to organise multiple feeds into a river of news sorted by date.</p>

<h3>Iterating in Controllers</h3>

<p>Before sorting and displaying stories, we need to collect them into a suitable data structure. An array will suffice (<code>app/scripts/controllers/main.js</code>):</p>
<div><pre><code><span>$scope</span><span>.</span><span>stories</span> <span>=</span> <span>[];</span>
</code></pre>
</div>
<p>Next we need to append stories to this collection, but only if they haven’t already been added. Let’s use a function to encapsulate that away from fetching stories:</p>
<div><pre><code><span>$scope</span><span>.</span><span>fetchFeed</span> <span>=</span> <span>function</span><span>(</span><span>feed</span><span>)</span> <span>{</span>
  <span>feed</span><span>.</span><span>items</span> <span>=</span> <span>[];</span>

  <span>var</span> <span>apiUrl</span> <span>=</span> <span>&quot;http://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20xml%20where%20url%3D&#39;&quot;</span><span>;</span>
  <span>apiUrl</span> <span>+=</span> <span>encodeURIComponent</span><span>(</span><span>feed</span><span>.</span><span>url</span><span>);</span>
  <span>apiUrl</span> <span>+=</span> <span>&quot;&#39;%20and%20itemPath%3D&#39;feed.entry&#39;&amp;format=json&amp;diagnostics=true&amp;callback=JSON_CALLBACK&quot;</span><span>;</span>

  <span>$http</span><span>.</span><span>jsonp</span><span>(</span><span>apiUrl</span><span>).</span>
    <span>success</span><span>(</span><span>function</span><span>(</span><span>data</span><span>)</span> <span>{</span>
      <span>if</span> <span>(</span><span>data</span><span>.</span><span>query</span><span>.</span><span>results</span><span>)</span> <span>{</span>
        <span>feed</span><span>.</span><span>items</span> <span>=</span> <span>data</span><span>.</span><span>query</span><span>.</span><span>results</span><span>.</span><span>entry</span><span>;</span>
      <span>}</span>
      <span>addStories</span><span>(</span><span>feed</span><span>.</span><span>items</span><span>);</span>
</code></pre>
</div>
<p>The <code>addStories</code> function just needs to loop over each feed item to determine if it’s already been added to <code>$scope.stories</code>. The <a href="http://docs.angularjs.org/api/angular.forEach">angular.forEach</a> API in module <code>ng</code> is the perfect way to do this:</p>
<div><pre><code><span>function</span> <span>addStories</span><span>(</span><span>stories</span><span>)</span> <span>{</span>
  <span>var</span> <span>changed</span> <span>=</span> <span>false</span><span>;</span>
  <span>angular</span><span>.</span><span>forEach</span><span>(</span><span>stories</span><span>,</span> <span>function</span><span>(</span><span>story</span><span>,</span> <span>key</span><span>)</span> <span>{</span>
    <span>if</span> <span>(</span><span>!</span><span>storyInCollection</span><span>(</span><span>story</span><span>))</span> <span>{</span>
      <span>$scope</span><span>.</span><span>stories</span><span>.</span><span>push</span><span>(</span><span>story</span><span>);</span>
      <span>changed</span> <span>=</span> <span>true</span><span>;</span>
    <span>}</span>
  <span>});</span>
<span>}</span>
</code></pre>
</div>
<p>As you can see, <code>forEach</code> accepts an array and a function to call for each item. The <code>storyInCollection</code> function now needs to loop over each existing story to see if it’s already been added. Figuring out which story is unique is easy because feeds have an <code>id</code> value:</p>
<div><pre><code><span>function</span> <span>storyInCollection</span><span>(</span><span>story</span><span>)</span> <span>{</span>
  <span>for</span> <span>(</span><span>var</span> <span>i</span> <span>=</span> <span>0</span><span>;</span> <span>i</span> <span>&lt;</span> <span>$scope</span><span>.</span><span>stories</span><span>.</span><span>length</span><span>;</span> <span>i</span><span>++</span><span>)</span> <span>{</span>
    <span>if</span> <span>(</span><span>$scope</span><span>.</span><span>stories</span><span>[</span><span>i</span><span>].</span><span>id</span> <span>===</span> <span>story</span><span>.</span><span>id</span><span>)</span> <span>{</span>
      <span>return</span> <span>true</span><span>;</span>
    <span>}</span>
  <span>}</span>
  <span>return</span> <span>false</span><span>;</span>
<span>}</span>
</code></pre>
</div>
<h3>Storing Data</h3>

<p>Now we’ve got a list of stories in our scope, we need to sort them by date just like a real feed reader. Whenever <code>addStories</code> changes the list of stories we should sort it. AngularJS doesn’t really have any fancy functional methods like <code>map</code> or <code>some</code>, which you can find in ECMAScript 5 anyway, but it does provide API access to the filtering and sorting modules that are typically used in templates.</p>

<p>To access this functionality you’ll need to load <code>$filter</code>:</p>
<div><pre><code><span>angular</span><span>.</span><span>module</span><span>(</span><span>&#39;djsreaderApp&#39;</span><span>)</span>
  <span>.</span><span>controller</span><span>(</span><span>&#39;MainCtrl&#39;</span><span>,</span> <span>function</span><span>(</span><span>$scope</span><span>,</span> <span>$http</span><span>,</span> <span>$timeout</span><span>,</span> <span>$filter</span><span>)</span> <span>{</span>
</code></pre>
</div>
<p><code>$filter</code> will return a function that knows how to sort or filter arrays. That means you need to call it with the name of the desired method, then call the value returned with an array and an expression: <code>$filter(filter)(array, expression)</code>. To add sorting to our feeds, call <code>$filter()()</code> and update the <code>$scope.stories</code> array:</p>
<div><pre><code><span>// At the end of addStories</span>
<span>if</span> <span>(</span><span>changed</span><span>)</span> <span>{</span>
  <span>$scope</span><span>.</span><span>stories</span> <span>=</span> <span>$filter</span><span>(</span><span>&#39;orderBy&#39;</span><span>)(</span><span>$scope</span><span>.</span><span>stories</span><span>,</span> <span>&#39;date&#39;</span><span>);</span>
<span>}</span>
</code></pre>
</div>
<p>The only thing left to do is update the template in <code>app/views/mail.html</code>:</p>
<div><pre><code><span>&lt;ul&gt;</span>
  <span>&lt;li</span> <span>ng-repeat=</span><span>&quot;item in stories&quot;</span><span>&gt;&lt;a</span> <span>href=</span><span>&quot;&quot;</span><span>&gt;&lt;/a&gt;&lt;/li&gt;</span>
<span>&lt;/ul&gt;</span>
</code></pre>
</div>
<p>If you add multiple feeds using the app’s web interface you should see them combined into a river of news.</p>

<h3>Conclusion</h3>

<p><img src="http://dailyjs.com/images/posts/angularjs-river-of-news.png" alt="The river of news view"></p>

<p>You can find this code in <a href="https://github.com/alexyoung/djsreader/commit/ff4d6a613e8732a19a768fead82044b5411dca0c">commit ff4d6a6</a>.</p>
   <img src="http://feeds.feedburner.com/~r/dailyjs/~4/9A6Te4aeg8Y" height="1" width="1">