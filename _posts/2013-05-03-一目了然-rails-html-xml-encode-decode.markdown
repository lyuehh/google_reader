---
layout: post
title:  "一目了然 rails html xml encode decode"
date:   2013-05-03 23:37:38
author: 
categories: program
---

## 一目了然 rails html xml encode decode
### by 
### at 2013-05-03 23:37:38
### original <http://hlee.iteye.com/blog/1859583>

<pre name="code">
1.9.2p320 :001 &gt; require &#39;htmlentities&#39;
1.9.2p320 :002 &gt; c = HTMLEntities.new
 #=&gt; #&lt;HTMLEntities:0x007fef0a5b0468 @flavor=&quot;xhtml1&quot;&gt;
1.9.2p320 :003 &gt; c.decode &quot;&amp;gt;&quot;
 #=&gt; &quot;&gt;&quot;
1.9.2p320 :004 &gt; require &#39;cgi&#39;
# =&gt; false
1.9.2p320 :005 &gt; CGI.unescapeHTML &quot;&amp;quot;&quot;
# =&gt; &quot;\&quot;&quot;
1.9.2p320 :006 &gt; helper.send :raw, &quot;&amp;lt;&quot;
# =&gt; &quot;&amp;lt;&quot;
1.9.2p320 :007 &gt; helper.send :raw, &quot;&lt;/p&gt;&quot;
# =&gt; &quot;&lt;/p&gt;&quot;
1.9.2p320 :008 &gt; helper.send :h, &quot;&lt;/p&gt;&quot;
# =&gt; &quot;&amp;lt;/p&amp;gt;&quot;

</pre>
<br>
<br>
<br>xml稍微特别处理
<br><pre name="code">
require &#39;rubygems&#39;
require &#39;builder&#39;

content = &lt;&lt;eos
SOME TEXT, GOES TO UPPERCASE
other text
&lt;em&gt;italics&lt;em&gt;


xml = Builder::XmlMarkup.new
  xml.instruct! :xml, :version =&gt; &#39;1.0&#39;
  xml.book :id =&gt; 1.0 do
    xml.keyPic &quot;keyPic1.jpg&quot;
    xml.parts do
      xml.part :partId =&gt; &quot;1&quot;, :name =&gt; &quot;name&quot; do
        xml.chapter :title =&gt; &quot;title&quot;, :subtitle =&gt; &quot;subtitle&quot; do
          xml.text content
        end
      end
    end
  end

#p xml
#When running from the CLI (Cygwin), I get the following:

&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;book id=&quot;1.0&quot;&gt;
  &lt;keyPic&gt;keyPic1.jpg&lt;/keyPic&gt;
    &lt;parts&gt;
      &lt;part partId=&quot;1&quot; name=&quot;name&quot;&gt;
        &lt;chapter title=&quot;title&quot; subtitle=&quot;subtitle&quot;&gt;
          &lt;text&gt;
          SOME TEXT, GOES TO UPPERCASE
          other text
          &amp;lt;em&amp;gt;italics&amp;lt;em&amp;gt;
          &lt;/text&gt;
        &lt;/chapter&gt;
      &lt;/part&gt;
    &lt;/parts&gt;
&lt;/book&gt;&lt;inspect/&gt;


&lt;text&gt;
SOME TEXT, GOES TO UPPERCASE
other text
&lt;em&gt;italics&lt;em/&gt;
&lt;/text&gt;

</pre>
<br>
<br>使用&lt;&lt; 操作符来添加不修改内容
<br>
<br><pre name="code">



xml.text do |t|
  t &lt;&lt; content
end

</pre>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1859583#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>