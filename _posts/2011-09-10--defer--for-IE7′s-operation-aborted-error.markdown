---
layout: post
title:  "“defer” for IE7′s operation aborted error"
date:   2011-09-10 07:52:56
author: stoyan
categories: program
---

## “defer” for IE7′s operation aborted error
### by stoyan
### at 2011-09-10 07:52:56
### original <http://www.jspatterns.com/defer-for-ie7s-operation-aborted-error/>

<p>IE has this "operation aborted" problem (<a href="http://support.microsoft.com/kb/927917">MSDN article</a>) when you try to change your parent while it's not ready.</p>
<p>This is the example they give that causes this error:</p>
<div>
<pre><span>&lt;</span><span>html</span><span>&gt;</span><span>
  </span><span>&lt;</span><span>body</span><span>&gt;</span><span>
    </span><span>&lt;</span><span>div</span><span>&gt;</span><span>
      </span><span>&lt;</span><span>script</span><span>&gt;</span>
<span>        </span><span>document</span><span>.</span><span>body</span><span>.</span><span>innerHTML</span><span>+=</span><span>&quot;</span><span>sample text</span><span>&quot;</span><span>;</span>
      <span>&lt;/</span><span>script</span><span>&gt;</span><span>
    </span><span>&lt;/</span><span>div</span><span>&gt;</span><span>
  </span><span>&lt;/</span><span>body</span><span>&gt;</span><span>
</span><span>&lt;/</span><span>html</span><span>&gt;</span></pre>
</div>
<p>Turns out you can easily solve it by just adding a <code>defer</code> attribute to the <code>script</code>. This works just fine:</p>
<div>
<pre><span>&lt;</span><span>html</span><span>&gt;</span><span>
  </span><span>&lt;</span><span>body</span><span>&gt;</span><span>
    </span><span>&lt;</span><span>div</span><span>&gt;</span><span>
      </span><span>&lt;</span><span>script</span> defer<span>&gt;</span>
<span>        </span><span>document</span><span>.</span><span>body</span><span>.</span><span>innerHTML</span><span>+=</span><span>&quot;</span><span>sample text</span><span>&quot;</span><span>;</span>
      <span>&lt;/</span><span>script</span><span>&gt;</span><span>
    </span><span>&lt;/</span><span>div</span><span>&gt;</span><span>
  </span><span>&lt;/</span><span>body</span><span>&gt;</span><span>
</span><span>&lt;/</span><span>html</span><span>&gt;</span></pre>
</div>
<p><a href="http://www.phpied.com/files/operation-aborted/oa.html">Demo page here</a></p>
<p>BTW, this operation aborted may bite you if you decide to append a script node to the body, like </p>
<pre>var js = document.createElement('script');
js.async = true;
js.src = "http://tools.w3clubs.com/pagr/1.sleep-2.js";
document.body.appendChild(js); </pre>
<p>So it's not a good idea to append to the body like this when you don't control the page you're adding this code to, although <code>document.body</code> <a href="http://www.phpied.com/files/bscope/autobody.html">seems to be always available</a> even when the page doesn&#39;t have a &lt;body&gt; tag</p>