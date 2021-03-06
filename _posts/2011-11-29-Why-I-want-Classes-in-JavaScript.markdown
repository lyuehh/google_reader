---
layout: post
title:  "Why I want Classes in JavaScript"
date:   2011-11-29 12:32:44
author: David
categories: program
---

## Why I want Classes in JavaScript
### by David
### at 2011-11-29 12:32:44
### original <http://www.davidflanagan.com/2011/11/why-i-want-clas.html>

<p> I had a little <a href="https://twitter.com/#!/__DavidFlanagan/status/136132446285606912">twitter</a>
<a href="https://twitter.com/#!/__DavidFlanagan/status/136132851484721153">rant</a>
this morning about classes in JavaScript. (Actually, the rant was
really about the attitude that seems to be behind much of the "don't
add classes to JavaScript" commentary I see.) So I should probably
explain in somewhat longer form why I want classes added to the language.

<ul>
<li>The JS standard library is object oriented: we write a.map(f), not
map(a,f). If the standard library defines classes, the language should
make it easy to define classes.

<li>A number of popular JS libraries include convenience methods for
defining and/or extending classes.  This demonstrates to me that
developers want to use classes and JavaScript, but that they want a
simpler way to define them.

<li>Once we get used to the idea of a class keyword in the language, I
think it is going to be really nice to be able to dash off little
classes with that keyword, instead of the two step define the
constructor and then set up its prototype object technique that we use
now. In particular, I think it is going to feel really old-fashioned
to define classes the old way once we've got modules in the language.
I can't articulate this point well, but it is actually the one that is
most compelling to me.  We're getting modules in the next version of
JS, and I don't think that the language will feel right without a
class syntax to go along.  

<li>The classes being proposed for JavaScript are all syntax sugar for
existing patterns.  Adding a class keyword will not change JS in any
fundamental way, it will just make it easier to use the patterns that
we already use.  If you like defining your classes the old way, you'll
still be able to do that.  And you'll still be able to add methods to
classes with C.prototype.new_method = function() {...} as you can
today. As Brendan Eich would say, this is just "paving the cow
paths". Here's what the <a href="http://wiki.ecmascript.org/doku.php?id=harmony:classes">classes proposal</a> [<b>Update link fixed now</b>] describes as its motivation: 
<blockquote>
ECMAScript already has excellent features for defining abstractions
for kinds of things. The trinity of constructor functions, prototypes,
and instances are more than adequate for solving the problems that
classes solve in other languages. The intent of this strawman is not
to change those semantics. Instead, it's to provide a terse and
declarative surface for those semantics so that programmer intent is
expressed instead of the underlying imperative machinery. 
</blockquote>


<p> 
I understand the reluctance that  many JS programmers feel about
adding classes.  I feel it too, a little bit. A class keyword isn&#39;t
&quot;javascripty&quot;. I like, and understand the appeal of Allen
Wirfs-Brock&#39;s approach: He defines extensions that make object
literals more expressive, and then uses the &lt;| and {. operators to
create a simple class definition pattern. But the problem is that it
is still a pattern rather than a new syntactic form. And to quote
Dave Herman: &quot;patterns are the sincerest form of feature request&quot;: if
we&#39;re using patterns to define classes, then the language ought to
just support classes.

<p> My desire to have classes in the next version of JavaScript of
course has no bearing on whether we'll actually get them.  The classes
proposal linked above is apparently not doing well in committee, for
reasons that I haven't tried to understand.  There have been recent
attempts to revive classes with <a href="https://gist.github.com/1329619">simpler</a> <a href="http://wiki.ecmascript.org/doku.php?id=strawman:minimal_classes">class</a>
<a href="https://gist.github.com/1332193">proposals</a>, so maybe
there is still some hope.</p></p></li></li></li></li></ul></p>