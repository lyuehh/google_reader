---
layout: post
title:  "ie6 css sprites重复加载"
date:   2012-07-13 11:41:39
author: Airen
categories: program
---

## ie6 css sprites重复加载
### by Airen
### at 2012-07-13 11:41:39
### original <http://www.w3cplus.com/code/328.html>

<div><div><div><p>如果你使用css sprites，那么在ie6下并不能发挥sprites的作用，它还是会每次再重新加载这个图片，解决方法为为ie6添加下面这条js：</p>
<pre>&lt;!--[if IE 6]&gt;
    &lt;script type=&quot;text/javascript&quot;&gt;
        document.execCommand(&quot;BackgroundImageCache&quot;,false,true);
    &lt;/script&gt;
&lt;![endif]--&gt;</pre></div></div></div><div><div><div><div>
      <div>2</div>
                  <a href="http://www.w3cplus.com/vote/node/328/1/vote/alternate/n3XSEcjMd5goSDJQuN-6Q16RJvMjfV3LSbUTwFcMuTo/nojs" rel="nofollow">
                <div title="Vote up!"></div>
          <div>Vote up!</div>
              </a>
                </div>
</div></div></div><div><ul><li><a href="http://www.w3cplus.com/blog/tags/159.html">ie6</a></li><li><a href="http://www.w3cplus.com/blog/tags/40.html">css sprite</a></li></ul></div><img src="http://www1.feedsky.com/t1/659938800/W3CPlus/feedsky/s.gif?r=http://www.w3cplus.com/code/328.html" border="0" height="0" width="0">