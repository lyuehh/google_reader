---
layout: post
title:  "关于watir selenium和webdriver soda cucumber"
date:   2011-11-29 01:34:09
author: 
categories: program
---

## 关于watir selenium和webdriver soda cucumber
### by 
### at 2011-11-29 01:34:09
### original <http://hlee.iteye.com/blog/1282183>

watir和selenium在自动化测试web测试领域强硬增长。这一趋势日渐强势，各种搭配和框架丛生。
<br>
<br>简单的搜集，我看到了cucumber配合watir的框架，看到例教，和类似的测试集成框架，也看到分布式测试框架
<br>
<br>
<br>简单使用：
<br>
<br>安装
<br><pre name="code">gem install watir-webdriver</pre>
<br><pre name="code">

require &#39;watir-webdriver&#39;
b = Watir::Browser.new
b.goto &#39;bit.ly/watir-webdriver-demo&#39;
b.text_field(:id =&gt; &#39;entry_0&#39;).set &#39;your name&#39;
b.select_list(:id =&gt; &#39;entry_1&#39;).select &#39;Ruby&#39;
b.select_list(:id =&gt; &#39;entry_1&#39;).selected? &#39;Ruby&#39;
b.div(:class =&gt; &#39;ss-form-entry&#39;).button.click
b.text.include? &#39;Thank you&#39;
#webdriver通用watir语法
browser.goto(&#39;http://myserver/mypage&#39;)
# to enter text into a text field - assuming the field is named &#39;username&#39;
browser.text_field(:name, &#39;username&#39;).set(&#39;Paul&#39;)
# if there was a text field that had an id of &#39;company_ID&#39;, you could set it to &#39;Ruby Co&#39;:
browser.text_field(:id ,&#39;company_ID&#39;).set(&#39;Ruby Co&#39;)
# to click a button that has a caption of &#39;Cancel&#39;
ie.button(:value, &#39;Cancel&#39;).click

</pre>
<br>
<br>这一篇，夜猪希望开个头，加强关注这个领域，列一些资源，有机会的话更多学习和接触。
<br>资源一部分：
<br>
<br><a href="http://watir.com/book/">书籍watirbook</a>
<br>
<br><a href="https://github.com/zeljkofilipin/watirbook/blob/master/installation/ubuntu.md">watir-webdriver不同系统的安装简单应用</a>
<br>
<br><a href="http://watirpodcast.com/">watir Podcast</a>
<br>
<br><a href="http://wiki.openqa.org/display/WTR/Cheat+Sheet">watir Cheat sheet</a>快速差语法用
<br>
<br><a href="http://wiki.openqa.org/display/WTR/Tutorial">watir不错的Tutorial</a>
<br>
<br>soda是用于测试nodejs的selenium client端
<br>
<br><a href="https://github.com/ph7/selenium-client">selenium的client</a>是之前夜猪自己就一直在用和cucumber配合用的。
<br>
<br>在测试的时候，很希望能停下来用firebug测debug一下，就会用到
<br><a href="https://github.com/jfirebaugh/capybara-firebug">capybara-firebug</a>
<br>
<br><pre name="code">
# located in features/support/capybara.rb or similar
require &#39;capybara/firebug&#39;
Selenium::WebDriver::Firefox::Profile.firebug_version = &#39;1.8.3&#39;

profile = Selenium::WebDriver::Firefox::Profile.new
profile.enable_firebug

Capybara::Driver::Selenium.new(app,
   :browser =&gt; :remote,
   :url =&gt; &quot;http://my.ip.add.ress:4444/wd/hub&quot;,
   :desired_capabilities =&gt; Selenium::WebDriver::Remote::Capabilities.firefox(:firefox_profile =&gt; profile))
</pre>
              
              <br><br>
              <span style="color:red">
                <a href="http://hlee.iteye.com/blog/1282183#comments" style="color:red">已有 <strong>0</strong> 人发表留言，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
              </span>
              <br><br><br>
<span style="color:#e28822">ITeye推荐</span>
<br>
<ul><li><a href="http://hlee.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>