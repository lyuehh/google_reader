---
layout: post
title:  "An Introduction To LESS, And Comparison To Sass"
date:   2011-09-09 17:57:58
author: Jeremy Hixon
categories: program
---

## An Introduction To LESS, And Comparison To Sass
### by Jeremy Hixon
### at 2011-09-09 17:57:58
### original <http://www.smashingmagazine.com/2011/09/09/an-introduction-to-less-and-comparison-to-sass/>

<table width="650">
<tr>
<td width="650">
<div style="width:650px">
      <img src="http://statisches.auslieferung.commindo-media-ressourcen.de/advertisement.gif" alt="" border="0"><br>
      <a href="http://auslieferung.commindo-media-ressourcen.de/random.php?mode=target&amp;collection=smashing-rss&amp;position=1"><img src="http://auslieferung.commindo-media-ressourcen.de/random.php?mode=image&amp;collection=smashing-rss&amp;position=1" border="0" alt=""></a> <a href="http://auslieferung.commindo-media-ressourcen.de/random.php?mode=target&amp;collection=smashing-rss&amp;position=2"><img src="http://auslieferung.commindo-media-ressourcen.de/random.php?mode=image&amp;collection=smashing-rss&amp;position=2" border="0" alt=""></a> <a href="http://auslieferung.commindo-media-ressourcen.de/random.php?mode=target&amp;collection=smashing-rss&amp;position=3"><img src="http://auslieferung.commindo-media-ressourcen.de/random.php?mode=image&amp;collection=smashing-rss&amp;position=3" border="0" alt=""></a>
    </div>
</td>
</tr>
</table>
<p>I’ve been using <a title="LESS « The Dynamic Stylesheet language" href="http://lesscss.org/">LESS</a> religiously ever since I stumbled upon it months ago. CSS was never really a problem for me, in and of itself, but I was intrigued by the idea of using variables to create something along the lines of a color palette for my websites and themes. Having a color palette with a fixed number of options to choose from helps prevent me from going color crazy and deviating from a chosen style.</p>
<p><img title="less-sass" src="http://coding.smashingmagazine.com/wp-content/uploads/2011/08/less-sass1.jpg" alt="" width="448" height="238"></p>
<p>As it turns out, LESS — and Sass for that matter — are so much more than that. LESS and Sass share a lot of similarities in syntax, including the following:</p>
<ul>
<li><strong>Mixins – </strong>Classes for classes.</li>
<li><strong>Parametric mixins – </strong>Classes to which you can pass parameters, like functions.</li>
<li><strong>Nested Rules – </strong>Classes within classes, which cut down on repetitive code.</li>
<li><strong>Operations – </strong>Math within CSS.</li>
<li><strong>Color functions – </strong>Edit your colors.</li>
<li><strong>Namespaces – </strong>Groups of styles that can be called by references.</li>
<li><strong>Scope – </strong>Make local changes to styles.</li>
<li><strong>JavaScript evaluation – </strong>JavaScript expressions evaluated in CSS.</li>
</ul>
<p>The main difference between LESS and Sass is the way in which they are processed. LESS is a JavaScript library and is, therefore, processed client-side.</p>
<p>Sass, on the other hand, runs on Ruby and is processed server-side. A lot of developers might not choose LESS because of the additional time needed for the JavaScript engine to process the code and output the modified CSS to the browser. There are a few ways around this. The way I get around it is to use LESS only during the development process. Once I’m finished, I copy and paste the LESS output into a minifier and then into a separate CSS file to be included in place of the LESS files. Another option is to use <a title="LESS.app For Mac OS X" href="http://incident57.com/less/">LESS.app</a> to compile and minify your LESS files. Both options will minimize the footprint of your styles, as well as avoid any problems that might result from the client’s browser not running JavaScript. While this is not likely, it’s always a possibility.</p>
<h3>LESS Is More</h3>
<h4>Installation</h4>
<p>Including LESS in something that you’re building is about as easy as it gets:</p>
<ol>
<li>Go get yourself a copy of <a title="Download less.js" href="http://lesscss.googlecode.com/files/less-1.1.3.min.js">less.js</a>;</li>
<li>Create a file to put your styles in, such as <em>style.less</em>;</li>
<li>Add the following code to your HTML’s <code>&lt;head&gt;</code>:</li>
</ol>
<pre>&lt;link rel=&quot;stylesheet/less&quot; type=&quot;text/css&quot; href=&quot;styles.less&quot;&gt;
&lt;script src=&quot;less.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;</pre>
<p>Note the <code>rel</code> attribute of the link. You are required to append the <code>/less</code> to the end of the value in order for LESS to work. You are also required to include the script immediately after the link to the style sheet. If you’re using HTML5 syntax, and I can’t imagine why you wouldn’t be, you can leave out the <code>type="text/css"</code> and the <code>type="text/javascript"</code>.</p>
<p>There’s also a <a href="http://lesscss.org/#-server-side-usage">server-side version of LESS</a>. The easiest way to install LESS on the server is with <a href="http://github.com/isaacs/npm">Node Package Manager</a> (NPM).</p>
<h4>Variables</h4>
<p>If you’re a developer, variables are one of your best friends. In the event that you’ll be using information repeatedly (in this case, a color), setting it to a variable makes sense. This way, you guarantee yourself consistency and probably less scrolling about looking for a hex value to copy and paste. You can even do some fun little adding and subtracting of hex values that you want to render. Take this example:</p>
<pre>@blue: #00c;
@light_blue: @blue + #333;
@dark_blue: @blue - #333;</pre>
<p>If we apply these styles to three divs, we can see the gradated effect created by adding and subtracting the hex values to and from the original blue:</p>
<p><img src="http://coding.smashingmagazine.com/wp-content/uploads/2011/07/blue-gradient.jpg" alt="A screenshot illustrating the transition from 3 shades of blue" width="316" height="316"></p>
<p><em>The transition from @light_blue to @blue to @dark_blue.</em></p>
<p>The only difference in variables between LESS and Sass is that, while LESS uses <code>@</code>, Sass uses <code>$</code>. There are some scope differences as well, which I’ll get to shortly.</p>
<h4>Mixins</h4>
<p>On occasion, we might create a style that’s intended to be used repeatedly throughout the style sheet. Nothing is stopping you from applying multiple classes to the elements in the HTML, but you could also do this without ever leaving your style sheet, using LESS. To illustrate this, I have pasted some sample code that one might use to style two elements on the page.</p>
<pre>.border {
border-top: 1px dotted #333;
}

article.post {
background: #eee;
.border;
}

ul.menu {
background: #ccc;
.border;
}</pre>
<p>This will give you something similar to what you would get if you had gone back to the HTML file and added the <code>.bordered</code> class to the two elements there — except you’ve done it without leaving the style sheet. And it works just as well:</p>
<p><img src="http://coding.smashingmagazine.com/wp-content/uploads/2011/07/bordered-elements.jpg" alt="A screenshot illustrating two elements that share a border style" width="316" height="233"></p>
<p><em>Both the article and the unordered list share the border style.</em></p>
<p>With Sass, you declare <code>@mixin</code> prior to the style to identify it as a mixin. Later, you declare <code>@include</code> to call it.</p>
<pre>@mixin border {
border-top: 1px dotted #333;
}

article.post {
background: #eee;
@include border;
}

ul.menu {
background: #ccc;
@include border;
}</pre>
<h4>Parametric Mixins</h4>
<p>Like having functions in your CSS (*swoon*), these can be immensely useful for those seemingly redundant tasks of modern-day CSS. The best and most useful example of their use relates to the many vendor prefixes that we struggle with during this transition from CSS2 to CSS3. Nettuts+ has a <a href="http://net.tutsplus.com/tutorials/html-css-techniques/quick-tip-never-type-a-vendor-prefix-again/">wonderful webcast and article</a> by Jeffrey Way, with details on including a file consisting entirely of useful parametric mixins that cover most of your favorite CSS3 properties in the respective vendor prefixes. For example, a simple mixin to handle rounded corners in their various forms:</p>
<pre>.border-radius( @radius: 3px ) {
-webkit-border-radius: @radius;
-moz-border-radius: @radius;
border-radius: @radius;
}</pre>
<p>In this case, the <code>.border-radius</code> class has a default radius of 3 pixels, but you can pass whatever value you like to get a rounded corner of that radius. Something like <code>.border-radius(10px)</code> will round the corners by 10 pixels.</p>
<p>The syntax in Sass is very similar to that of LESS. Just use the <code>$</code> for variables, and call the mixins with the <code>@mixin</code> and <code>@include</code> method mentioned earlier.</p>
<h4>Selector Inheritance</h4>
<p>Here’s something not provided in LESS. With this ability, you can append a selector to a previously established selector without the need to add it in a comma-separated format.</p>
<pre>.menu {
	border: 1px solid #ddd;
}

.footer {
	@extend .menu;
}

/* will render like so: */
.menu, .footer {
	border: 1px solid #ddd;
}</pre>
<h4>Nested Rules</h4>
<p>Nesting classes and ids in CSS can be one of the only methods to keep your styles from interfering with and from being interfered with any other styles that may be added along the way. But this can get very messy. Using a selector like <code>#site-body .post .post-header h2</code> is unappealing and takes up a lot of unnecessary space. With LESS, you can nest ids, classes and elements as you go. Using the example above, you could do something like this:</p>
<pre>#site-body { …

    .post { …

        .post-header { …

            h2 { … }

            a { …

            	&amp;:visited { … }
            	&amp;:hover { … }
            }
        }
    }
}</pre>
<p>The above code is essentially the same as the ugly selector in the previous paragraph, but it’s much easier to read and understand, and it takes up much less space. You can also refer in element styles to their pseudo-elements by using the <code>&amp;</code>, which in this case functions similar to <code>this</code> in JavaScript.</p>
<h4>Operations</h4>
<p>This is about what you would expect: using fixed numbers or variables to perform mathematical operations in your styles.</p>
<pre>@base_margin: 10px;
@double_margin: @base_margin * 2;

@full_page: 960px;
@half_page: @full_page / 2;
@quarter_page: (@full_page / 2) / 2;</pre>
<p>For the record, I am aware that I could have also divided by four to get the <code>@quarter_page</code> variable, but I wanted to illustrate that the parentheses rule from the “order of operations” also applies. Parentheses are also required if you’re going to perform operations within compound properties; for example, <code>border: (@width / 2) solid #000</code>.</p>
<p>Sass is a lot more versatile with numbers than LESS. It has built into it conversion tables to combine comparable units. Sass can work with unrecognized units of measurement and print them out. This feature was apparently introduced in an attempt to future-proof the library against changes made by the W3C.</p>
<pre>/* Sass */
2in + 3cm + 2pc = 3.514in

/* LESS */
2in + 3cm + 2pc = Error</pre>
<h4>Color Functions</h4>
<p>Earlier, I mentioned how LESS helps me stick to a color scheme in my coding process. One of the parts that contributes to this the most is the color function. Suppose you use a standard blue throughout your styles, and you want to use this color for a gradated “Submit” button in a form. You could go into Photoshop or another editor to get the hex value for a slightly lighter or darker shade than the blue for the gradient. Or you could just use a color function in LESS.</p>
<pre>@blue: #369;

.submit {
    padding: 5px 10px;
    border: 1px solid @blue;
    background: -moz-linear-gradient(top, lighten(@blue, 10%), @blue 100%); /*Moz*/
    background: -webkit-gradient(linear, center top, center bottom, from(lighten(@blue, 10%)), color-stop(100%, @blue)); /*Webkit*/
    background: -o-linear-gradient(top, lighten(@blue, 10%) 0%, @blue 100%); /*Opera*/
    background: -ms-linear-gradient(top, lighten(@blue, 10%) 0%, @blue 100%); /*IE 10+*/
    background: linear-gradient(top, lighten(@blue, 10%) 0%, @blue 100%); /*W3C*/
    color: #fff;
    text-shadow: 0 -1px 1px rgba(0,0,0,0.4);
}</pre>
<p>The <code>lighten</code> function literally lightens the color by a percentage value. In this case, it will lighten the base blue by 10%. This method enables us to change the color of gradated elements and any other elements with that color simply by changing the base color itself. This could prove immensely helpful in theming. Plus, if you used a parametric function, like the ones listed above, you could alleviate some of that browser-prefix tedium with something as simple as <code>.linear-gradient(lighten(@blue), @blue, 100%);</code>.</p>
<p>Either way, you get an effect that’s rather nice:</p>
<p><img src="http://coding.smashingmagazine.com/wp-content/uploads/2011/07/button-submit.jpg" alt="Screenshot of a styled submit button" width="74" height="37"></p>
<p><em>Our nicely gradated, variable-based “Submit” button.</em></p>
<p>There are a lot of other color functions for darkening and saturating colors and even spinning the color wheel to other colors. I recommend trying them out to see what you can come up with.</p>
<p>Sass seems to have a lot more color options — not that I would need them all. Lighten and darken are the only ones that I see myself using often. For more detail, Nex3 has an <a href="http://nex-3.com/posts/89-powerful-color-manipulation-with-Sass">in-depth article on the topic</a>.</p>
<h4>Conditionals and Control</h4>
<p>This is rather nifty, and another thing not provided by LESS. With Sass, you have the ability to use <code>if { } else { }</code> conditional statements, as well as <code>for { }</code> loops. It supports <code>and</code>, <code>or</code> and <code>not</code>, as well as the <code>&lt;</code>, <code>&gt;</code>, <code>&lt;=</code>, <code>&gt;=</code> and <code>==</code> operators.</p>
<pre>/* Sample Sass &quot;if&quot; statement */
@if lightness($color) &gt; 30% {
  background-color: #000;
} @else {
  background-color: #fff;
}

/* Sample Sass &quot;for&quot; loop */
@for $i from 1px to 10px {
  .border-#{i} {
    border: $i solid blue;
  }
}</pre>
<h4>Namespaces</h4>
<p>Namespaces can be used to add another level of organization to our CSS, by allowing us to create groups of commonly used styles and then pick from them along the way. For instance, if we created a group of styles called <code>defaults</code>, we could pull from this group when we come across an element that needs it.</p>
<pre>#defaults {
	.nav_list () {
		list-style: none;
		margin: 0; padding: 0;
	}
	.button () { … }
	.quote () { … }
}</pre>
<p>Later, in our code, if we come across a <code>ul</code> element within a <code>nav</code> element, we would know that we’ll need our default style. So, we can simply call it, and it will be applied.</p>
<pre>nav ul {
	#defaults &gt; .nav_list;
}</pre>
<h4>Scope</h4>
<p>Scoping is standard throughout programming and thus standard in LESS. If you define a variable at the root level of your style sheet, it will be available and consistent throughout the document. If, however, you redefine the variable from within a selector such as an id or class, then it will be available — with the new value — only within that selector.</p>
<pre>@color: #00c; /* blue */

#header {
	@color: #c00; /* red */

	border: 1px solid @color; /* will have a red border */
}

#footer {
	border: 1px solid @color; /* will have a blue border */
}</pre>
<p>Because we’ve restated the variable within the <code>#header</code> selector, the value for that variable will be different and will apply only within that selector. Anything before or after it will retain the value of the original statement.</p>
<p>Scope is handled a little differently in Sass. In the above code, when the <code>@color</code> variable is changed to <code>red</code>, it will be interpreted as such from that point on within the code.</p>
<h4>Comments</h4>
<p>This part is pretty basic. Two types of comments are valid in LESS. The standard CSS comment, <code>/* comment */</code>, is valid and will get passed through the processing and outputted. Single-line comments, <code>// comment</code>, work as well but will not get passed and outputted and, as a result, are “silent.”</p>
<h4>Importing</h4>
<p>Importing is pretty standard, too. The standard <code>@import: 'classes.less';</code> works just fine. If, however, you’re importing another LESS file, then the file extension is optional, so <code>@import 'classes';</code> would work as well. If you want to import something without LESS processing it, you can use the <code>.css</code> extension (for example, <code>@import: 'reset.css';</code>).</p>
<h4>String Interpolation</h4>
<p>String values can also be used in variables and called within styles via <code>@{name}</code>.</p>
<pre>@base_url = 'http://coding.smashingmagazine.com';
background-image: url("@{base_url}/images/background.png");</pre>
<h4>Escaping</h4>
<p>There will be times when you need to include a value that is not valid CSS syntax or that LESS doesn’t recognize. More often than not, it will be some crazy Microsoft hack. To avoid throwing errors and breaking LESS, you will need to escape them.</p>
<pre>.class {
	filter: ~"progid:DXImageTransform.Microsoft.Alpha(opacity=20)";
}

/* Will actually be outputted like this: */
.class {
	filter: progid:DXImageTransform.Microsoft.Alpha(opacity=20);
}</pre>
<h4>JavaScript Evaluation</h4>
<p>This is one of my favorite parts of LESS: using Javascript in style sheets — simply brilliant. You can use expressions and also reference aspects of the environment using backticks.</p>
<pre>@string: `'howdy'.toUpperCase()`; /* @string becomes 'HOWDY' */

/* You can also use the previously mentioned interpolation: */
@string: 'howdy';
@var: ~`'@{string}'.topUpperCase()`; /* becomes 'HOWDY' */

/* Here we access part of the document */
@height = `document.body.clientHeight`;</pre>
<h4>Output Formatting</h4>
<p>Whereas LESS has no output settings, Sass provides four output versions: nested, compact, compressed and expanded.</p>
<h3>Final Thoughts</h3>
<p>These two libraries share a lot of basics. Both of them are fantastic tools for designers who code, and they can also help developers work more efficiently and quickly. If you’re a fan of Ruby or HAML, then Sass might be right up your ally. For me, being a PHP and JavaScript geek, I tend to lean towards LESS for its ease of inclusion and access to JavaScript’s expressions and document attributes. I doubt that I’ve even come close to truly grasping the possibilities of programming in my style sheets, but I am intent on trying. If you’ve been using either, or both, of these in your work, I’d love to hear more about it and see some of your results. Tips, tricks and corrections are, of course, always welcome as well.</p>
<p><em>(al)</em></p>
<hr>
<p><small>© Jeremy Hixon for <a href="http://www.smashingmagazine.com">Smashing Magazine</a>, 2011.</small></p>