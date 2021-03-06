---
layout: post
title:  "Plans for supporting Web Components in AngularJS and Ember.js"
date:   2013-05-23 20:29:12
author: Axel Rauschmayer
categories: program
---

## Plans for supporting Web Components in AngularJS and Ember.js
### by Axel Rauschmayer
### at 2013-05-23 20:29:12
### original <http://feedproxy.google.com/~r/2ality/~3/TuKuFE60neE/web-components-angular-ember.html>

Web Components <a>[1]</a> are an upcoming standard for custom HTML5 user interface elements. Those UI elements will eventually become interchangeable between frameworks. Now the people behind AngularJS and Ember.js have described their plans for supporting Web Components.
<a name="more"></a>
<p>
Below, you’ll see mentions of Google’s new framework, Polymer <a>[1]</a>. It is built directly on top of Web Components. One of Polymer’s goals is to help refine and fully figure out that standard.

<h3>AngularJS</h3>

In an <a href="https://groups.google.com/d/msg/polymer-dev/4RSYaKmbtEk/uYnY3900wpIJ">entry</a> on Google Groups, AngularJS co-creator Miško Hevery writes:
<blockquote>
We're in early stages of designing Angular 2.0, but some of our goals are:
<ul>
<li>Angular will use the underlying web platform features available to it (e.g. Node.bind, template integration, Custom Elements, etc...)</li>
<li>Web Components (Polymer, Ember, or any other framework/library) will work seamlessly within Angular apps and directives.</li>
<li>Components written in Angular will export to Web Components (to be used by Polymer, Ember, or any other framework/library).</li>
</ul>
We're working actively with the MDV, Web Components, and Polymer teams to make sure that our approaches remain compatible as all these projects evolve (and they will still evolve).
</blockquote>

<h3>Ember.js</h3>

Yehuda Katz, one of Ember‘s creators, has written a Gist with the title “<a href="https://gist.github.com/wycats/9144666b0c606d1838be">Ember and Web Components</a>”:
<blockquote>
The goal of this document is to describe how Ember could adopt semantics similar to web components and MDV. It relies on the HTMLBars templating engine, which allows Ember to directly control how templates are parsed and converted into DOM. [<a href="https://github.com/tildeio/htmlbars">HTMLBars</a> uses the same Handlebar syntax as current Ember, but compiles it to code that constructs DOM nodes instead of code that concatenates strings. That leads to the above mentioned increase in control.]
<p>
It builds on the work of Rafael Weinstein and the Polymer team, and attempts to harmonize that work with Ember's architecture.
<p>
Ember's scope is much wider than components, and is mostly focused on application architecture and URL-driven design. Today, we need a system to manage the lifecycle of data-binding and custom views, so we include such a system alongside our architectural tools.
<p>
Once the web provides its own tools for managing components, and eventually data bindings, Ember will embrace them and transition away from our Ember-specific solution. This document is how we get from here to there, continuing to build ambitious and stable applications in the meantime.
<p>
[...]
</p></p></p></p></blockquote>
The document is a great primer on how Web Components differ from Ember.

<h3>Reference</h3>

<ol>
    <li><a href="http://www.2ality.com/2013/05/google-polymer.html">Google’s Polymer and the future of web UI frameworks</a>
        [describes both Web Components and Polymer]
    </li>
</ol>
<img src="http://feeds.feedburner.com/~r/2ality/~4/TuKuFE60neE" height="1" width="1"></p>