---
layout: post
title:  "HTML5: How To Create Downloads On The Fly"
date:   2011-08-19 15:39:57
author: Andrea Giammarchi
categories: program
---

## HTML5: How To Create Downloads On The Fly
### by Andrea Giammarchi
### at 2011-08-19 15:39:57
### original <http://feedproxy.google.com/~r/WebReflection/~3/RnxEaGsYBaw/html5-how-to-create-downloads-on-fly.html>

this is a quick one I have implemented already in <a href="http://fuckn.es">fuckn.es</a> in the <em>create angry memory</em> button logic ...
<br>
<br><h3>The New Download Attribute</h3>Hopefully soon, most updated browser will implement <a href="http://www.whatwg.org/specs/web-apps/current-work/multipage/links.html#attr-hyperlink-download">the download attribute</a> in hypertext links (aka: &lt;a&gt; tag)
<br>The quick summary is this one:
<br><blockquote>The download attribute, if present, indicates that the author intends the hyperlink to be used for downloading a resource. The attribute may have a value; the value, if any, specifies the default filename that the author recommends for use in labeling the resource in a local file system.</blockquote>
<br>And this is a basic example:
<br><pre>
<br>click here to
<br>&lt;a
<br>    href=&quot;resource234.txt&quot;
<br>    download=&quot;license.txt&quot;
<br>&gt;
<br>    download the license
<br>&lt;/a&gt;
<br></pre>
<br>
<br><h3>What Is Download For</h3>Well, I am pretty sure you have read at least once in your life this kind of extra info beside a link:
<br><em>right click to download the content and "Save As ..."</em>
<br>Moreover, I am pretty sure you have created at least once in your server a page able to <a href="http://devpro.it/code/72.html">force a generic file download</a>.
<br>All these instructions and server side headers/files may disappear thanks to this new attribute, also because <em>there's no such thing as "right button" on touch screens</em>, neither in some newer device pointer.
<br>If the file is meant to be download, it will ... cool?
<br>
<br><h3>Create Downloads On The Fly Via JavaScript</h3>When I have read about it, I have instantly realized the potentials of this attribute combined with <a href="http://en.wikipedia.org/wiki/Data_URI_scheme">inline data uri scheme</a>.
<br>
<br><h3>Only HTML5 ?</h3>Nope! Please note that many browsers let us already practice this technique. Some may open the file in a new blank page while some other may download directly the file as is for Chrome and the CSV example.
<br>As graceful degradation, the "<em>right click</em>" procedure will still do the trick.
<br>
<br><h4>Downlaod Canvas As Image Example</h4>First example is a classic one: <em>how to save a canvas snapshot as image via "click"</em>.
<br><pre>
<br>// basic example
<br>function createDownloadLink(canvas, name) {
<br>    var a = document.createElement("a");
<br>    a.download = name;
<br>    a.title = "download snapshot";
<br>    a.href = canvas.toDataURL();
<br>    return a;
<br>}
<br>
<br>// some paragraph in the page
<br>document.querySelector(
<br>    "p.snapshot"
<br>).appendChild(createDownloadLink(
<br>    document.querySelector("#game"),
<br>    "snapshot" + (-new Date) + ".png"
<br>));
<br></pre>
<br>When the user will tap/click on the link, the browser will simply start the download. <strong>No server side involved at all</strong>!
<br>
<br><h4>Save A Page As PDF</h4>Thanks to <a href="http://badassjs.com/post/708922912/pdf-js-create-pdfs-in-javascript">this technique</a> we may use same trick to produce a PDF file out of whatever web page.
<br><pre>
<br>// basic example
<br>function createPDFLink(fileName) {
<br>    var doc = new pdf();
<br>    // whatever content you want to download
<br>    var a = document.createElement("a");
<br>    a.download = fileName;
<br>    a.title = "download as PDF";
<br>    a.href = doc.output('datauri',{"fileName":name});
<br>    return a;
<br>}
<br>
<br>// some paragraph in the page
<br>document.querySelector(
<br>    "p.saveaspdf"
<br>).appendChild(createPDFLink(
<br>    "document-" + document.title + ".pdf"
<br>));
<br></pre>
<br>Of course if the page content changes we can replace the old link with a freshly new created one.
<br>
<br><h4>Save Table As CSV</h4>Well, another classic here, the csv format out of a table. This is a basic but <strong>working example</strong> ;)
<br><pre>
<br>&lt;script&gt;
<br>// really basic example
<br>function tableToCSV(table) {
<br>    for (var
<br>        header = table.querySelectorAll(&quot;tr th&quot;),
<br>        rows = table.querySelectorAll(&quot;tr td&quot;),
<br>        hlength = header.length,
<br>        length = hlength + rows.length,
<br>        result = Array(hlength),
<br>        i = hlength,
<br>        j;
<br>        i &lt; length; ++i
<br>    ) {
<br>        j = i % hlength;
<br>        j || result.push(&quot;\n&quot;);
<br>        result.push(rows[j].innerHTML);
<br>        ++j % hlength &amp;&amp; result.push(&quot;,&quot;);
<br>    }
<br>    i = 0;
<br>    while (i &lt; hlength) {
<br>        result[i] = header[i].innerHTML + (
<br>            ++i &lt; hlength ? &quot;,&quot; : &quot;&quot;
<br>        );
<br>    }
<br>    return result.join(&quot;&quot;);
<br>}
<br>this.onload = function () {
<br>    var a = document.body.appendChild(
<br>        document.createElement(&quot;a&quot;)
<br>    );
<br>    a.download = &quot;table.csv&quot;;
<br>    a.href = &quot;data:text/csv;base64,&quot; + btoa(
<br>        tableToCSV(document.querySelector(&quot;table&quot;))
<br>    );
<br>    a.innerHTML = &quot;download csv&quot;;
<br>};
<br>&lt;/script&gt;
<br>&lt;table&gt;
<br>    &lt;tr&gt;
<br>        &lt;th&gt;name&lt;/th&gt;
<br>        &lt;th&gt;age&lt;/th&gt;
<br>    &lt;/tr&gt;
<br>    &lt;tr&gt;
<br>        &lt;td&gt;Dan&lt;/td&gt;
<br>        &lt;td&gt;33&lt;/td&gt;
<br>    &lt;/tr&gt;
<br>    &lt;tr&gt;
<br>        &lt;td&gt;John&lt;/td&gt;
<br>        &lt;td&gt;32&lt;/td&gt;
<br>    &lt;/tr&gt;
<br>&lt;/table&gt;
<br></pre>
<br>Most likely we can test already above example even if the name won't probably be the chosen one.
<br>For safe base64 encode, compatible with UTF-8 pages, <a href="http://devpro.it/javascript_id_214.html">have a look at this script</a> ( <em>base64.encode()</em> and <em>base64.decode()</em> ).
<br>
<br><h3>Compatibility</h3>
<br>Different developers asked already about compatibility.
<br>As I have said before, we need to differentiate between "<em>download</em>" attribute compatibility AND inline data uri link compatibility.
<br>In the first case I don't know browsers that will force the download with the specified name yet, but I'll update this section as soon as I know someone.
<br>In the latter case, IE9, Chrome, Firefox, Safari, Webkit based, and Opera seem to be already compatible.
<br>The main problem/limit I have spotted in <em>fuckn.es</em> is the size of the data uri, in certain cases we may need a decent/fast machine otherwise we may end up killing RAM and CPU performances.
<br>IE8 is compatible as well except IE8 has limited data uri for CSS images, as example, and I expect same limit for this technique.
<br>Bear in mind when all browsers will be compatible, we will still have data stream limit problem, or better, really big files has to be parsed on the fly in one shot, no "<em>download progress</em>" possibility.
<br>
<br>
<br><h3>As Summary</h3>... now you know ... ;)<div><img width="1" height="1" src="https://blogger.googleusercontent.com/tracker/34454975-7461690838632776876?l=webreflection.blogspot.com" alt=""></div>