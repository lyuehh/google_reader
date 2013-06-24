---
layout: post
title:  "jquery对于checkbox一些处理"
date:   2011-06-09 10:27:34
author: 
categories: program
---

## jquery对于checkbox一些处理
### by 
### at 2011-06-09 10:27:34
### original <http://hlee.iteye.com/blog/1073950>

<br><pre name="code">&lt;input type=&quot;checkbox&quot; name=&quot;foo&quot; value=&quot;bar&quot; /&gt;
&lt;input type=&quot;button&quot; onclick=&quot;show_checked()&quot; value=&quot;Show&quot; /&gt;
&lt;input type=&quot;button&quot; onclick=&quot;set_checked(true)&quot; value=&quot;On&quot; /&gt;
&lt;input type=&quot;button&quot; onclick=&quot;set_checked(false)&quot; value=&quot;Off&quot; /&gt;
</pre>
<br>
<br><pre name="code">
#判断是否选中
	$('input[name=foo]').is(':checked')
	$('input[name=foo]').attr('checked')
</pre>
<br><pre name="code">
#选中和不选中
	$('input[name=foo]').attr('checked', true);
	$('input[name=foo]').attr('checked', false);
</pre>
<br>
<br>
<br>
<br>判断是否选中，开始用的办法
<br>
<br>
<br>
<br><pre name="code">
$(function() {
  $('#isOther').click(function() {
    var other = $('#isOther:checked').val();
    if (other != undefined) $('#other').removeAttr('readonly');
    else $('#other').attr('readonly', 'readonly');
  });
});

</pre>
<br>
<br>其实可以
<br>
<br><pre name="code">var checked = $('input[type=checkbox]').attr('checked');</pre>
<br>
<br>返回一个jason对象，是有结构的
<br><pre name="code">
var myJSONObject = {&quot;bindings&quot;: [
        {&quot;ircEvent&quot;: &quot;PRIVMSG&quot;, &quot;method&quot;: &quot;newURI&quot;, &quot;regex&quot;: &quot;^http://.*&quot;},
        {&quot;ircEvent&quot;: &quot;PRIVMSG&quot;, &quot;method&quot;: &quot;deleteURI&quot;, &quot;regex&quot;: &quot;^delete.*&quot;},
        {&quot;ircEvent&quot;: &quot;PRIVMSG&quot;, &quot;method&quot;: &quot;randomURI&quot;, &quot;regex&quot;: &quot;^random.*&quot;}
    ]
};

myData = JSON.parse(text, function (key, value) {
    var type;
    if (value &amp;&amp; typeof value === &#39;object&#39;) {
        type = value.type;
        if (typeof type === &#39;string&#39; &amp;&amp; typeof window[type] === &#39;function&#39;) {
            return new (window[type])(value);
        }
    }
    return value;
});

</pre>
<br>
<br>一些参考：
<br>都是把一组复选框是否选中情况，得到。等着ajax发出去
<br>
<br><pre name="code">var sList = "";
$('input[type=checkbox]').each(function () {
    var sThisVal = (this.checked ? "1" : "0");
    sList += (sList=="" ? sThisVal : "," + sThisVal);
});
console.log (sList);</pre>
<br>另一个
<br>
<br><pre name="code">var str = "";

$(':checkbox').each(function() {
    str += this.checked ? "1," : "0,";
});

str = str.substr(0, str.length - 1);    //Remove the trailing comma</pre>
<br>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1073950#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>