---
layout: post
title:  "Experimenting with ECMAScript 6 proxies"
date:   2011-09-16 03:53:31
author: Nicholas C. Zakas
categories: program
---

## Experimenting with ECMAScript 6 proxies
### by Nicholas C. Zakas
### at 2011-09-16 03:53:31
### original <http://feedproxy.google.com/~r/nczonline/~3/syEo9aX-9qg/>

<p>ECMAScript 6, aka “Harmony”, introduces a new type of object called a proxy. Proxies are objects whose default behavior in common situations can be controlled, eliminated, or otherwise changed. This includes definition what happens when the object is used in a <code>for-in</code> look, when its properties are used with <code>delete</code>, and so on. </p>
<p>The behavior of proxies is defined through traps, which are simply functions that “trap” a specific behavior so you can respond appropriately. There are several different traps available, some that are fundamental and some that are derived. The fundamental traps define low-level behavior, such as what happens when calling <code>Object.defineProperty()</code> on the object, while derived traps define slightly higher-level behavior such as reading from and writing to properties. The fundamental traps are recommended to always be implemented while the derived traps are considered optional (when derived traps are undefined, the default implementation uses the fundamental traps to fill the gap).</p>
<p>My experiments focused largely on the derived <code>get</code> trap. The <code>get</code> trap defines what happens when a property is read from the object. Think of the <code>get</code> trap as a global getter that is called for every property on the object. This made me realize that my <a href="http://www.nczonline.net/blog/2009/02/17/mozilla-javascript-extension-nosuchmethod/">earlier experiments</a> with the proprietary <code>__noSuchMethod__()</code> might be applicable. After some tinkering, I ended up with the following HTML writer prototype:</p>
<pre><code>/*
 * The constructor name I want is HTMLWriter.
 */
var HTMLWriter = (function(){

    /*
     * Lazily-incomplete list of HTML tags.
     */
    var tags = [
        &quot;a&quot;, &quot;abbr&quot;, &quot;acronym&quot;, &quot;address&quot;, &quot;applet&quot;, &quot;area&quot;,
        &quot;b&quot;, &quot;base&quot;, &quot;basefont&quot;, &quot;bdo&quot;, &quot;big&quot;, &quot;blockquote&quot;,
        &quot;body&quot;, &quot;br&quot;, &quot;button&quot;,
        &quot;caption&quot;, &quot;center&quot;, &quot;cite&quot;, &quot;code&quot;, &quot;col&quot;, &quot;colgroup&quot;,
        &quot;dd&quot;, &quot;del&quot;, &quot;dir&quot;, &quot;div&quot;, &quot;dfn&quot;, &quot;dl&quot;, &quot;dt&quot;,
        &quot;em&quot;,
        &quot;fieldset&quot;, &quot;font&quot;, &quot;form&quot;, &quot;frame&quot;, &quot;frameset&quot;,
        &quot;h1&quot;, &quot;h2&quot;, &quot;h3&quot;, &quot;h4&quot;, &quot;h5&quot;, &quot;h6&quot;, &quot;head&quot;, &quot;hr&quot;, &quot;html&quot;,
        &quot;i&quot;, &quot;iframe&quot;, &quot;img&quot;, &quot;input&quot;, &quot;ins&quot;, &quot;isindex&quot;,
        &quot;kbd&quot;,
        &quot;label&quot;, &quot;legend&quot;, &quot;li&quot;, &quot;link&quot;,
        &quot;map&quot;, &quot;menu&quot;, &quot;meta&quot;,
        &quot;noframes&quot;, &quot;noscript&quot;,
        &quot;object&quot;, &quot;ol&quot;, &quot;optgroup&quot;, &quot;option&quot;,
        &quot;p&quot;, &quot;param&quot;, &quot;pre&quot;,
        &quot;q&quot;,
        &quot;s&quot;, &quot;samp&quot;, &quot;script&quot;, &quot;select&quot;, &quot;small&quot;, &quot;span&quot;, &quot;strike&quot;,
        &quot;strong&quot;, &quot;style&quot;, &quot;sub&quot;, &quot;sup&quot;,
        &quot;table&quot;, &quot;tbody&quot;, &quot;td&quot;, &quot;textarea&quot;, &quot;tfoot&quot;, &quot;th&quot;, &quot;thead&quot;,
        &quot;title&quot;, &quot;tr&quot;, &quot;tt&quot;,
        &quot;u&quot;, &quot;ul&quot;,
        &quot;var&quot;
    ];

    /*
     * Define an internal-only type.
     */
    function InternalHTMLWriter(){
        this._work = [];
    }

    InternalHTMLWriter.prototype = {

        escape: function (text){
            return text.replace(/[&gt;&lt; &quot;&amp;]/g, function(c){
                switch(c){
                    case &quot;&gt;&quot;: return &quot;&amp;gt;&quot;;
                    case &quot;&lt; &quot;: return &quot;&amp;lt;&quot;;
                    case &quot;\&quot;&quot;: return &quot;&amp;quot;&quot;;
                    case &quot;&amp;&quot;: return &quot;&amp;amp;&quot;;
                }
            });
        },

        startTag: function(tagName, attributes){
            this._work.push(&quot;&lt;&quot; + tagName);

            if (attributes){
                var name, value;
                for (name in attributes){
                    if (attributes.hasOwnProperty(name)){
                        value = this.escape(attributes[name]);
                        this._work.push(&quot; &quot; + name + &quot;=\&quot;&quot; + value + &quot;\&quot;&quot;);
                    }
                }
            }

            this._work.push(&quot;&gt;&quot;);
            return this;
        },

        text: function(text){
            this._work.push(this.escape(text));
            return this;
        },

        endTag: function(tagName){
            this._work.push(&quot;&lt;/&quot; + tagName + &quot;&gt;&quot;);
            return this;
        },

        toString: function(){
            return this._work.join(&quot;&quot;);
        }

    };

    /*
     * Output a pseudo-constructor. It&#39;s not a real constructor,
     * since it just returns the proxy object, but I like the
     * &quot;new&quot; pattern vs. factory functions.
     */
    return function(){
        var writer = new InternalHTMLWriter(),
            proxy = Proxy.create({

                /*
                 * Only really need getter, don&#39;t want anything else going on.
                 */
                get: function(receiver, name){
                    var tagName,
                        closeTag = false;

                    if (name in writer){
                        return writer[name];
                    } else {

                        if (tags.indexOf(name) &gt; -1){
                            tagName = name;
                        } else if (name.charAt(0) == &quot;x&quot; &amp;&amp; tags.indexOf(name.substring(1)) &gt; -1){
                            tagName = name.substring(1);
                            closeTag = true;
                        }

                        if (tagName){
                            return function(){
                                if (!closeTag){
                                    writer.startTag(tagName, arguments[0]);
                                } else {
                                    writer.endTag(tagName);
                                }
                                return receiver;
                            };
                        }
                    }
                }

            });

        return proxy;
    };
})();
</code></pre>
<p>This uses the same basic approach as my earlier experiment, which is to define a getter that interprets property names as HTML tag names. When the property matches an HTML tag name, a function is returned that calls the <code>startTag()</code> method, likewise a property beginning with an “x” and followed by the tag name receives a function that calls <code>endTag()</code>. All other methods are passed through to the interal <code>writer</code> object.</p>
<p>The <code>InternalHTMLWriter</code> type is defined inside of a function so it cannot be accessed outside; the <code>HTMLWriter</code> type is the preferred way to use this code, making the implementation transparent. Each called to <code>HTMLWriter</code> creates a new proxy which, in turn, has reference to its own internal <code>writer</code> object. Basic usage is:</p>
<pre><code>var w = new HTMLWriter();

w.html()
    .head().title().text(&quot;Example &amp; Test&quot;).xtitle().xhead()
    .body().text(&quot;Hello world!&quot;).xbody()
.xhtml();

console.log(w);</code></pre>
<p>Ugly method names aside, the prototype works as you’d expect. What I really like about this type of pattern is that the methods can be easily updated to support new HTML tags by modifying the <code>tags</code> array.</p>
<p>The more I thought about proxies and the <code>get</code> trap, the more ideas I came up with. Developers have long tried to figure out ways to inherit from <code>Array</code> to create their own array-like structures, but we’ve also been unable to get there due to a number of issues. With proxies, implementing array-like data structures are trivial. </p>
<p>I decided that I’d like to make a stack implementation in JavaScript that uses an array underneath it all. I wanted the stack to be old-school, just <code>push()</code>, <code>pop()</code>, and <code>length</code> members (no numeric index support). Basically, I would just need to filter the members being accessed in the <code>get</code> trap. Here’s the result:</p>
<pre><code>var Stack = (function(){

    var stack = [],
        allowed = [ &quot;push&quot;, &quot;pop&quot;, &quot;length&quot; ];

    return Proxy.create({
        get: function(receiver, name){;
            if (allowed.indexOf(name) &gt; -1){
                if(typeof stack[name] == &quot;function&quot;){
                    return stack[name].bind(stack);
                } else {
                    return stack[name];
                }
            } else {
                return undefined;
            }
        }

    });

});

var mystack = new Stack();

mystack.push(&quot;hi&quot;);
mystack.push(&quot;goodbye&quot;);

console.log(mystack.length);    //1

console.log(mystack[0]);        //undefined
console.log(mystack.pop());     //&quot;goodbye&quot;</code></pre>
<p>Here, I’m using a private <code>stack</code> array for each instance of my stack. Each instance also has a single proxy that is returned and used as the interface. So every method I want to allow ends up being executed on the array rather than the proxy object itself.</p>
<p>This pattern of object member filtering allowed me to easily enable the members I wanted while disabling the ones I didn’t. The one tricky part was ensuring the methods were bound to the correct <code>this</code> value. In my first try, I simply returned the method from the array, but ended up with multiple errors because <code>this</code> was the proxy object instead of the array. I added the use of the ECMAScript 5 <code>bind()</code> method to ensure the <code>this</code> value remained correct for the methods and everything worked fine.</p>
<p>A few caveats as you start playing with proxies. First, it’s only currently supported in Firefox 6+. Second, the specification is still in flux and the syntax, order of arguments, etc. may change in the future. Third, the patterns I’ve explained here are not and should not be considered best practices for using proxies. These are just some experiments I hacked together to explore the possibilities. Proxies aren’t ready for production use but are a lot of fun for experimentation.</p>
<p><strong>Update (2011-Sept-18)</strong>: Fixed escaping issue in code.</p>
<div>
<a href="http://feeds.feedburner.com/~ff/nczonline?a=syEo9aX-9qg:cYVwLlmktMc:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/nczonline?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=syEo9aX-9qg:cYVwLlmktMc:V_sGLiPBpWU"><img src="http://feeds.feedburner.com/~ff/nczonline?i=syEo9aX-9qg:cYVwLlmktMc:V_sGLiPBpWU" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=syEo9aX-9qg:cYVwLlmktMc:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/nczonline?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=syEo9aX-9qg:cYVwLlmktMc:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/nczonline?i=syEo9aX-9qg:cYVwLlmktMc:F7zBnMyn0Lo" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/nczonline/~4/syEo9aX-9qg" height="1" width="1">