---
layout: post
title:  "CSS Style Guides"
date:   2012-08-10 23:08:50
author: Chris Coyier
categories: program
---

## CSS Style Guides
### by Chris Coyier
### at 2012-08-10 23:08:50
### original <http://css-tricks.com/css-style-guides/>

<p>As we wrap up our recent poll on <a href="http://css-tricks.com/poll-results-how-do-you-order-your-css-properties/">ordering CSS properties</a>, it brings up the larger issue of CSS style guides. Ordering properties is just one choice you have to make that makes up a complete styling strategy. Naming is a part of it. Sectioning is a part of it. Commenting, indentation, overall file structure... it all makes up a complete CSS style guide. </p>
<p>Let's round up some existing ones.</p>
<p><span></span></p>
<h3>But first... Not Pattern Libraries.</h3>
<p>I <strong>love</strong> pattern libraries. Think <a href="http://twitter.github.com/bootstrap/">Twitter Bootstrap</a> or <a href="http://www.bbc.co.uk/gel">GEL</a>. I think they are a fantastic way to work particularly in large sites and web apps. This post isn't about those. We'll do a roundup of those some time, because I think that would be valuable too. This is about style guides <em>for CSS itself</em>.</p>
<p></p>
<h3>The List</h3>
<p>I'll list some excerpts from each that I like below.</p>
<h4>GitHub</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/github-css-styleguide.jpg" alt="" title="github-css-styleguide" width="856" height="680"></p>
<p><a href="https://github.com/styleguide/css">GitHub CSS Style Guide →</a></p>
<blockquote><p>As a rule of thumb, don't nest further than 3 levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).</p></blockquote>
<blockquote><p>Unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.</p></blockquote>
<h4>Google</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/google-style-guide.jpg" alt="" title="google-style-guide" width="800" height="654"></p>
<p><a href="http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml">Google HTML/CSS Style Guide →</a></p>
<blockquote><p>Use ID and class names that are as short as possible but as long as necessary.</p></blockquote>
<p>E.g. <code>#nav</code> not <code>#navigation</code>, <code>.author</code> not <code>.atr</code></p>
<blockquote><p>Do not concatenate words and abbreviations in selectors by any characters (including none at all) other than hyphens, in order to improve understanding and scannability.</p></blockquote>
<p>E.g. <code>.demo-image</code> not <code>.demoimage</code> or <code>.demo_image</code></p>
<h4>Idiomatic CSS</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/idiomatic-css.jpg" alt="" title="idiomatic-css" width="830" height="680"></p>
<p><a href="https://github.com/necolas/idiomatic-css">Nicolas Gallagher&#39;s Idiomatic CSS →</a></p>
<blockquote><p>Configure your editor to "show invisibles". This will allow you to eliminate end of line whitespace, eliminate unintended blank line whitespace, and avoid polluting commits.</p></blockquote>
<blockquote><p>Long, comma-separated property values - such as collections of gradients or shadows - can be arranged across multiple lines in an effort to improve readability and produce more useful diffs.</p></blockquote>
<blockquote><p>Use separate files (concatenated by a build step) to help break up code for distinct components.</p></blockquote>
<h4>CSS Wizardry</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/roberts-css-style.jpg" alt="" title="roberts-css-style" width="802" height="654"></p>
<p><a href="http://csswizardry.com/2012/04/my-html-css-coding-style/">Harry Robert&#39;s CSS Style →</a></p>
<blockquote><p>I have a blanket-ban on IDs in CSS. There is literally no point in them, and they only ever cause harm.</p></blockquote>
<blockquote><p>This section heading is also prepended with a $. This is so that—when I do a find for a section—I actually do a find for <code>$MAIN</code> and not <code>MAIN</code>.</p></blockquote>
<blockquote><p>In situations where it would be useful for a developer to know exactly how a chunk of CSS applies to some HTML, I often include a snippet of HTML in a CSS comment.</p></blockquote>
<h4>Smashing Magazine</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/smashingmag-css.jpg" alt="" title="smashingmag-css" width="784" height="640"></p>
<p><a href="http://coding.smashingmagazine.com/2008/05/02/improving-code-readability-with-css-styleguides/">Vitaly Friedman&#39;s &quot;Improving Code Readability With CSS Styleguides&quot; →</a></p>
<blockquote><p>For large projects or large development team it is also useful to have a brief update log.</p></blockquote>
<blockquote><p>For better overview of your code you might consider using one-liners for brief fragments of code. </p></blockquote>
<h4>ThinkUp</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/thinkup-css-guide.jpg" alt="" title="thinkup-css-guide" width="798" height="652"></p>
<p><a href="https://github.com/ginatrapani/ThinkUp/wiki/Code-Style-Guide:-CSS">ThinkUp CSS Style Guide →</a></p>
<blockquote><p>If the value of the width or height is 0, do not specify units.</p></blockquote>
<blockquote><p>Comments that refer to selector blocks should be on a separate line immediately before the block to which they refer.</p></blockquote>
<h4>WordPress</h4>
<p><a href="http://cdn.css-tricks.com/wp-content/uploads/2012/08/wordpress-css-standards-1.jpg"><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/wordpress-css-standards-1.jpg" alt="" title="wordpress-css-standards-1" width="802" height="620"></a></p>
<p><a href="http://make.wordpress.org/core/handbook/coding-standards/css/">WordPress CSS Coding Standards →</a></p>
<blockquote><p>Add two blank lines between sections and one blank line between blocks in a section.</p></blockquote>
<blockquote><p>Broad selectors allow us to be efficient, yet can have adverse consequences if not tested. Location-specific selectors can save us time, but will quickly lead to a cluttered stylesheet. Exercise your best judgement.</p></blockquote>
<blockquote><p>Magic Numbers are unlucky. These are numbers that are used as quick fixes on a one-off basis. Example: <code>.box { margin-top: 37px }</code>.</p></blockquote>
<h4>SMACSS</h4>
<p><img src="http://cdn.css-tricks.com/wp-content/uploads/2012/08/smacss.jpg" alt="" title="smacss" width="800" height="692"></p>
<p><a href="http://smacss.com/">Jonathan Snook&#39;s Scalable and Modular Architecture for CSS →</a></p>
<p>This thing is a beast and it would be hard to pull just a few quotes. But...</p>
<blockquote><p>Throwing every new style you create onto the end of a single file would make finding things more difficult and would be very confusing for anybody else working on the project.</p></blockquote>
<blockquote><p>If done right, Modules can easily be moved to different parts of the layout without breaking.</p></blockquote>
<blockquote><p>Only include a selector that includes semantics. A span or div holds none. A heading has some. A class defined on an element has plenty.</p></blockquote>
<h3>More?</h3>
<p>Does your organization maintain and use a public style guide? Post it!</p>
<p><a href="http://css-tricks.com/css-style-guides/">CSS Style Guides</a> is a post from <a href="http://css-tricks.com">CSS-Tricks</a></p>