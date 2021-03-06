---
layout: post
title:  "Stuff you can do with the “Checkbox Hack”"
date:   2011-12-22 03:36:30
author: Chris Coyier
categories: program
---

## Stuff you can do with the “Checkbox Hack”
### by Chris Coyier
### at 2011-12-22 03:36:30
### original <http://css-tricks.com/the-checkbox-hack/>

<p>The "Checkbox Hack" is where you use a connected label and checkbox input and usually some other element you are trying to control, like this:</p><pre rel="HTML"><code>&lt;label for=&quot;toggle-1&quot;&gt;Do Something&lt;/label&gt;
&lt;input type=&quot;checkbox&quot; id=&quot;toggle-1&quot;&gt;
&lt;div&gt;Control me&lt;/div&gt;</code></pre><p>Then with CSS, you hide the checkbox entirely. Probably by kicking it off the page with absolute positioning or setting its opacity to zero. But just because the checkbox is hidden, clicking the <code>&lt;label&gt;</code> still toggles its value on and off. Then you can use the adjacent sibling combinator to style the <code>&lt;div&gt;</code> differently based on the <code>:checked</code> state of the input.</p><pre rel="CSS"><code>input[type=checkbox] {
   position: absolute;
   top: -9999px;
   left: -9999px;
   /* For mobile, it's typically better to position checkbox on top of clickable
      area and turn opacity to 0 instead. */
}

/* Default State */
div {
   background: green;
   width: 400px;
   height: 100px;
   line-height: 100px;
   color: white;
   text-align: center;
}

/* Toggled State */
input[type=checkbox]:checked ~ div {
   background: red;
}</code></pre><p><a href="http://dabblet.com/gist/1506530">View Demo</a></p><p>So you can style an element completely differently depending on the state of that checkbox, which you don't even see. Pretty neat. Let's look at a bunch of things the "Checkbox Hack" can do.</p><div><strong>Disclaimer:</strong> Some of this stuff crosses the line of what you "should" do with CSS and introduces some bad semantics. It's still wicked fun to play with and cool that it's possible, but in general functional behavior should be controlled by JavaScript.</div><h3>Custom Designed Radio Buttons and Checkboxes</h3> <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/5762393149.png" alt="" title="5762393149" width="607" height="335"><p>Hide the default UI of a radio button or checkbox, and display a custom version right on top of it. Probably not generally good practice, as users are familiar with default form elements and how they work. But can be good for enforcing cross browser consistency or in situations where the UI is so obvious anyway it's just for fun.</p><ul><li><a href="http://www.thecssninja.com/css/custom-inputs-using-css">Pioneered by Ryan Seddon</a></li><li><a href="http://acidmartin.wordpress.com/2011/02/24/custom-crossbrowser-styling-for-checkboxes-and-radio-buttons/">Similar by Martin Ivanov</a></li><li><a href="http://www.wufoo.com/2011/06/13/custom-radio-buttons-and-checkboxes/">For use on Wufoo forms by me</a></li></ul><h3>File system like "tree menu"</h3> <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/treemenu.png" alt="" title="treemenu" width="207" height="355"> <a href="http://www.thecssninja.com/css/css-tree-menu">Demo by Ryan Seddon</a> <h3>Tabbed Areas</h3><p>The "tabs" design pattern is just toggling on and off of areas, perfect for the checkbox hack. But instead of checkboxes, in which any checkbox can be on or off independently of one another, these tabs use radio buttons in which only one per group can be on at a time (like how only one tab can be active at a time).</p> <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/tabcat.jpeg" alt="" title="tabcat" width="424" height="238"> <a href="http://css-tricks.com/functional-css-tabs-revisited/">Functional CSS tabs revisited</a> <h3>Dropdown Menus</h3> <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/dropdowns.png" alt="" title="dropdowns" width="358" height="251"> <a href="http://jsfiddle.net/paullferguson/Sv54G/3/">Original by paullferguson</a> and then <a href="http://dabblet.com/gist/1507175">Forked for betterness by me</a> <h3>Push Toggles</h3> <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/pushtoggles.png" alt="" title="pushtoggles" width="385" height="243"> From <a href="http://www.weblaunchr.com/whatsmympg/">What's My MPG?</a>  <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/dabblettoggles.png" alt="" title="dabblettoggles" width="446" height="217"> Options from <a href="http://dabblet.com">Dabblet</a> <h3>FAQ Answer Revealing</h3> <img src="http://cdn.css-tricks.com/wp-content/uploads/2011/12/faq.png" alt="" title="faq" width="476" height="224"><br> <a href="http://dabblet.com/gist/1507019">View Demo</a><br> <p><a href="http://css-tricks.com/the-checkbox-hack/">Stuff you can do with the “Checkbox Hack”</a> is a post from <a href="http://css-tricks.com">CSS-Tricks</a></p>