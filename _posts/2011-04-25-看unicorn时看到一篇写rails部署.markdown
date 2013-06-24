---
layout: post
title:  "看unicorn时看到一篇写rails部署"
date:   2011-04-25 03:20:49
author: 
categories: program
---

## 看unicorn时看到一篇写rails部署
### by 
### at 2011-04-25 03:20:49
### original <http://hlee.iteye.com/blog/1014575>

<p><span style="border-collapse:separate;font-size:16px;font-style:normal;font-variant:normal;font-weight:normal;letter-spacing:normal;line-height:normal;text-indent:0px;white-space:normal;font-family:STHeiti;color:#000000"><span style="line-height:24px;text-align:left;font-family:Palatino,&#39;Palatino Linotype&#39;,Georgia,Times,&#39;Times New Roman&#39;,serif">
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Ruby on Rails 在佈署上面，有兩種模型：<span style="color:#ff0000"><span style="background-color:#ffffff">出自ihower.tw</span>
</span>
</p>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">全自動模型 Apache/Nginx + Passenger</h3>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline"><a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://www.modrails.com/">Passenger</a>
<span> </span>
又叫做 mod_rails，是目前佈署 Ruby on Rails 最方便的方式，直接將對 Rails 的支援變成 Apache 或 Nginx 的模組，就像 mod_php 一樣。</p>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline"><a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://httpd.apache.org/">Apache</a>
<span> </span>
是一套功能非常豐富、非常多人使用的開放原始碼 HTTP 伺服器，要在 Ubuntu 上安裝 Apache + Passenger 指令如下：</p>
</span>
</span>
</p>
<pre name="code">sudo apt-get install -y apache2-mpm-prefork  apache2-prefork-dev libapr1-dev libaprutil1-dev
sudo gem install passenger
sudo passenger-install-apache2-module
</pre>
 
<p><span style="border-collapse:separate;font-size:16px;font-style:normal;font-variant:normal;font-weight:normal;letter-spacing:normal;line-height:normal;text-indent:0px;white-space:normal;font-family:STHeiti;color:#000000"><span style="line-height:24px;text-align:left;font-family:Palatino,&#39;Palatino Linotype&#39;,Georgia,Times,&#39;Times New Roman&#39;,serif">
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">執行完 passenger-install-apache2-module 後會有一段設定，請將此設定加入 /etc/apache2/conf.d/mod_rails 檔案之中，例如：</p>
<pre>passenger_module /usr/local/lib/ruby/gems/1.8/gems/passenger-2.2.15/ext/apache2/mod_passenger.so
PassengerRoot /usr/local/lib/ruby/gems/1.8/gems/passenger-2.2.15
PassengerRuby /usr/local/bin/ruby
 </pre>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">假設你的 Rails 專案放在 /home/ihower/your_rails_app 目錄下，那麼可以新增 /etc/apache2/sites-enabled/your_rails_app.conf 這個專案的設定，例如：</p>
<pre>&lt;VirtualHost *:80&gt;
    ServerAdmin ihower@gmail.com
    ServerName your_rails_app.ruby.tw
    DocumentRoot /home/ihower/your_rails_app/public
    ErrorLog /var/log/apache2/error-your_rails_app.log
    CustomLog /var/log/apache2/access-your_rails_app.log combined

    # Deflate
    AddOutputFilterByType DEFLATE text/html text/xml text/plain text/css application/x-javascript text/javascript;
    BrowserMatch ^Mozilla/4 gzip-only-text/html
    BrowserMatch ^Mozilla/4\.0[678] no-gzip
    BrowserMatch \bMSIE !no-gzip !gzip-only-text/html

    &lt;Directory &quot;/home/ihower/your_rails_app/public&quot;&gt;
        Options FollowSymLinks
        Order allow,deny 
        Allow from all 
        AllowOverride all
        Options -MultiViews
        FileETag none
    &lt;/Directory&gt;

&lt;/VirtualHost&gt;
 </pre>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">注意到 DocumentRoot 和 Directory 是指向 public 這個靜態檔案的目錄。設定好之後，執行 sudo apache2ctl restart 便會啟用。如果之後你的 Rails 有任何修改要重新載入，但是並不想把 Apache 重開，請在你的 Rails 應用程式目錄下執行 touch tmp/restart.txt，這樣 mod_rails 就會知道要重新載入 Rails，而不需要重開 Apache。</p>
<blockquote style="margin:0px 10px 1.5em;padding:10px;border-width:0px;font-weight:inherit;font-style:normal;font-size:0.8em;font-family:inherit;vertical-align:baseline;color:#000000;overflow:hidden;background-color:#eeeeee">
<p style="margin:0px 0px 0.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:13px;font-family:inherit;vertical-align:baseline">Passenger 預設的 Rails 運行環境會是 production。在 production 環境下操作 Rails 指令有些必須加上環境變數，例如 rake db:migrate RAILS_ENV=production 或是主控台 rails console production</p>
</blockquote>
<h4 style="margin:0px 0px 1.25em;padding:0px;border-width:0px;font-weight:normal;font-style:inherit;font-size:1.2em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1.25">Nginx + Passenger</h4>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline"><a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://nginx.org/">Nginx</a>
<span> </span>
則是另一套在 Rails 世界上還蠻常被使用的第二選擇，相較於 Apache 雖然功能較少，但執行效率更為良好。要讓 Nginx 裝上 Passgener 不需要先裝 Nginx，只需要執行以下指令：</p>
<pre><code>sudo passenger-install-nginx-module</code>

</pre>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">這是因為 Passenger 必須與 Nginx 一起編譯安裝的關係，所以 Passenger 的安裝指令就包括了安裝 Nginx。</p>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">反向代理(Reverse proxy)模型 Apache/Nginx + Mongrel/Thin/Unicorn</h3>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">反向代理(Reverse proxy)模型就比較複雜了，它分成 Web 伺服器和 Application 伺服器，圖示如下：</p>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline"><img style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline" src="http://ihower.tw/rails3/images/deployment-1.jpg" alt="images/deployment-1.jpg"></p>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">其中 Web 伺服器會是 Apache 或 Nginx，但是它除了提供靜態檔案之外，其餘的任務就只是做 reverse proxy 將 request 分發到 Application 伺服器。</p>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">而 Application 伺服器負責執行 Ruby on Rails 程式，這有不少選擇：</p>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Mongrel<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://mongrel.rubyforge.org/">http://mongrel.rubyforge.org/</a>
</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Thin<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://code.macournoyer.com/thin/">http://code.macournoyer.com/thin/</a>
</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Unicorn<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://unicorn.bogomips.org/">http://unicorn.bogomips.org/</a>
</li>
</ul>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">相較於 Passenger，設定上會比較複雜。</p>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">多執行序架構 和 EventMachine 架構</h3>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">(TODO) 參考<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://blog.phusion.nl/2010/06/09/does-rails-performance-need-an-overhaul/">Does Rails Performance Need an Overhaul?</a>
<span> </span>
這一篇文章</p>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">Ruby on Rails 主機代管服務</h3>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">可以參考這篇文章<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://antoniocangiano.com/the-best-ruby-on-rails-hosting-services/">The Best Ruby on Rails Hosting Services</a>
，這些服務可以概分為:</p>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Dedicated Server 專屬主機租用，一整台機器給你用</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">VPS (Vitual private server)，使用 VM 技術將一整台機器分租給多人，因此你可以獲得 root 權限安裝你想裝的軟體。</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Shared Host，提供固定的執行環境，例如只能執行 PHP 或 Rails。一台機器同時租用給非常多人。</li>
</ul>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">相較於 PHP，Rails 的確是比較耗費資源的，所以會推薦 VPS 等級以上。</p>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">以上的租用方式都是以月來計算，比較沒有彈性。如果需要以小時計算、租用資源非常彈性的服務，那就是雲端了：</p>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">IaaS，例如<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://aws.amazon.com/ec2/">Amazon EC2</a>
<span> </span>
服務，你可以獲得一整台的 root 權限。</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">PaaS 則是固定的執行環境，例如比較有名的 Google App Engine (只提供 Java 和 Python 環境，如果要執行 Rails 需改用 JRuby)。而 Rails 也有專屬的 PaaS 服務，<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://heroku.com/">Heroku</a>
，非常推薦一用，它的最基本方案是免費的。</li>
</ul>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">Heroku</h3>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://ihower.tw/blog/archives/4644">Heroku 簡介</a>
</li>
</ul>
<h4 style="margin:0px 0px 1.25em;padding:0px;border-width:0px;font-weight:normal;font-style:inherit;font-size:1.2em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1.25">安裝步驟</h4>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">安裝 Heroku 套件 gem install heroku</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">在你的 Rails 目錄下執行 heroku create your_app_name</li>
</ul>
<blockquote style="margin:0px 10px 1.5em;padding:10px;border-width:0px;font-weight:inherit;font-style:normal;font-size:0.8em;font-family:inherit;vertical-align:baseline;color:#000000;overflow:hidden;background-color:#eeeeee">
<p style="margin:0px 0px 0.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:13px;font-family:inherit;vertical-align:baseline">如果是 Windwos 使用者，Heroku 可能會無法正確找到你的 public key 位置，請執行以下步驟手動上傳：</p>
</blockquote>
<pre><code>$ heroku keys:add "%homedrive%%homepath%/.ssh/id_rsa.pub"</code>

</pre>
<h4 style="margin:0px 0px 1.25em;padding:0px;border-width:0px;font-weight:normal;font-style:inherit;font-size:1.2em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1.25">發佈步驟</h4>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">git push heroku master (發佈程式到 Heroku 上)</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">heroku rake db:migrate (第一次發佈或有新 migration 需要執行時)</li>
</ul>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">第三方服務</h3>
<h4 style="margin:0px 0px 1.25em;padding:0px;border-width:0px;font-weight:normal;font-style:inherit;font-size:1.2em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1.25">處理例外</h4>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">(TODO) 請參考這篇文章<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://ihower.tw/blog/archives/1773">Why? 主動攔截 Rails exception 錯誤</a>
</p>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Hoptoad<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://www.hoptoadapp.com/">http://www.hoptoadapp.com/</a>
</li>
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Exceptional<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://getexceptional.com/">http://getexceptional.com/</a>
</li>
</ul>
<h4 style="margin:0px 0px 1.25em;padding:0px;border-width:0px;font-weight:normal;font-style:inherit;font-size:1.2em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1.25">效能監控</h4>
<p style="margin:0px 0px 1.5em;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">(TODO)<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://www.newrelic.com/">NewRelic</a>
</p>
<h3 style="margin:0px 0px 0.5em;padding:0px 0px 0.5em;border-width:0px 0px 1px;font-weight:normal;font-style:inherit;font-size:1.5em;font-family:inherit;vertical-align:baseline;color:#000000;line-height:1;border-bottom:1px solid #cccccc">自動化部署</h3>
<ul style="margin:0px 1.5em 1.5em 0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">
<li style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline">Capistrano<span> </span>
<a style="margin:0px;padding:0px;border-width:0px;font-weight:inherit;font-style:inherit;font-size:16px;font-family:inherit;vertical-align:baseline;color:#990066;text-decoration:none" href="http://www.capify.org/">http://www.capify.org</a>
</li>
</ul></span>
</span>
</p>
              
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1014575#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>