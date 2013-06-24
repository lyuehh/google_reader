---
layout: post
title:  "Tabs with Round Out Borders"
date:   2011-09-05 07:37:48
author: Chris Coyier
categories: program
---

## Tabs with Round Out Borders
### by Chris Coyier
### at 2011-09-05 07:37:48
### original <http://css-tricks.com/14001-tabs-with-round-out-borders/>

<p>Rounded corners are now trivially easy to achieve via <code>border-radius</code>. But that only allows us to <em>cut into</em> the shape. What if we want to connect a shape to another with a rounded <em>outward</em> corner. Much easier to explain with a graphic:</p>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/roundouttabs.png" alt="" title="roundouttabs" width="565" height="140"> The top corners are easy, just <code>border-radius</code>. The bottom corners, less easy.<br> <a href="http://css-tricks.com/examples/RoundOutTabs/">View Demo</a>   <a href="http://css-tricks.com/examples/RoundOutTabs.zip">Download Files</a> <h3>Clean HTML</h3><p>Of course, on the web, just about anything visual is possible. Worst-case-scenario you can use images. Our goal here, as ever, is to use no images (quick! accessible! easy to update!) and use completely clean semantic HTML (quick! accessible! easy to update!). So here's the markup:</p><pre rel="HTML"><code>&lt;ul class=&quot;tabs group&quot;&gt;
  &lt;li class=&quot;active&quot;&gt;&lt;a href=&quot;#one&quot;&gt;One&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;#two&quot;&gt;Two&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;#three&quot;&gt;Three&lt;/a&gt;&lt;/li&gt;
  &lt;li&gt;&lt;a href=&quot;#three&quot;&gt;Four&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;</code></pre><p>A class of <code>active</code> indicates which tab reflects the current page.</p><h3>How this is going down</h3><p>The reason this is tricky is that we need a shape to stick out of the tab element. To get this done while keeping our markup clean, we'll use pseudo elements. If you need a refresher, you can learn about them <a href="http://css-tricks.com/video-screencasts/94-intro-to-pseudo-elements/">here</a> and <a href="http://css-tricks.com/9516-pseudo-element-roundup/">here</a>. Essentially, they can add extra elements to the page that we can style, directly through CSS. Every element can have two - <code>:before</code> and <code>:after</code>. Ultimately we'll be using four per tab, which is possible because each tab is make from two elements, the list item and the anchor link.</p><p>Let's visualize this step by step, without looking out any code just yet.</p><h4>1) Natural State</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.071.png" alt="" title="Pseudos.071" width="1024" height="768"> List items are naturally block level and anchor links are inline, so the layout is like this. <h4>2) Float</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.072.png" alt="" title="Pseudos.072" width="1024" height="768"> By floating the list items to the left, the list items will line up next to each other and shrink to the size of the anchor links inside them. <h4>3) Same Size</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.073.png" alt="" title="Pseudos.073" width="1024" height="768"> The list items themselves have no margin or padding, so really the list items and anchor links are the exact same size, directly on top of each other. <h4>4) Just one</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.074.png" alt="" title="Pseudos.074" width="1024" height="768"> Let's focus on just one of them... <h4>5) Circles</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.075.png" alt="" title="Pseudos.075" width="1024" height="768"><br> We'll use two of our four available pseudo elements to place <a href="http://css-tricks.com/examples/ShapesOfCSS/">circles</a> on the bottom left and bottom right of the tab.<br> <h4>6) Squares</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.076.png" alt="" title="Pseudos.076" width="1024" height="768"> With the other two pseudo elements we'll make smaller squares. <h4>7) Colorize the tab and content</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.077.png" alt="" title="Pseudos.077" width="1024" height="768"> The "active" tab and the content will share the same background color. <h4>8) Colorize the pseudo elements</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.078.png" alt="" title="Pseudos.078" width="1024" height="768"> The squares match the color of the active tab, the circles match the background behind them. <h4>9) Stacking</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.079.png" alt="" title="Pseudos.079" width="1024" height="768"> With z-index, we'll make sure the circle sits on top and cuts off the color of the square. <h4>10) No borders</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.080.png" alt="" title="Pseudos.080" width="1024" height="768"> The borders were just for illustration, really it would look more like this. <h4>11) Finishing</h4>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/09/Pseudos.081.png" alt="" title="Pseudos.081" width="1024" height="768"> Add the same concept to the outer tabs and round the tops with <code>border-radius</code> and it's done! <h3>CSS</h3><p>This is a big ass block of CSS, but I've tried to comment it up so that each part makes sense.</p><pre rel="HTML"><code>.tabs li {
  /* Makes a horizontal row */
  float: left; 

  /* So the psueudo elements can be
     abs. positioned inside */
  position: relative;
}
.tabs a {
  /* Make them block level
     and only as wide as they need */
  float: left;
  padding: 10px 40px;
  text-decoration: none;

  /* Default colors */
  color: black;
  background: #ddc385; 

  /* Only round the top corners */
  -webkit-border-top-left-radius: 15px;
  -webkit-border-top-right-radius: 15px;
  -moz-border-radius-topleft: 15px;
  -moz-border-radius-topright: 15px;
  border-top-left-radius: 15px;
  border-top-right-radius: 15px;
}
.tabs .active {
  /* Highest, active tab is on top */
  z-index: 3;
}
.tabs .active a {
  /* Colors when tab is active */
  background: white;
  color: black;
}
.tabs li:before, .tabs li:after,
.tabs li a:before, .tabs li a:after {
  /* All pseudo elements are
     abs. positioned and on bottom */
  position: absolute;
  bottom: 0;
}
/* Only the first, last, and active
   tabs need pseudo elements at all */
.tabs li:last-child:after,   .tabs li:last-child a:after,
.tabs li:first-child:before, .tabs li:first-child a:before,
.tabs .active:after,   .tabs .active:before,
.tabs .active a:after, .tabs .active a:before {
  content: "";
}
.tabs .active:before, .tabs .active:after {
  background: white; 

  /* Squares below circles */
  z-index: 1;
}
/* Squares */
.tabs li:before, .tabs li:after {
  background: #ddc385;
  width: 10px;
  height: 10px;
}
.tabs li:before {
  left: -10px;
}
.tabs li:after {
  right: -10px;
}
/* Circles */
.tabs li a:after, .tabs li a:before {
  width: 20px;
  height: 20px;
  /* Circles are circular */
  -webkit-border-radius: 10px;
  -moz-border-radius:    10px;
  border-radius:         10px;
  background: #222;

  /* Circles over squares */
  z-index: 2;
}
.tabs .active a:after, .tabs .active a:before {
  background: #ddc385;
}
/* First and last tabs have different
   outside color needs */
.tabs li:first-child.active a:before,
.tabs li:last-child.active a:after {
  background: #222;
}
.tabs li a:before {
  left: -20px;
}
.tabs li a:after {
  right: -20px;
}</code></pre><p>That's that!</p><p><a href="http://css-tricks.com/examples/RoundOutTabs/">View Demo</a>   <a href="http://css-tricks.com/examples/RoundOutTabs.zip">Download Files</a></p><p>Should work in just about any decent browser and also IE 9 and up. Should also fall back fine (just no round-outs) in older browsers.</p><h3>Steve Smith Method</h3><p>About the exact time as I was creating this to include in a talk I was doing about pseudo elements. Steve Smith of Ordered List published a <a href="http://orderedlist.com/blog/articles/flared-borders-with-css/">very similar method</a>. Steve's method has borders around the tabs, mine has tabs butted up against each other. Both cool if you ask me.</p><p><a href="http://css-tricks.com/14001-tabs-with-round-out-borders/">Tabs with Round Out Borders</a> is a post from <a href="http://css-tricks.com">CSS-Tricks</a></p>