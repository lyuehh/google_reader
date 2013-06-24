---
layout: post
title:  "javascript题目，如何在重写alert后还能正常弹出alert"
date:   2012-02-03 09:21:00
author: 司徒正美
categories: program
---

## javascript题目，如何在重写alert后还能正常弹出alert
### by 司徒正美
### at 2012-02-03 09:21:00
### original <http://www.cnblogs.com/rubylouvre/archive/2012/02/03/2335946.html>

<p>今天在群里遇到一道很有意思的题目，大家发挥所能，给出的答案五花八门。特意整理成博文发表出来：</p>
<pre>//原题目：
window.alert = function(){};______;alert(1); 填空，使后面的alert(1)能正确弹出，至少列举两种不同思路。
</pre>
<div>
<p>解法一，创建新的执行环境，使用iframe沙箱</p>
<pre>window.alert = function(){};
window.alert=function(obj){
    var iframe=document.createElement("iframe");
    iframe.src="javascript:void(0);"
    document.body.appendChild(iframe)
    iframe.contentWindow.alert(obj);
}
alert(1)
</pre>
<p>解法二，创建新的执行环境，打开新窗口</p>
<pre>window.alert = function(){};
window.alert = function(a){
    window.open('','').alert(a)
    //window.createPopup().document.parentWindow.alert(a) //IE only
}
alert(1);
</pre>
<p>解法三, 弄回原来的alert（IE下失败）</p>
<pre>window.alert = function(){};
window.alert = function(a){
    delete window.alert;
    window.alert(a);
}
alert(1);
</pre>
<p>解法四，思路同三（IE下失败）</p>
<pre>window.alert = function(){};
window.alert = function(a){
   window.__proto__.alert.call(window,a);
  //window.constructor.prototype.alert.call(window,a);
}
 alert(1);
</pre>
<p>解法五（IE only）</p>
<pre>window.alert = function(){};
execScript('sub alert(msg):msgbox msg:end sub', 'vbscript');
alert(1);
</pre>

<p>解法六，就是一晕招</p>
<pre>window.alert = function(){};
window.alert = function(a){
    window.confirm(a)
}
alert(1);
</pre>
</div>

<p></p>
<img src="http://www.cnblogs.com/rubylouvre/aggbug/2335946.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/rubylouvre/archive/2012/02/03/2335946.html">本文链接</a></p>