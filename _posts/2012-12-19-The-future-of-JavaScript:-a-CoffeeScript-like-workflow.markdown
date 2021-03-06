---
layout: post
title:  "The future of JavaScript: a CoffeeScript-like workflow"
date:   2012-12-19 02:56:59
author: Axel Rauschmayer
categories: program
---

## The future of JavaScript: a CoffeeScript-like workflow
### by Axel Rauschmayer
### at 2012-12-19 02:56:59
### original <http://feedproxy.google.com/~r/2ality/~3/P2e0nXYKOUk/es6-workflow.html>

ECMAScript 6 <a>[1]</a> will bring many new features to the language. However, it will be years before we can rely on it being supported in most browsers that are in use. This post examines tools that will allow us to program with ECMAScript 6 much sooner.

<a name="more"></a>

<h3>Evolving a web programming language</h3>

JavaScript is a web programming language. You will thus always encounter a wide variety of language versions:
<ol>
    <li>Applications are written in many different ECMAScript versions. Even a single application is likely to be a mix of several versions, considering that it often includes libraries and external code (e.g. for analytics).
    </li>
    <li>Many different browser versions are in use, even quite old ones. In every browser, there is exactly one version of ECMAScript available. Users have little control over which version it is, because browsers often update themselves automatically (making new versions forced upgrades).
    </li>
</ol>
This imposes constraints on what can be done with ECMAScript 6: First, this version cannot introduce any changes that would break existing code. Second, app developers will have to wait years until they can use it – when it is supported by most browsers.
<p>
It is interesting to compare that with how things work with traditional programming languages: You have much more control. End users decide when to upgrade engine and app versions. As an app developer, you can usually demand that a reasonably recent version of a language be installed and can target that version.
Breaking changes to a language can be introduced by letting users install two versions in parallel (which is what Python did with version 3).

<h4>Why not versioning?</h4>

One possible solution is to simply annotate each piece of code with a language version. That would allow new versions to introduce breaking changes, because those won’t affect the old code. However, there are two problems with this approach: First, it’s a usability problem – who wants to start each code fragment with a version number? For ECMAScript 5 strict mode <a>[2]</a>, you have to do that and it hampered its adoption rate. Second, it’s a maintenance problem – you have to maintain several language versions in parallel inside a single engine. That would make browsers even more bloated.
Lastly, we still haven’t solved the issue of app developers not being able to use ECMAScript 6 right away. For the remainder of this post, we will focus on this issue and ignore the issues that engines are faced with.

<h4>Solution for app developers: a CoffeeScript-like workflow</h4>

If we want to use ECMAScript 6 as app developers, but still want to run our software on old browsers, we only have one option: compile ECMAScript 6 to ECMAScript 3 (possibly ECMAScript 5 in 1–2 years). The precedent is obvious: CoffeeScript has been doing this since December 2010. With source maps <a>[3]</a>, you can even debug its code in the browser – the JavaScript it has been compiled to is hidden from you. Several ECMAScript 6 features have been influenced by CoffeeScript (arrow functions <a>[4]</a>, classes <a>[5]</a>). Then why not use CoffeeScript and be done with it? There are several reasons:
<ul>
    <li>Different syntax: when CoffeeScript was created, giving it a different syntax made sense, because it wasn’t JavaScript and pretending so would have clashed with future versions of that language. However, now that CoffeeScript’s most important “fixes” are available in ECMAScript 6, we can go native again. This point is obviously controversial – people who like CoffeeScript often do so <i>because</i> of its different syntax, not despite it. I, however, agree with Nicholas Zakas’ <a href="http://www.nczonline.net/blog/2012/10/04/thoughts-on-typescript/">assertion</a>:
        <blockquote>
            There is a very real problem in the web development industry and that problem is a significant lack of good JavaScript developers.
            <p>
            [...] [I] want the web as a whole to continue to grow and get better, and that only happens when we have more competent developers entering the workforce.
            <p>
            I see compile-to-JavaScript languages as a barrier to that goal. We should be convincing more people to learn JavaScript rather than giving them more options to not write JavaScript. I often wonder what would happen if all of the teams and companies who spent time, energy, personnel, and money to develop these alternatives instead used those resources on improving JavaScript and teaching it.
        </p></p></blockquote>
    </li>
    <li>No compilation during development: Soon, you’ll be able to use an ECMAScript 6 capable browser during development and won’t have to compile. It’s a small thing, but still less complexity to worry about.
    </li>
    <li>New features: ECMAScript 6 can do things that CoffeeScript can’t. Two examples: modules and generators <a>[6]</a>.
    </li>
</ul>

<h3>Compiling ECMAScript 6 to ECMAScript 3</h3>

There are already several solutions for compiling ECMAScript 6 to ECMAScript 3 (or ECMAScript 5):
<ul>
    <li><a href="http://code.google.com/p/traceur-compiler/">Traceur</a>: compiles ECMAScript 6 to ECMAScript 3, on the fly. You can thus already play with ECMAScript 6 now. Dynamic compilation is an interesting alternative to compiling before deployment. Possible problems are performance and debugging.
    </li>
    <li><a href="http://modernizr.com">Harmonizr</a>: statically compiles to ECMAScript 3. It supports ECMAScript 6 features such as modules, arrow functions and classes.
    </li>
    <li><a href="http://www.typescriptlang.org/">TypeScript</a>: statically compiles to ECMAScript 3 and has several advanced features, some of them borrowed from ECMAScript 6 (classes, modules). Interestingly, feedback from TypeScript now helps with evolving ECMAScript. Issues encountered here let us discover potential problems with ECMAScript 6 – before it exists. Note that TypeScript is not always completely compatible with ECMAScript 6 (especially w.r.t. how it handles private properties).
    </li>
    <li>Esprima: Ariya Hidayat has written articles on how to use his ECMAScript parser <a href="http://esprima.org">Esprima</a> to compile ECMAScript 6 <a href="http://ariya.ofilabs.com/2012/06/esprima-and-harmony-module.html">modules</a> and <a href="http://ariya.ofilabs.com/2012/09/javascripts-future-class-syntax.html">classes</a> to earlier ECMAScript versions.
    </li>
</ul>

<h4>Compiling <tt>let</tt> declarations</h4>

Possibly the most difficult thing for compilers will be how to translate the block-scoped <tt>let</tt> variable declarations to the function-scoped <tt>var</tt> declarations. For example, the following code can remain largely unchanged, if <tt>tmp</tt> isn’t used outside the then-block.
<pre>
    // ECMAScript 6
    function foo(x, y) {
        if (x === 1) {
            let tmp = x + 1;
            ...
        }
    }
</pre>
This compiles to:
<pre>
    // ECMAScript 3
    function foo(x, y) {
        if (x === 1) {
            var tmp = x + 1;
            ...
        }
    }
</pre>
However, if <tt>tmp</tt> is used elsewhere then things are more complicated:
<pre>
    // ECMAScript 6
    function foo(x, y) {
        if (x === 1) {
            let tmp = x + 1;
            ...
        }
        if (y === 1) {
            let tmp = y + 1;
            ...
        }
    }
</pre>
Then you have the option to rename <tt>tmp</tt>:
<pre>
    // ECMAScript 3
    function foo(x, y) {
        if (x === 1) {
            var tmp_1 = x + 1;
            ...
        }
        if (y === 1) {
            var tmp_2 = y + 1;
            ...
        }
    }
</pre>
Or you can insert IIFEs <a>[7]</a>:
<pre>
    // ECMAScript 3
    function foo(x, y) {
        if (x === 1) (function () {
            var tmp = x + 1;
            ...
        }());
        if (y === 1) (function () {
            var tmp = y + 1;
            ...
        }());
    }
</pre>

<h4>The standard library</h4>

ECMAScript 6 will also have several useful additions to the standard library. For example: a map data structure (with arbitrary keys, as opposed to the string-only keys of objects) and additional string methods (<tt>startsWith()</tt>, <tt>endsWith()</tt>, <tt>repeat()</tt> etc.). Most of them can be shimmed on ECMAScript 5 an earlier via a library <a>[8]</a>. ECMAScript 6 compilers will probably provide this functionality in the same manner.

<h4>Using new language features directly</h4>

But when do we get to use ECMAScript 6 natively? I see three options:
<ol>
    <li>Wait until ECMAScript 6 is the dominant version, switch to it completely. That will take a while.
    </li>
    <li>Dynamically feature-detect what a browser supports and shim less functionality.
    </li>
    <li>Determine what ECMAScript version a browser supports before delivering JavaScript source files from the server. If it supports ECMAScript 6, deliver the original source code. If it doesn’t, deliver the compiled source code.
    </li>
</ol>
As far as I can tell, implementing (3) is currently difficult. Maybe there should be better support for it in browsers (by sending the supported ECMAScript version to the server?).

<h3>Conclusion</h3>

There are good times ahead for JavaScript. ECMAScript 6 will introduce many great features, compilation will ensure that we’ll actually be able to program with them. If you want to find out what ECMAScript 6 can do, take a look at the <a href="http://www.2ality.com/2012/11/guide-esnext.html">guide</a> to posts about ECMAScript.next (ECMAScript 6’s code name) on 2ality.

<h3>References</h3>

<ol>
    <li><a href="http://www.2ality.com/2011/06/ecmascript.html">ECMAScript: ES.next versus ES 6 versus ES Harmony</a></li>
    <li><a href="http://www.2ality.com/2011/01/javascripts-strict-mode-summary.html">JavaScript’s strict mode: a summary</a></li>
    <li><a href="http://www.2ality.com/2011/07/firefox-sourcemap.html">SourceMap on Firefox: source debugging for languages compiled to JavaScript [update: WebKit, too]</a></li>
    <li><a href="http://www.2ality.com/2012/04/arrow-functions.html">ECMAScript.next: arrow functions and method definitions</a></li>
    <li><a href="http://www.2ality.com/2012/07/esnext-classes.html">ECMAScript.next: classes</a></li>
    <li><a href="http://www.2ality.com/2012/06/for-of-ff13.html">ECMAScript.next’s for-of loop</a> [explains what generators are]</li>
    <li><a href="http://www.2ality.com/2011/02/javascript-variable-scoping-and-its.html">JavaScript variable scoping and its pitfalls</a></li>
    <li><a href="http://www.2ality.com/2011/12/es6-shim.html">es6-shim – ECMAScript 6 functionality on ECMAScript 5</a></li>
</ol><img src="http://feeds.feedburner.com/~r/2ality/~4/P2e0nXYKOUk" height="1" width="1"></p>