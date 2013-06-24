---
layout: post
title:  "OAuth gem for rails，支持豆瓣，新浪微薄，腾讯微博，搜狐微博，网易微博"
date:   2011-03-26 03:13:17
author: 
categories: program
---

## OAuth gem for rails，支持豆瓣，新浪微薄，腾讯微博，搜狐微博，网易微博
### by 
### at 2011-03-26 03:13:17
### original <http://www.javaeye.com/topic/975805>

地址：<a href="https://github.com/hooopo/oauth_china">https://github.com/hooopo/oauth_china</a>
<br>目前完成oauth认证和发微薄功能，欢迎测试或者fork。
<br>
<br><strong>简介</strong>
<br><div>引用</div><div>OAuth gem for rails3，支持豆瓣，新浪微薄，腾讯微博，搜狐微博，网易微博。</div>
<br><strong>安装</strong>
<br><pre name="code">gem install oauth_china</pre>
<br><strong>使用</strong>
<br>在Gemfile里添加:
<br>
<br><pre name="code">gem 'oauth' 
gem 'oauth_china'</pre>
<br>
<br>添加配置文件
<br>
<br> 配置文件路径：
<br> <div>引用</div><div>config/oauth/douban.yml
<br> config/oauth/sina.yml
<br> config/oauth/qq.yml
<br> config/oauth/sohu.yml
<br> config/oauth/netease.yml</div>
<br>
<br> 配置文件格式：
<br><div>引用</div><div> development:
<br>   key:    &quot;you api key&quot;
<br>   secret: &quot;your secret&quot;
<br>   url:    &quot;http://yoursite.com&quot;
<br>   callback: &quot;http://localhost:3000/your_callback_url&quot;
<br> production:
<br>   key:    &quot;you api key&quot;
<br>   secret: &quot;your secret&quot;
<br>   url:    &quot;http://yoursite.com&quot;
<br>   callback: &quot;http://localhost:3000/your_callback_url&quot;</div>
<br><strong>演示</strong>
<br>
<br>  <pre name="code">   #config/oauth/sina.yml
     development:
           key:    &quot;you api key&quot;
           secret: &quot;your secret&quot;
           url:    &quot;http://yoursite.com&quot;
           callback: &quot;http://localhost:3000/syncs/sina/callback&quot;
         production:
           key:    &quot;you api key&quot;
           secret: &quot;your secret&quot;
           url:    &quot;http://yoursite.com&quot;
           callback: &quot;http://localhost:3000/syncs/sina/callback&quot;


     #config/routes.rb
     match &quot;syncs/:type/new&quot; =&gt; &quot;syncs#new&quot;, :as =&gt; :sync_new
     match &quot;syncs/:type/callback&quot; =&gt; &quot;syncs#callback&quot;, :as =&gt; :sync_callback

     #app/controllers/syncs_controller.rb
     # encoding: UTF-8
     class SyncsController &lt; ApplicationController

       before_filter :login_required

       def new
         client = OauthChina::Sina.new
         authorize_url = client.authorize_url
         Rails.cache.write(build_oauth_token_key(client.name, client.oauth_token), client.dump)
         redirect_to authorize_url
       end

       def callback
         client = OauthChina::Sina.load(Rails.cache.read(build_oauth_token_key(params[:type], params[:oauth_token])))
         client.authorize(:oauth_verifier =&gt; params[:oauth_verifier])

         results = client.dump

         if results[:access_token] &amp;&amp; results[:access_token_secret]
           #在这里把access token and access token secret存到db
           #下次使用的时候:
           #client = OauthChina::Sina.load(:access_token =&gt; &quot;xx&quot;, :access_token_secret =&gt; &quot;xxx&quot;)
           #client.add_status(&quot;同步到新浪微薄..&quot;)
           flash[:notice] = &quot;授权成功！&quot;
         else
           flash[:notice] = &quot;授权失败!&quot;
         end
         redirect_to account_syncs_path
       end

       private
       def build_oauth_token_key(name, oauth_token)
         [name, oauth_token].join(&quot;_&quot;)
       end

     end</pre>
<br><strong>注意</strong>
<br>系统时间要正确设置。否则会出现timstamps refused错误
<br>
<br>ps.抱怨一下，国内这些开放api接口新浪的是最方便的，无论文档还是认证流程。其他都是各种不按标准。。各种坑人啊。。尤其是搜狐网易。<img src="http://www.iteye.com/images/smiles/icon_arrow.gif"> 
          
          <br><br>
          作者: <a href="http://hooopo.javaeye.com">Hooopo</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/975805" style="color:red">已有 <strong>2</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>