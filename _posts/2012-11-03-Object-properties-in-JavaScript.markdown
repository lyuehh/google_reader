---
layout: post
title:  "Object properties in JavaScript"
date:   2012-11-03 00:23:17
author: Axel Rauschmayer
categories: program
---

## Object properties in JavaScript
### by Axel Rauschmayer
### at 2012-11-03 00:23:17
### original <http://feedproxy.google.com/~r/2ality/~3/UtqqnFYIvP0/javascript-properties.html>

Properties determine the state of an object in JavaScript. This blog post examines in detail how they work.

<a name="more"></a>

<h3>Kinds of properties</h3>

JavaScript has three different kinds of properties: named data properties, named accessor properties and internal properties.

<h4>Named data properties (“properties”)</h4>

“Normal” properties of objects map string names to values. For example, the following object <tt>obj</tt> has a data property whose name is the string <tt>"prop"</tt> and whose value is the number 123.
<pre>
    var obj = {
        prop: 123
    };
</pre>
You can get (read) a property:
<pre>
    console.log(obj.prop); // 123
    console.log(obj["prop"]); // 123
</pre>
And you can set (write) a property:
<pre>
    obj.prop = "abc";
    obj["prop"] = "abc";
</pre>

<h4>Named accessor properties</h4>

Alternatively, getting and setting a property value can be handled via functions. Those functions are called accessor functions. A function that handles getting is called a getter. A function that handles setting is called a setter.
<pre>
    var obj = {
        get prop() {
            return "Getter";
        },
        set prop(value) {
            console.log("Setter: "+value);
        }
    }
</pre>
Let’s interact with <tt>obj</tt>:
<pre>
    &gt; obj.prop
    &#39;Getter&#39;
    &gt; obj.prop = 123;
    Setter: 123
</pre>

<h4>Internal properties</h4>

Some properties are only used by the specification. They are called “internal”, because they are not directly accessible via the language, but they do influence its behavior. Internal properties have special names that are written in double square brackets. Two examples:
<ul>
    <li>The internal property [[Prototype]] points to the prototype of an object. It can be read via <tt>Object.getPrototypeOf()</tt>. Its value can only be set by creating a new object that has a given prototype, e.g. via <tt>Object.create()</tt> or <tt>__proto__</tt> <a>[1]</a>.
    </li>
    <li>The internal property [[Extensible]] determines whether or not one can add properties to an object. It can be read via <tt>Object.isExtensible()</tt>. It can be set <tt>false</tt> via <tt>Object.preventExtensions()</tt>. Once <tt>false</tt>, it cannot be become <tt>true</tt> again.
    </li>
</ul>

<h3>Property attributes</h3>

All of the state of a property, both its data and its meta-data, is stored in <i>attributes</i>. They are fields that a property has, much like an object has properties. Attribute keys are often written in double brackets.
<p>
The following attributes are specific to named data properties:
<ul>
    <li>[[Value]] hold the property’s value, its data.
    </li>
    <li>[[Writable]] holds a boolean indicating whether the value of a property can be changed.
    </li>
</ul>

The following attributes are specific to named accessor properties:
<ul>
    <li>[[Get]] holds the <i>getter</i>, a function that is called when a property is read. That function computes the result of the read access.
    </li>
    <li>[[Set]] holds the <i>setter</i>, a function that is called when a property is set to a value. The function receives that value as a parameter.
    </li>
</ul>

All properties have the following attributes:
<ul>
    <li>[[Enumerable]] holds a boolean. Making a property non-enumerable hides it from some operations (see below).
    </li>
    <li>[[Configurable]] holds a boolean. If false, you cannot delete a property, change any of its attributes (except [[Value]]) or convert between data property and accessor property.
    In other words, [[Configurable]] controls the writability of a property’s meta-data.
    </li>
</ul>

<h4>Default values</h4>

If you don’t specify attributes, the following defaults are used:
<p>
<table>
    <tr><td><b>Attribute key</b></td><td><b>Default value</b></td></tr>
    <tr><td>[[Value]]</td><td><tt>undefined</tt></td></tr>
    <tr><td>[[Get]]</td><td><tt>undefined</tt></td></tr>
    <tr><td>[[Set]]</td><td><tt>undefined</tt></td></tr>
    <tr><td>[[Writable]]</td><td><tt>false</tt></td></tr>
    <tr><td>[[Enumerable]]</td><td><tt>false</tt></td></tr>
    <tr><td>[[Configurable]]</td><td><tt>false</tt></td></tr>
</table>
<p>
These defaults are especially important for property descriptors (see below).


<h3>Property descriptors</h3>

A property descriptor encodes the attributes of a property as an object. Each of the properties of that object corresponds to an attribute. For example, the following is the descriptor of a read-only property whose value is 123:
<pre>
    {
        value: 123,
        writable: false,
        enumerable: true,
        configurable: false
    }
</pre>
You can achieve the same goal, immutability, via accessors. Then the descriptor looks as follows:
<pre>
    {
        get: function () { return 123 },
        enumerable: true,
        configurable: false
    }
</pre>

<h4>Functions that use property descriptors</h4>

The following functions allow you to work with property descriptors:
<ul>
    <li><tt>Object.defineProperty(obj, propName, propDesc)</tt><br>
        Create or change a property on <tt>obj</tt> whose name is <tt>propName</tt> and whose attributes are specified via <tt>propDesc</tt>. Return the modified object. Example:
<pre>
    var obj = Object.defineProperty({}, "foo", {
        value: 123,
        enumerable: true
        // writable and configurable via defaults
    });
</pre>
    </li>
    <li><tt>Object.defineProperties(obj, propDescObj)</tt><br>
        The batch version of <tt>Object.defineProperty()</tt>. Each property of <tt>propDescObj</tt> holds a property descriptor. The names of the properties and their values tell <tt>Object.defineProperties</tt> what properties to create or change on <tt>obj</tt>.
        Example:
<pre>
    var obj = Object.defineProperties({}, {
        foo: { value: 123, enumerable: true },
        bar: { value: "abc", enumerable: true }
    });
</pre>
    </li>
    <li><tt>Object.create(proto, propDescObj?)</tt><br>
        First, create an object whose prototype is <tt>proto</tt>. Then, if the optional parameter <tt>propDescObj</tt> has been specified, add properties to it – in the same manner as <tt>Object.defineProperties</tt>. Finally, return the result.
        For example, the following code snippet produces the same result as the previous snippet:
<pre>
    var obj = Object.create(Object.prototype, {
        foo: { value: 123, enumerable: true },
        bar: { value: "abc", enumerable: true }
    });
</pre>
    </li>
    <li><tt>Object.getOwnPropertyDescriptor(obj, propName)</tt><br>
        Returns the descriptor of the own (non-inherited) property of <tt>obj</tt> whose name is <tt>propName</tt>. If there is no such property, <tt>undefined</tt> is returned.
<pre>
    &gt; Object.getOwnPropertyDescriptor(Object.prototype, &quot;toString&quot;)
    { value: [Function: toString],
      writable: true,
      enumerable: false,
      configurable: true }

    &gt; Object.getOwnPropertyDescriptor({}, &quot;toString&quot;)
    undefined
</pre>
    </li>
</ul>

<h3>Enumerability</h3>

This section explains which operations are influenced by enumerability and which aren’t. Below, we are assuming that the following definitions have been made:
<pre>
    var proto = Object.defineProperties({}, {
        foo: { value: 1, enumerable: true },
        bar: { value: 2, enumerable: false }
    });
    var obj = Object.create(proto, {
        baz: { value: 1, enumerable: true },
        qux: { value: 2, enumerable: false }
    });
</pre>
Note that objects (including <tt>proto</tt>) normally have at least the prototype <tt>Object.prototype</tt> <a>[2]</a>:
<pre>
    &gt; Object.getPrototypeOf({}) === Object.prototype
    true
</pre>
<tt>Object.prototype</tt> is where standard methods such as <tt>toString</tt> and <tt>hasOwnProperty</tt> are defined.

<h4>Operations affected by enumerability</h4>

Enumerability only affects two operations: The for-in loop and <tt>Object.keys()</tt>. 
<p>
The for-in loop iterates over the names of all enumerable properties, including inherited ones (note that none of the non-enumerable properties of <tt>Object.prototype</tt> show up):
<pre>
    &gt; for (var x in obj) console.log(x);
    baz
    foo
</pre>
<p>
<tt>Object.keys()</tt> returns the names of all own (non-inherited) enumerable properties:
<pre>
    &gt; Object.keys(obj)
    [ &#39;baz&#39; ]
</pre>
If you want the names of all own properties, you need to use <tt>Object.getOwnPropertyNames()</tt> (see example below).

<h4>Operations that ignore enumerability</h4>

All other operations ignore enumerability.
Some read operations take inheritance into consideration:
<pre>
    &gt; &quot;toString&quot; in obj
    true
    &gt; obj.toString
    [Function: toString]
</pre>
Other read operations only work with own properties:
<pre>
    &gt; Object.getOwnPropertyNames(obj)
    [ &#39;baz&#39;, &#39;qux&#39; ]

    &gt; obj.hasOwnProperty(&quot;qux&quot;)
    true
    &gt; obj.hasOwnProperty(&quot;toString&quot;)
    false

    &gt; Object.getOwnPropertyDescriptor(obj, &quot;qux&quot;)
    { value: 2,
      writable: false,
      enumerable: false,
      configurable: false }
    &gt; Object.getOwnPropertyDescriptor(obj, &quot;toString&quot;)
    undefined
</pre>
Creating, deleting and defining properties only affects the first object in a prototype chain:
<pre>
    obj.propName = value
    obj["propName"] = value

    delete obj.propName
    delete obj["propName"]

    Object.defineProperty(obj, propName, desc)
    Object.defineProperties(obj, descObj)
</pre>

<h3>Best practices</h3>

The general rule is that properties created by the system are non-enumerable, while properties created by 
users are enumerable:
<pre>
    &gt; Object.keys([])
    []
    &gt; Object.getOwnPropertyNames([])
    [ &#39;length&#39; ]
    &gt; Object.keys([&#39;a&#39;])
    [ &#39;0&#39; ]
</pre>
That especially holds for the methods in prototype objects:
<pre>
    &gt; Object.keys(Object.prototype)
    []
    &gt; Object.getOwnPropertyNames(Object.prototype)
    [ hasOwnProperty&#39;,
      &#39;valueOf&#39;,
      &#39;constructor&#39;,
      &#39;toLocaleString&#39;,
      &#39;isPrototypeOf&#39;,
      &#39;propertyIsEnumerable&#39;,
      &#39;toString&#39; ]
</pre>
Thus, for your code, you should ignore enumerability. You normally shouldn’t add properties to built-in prototypes and objects, but if you do, you should make them non-enumerable to avoid breaking code.
<p>
As we have seen, non-enumerability mostly benefits for-in and ensures that legacy code using it won’t break. The non-enumerable properties create the illusion that for-in only iterates over the user-created own properties of an object. In your code, you should avoid for-in if you can <a>[3]</a>.
<p>
If you use objects as maps from strings to values, you should only work with own properties and ignore enumerability. But there are more pitfalls for this use case <a>[4]</a>.

<h3>Conclusion</h3>

In this post, we have examined the nature of properties, which is defined via so-called attributes. Note that actual JavaScript implementations do not necessarily organize properties via attributes, they are mainly an abstraction used by the ECMAScript specification. But one that is sometimes visible in the language itself, for example in property descriptors.
<p>
Further reading on 2ality:
<ul>
    <li>Read “<a href="http://www.2ality.com/2011/07/js-properties.html">JavaScript properties: inheritance and enumerability</a>” for more information on how inheritance and enumerability affect property-related operations.</li>
    <li>Read “<a href="http://www.2ality.com/2012/01/js-inheritance-by-example.html">JavaScript inheritance by example</a>” for an introduction to JavaScript inheritance.</li>
</ul>

<h3>References</h3>

<ol>
    <li><a href="http://www.2ality.com/2012/10/proto.html">JavaScript: __proto__</a></li>
    <li><a href="http://www.2ality.com/2012/08/instanceof-object.html">What object is not an instance of Object?</a></li>
    <li><a href="http://www.2ality.com/2011/04/iterating-over-arrays-and-objects-in.html">Iterating over arrays and objects in JavaScript</a></li>
    <li><a href="http://www.2ality.com/2012/01/objects-as-maps.html">The pitfalls of using objects as maps in JavaScript</a></li>
</ol>
<div><img width="1" height="1" src="https://blogger.googleusercontent.com/tracker/8100407163665430627-7445249494011454824?l=www.2ality.com" alt=""></div><img src="http://feeds.feedburner.com/~r/2ality/~4/UtqqnFYIvP0" height="1" width="1"></p></p></p></p></p></p></p></p>