---
layout: post
title:  "A New Streaming API for Node v0.10"
date:   2012-12-21 08:45:13
author: Isaac Z. Schlueter
categories: program
---

## A New Streaming API for Node v0.10
### by Isaac Z. Schlueter
### at 2012-12-21 08:45:13
### original <http://blog.nodejs.org/2012/12/21/streams2/>

<p><strong>tl;dr</strong>

</p>
<ul>
<li>Node streams are great, except for all the ways in which they&#39;re
terrible.</li>
<li>A new Stream implementation is coming in 0.10, that has gotten the
nickname &quot;streams2&quot;.</li>
<li>Readable streams have a <code>read()</code> method that returns a buffer or
null.  (More documentation included below.)</li>
<li><code>&#39;data&#39;</code> events, <code>pause()</code>, and <code>resume()</code> will still work as before
(except that they&#39;ll actully work how you&#39;d expect).</li>
<li>Old programs will <strong>almost always</strong> work without modification, but
streams start out in a paused state, and need to be read from to be
consumed.</li>
<li><strong>WARNING</strong>: If you never add a <code>&#39;data&#39;</code> event handler, or call
<code>resume()</code>, then it&#39;ll sit in a paused state forever and never
emit <code>&#39;end&#39;</code>.</li>
</ul>
<hr>
<p>Throughout the life of Node, we&#39;ve been gradually iterating on the
ideal event-based API for handling data.  Over time, this developed
into the &quot;Stream&quot; interface that you see throughout Node&#39;s core
modules and many of the modules in npm.

</p>
<p>Consistent interfaces increase the portability and reliability of our
programs and libraries.  Overall, the move from domain-specific events
and methods towards a unified stream interface was a huge win.
However, there are still several problems with Node&#39;s streams as of
v0.8.  In a nutshell:

</p>
<ol>
<li>The <code>pause()</code> method doesn&#39;t pause.  It is advisory-only.  In
Node&#39;s implementation, this makes things much simpler, but it&#39;s
confusing to users, and doesn&#39;t do what it looks like it does.</li>
<li><code>&#39;data&#39;</code> events come right away (whether you&#39;re ready or not).
This makes it unreasonably difficult to do common tasks like load a
user&#39;s session before deciding how to handle their request.</li>
<li>There is no way to consume a specific number of bytes, and then
leave the rest for some other part of the program to deal with.</li>
<li>It&#39;s unreasonably difficult to implement streams and get all the
intricacies of pause, resume, write-buffering, and data events
correct.  The lack of shared classes mean that we all have to solve
the same problems repeatedly, making similar mistakes and similar
bugs.</li>
</ol>
<p>Common simple tasks should be easy, or we aren&#39;t doing our job.
People often say that Node is better than most other platforms at this
stuff, but in my opinion, that is less of a compliment and more of an
indictment of the current state of software.  Being better than the
next guy isn&#39;t enough; we have to be the best imaginable.  While they
were a big step in the right direction, the Streams in Node up until
now leave a lot wanting.

</p>
<p>So, just fix it, right?

</p>
<p>Well, we are sitting on the results of several years of explosive
growth in the Node community, so any changes have to be made very
carefully.  If we break all the Node programs in 0.10, then no one
will ever want to upgrade to 0.10, and it&#39;s all pointless.  We had
this conversation around 0.4, then again around 0.6, then again around
0.8.  Every time, the conclusion has been &quot;Too much work, too hard to
make backwards-compatible&quot;, and we always had more pressing problems
to solve.

</p>
<p>In 0.10, we cannot put it off any longer.  We&#39;ve bitten the bullet and
are making a significant change to the Stream implementation.  You may
have seen conversations on twitter or IRC or the mailing list about
&quot;streams2&quot;.  I also gave <a href="https://dl.dropbox.com/u/3685/presentations/streams2/streams2-ko.pdf">a talk in
November</a>
about this subject.  A lot of node module authors have been involved
with the development of streams2 (and of course the node core team).

</p>
<h2>streams2</h2>
<p>The feature is described pretty thoroughly in the documentation, so
I&#39;m including it below.  Please read it, especially the section on
&quot;compatibility&quot;.  There&#39;s a caveat there that is unfortunately
unavoidable, but hopefully enough of an edge case that it&#39;s easily
worked around.

</p>
<p>The first preview release with this change will be 0.9.4.  I highly
recommend trying this release and providing feedback before it lands
in a stable version.

</p>
<p>As of writing this post, there are some known performance regressions,
especially in the http module.  We are fanatical about maintaining
performance in Node.js, so of course this will have to be fixed before
the v0.10 stable release.  (Watch for a future blog post on the tools
and techniques that have been useful in tracking down these issues.)

</p>
<p>There may be minor changes as necessary to fix bugs and improve
performance, but the API at this point should be considered feature
complete.  It correctly does all the things we need it to do, it just
doesn&#39;t do them quite well enough yet.  As always, be wary of running
unstable releases in production, of course, but I encourage you to try
it out and see what you think.  Especially, if you have tests that you
can run on your modules and libraries, that would be extremely useful
feedback.

</p>
<hr>
<h1>Stream</h1>
<pre><code>Stability: 2 - Unstable</code></pre>
<p>A stream is an abstract interface implemented by various objects in
Node.  For example a request to an HTTP server is a stream, as is
stdout. Streams are readable, writable, or both. All streams are
instances of EventEmitter.

</p>
<p>You can load the Stream base classes by doing <code>require(&#39;stream&#39;)</code>.
There are base classes provided for Readable streams, Writable
streams, Duplex streams, and Transform streams.

</p>
<h2>Compatibility</h2>
<p>In earlier versions of Node, the Readable stream interface was
simpler, but also less powerful and less useful.

</p>
<ul>
<li>Rather than waiting for you to call the <code>read()</code> method, <code>&#39;data&#39;</code>
events would start emitting immediately.  If you needed to do some
I/O to decide how to handle data, then you had to store the chunks
in some kind of buffer so that they would not be lost.</li>
<li>The <code>pause()</code> method was advisory, rather than guaranteed.  This
meant that you still had to be prepared to receive <code>&#39;data&#39;</code> events
even when the stream was in a paused state.</li>
</ul>
<p>In Node v0.10, the Readable class described below was added.  For
backwards compatibility with older Node programs, Readable streams
switch into &quot;old mode&quot; when a <code>&#39;data&#39;</code> event handler is added, or when
the <code>pause()</code> or <code>resume()</code> methods are called.  The effect is that,
even if you are not using the new <code>read()</code> method and <code>&#39;readable&#39;</code>
event, you no longer have to worry about losing <code>&#39;data&#39;</code> chunks.

</p>
<p>Most programs will continue to function normally.  However, this
introduces an edge case in the following conditions:

</p>
<ul>
<li>No <code>&#39;data&#39;</code> event handler is added.</li>
<li>The <code>pause()</code> and <code>resume()</code> methods are never called.</li>
</ul>
<p>For example, consider the following code:

</p>
<pre><code>// WARNING!  BROKEN!
net.createServer(function(socket) {

  // we add an &#39;end&#39; method, but never consume the data
  socket.on(&#39;end&#39;, function() {
    // It will never get here.
    socket.end(&#39;I got your message (but didnt read it)\n&#39;);
  });

}).listen(1337);</code></pre>
<p>In versions of node prior to v0.10, the incoming message data would be
simply discarded.  However, in Node v0.10 and beyond, the socket will
remain paused forever.

</p>
<p>The workaround in this situation is to call the <code>resume()</code> method to
trigger &quot;old mode&quot; behavior:

</p>
<pre><code>// Workaround
net.createServer(function(socket) {

  socket.on(&#39;end&#39;, function() {
    socket.end(&#39;I got your message (but didnt read it)\n&#39;);
  });

  // start the flow of data, discarding it.
  socket.resume();

}).listen(1337);</code></pre>
<p>In addition to new Readable streams switching into old-mode, pre-v0.10
style streams can be wrapped in a Readable class using the <code>wrap()</code>
method.

</p>
<h2>Class: stream.Readable</h2>


<p>A <code>Readable Stream</code> has the following methods, members, and events.

</p>
<p>Note that <code>stream.Readable</code> is an abstract class designed to be
extended with an underlying implementation of the <code>_read(size, cb)</code>
method. (See below.)

</p>
<h3>new stream.Readable([options])</h3>
<ul>
<li><code>options</code> {Object}<ul>
<li><code>bufferSize</code> {Number} The size of the chunks to consume from the
underlying resource. Default=16kb</li>
<li><code>lowWaterMark</code> {Number} The minimum number of bytes to store in
the internal buffer before emitting <code>readable</code>.  Default=0</li>
<li><code>highWaterMark</code> {Number} The maximum number of bytes to store in
the internal buffer before ceasing to read from the underlying
resource.  Default=16kb</li>
<li><code>encoding</code> {String} If specified, then buffers will be decoded to
strings using the specified encoding.  Default=null</li>
</ul>
</li>
</ul>
<p>In classes that extend the Readable class, make sure to call the
constructor so that the buffering settings can be properly
initialized.

</p>
<h3>readable._read(size, callback)</h3>
<ul>
<li><code>size</code> {Number} Number of bytes to read asynchronously</li>
<li><code>callback</code> {Function} Called with an error or with data</li>
</ul>
<p>All Readable stream implementations must provide a <code>_read</code> method
to fetch data from the underlying resource.

</p>
<p><strong>This function MUST NOT be called directly.</strong>  It should be
implemented by child classes, and called by the internal Readable
class methods only.

</p>
<p>Call the callback using the standard <code>callback(error, data)</code> pattern.
When no more data can be fetched, call <code>callback(null, null)</code> to
signal the EOF.

</p>
<p>This method is prefixed with an underscore because it is internal to
the class that defines it, and should not be called directly by user
programs.  However, you <strong>are</strong> expected to override this method in
your own extension classes.


</p>
<h3>readable.wrap(stream)</h3>
<ul>
<li><code>stream</code> {Stream} An &quot;old style&quot; readable stream</li>
</ul>
<p>If you are using an older Node library that emits <code>&#39;data&#39;</code> events and
has a <code>pause()</code> method that is advisory only, then you can use the
<code>wrap()</code> method to create a Readable stream that uses the old stream
as its data source.

</p>
<p>For example:

</p>
<pre><code>var OldReader = require(&#39;./old-api-module.js&#39;).OldReader;
var oreader = new OldReader;
var Readable = require(&#39;stream&#39;).Readable;
var myReader = new Readable().wrap(oreader);

myReader.on(&#39;readable&#39;, function() {
  myReader.read(); // etc.
});</code></pre>
<h3>Event: &#39;readable&#39;</h3>
<p>When there is data ready to be consumed, this event will fire.  The
number of bytes that are required to be considered &quot;readable&quot; depends
on the <code>lowWaterMark</code> option set in the constructor.

</p>
<p>When this event emits, call the <code>read()</code> method to consume the data.

</p>
<h3>Event: &#39;end&#39;</h3>
<p>Emitted when the stream has received an EOF (FIN in TCP terminology).
Indicates that no more <code>&#39;data&#39;</code> events will happen. If the stream is
also writable, it may be possible to continue writing.

</p>
<h3>Event: &#39;data&#39;</h3>
<p>The <code>&#39;data&#39;</code> event emits either a <code>Buffer</code> (by default) or a string if
<code>setEncoding()</code> was used.

</p>
<p>Note that adding a <code>&#39;data&#39;</code> event listener will switch the Readable
stream into &quot;old mode&quot;, where data is emitted as soon as it is
available, rather than waiting for you to call <code>read()</code> to consume it.

</p>
<h3>Event: &#39;error&#39;</h3>
<p>Emitted if there was an error receiving data.

</p>
<h3>Event: &#39;close&#39;</h3>
<p>Emitted when the underlying resource (for example, the backing file
descriptor) has been closed. Not all streams will emit this.

</p>
<h3>readable.setEncoding(encoding)</h3>
<p>Makes the <code>&#39;data&#39;</code> event emit a string instead of a <code>Buffer</code>. <code>encoding</code>
can be <code>&#39;utf8&#39;</code>, <code>&#39;utf16le&#39;</code> (<code>&#39;ucs2&#39;</code>), <code>&#39;ascii&#39;</code>, or <code>&#39;hex&#39;</code>.

</p>
<p>The encoding can also be set by specifying an <code>encoding</code> field to the
constructor.

</p>
<h3>readable.read([size])</h3>
<ul>
<li><code>size</code> {Number | null} Optional number of bytes to read.</li>
<li>Return: {Buffer | String | null}</li>
</ul>
<p>Call this method to consume data once the <code>&#39;readable&#39;</code> event is
emitted.

</p>
<p>The <code>size</code> argument will set a minimum number of bytes that you are
interested in.  If not set, then the entire content of the internal
buffer is returned.

</p>
<p>If there is no data to consume, or if there are fewer bytes in the
internal buffer than the <code>size</code> argument, then <code>null</code> is returned, and
a future <code>&#39;readable&#39;</code> event will be emitted when more is available.

</p>
<p>Note that calling <code>stream.read(0)</code> will always return <code>null</code>, and will
trigger a refresh of the internal buffer, but otherwise be a no-op.

</p>
<h3>readable.pipe(destination, [options])</h3>
<ul>
<li><code>destination</code> {Writable Stream}</li>
<li><code>options</code> {Object} Optional<ul>
<li><code>end</code> {Boolean} Default=true</li>
</ul>
</li>
</ul>
<p>Connects this readable stream to <code>destination</code> WriteStream. Incoming
data on this stream gets written to <code>destination</code>.  Properly manages
back-pressure so that a slow destination will not be overwhelmed by a
fast readable stream.

</p>
<p>This function returns the <code>destination</code> stream.

</p>
<p>For example, emulating the Unix <code>cat</code> command:

</p>
<pre><code>process.stdin.pipe(process.stdout);</code></pre>
<p>By default <code>end()</code> is called on the destination when the source stream
emits <code>end</code>, so that <code>destination</code> is no longer writable. Pass <code>{ end:
false }</code> as <code>options</code> to keep the destination stream open.

</p>
<p>This keeps <code>writer</code> open so that &quot;Goodbye&quot; can be written at the
end.

</p>
<pre><code>reader.pipe(writer, { end: false });
reader.on(&quot;end&quot;, function() {
  writer.end(&quot;Goodbye\n&quot;);
});</code></pre>
<p>Note that <code>process.stderr</code> and <code>process.stdout</code> are never closed until
the process exits, regardless of the specified options.

</p>
<h3>readable.unpipe([destination])</h3>
<ul>
<li><code>destination</code> {Writable Stream} Optional</li>
</ul>
<p>Undo a previously established <code>pipe()</code>.  If no destination is
provided, then all previously established pipes are removed.

</p>
<h3>readable.pause()</h3>
<p>Switches the readable stream into &quot;old mode&quot;, where data is emitted
using a <code>&#39;data&#39;</code> event rather than being buffered for consumption via
the <code>read()</code> method.

</p>
<p>Ceases the flow of data.  No <code>&#39;data&#39;</code> events are emitted while the
stream is in a paused state.

</p>
<h3>readable.resume()</h3>
<p>Switches the readable stream into &quot;old mode&quot;, where data is emitted
using a <code>&#39;data&#39;</code> event rather than being buffered for consumption via
the <code>read()</code> method.

</p>
<p>Resumes the incoming <code>&#39;data&#39;</code> events after a <code>pause()</code>.


</p>
<h2>Class: stream.Writable</h2>


<p>A <code>Writable</code> Stream has the following methods, members, and events.

</p>
<p>Note that <code>stream.Writable</code> is an abstract class designed to be
extended with an underlying implementation of the <code>_write(chunk, cb)</code>
method. (See below.)

</p>
<h3>new stream.Writable([options])</h3>
<ul>
<li><code>options</code> {Object}<ul>
<li><code>highWaterMark</code> {Number} Buffer level when <code>write()</code> starts
returning false. Default=16kb</li>
<li><code>lowWaterMark</code> {Number} The buffer level when <code>&#39;drain&#39;</code> is
emitted.  Default=0</li>
<li><code>decodeStrings</code> {Boolean} Whether or not to decode strings into
Buffers before passing them to <code>_write()</code>.  Default=true</li>
</ul>
</li>
</ul>
<p>In classes that extend the Writable class, make sure to call the
constructor so that the buffering settings can be properly
initialized.

</p>
<h3>writable._write(chunk, callback)</h3>
<ul>
<li><code>chunk</code> {Buffer | Array} The data to be written</li>
<li><code>callback</code> {Function} Called with an error, or null when finished</li>
</ul>
<p>All Writable stream implementations must provide a <code>_write</code> method to
send data to the underlying resource.

</p>
<p><strong>This function MUST NOT be called directly.</strong>  It should be
implemented by child classes, and called by the internal Writable
class methods only.

</p>
<p>Call the callback using the standard <code>callback(error)</code> pattern to
signal that the write completed successfully or with an error.

</p>
<p>If the <code>decodeStrings</code> flag is set in the constructor options, then
<code>chunk</code> will be an array rather than a Buffer.  This is to support
implementations that have an optimized handling for certain string
data encodings.

</p>
<p>This method is prefixed with an underscore because it is internal to
the class that defines it, and should not be called directly by user
programs.  However, you <strong>are</strong> expected to override this method in
your own extension classes.


</p>
<h3>writable.write(chunk, [encoding], [callback])</h3>
<ul>
<li><code>chunk</code> {Buffer | String} Data to be written</li>
<li><code>encoding</code> {String} Optional.  If <code>chunk</code> is a string, then encoding
defaults to <code>&#39;utf8&#39;</code></li>
<li><code>callback</code> {Function} Optional.  Called when this chunk is
successfully written.</li>
<li>Returns {Boolean}</li>
</ul>
<p>Writes <code>chunk</code> to the stream.  Returns <code>true</code> if the data has been
flushed to the underlying resource.  Returns <code>false</code> to indicate that
the buffer is full, and the data will be sent out in the future. The
<code>&#39;drain&#39;</code> event will indicate when the buffer is empty again.

</p>
<p>The specifics of when <code>write()</code> will return false, and when a
subsequent <code>&#39;drain&#39;</code> event will be emitted, are determined by the
<code>highWaterMark</code> and <code>lowWaterMark</code> options provided to the
constructor.

</p>
<h3>writable.end([chunk], [encoding])</h3>
<ul>
<li><code>chunk</code> {Buffer | String} Optional final data to be written</li>
<li><code>encoding</code> {String} Optional.  If <code>chunk</code> is a string, then encoding
defaults to <code>&#39;utf8&#39;</code></li>
</ul>
<p>Call this method to signal the end of the data being written to the
stream.

</p>
<h3>Event: &#39;drain&#39;</h3>
<p>Emitted when the stream&#39;s write queue empties and it&#39;s safe to write
without buffering again. Listen for it when <code>stream.write()</code> returns
<code>false</code>.

</p>
<h3>Event: &#39;close&#39;</h3>
<p>Emitted when the underlying resource (for example, the backing file
descriptor) has been closed. Not all streams will emit this.

</p>
<h3>Event: &#39;pipe&#39;</h3>
<ul>
<li><code>source</code> {Readable Stream}</li>
</ul>
<p>Emitted when the stream is passed to a readable stream&#39;s pipe method.

</p>
<h3>Event &#39;unpipe&#39;</h3>
<ul>
<li><code>source</code> {Readable Stream}</li>
</ul>
<p>Emitted when a previously established <code>pipe()</code> is removed using the
source Readable stream&#39;s <code>unpipe()</code> method.

</p>
<h2>Class: stream.Duplex</h2>


<p>A &quot;duplex&quot; stream is one that is both Readable and Writable, such as a
TCP socket connection.

</p>
<p>Note that <code>stream.Duplex</code> is an abstract class designed to be
extended with an underlying implementation of the <code>_read(size, cb)</code>
and <code>_write(chunk, callback)</code> methods as you would with a Readable or
Writable stream class.

</p>
<p>Since JavaScript doesn&#39;t have multiple prototypal inheritance, this
class prototypally inherits from Readable, and then parasitically from
Writable.  It is thus up to the user to implement both the lowlevel
<code>_read(n,cb)</code> method as well as the lowlevel <code>_write(chunk,cb)</code> method
on extension duplex classes.

</p>
<h3>new stream.Duplex(options)</h3>
<ul>
<li><code>options</code> {Object} Passed to both Writable and Readable
constructors. Also has the following fields:<ul>
<li><code>allowHalfOpen</code> {Boolean} Default=true.  If set to <code>false</code>, then
the stream will automatically end the readable side when the
writable side ends and vice versa.</li>
</ul>
</li>
</ul>
<p>In classes that extend the Duplex class, make sure to call the
constructor so that the buffering settings can be properly
initialized.

</p>
<h2>Class: stream.Transform</h2>
<p>A &quot;transform&quot; stream is a duplex stream where the output is causally
connected in some way to the input, such as a zlib stream or a crypto
stream.

</p>
<p>There is no requirement that the output be the same size as the input,
the same number of chunks, or arrive at the same time.  For example, a
Hash stream will only ever have a single chunk of output which is
provided when the input is ended.  A zlib stream will either produce
much smaller or much larger than its input.

</p>
<p>Rather than implement the <code>_read()</code> and <code>_write()</code> methods, Transform
classes must implement the <code>_transform()</code> method, and may optionally
also implement the <code>_flush()</code> method.  (See below.)

</p>
<h3>new stream.Transform([options])</h3>
<ul>
<li><code>options</code> {Object} Passed to both Writable and Readable
constructors.</li>
</ul>
<p>In classes that extend the Transform class, make sure to call the
constructor so that the buffering settings can be properly
initialized.

</p>
<h3>transform._transform(chunk, outputFn, callback)</h3>
<ul>
<li><code>chunk</code> {Buffer} The chunk to be transformed.</li>
<li><code>outputFn</code> {Function} Call this function with any output data to be
passed to the readable interface.</li>
<li><code>callback</code> {Function} Call this function (optionally with an error
argument) when you are done processing the supplied chunk.</li>
</ul>
<p>All Transform stream implementations must provide a <code>_transform</code>
method to accept input and produce output.

</p>
<p><strong>This function MUST NOT be called directly.</strong>  It should be
implemented by child classes, and called by the internal Transform
class methods only.

</p>
<p><code>_transform</code> should do whatever has to be done in this specific
Transform class, to handle the bytes being written, and pass them off
to the readable portion of the interface.  Do asynchronous I/O,
process things, and so on.

</p>
<p>Call the callback function only when the current chunk is completely
consumed.  Note that this may mean that you call the <code>outputFn</code> zero
or more times, depending on how much data you want to output as a
result of this chunk.

</p>
<p>This method is prefixed with an underscore because it is internal to
the class that defines it, and should not be called directly by user
programs.  However, you <strong>are</strong> expected to override this method in
your own extension classes.

</p>
<h3>transform._flush(outputFn, callback)</h3>
<ul>
<li><code>outputFn</code> {Function} Call this function with any output data to be
passed to the readable interface.</li>
<li><code>callback</code> {Function} Call this function (optionally with an error
argument) when you are done flushing any remaining data.</li>
</ul>
<p><strong>This function MUST NOT be called directly.</strong>  It MAY be implemented
by child classes, and if so, will be called by the internal Transform
class methods only.

</p>
<p>In some cases, your transform operation may need to emit a bit more
data at the end of the stream.  For example, a <code>Zlib</code> compression
stream will store up some internal state so that it can optimally
compress the output.  At the end, however, it needs to do the best it
can with what is left, so that the data will be complete.

</p>
<p>In those cases, you can implement a <code>_flush</code> method, which will be
called at the very end, after all the written data is consumed, but
before emitting <code>end</code> to signal the end of the readable side.  Just
like with <code>_transform</code>, call <code>outputFn</code> zero or more times, as
appropriate, and call <code>callback</code> when the flush operation is complete.

</p>
<p>This method is prefixed with an underscore because it is internal to
the class that defines it, and should not be called directly by user
programs.  However, you <strong>are</strong> expected to override this method in
your own extension classes.


</p>
<h2>Class: stream.PassThrough</h2>
<p>This is a trivial implementation of a <code>Transform</code> stream that simply
passes the input bytes across to the output.  Its purpose is mainly
for examples and testing, but there are occasionally use cases where
it can come in handy.
</p>