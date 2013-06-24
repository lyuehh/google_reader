---
layout: post
title:  "Playing With DOM And ES5"
date:   2011-10-20 13:51:35
author: Andrea Giammarchi
categories: program
---

## Playing With DOM And ES5
### by Andrea Giammarchi
### at 2011-10-20 13:51:35
### original <http://feedproxy.google.com/~r/WebReflection/~3/fGLX5FrWhWI/playing-with-dom-and-es5.html>

A quick fun post about "<i>how would you solve this real world problem</i>".<br><br><h3>The Problem</h3>Given a generic array of strings, create an unordered list of items where each item contains the text of the relative array index without creating a singe leak or reference during the whole procedure.<br>As plus, make each item in the list clickable so that an <i>alert</i> with current text content occur once clicked.<br><br>The assumption is that we are in a standard W3C environment with ES5+ support.<br><br><h3>The Reason</h3>I think is one of the most common tasks in Ajax world. We receive an array with some info, we want to display this info to the user and we want to react once the user interact with the list.<br>If we manage to avoid references we are safer about leaks. If we manage to optimize the procedure, we are also safe about memory consumption over a simplified DOM logic ... <br>How would you solve this ? Give it a try, then come back for my solution.<br><br><img src="http://www.3site.eu/images/monkey-on-computer.jpg"><br><br><h3>The Solution</h3>Here mine:<br><pre><br>document.body.appendChild(<br>  /* input */["a","b","c"].map(<br>    function (s, i) {<br>      this.appendChild(<br>        document.createElement("li")<br>      ).textContent = s;<br>      return this;<br>    },<br>    document.createElement("ul")<br>  )[0]<br>).addEventListener(<br>  "click",<br>  function (e) {<br>    if (e.target.nodeName === "LI") {<br>      alert(e.target.textContent);<br>    }<br>  },<br>  false<br>);<br></pre><br>Did you solve it the same way ? :)<div><img width="1" height="1" src="https://blogger.googleusercontent.com/tracker/34454975-8349619330375041121?l=webreflection.blogspot.com" alt=""></div>