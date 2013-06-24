---
layout: post
title:  "Mozilla Secure Coding Guidelines, Raphaël 2.0, cryptico.js"
date:   2011-10-10 07:00:00
author: 
categories: program
---

## Mozilla Secure Coding Guidelines, Raphaël 2.0, cryptico.js
### by 
### at 2011-10-10 07:00:00
### original <http://feedproxy.google.com/~r/dailyjs/~3/kuF3S2cBURI/sec-raphael-cryptico>

<h3>Mozilla Secure Coding Guidelines</h3>
<p>Mozilla’s <a href="https://wiki.mozilla.org/WebAppSec/Secure_Coding_Guidelines">WebAppSec/Secure Coding Guidelines</a> is a set of coding guidelines for developing secure applications.  There’s a lot information about securing application layer communications, but there’s also some JavaScript-specific advice.  JavaScript input validation is considered, along with preventing <span>XSS</span> attacks, and uploads as a JavaScript-based <span>XSS</span> attack vector.</p>
<p>Mozilla also <a href="http://hacks.mozilla.org/2011/09/introducing-aurora-9/">introduced Aurora 9</a> recently, which includes a JavaScript interface for <a href="https://developer.mozilla.org/en/The_Do_Not_Track_Field_Guide/Introduction/How_Do_Not_Track_works">Do Not Track</a>, and the addition of type inference.</p>
<h3>Raphaël 2.0</h3>
<p>Dmitry Baranovskiy has released <a href="http://raphaeljs.com/">Raphaël 2.0</a> (GitHub: <a href="https://github.com/DmitryBaranovskiy/raphael">DmitryBaranovskiy / raphael</a>). Dmitry wrote a post on February 10th about the <a href="http://groups.google.com/group/raphaeljs/browse_thread/thread/f76614f79da88478">planned features for Raphaël 2.0</a>.  The GitHub history indicates that this version has a new <span>VRML</span> version, and the project has been split up into three files: raphael.svg.js, raphael.vml.js, and raphael.core.js.</p>
<p>If you want to figure out the other changes, either look through <a href="http://raphaeljs.com/reference.html">Raphaël’s documentation</a> or try to read more of the history on GitHub.</p>
<h3>cryptico.js</h3>
<p><a href="http://cryptico.wwwtyro.net/">cryptico.js</a> (Google Code: <a href="http://code.google.com/p/cryptico/">cryptico</a>, License: <em>New <span>BSD</span> License</em>) is a public key cryptography library that can generate <span>RSA</span> key pairs, encrypt and decrypt messages.</p>
<p>Keys can be generated with <code>cryptico.generateRSAKey(passPhrase, 1024)</code>, and messages can be encrypted with <code>cryptico.encrypt(message, publicKeyString)</code>.</p>
<p>The <a href="http://code.google.com/p/cryptico/">cryptico documentation</a> includes notes on the library’s implementation:</p>
<blockquote>
<p>A hash is generated of the user’s passphrase using the SHA256 algorithm found at webtoolkit.info. This hash is used to seed David Bau’s seedable random number generator. A (seeded) random <span>RSA</span> key is generated with Tom Wu’s <span>RSA</span> key generator with 3 as a hard-coded public exponent.</p>
</blockquote><img src="http://feeds.feedburner.com/~r/dailyjs/~4/kuF3S2cBURI" height="1" width="1">