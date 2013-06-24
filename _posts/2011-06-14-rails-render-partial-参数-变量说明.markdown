---
layout: post
title:  "rails render partial 参数 变量说明"
date:   2011-06-14 01:13:28
author: 夜鸣猪
categories: program
---

## rails render partial 参数 变量说明
### by 夜鸣猪
### at 2011-06-14 01:13:28
### original <http://hlee.iteye.com/blog/1084702>

这里的conventions总是容易记不清楚 就是locals collection object
<br>
<br>
<br>1. 默认参数 
<br>
<br><pre name="code">&lt;%= render :partial =&gt; &quot;account&quot; %&gt;</pre>
<br>默认本地有个变量@account, 传递过去，render到的partial（_account.erb）有个变量account 
<br>
<br>2. 单独参数 locals
<br>locals传递一组hash参数hash 值是本地的变量，hash的key是partial里的变量
<br>
<br><pre name="code">&lt;%= render :partial =&gt; &quot;account&quot;, :locals =&gt; { :account =&gt; @buyer } %&gt;

&lt;% for ad in @advertisements %&gt;
  &lt;%= render :partial =&gt; &quot;ad&quot;, :locals =&gt; { :ad =&gt; ad } %&gt;
&lt;% end %&gt; </pre>
<br>上面两个render分别 
<br>传递本地变量@buyer到_account.erb里叫account的参数
<br>传递本地变量ad到_ad.erb里叫ad
<br>
<br>3 
<br>
<br>根据1默认参数下面两个是一样的
<br>
<br><pre name="code">&lt;%= render :partial =&gt; &quot;contract&quot;, :locals =&gt; { :contract  =&gt; @contract } %&gt;

&lt;%= render :partial =&gt; &quot;contract&quot; %&gt;</pre>
<br>
<br>
<br>4. as使用
<br>
<br>用来改变传递之后，partial里变量的名称，如下，render方式是一样的。
<br>
<br><pre name="code">
&lt;%= render :partial =&gt; &quot;contract&quot;, :as =&gt; :agreement

&lt;%= render :partial =&gt; &quot;contract&quot;, :locals =&gt; { :agreement =&gt; @contract }</pre>
<br>传递@contract到_contract.erb里，partial里的变量名为agreement
<br>
<br>5. object使用
<br>
<br>object最简单，就是把一个变量原名传递到partial里，<span style="color:blue">
<br>所以什么都记不清楚的时候，就用object多写点都能表达</span>
<br>
<br>如下：
<br><pre name="code">
&lt;%= render :partial =&gt; &quot;account&quot;, :object =&gt; @buyer %&gt;

&lt;% for ad in @advertisements %&gt;
  &lt;%= render :partial =&gt; &quot;ad&quot;, :object =&gt; ad %&gt;
&lt;% end %&gt;</pre>
<br>传递@buyer到_account.erb的partial里变量名还是@buyer
<br>传递ad到_ad.erb的partial里变量名还是ad
<br>
<br>6 object和as合用
<br>
<br><pre name="code">&lt;%= render :partial =&gt; &quot;contract&quot;, :object =&gt; @contract, :as =&gt; :contract %&gt;

&lt;%= render :partial =&gt; &quot;contract&quot; %&gt;</pre>
<br>
<br>7 collection
<br>
<br><pre name="code">&lt;%= render :partial =&gt; &quot;ad&quot;, :collection =&gt; @advertisements %&gt;</pre>
<br>
<br>@advertisements是一个array，partial里_ad.erb有个ad变量是@advertisements的成员。就是_ad.erb不用写loop只是一条广告显示。
<br>
<br><pre name="code">&lt;%= render :partial =&gt; &quot;ad&quot;, :collection =&gt; @advertisements, :spacer_template =&gt; &quot;ad_divider&quot; %&gt;</pre>
<br>同上，循环显示_ad.erb _ad_divider.erb 显示@advertisements.size次其中partial_ad_counter是默认计数器表示第几条广告
<br><span style="color:darkred">
<br>8 默认</span>
<br>
<br>看你要partial的变量是一组记录还是一条记录，会对应约定用locals和collection
<br><pre name="code">
#@account是一条记录
# &lt;%= render :partial =&gt; &quot;accounts/account&quot;, :locals =&gt; { :account =&gt; @account} %&gt;
&lt;%= render :partial =&gt; @account %&gt;

# @posts是一组记录
# &lt;%= render :partial =&gt; &quot;posts/post&quot;, :collection =&gt; @posts %&gt;
&lt;%= render :partial =&gt; @posts %&gt;</pre>
<br>这是很人性的约定，可有时候就是忘了
<br>
<br>9 一些漂亮的简写
<br>
<br><pre name="code">#&lt;%= render :partial =&gt; &quot;account&quot; %&gt;可用下面代替
&lt;%= render &quot;account&quot; %&gt;

#&lt;%= render :partial =&gt; &quot;account&quot;, :locals =&gt; { :account =&gt; @buyer } %&gt;可用下面代替
&lt;%= render &quot;account&quot;, :account =&gt; @buyer %&gt;

# @account是一条记录
# &lt;%= render :partial =&gt; &quot;accounts/account&quot;, :locals =&gt; { :account =&gt; @account } %&gt;可用下面代替
&lt;%= render(@account) %&gt;

# @posts是一组记录
# &lt;%= render :partial =&gt; &quot;posts/post&quot;, :collection =&gt; @posts %&gt;可用下面代替
&lt;%= render(@posts) %&gt;</pre>
<br>
<br>10 layout
<br>
<br><pre name="code">
&lt;%# app/views/users/index.html.erb &amp;&gt;
Here&#39;s the administrator:
&lt;%= render :partial =&gt; &quot;user&quot;, :layout =&gt; &quot;administrator&quot;, :locals =&gt; { :user =&gt; administrator } %&gt;

Here&#39;s the editor:
&lt;%= render :partial =&gt; &quot;user&quot;, :layout =&gt; &quot;editor&quot;, :locals =&gt; { :user =&gt; editor } %&gt;

&lt;%# app/views/users/_user.html.erb &amp;&gt;
Name: &lt;%= user.name %&gt;

&lt;%# app/views/users/_administrator.html.erb &amp;&gt;
&lt;div id=&quot;administrator&quot;&gt;
  Budget: $&lt;%= user.budget %&gt;
  &lt;%= yield %&gt;
&lt;/div&gt;

&lt;%# app/views/users/_editor.html.erb &amp;&gt;
&lt;div id=&quot;editor&quot;&gt;
  Deadline: &lt;%= user.deadline %&gt;
  &lt;%= yield %&gt;
&lt;/div&gt;

</pre>
<br>
<br>参考文档：
<br><a href="http://apidock.com/rails/ActionController/Base/render">http://apidock.com/rails/ActionController/Base/render</a>
<br><a href="http://johnnyhg.iteye.com/blog/499182">http://johnnyhg.iteye.com/blog/499182</a>
<br><a href="http://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to">http://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to</a>
          
          <br><br>
          <span style="color:red">
            <a href="http://hlee.iteye.com/blog/1084702#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>