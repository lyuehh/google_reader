---
layout: post
title:  "JavaScript: __proto__"
date:   2012-11-08 18:17:27
author: Axel Rauschmayer
categories: program
---

## JavaScript: __proto__
### by Axel Rauschmayer
### at 2012-11-08 18:17:27
### original <http://feedproxy.google.com/~r/2ality/~3/xBQstor_j1E/proto.html>

[This post is part of a <a href="http://www.2ality.com/search/label/__proto__">series</a> on the special property <tt>__proto__</tt>]
<p>
This blog post looks at the special property <tt>__proto__</tt>, which allows you to get and set the prototype of an object. In order to understand this post, you should be familiar with JavaScript’s prototypal inheritance <a>[1]</a>.

<a name="more"></a>

<h3>The special property __proto__</h3>

The ECMAScript standard specifies objects as pointing to their prototype via the internal property <tt>[[Prototype]]</tt>. That property cannot be directly modified in ECMAScript 5, but one can use <tt>Object.getPrototypeOf()</tt> to read it and <tt>Object.create()</tt> to create new objects that have a given prototype. For example, the following code creates an object <tt>obj</tt> whose prototype is <tt>myProto</tt>.
<pre>
    &gt; var myProto = {};
    &gt; var obj = Object.create(myProto);

    &gt; Object.getPrototypeOf(obj) === myProto
    true
</pre>
<p>
<tt>__proto__</tt> (pronounced “dunder proto”, from “double underscore” <a>[2]</a>) first appeared in Firefox and is an alias for <tt>[[Prototype]]</tt>. Using <tt>__proto__</tt>, the above code becomes:
<pre>
    &gt; var myProto = {};
    &gt; var obj = { __proto__: myProto };

    &gt; obj.__proto__ === myProto
    true
</pre>
The following also holds:
<pre>
    &gt; obj.__proto__ === Object.getPrototypeOf(obj)
    true
</pre>
Once it appeared in Firefox, <tt>__proto__</tt> proved so popular that it is now also supported by V8 (Chrome, Node.js) and Nitro (Safari). As of ECMAScript 5, it is still non-standard, but due to its popularity, it will become part of ECMAScript 6.

<h4>Checking whether __proto__ is supported</h4>

The following expression returns true if a JavaScript engine supports <tt>__proto__</tt>:
<pre>
    Object.getPrototypeOf({ __proto__: null }) === null
</pre>

<h4>Caveat: objects as maps</h4>

If you use an object as a map from strings to values (with arbitrary keys) then you have to be careful about keys whose value is <tt>"__proto__"</tt> <a>[3]</a>. For example:
<pre>
    function escapeKey(key) {
        // We need to escape "__proto__", including the
        // (n times) escaped version of it, to avoid clashes
        if (key.indexOf("__proto__") === 0) {
            return key+"%";
        } else {
            return key;
        }
    }
</pre>
Obviously, you have to escape keys for both read and write access. Thus, you never need to un-escape.

<h3>Two use cases</h3>

Two important use cases for <tt>__proto__</tt> are: Creating objects with a given prototype and subtyping built-in types.

<h4>Creating objects with a given prototype</h4>

The usability advantage of <tt>__proto__</tt> becomes apparent if you create a non-empty object:
<pre>
    var obj = {
        __proto__: myProto,
        foo: 123,
        bar: "abc"
    };
</pre>
With <tt>Object.create()</tt>, you have two, equally unappealing, alternatives:
<pre>
    // Alternative 1: create empty object, assign
    var obj = Object.create(myProto);
    obj.foo = 123;
    obj.bar = "abc";

    // Alternative 2: property descriptors
    var obj = Object.create(myProto, {
        foo: {
            value: 123,
            writable: true,
            enumerable: true,
            configurable: true
        },
        bar: {
            value: "abc",
            writable: true,
            enumerable: true,
            configurable: true
        }
    });
</pre>

<h4>Subtyping built-in types</h4>

JavaScript’s built-ins are notoriously hard to subtype <a>[4]</a>, for two reasons.
First, the standard subtyping pattern in JavaScript is that a sub-constructor initially passes its instance to the super-constructor so that it can add its properties. It does so by calling the super-constructor as a function with <tt>this</tt> pointing to the instance. Alas, most built-in constructors don’t let you do that, they ignore a passed-in <tt>this</tt>.
Second, some built-ins have instances that are special, so even if you could pass the sub-instance to the super-constructor, you would never end up with a working instance. The main example is arrays, whose <tt>length</tt> property is updated if new elements are added.

<pre>
    var MyArrayProto = Object.create(Array.prototype);
    MyArrayProto.foo = function (...) { ... };
    function createMyArray() {
        var arr = Array.prototype.slice.call(arguments);
        arr.__proto__ = MyArrayProto;
    }
    var myarr = createMyArray();
</pre>

<h3>ECMAScript 6</h3>

As mentioned before, ECMAScript 6 will include <tt>__proto__</tt>. At the very least, you’ll be able to use it to retrieve the prototype of an object and to set the prototype in an object literal. The combination of object literal plus <tt>__proto__</tt> can be considered a more user-friendly <tt>Object.create()</tt> that is good enough for most use cases.
<p>
It is not yet certain that you’ll also be able to change the prototype of an existing object via <tt>__proto__</tt>. The most important use case for that is to subtype <tt>Array</tt>, which can be better supported by other means, e.g. via a function <tt>Array.createArray(proto)</tt>.
<p>
Lastly, ECMAScript 6 will probably also provide ways for switching off <tt>__proto__</tt> for some objects, possibly even for all objects.

<h3>More material on the web</h3>

<ul>
    <li>MDN: “<a href="https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Object/proto">__proto__</a>”</li>
    <li>“<a href="http://asenbozhilov.com/articles/proto.html">The magic __proto__ property</a>” by Asen Bozhilov</li>
</ul>

<h3>References</h3>

<ol>
    <li><a href="http://www.2ality.com/2011/06/prototypes-as-classes.html">Prototypes as classes – an introduction to JavaScript inheritance</a></li>
    <li><a href="http://www.2ality.com/2012/10/dunder.html">How to pronounce __proto__</a></li>
    <li><a href="http://www.2ality.com/2012/01/objects-as-maps.html">The pitfalls of using objects as maps in JavaScript</a></li>
    <li><a href="http://www.2ality.com/2011/12/subtyping-builtins.html">Subtyping JavaScript built-ins</a></li>
</ol>
<div><img width="1" height="1" src="https://blogger.googleusercontent.com/tracker/8100407163665430627-321305066977845050?l=www.2ality.com" alt=""></div><img src="http://feeds.feedburner.com/~r/2ality/~4/xBQstor_j1E" height="1" width="1"></p></p></p></p>