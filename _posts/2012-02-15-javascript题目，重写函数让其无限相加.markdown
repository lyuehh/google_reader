---
layout: post
title:  "javascript题目，重写函数让其无限相加"
date:   2012-02-15 10:19:00
author: 司徒正美
categories: program
---

## javascript题目，重写函数让其无限相加
### by 司徒正美
### at 2012-02-15 10:19:00
### original <http://www.cnblogs.com/rubylouvre/archive/2012/02/15/2351991.html>

<p>群里有个出了一道有趣的题目，分享出来让大家看看。</p>
<pre>function add(x) {________}; alert(add(2)(3)(4)); //填空，使结果为9
</pre>


<div>
<p>解法一，</p>
<pre>//貘大
function add(x) {
    var c = 0; 
    return function(x) {
        c = c + x ; arguments.callee.toString = function(){
            return c;
        }; 
        return arguments.callee;
    }(x);
}; 
alert(add(2)(3)(4)); 
</pre>
<p>解法二，</p>
<pre>//三桂
 function add(x) {
    return function(y){
        return function(z){
            return x+y+z;
        }
    }
}; 
alert(add(2)(3)(4));
</pre>
<p>解法三, </p>
<pre>//司徒正美
function add (a){
                if(!isFinite(add.i)){
                    add.i = a
                }else {
                    add.i += a;
                }
                add.valueOf = add.toString = function(){
                    return add.i
                }
                return add;
            }
            alert(add(2)(3)(4))
</pre>

</div>

<p></p>

<p>其实上题就是考curry，详见我<a href="http://www.cnblogs.com/rubylouvre/archive/2010/01/05/1598761.html">另一篇博文</a>。</p>
<p>如果你有不同的解法，也请多多指教！</p><img src="http://www.cnblogs.com/rubylouvre/aggbug/2351991.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/rubylouvre/archive/2012/02/15/2351991.html">本文链接</a></p>