---
layout: post
title:  "Chrome Apps: JavaScript Desktop Development"
date:   2012-07-05 07:00:00
author: 
categories: program
---

## Chrome Apps: JavaScript Desktop Development
### by 
### at 2012-07-05 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/W6eLBjl9DWk/evolution-chrome-apps>

<p><img alt="Evolution of Chrome Apps" src="http://dailyjs.com/images/posts/evolution-chrome-apps.png"></p>

<p>June was a month of tech conferences, but it was Google I/O that really stole the thunder of E3 and Apple’s WWDC. People are understandably excited about the Nexus 7, Jelly Bean, and Chrome for iOS, but what was there for us JavaScript hackers? All of the <a href="https://developers.google.com/events/io/sessions">Google I/O sessions are available for viewing</a>, and one that caught my eye was <a href="https://developers.google.com/events/io/sessions/gooio2012/200/">The Next Evolution of Chrome Apps</a>.</p>

<p>I saw this video <a href="https://twitter.com/tjholowaychuk/status/220753005593632768">referenced on Twitter by TJ Holowaychuk</a>, and given that his JavaScript aesthetics align with mine I wanted to know why he was talking about it. It turns out there’s a lot to like about the future direction of Chrome apps and Chromebooks from a JavaScript developer’s perspective. Offline is now being taken extremely seriously – partly because existing APIs are being adapted to cope with intermittent connections, but also because Chrome apps will be integrated with the OS (whether it’s Google Chrome OS, Mac OS, Linux, or Windows). This addresses a key usability problem people have with offline web apps: how are they launched? Rather than digging around in bookmarks or Chrome’s Apps page, we’ll now be able to launch them from the OS.</p>

<p>After seeing this talk, here’s my hyperbolic claim: Google is going to revolutionise desktop development. Watch the talk to see if you agree.</p>

<p>The part of the talk that stole the show was the Arduino demo. The new APIs allow applications to access OS-level features: networking, bluetooth, and even serial IO. Mihai Parparita (Chrome Apps Tech Lead, Google Reader) demonstrated an Arduino board that controlled a motor with a potentiometer, and a Chromebook (on the latest Canary build) was able to read and write data to it.</p>

<p>I’ve summarised the talk below, but if you’re desperate to see sample code then take a look at the <a href="https://github.com/GoogleChrome">GoogleChrome GitHub account</a> and <a href="http://developer.chrome.com/trunk/apps/about_apps.html">developer.chrome.com/apps</a>.</p>

<h3>Breaking out of the Browser</h3>

<p>In <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=379s">Breaking out of the Browser</a>, system-level integration is discussed:</p>

<ul>
<li>Launch apps from outside the browser</li>

<li>First-class OS windows – rather than just using the DOM, windows can be created that work with native controls and shortcuts (including cmd/alt-tab)</li>

<li>Full window frame without browser chrome</li>

<li>Links open in browsers, not the app</li>
</ul>

<p>The <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=482s">Bizarro World Window Demo</a> demonstrates this. This is all done with JavaScript.</p>

<h3>Offline by Default</h3>

<p>The <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=521s">Offline by Default</a> section deals with how new packaged apps are better able to cope with offline mode or intermittent connectivity.</p>

<ul>
<li>The interface and application logic are entirely local</li>

<li>There’s an enforced separation of data and UI to help prevent apps from getting into a broken state</li>

<li>An application should work without ever having connected to the network</li>

<li>Older APIs have been updated to perform better during poor connectivity</li>

<li>The fact apps launch from outside the browser helps people realise they work outside the browser and will function offline</li>
</ul>

<p>There’s a demo of <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=743s">a diff tool</a> that works differently offline. When the connection is lost, it immediately displays an offline notice and only allows local files to be opened. When it’s offline remote files can be opened instead.</p>

<h3>New APIs</h3>

<p>The <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=871s">new APIs</a> are mainly concerned with OS-level integration.</p>

<ul>
<li>System APIs: interacting with the hardware (USB/bluetooth) or OS (TCP)</li>

<li>Shared data APIs: interoperating with other apps (photos/contacts/calendar) accessed safely from multiple apps</li>

<li>(Google) Web service APIs that run well under poor network conditions: analytics, ads</li>

<li>Raw sockets, which Google employees have been using to create things like IRC clients</li>
</ul>

<p>There’s a <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=949s">demo showing an Arduino board interacting with a Chromebook</a>.</p>

<h3>Programming Model</h3>

<p>Of course, to facilitate all of these changes the <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=1027s">programming model</a> has changed somewhat:</p>

<ul>
<li>Applications use a “background page” with a main event</li>

<li>The application life cycle is event-based</li>

<li>System-level signals are accessible through an event-based API</li>

<li>The APIs have bindings to more languages – Dart is mentioned, but I’m not sure what else they’ll ship with</li>

<li>Background applications can be created</li>

<li>Apps should be single page with no navigation – basically designed like desktop applications instead of web applications</li>
</ul>

<p>There’s a bigger emphasis on the difference between <em>extensions</em> and <em>apps</em>. Extensions are now seen as something that modifies the browser itself, while apps are more like desktop applications. Some apps on the Chrome Web Store currently use extension-level functionality, but these will have to be changed to become extensions instead. I’m not sure if a package app can be distributed with an extension, because I’m sure there are some cases where the boundary is blurred – how does 1Password or LastPass fit into this model?</p>

<h3>Security Model</h3>

<p>The <a href="http://www.youtube.com/watch?feature=player_detailpage&amp;v=j8oFAr1YR-0#t=1331s">Security Model</a> reviews Chrome’s existing sandbox approach, but also details some new features:</p>

<ul>
<li>Storage isolation ensures applications can’t modify data they shouldn’t have access to</li>

<li>Applications can be restricted using a new permission system</li>

<li>There’s a new <code>&lt;browser&gt;</code> element that allows apps to render a page in a similar way to an iframe, but without the security issues</li>
</ul>

<h3>Systems Applications Working Group</h3>

<p>It certainly seems like these changes will open up new possibilities for developers interested in targeting Chrome and Chromebooks, but doesn’t this mean we’re investing in vendor-specific technology that we can’t use elsewhere? Well, Erik and Mihai addressed this by announcing that Google is working with Mozilla, Samsung, and Intel on the <a href="http://www.w3.org/2012/05/sysapps-wg-charter.html">System Applications Working Group</a>:</p>

<blockquote>
<p>The mission of the System Applications Working Group is to define an runtime environment, security model, and associated APIs for building Web applications that integrate with a host operating system.</p>
</blockquote>

<p>They’re also in the early stages of looking at adapting this technology for mobile platforms.</p><img src="http://feeds.feedburner.com/~r/dailyjs/~4/W6eLBjl9DWk" height="1" width="1">