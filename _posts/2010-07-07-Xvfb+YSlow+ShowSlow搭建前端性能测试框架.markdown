---
layout: post
title:  "Xvfb+YSlow+ShowSlow搭建前端性能测试框架"
date:   2010-07-07 11:34:09
author: 黑三
categories: program
---

## Xvfb+YSlow+ShowSlow搭建前端性能测试框架
### by 黑三
### at 2010-07-07 11:34:09
### original <http://ued.taobao.com/blog/2010/07/07/xvfb_yslow_showslow-2/>

<p><strong>工具介绍</strong></p>
<p><a href="http://en.wikipedia.org/wiki/Xvfb">Xvfb</a>:  主要就是通过内存计算模拟出图形界面，没有平常所见的操作界面，分为客户端和服务器</p>
<p><span style="color:#0000ff"><strong><a href="http://developer.yahoo.com/yslow/">YSlow</a></strong></span>: 当Firefox浏览网页时，可以分析网站的页面（<a href="http://developer.yahoo.com/performance/rules.html">基于Yahoo 14条评分原则</a>），并指出如何进行优化提高网站性能</p>
<p><strong><span style="color:#0000ff"><a href="http://www.showslow.com/">ShowSlow</a>:<span style="font-weight:normal"><span style="color:#000000">收集YSlow的测试结果并显示出来</span></span></span></strong></p>
<p><a href="http://www.ubuntu.com/">Ubuntu</a>：开源的Linux系统，界面越来越向Windows靠近</p>
<p><strong>框架简介</strong></p>
<p>对于前端的童鞋我相信YSlow已经耳熟能详了，基于雅虎的评分规则对页面进行评分的Firefox插件，从中我们可以看出我们页面上的很多不足，并且可以知道我们如何改进和优化，配合将测试报告发送到本地的ShowSlow平台以提供给开发人员随时查看。在Xvfb的辅助下，<span style="color:#ff0000"><strong>这个框架最大的优点就是可以在无显示设备的环境下稳定运行</strong></span><span style="color:#ff0000"><strong>！</strong></span></p>
<p><span style="color:#ff0000"><strong><span></span><br>
</strong></span></p>
<p><strong>环境配置</strong></p>
<p>典型的LAMP配置，网上资料很多，当然你也可以<a href="http://www.besteric.com/?p=554">点此围观</a></p>
<p><span style="color:#0000ff"><strong>搭建本地ShowSlow测试平台</strong></span></p>
<p>这个我之前在Windows Server 2003搭建过（<a href="http://www.besteric.com/?p=479">点此围观</a>），但是这次在Ubuntu下还是有所区别的（<span style="color:#ff0000"><strong>所有命令都在终端输入</strong></span>）</p>
<ol>
<li><span style="color:#000000">sudo mkdir /var/www/showslow</span> (建立一个空文件夹)</li>
<li>sudo svn checkout http://showslow.googlecode.com/svn/trunk/ /var/www/showslow (将ShowSlow的源代码下载到本地，这一步会报错连接不上http://svn.facebook.com，首先要感谢国家，其次要感谢功夫网，最后我要说的是无视…)</li>
<li>sudo mv config.sample.php config.php (修改文件夹名)</li>
<li>sudo gedit config.php</li>
<li>根据实际情况修改<strong><span style="color:#ff0000">$db</span>,<span style="color:#ff0000">$user</span>,<span style="color:#ff0000">$pass</span></strong>可以根据实际情况修改</li>
<li>按照上一步修改的数据创建相应的Mysql数据库</li>
<blockquote><p>//以root用户权限进入mysql<br>
<span style="color:#ff0000"><strong> mysql -uroot -p</strong></span><br>
//创建一个数据库，名字和第二步你填写的保持一致<br>
<strong><span style="color:#ff0000"> create database ‘DBName‘;</span></strong><br>
//切换到新建的数据库<br>
<strong><span style="color:#ff0000"> use ‘DBName’;</span></strong><br>
// 将ShowSlow文件夹的tables.sql（数据库表）导入到新建的数据库中，注意无分号<br>
<span style="color:#ff0000"><strong>source /var/www/showslow/tables.sql</strong></span><br>
//查看下是否导入成功了，貌似有个表名叫ShowSlow2，汗<br>
<span style="color:#ff0000"><strong> show tables;</strong></span></p></blockquote>
</ol>
<p><strong><span style="color:#000000">自动化脚本</span></strong></p>
<p>这个是我们这个框架最重要的部分，具体请参考<a href="http://www.sergeychernyshev.com/blog/automating-yslow-and-pagespeed-using-xvfb/">Sergey Chernyshev</a>的博客以及自动化脚本作者<a href="http://www.linkedin.com/in/xiphoidindustries">Aaron Kulick</a> ，现在最新的Showslow的子文件夹automation有三个文件——<strong><span style="color:#ff0000">monitor.sh</span></strong> (配置文件) <span style="color:#ff0000"><strong>test_harness.pl</strong></span> （自动化脚本） <span style="color:#ff0000"><strong>ReadMe</strong></span>（<span style="color:#ff0000"><strong>框架说明文件，强烈推荐各位童鞋仔细阅读，因为这关系到后面脚本的修改问题</strong></span>）</p>
<p><strong>Xvfb</strong></p>
<p>sudo apt-get install xvfb</p>
<p>Xvfb :1 -screen 0 1152×864x24 +extension RANDR</p>
<blockquote><p>我在此处发现报错：<span style="color:#ff0000"><strong>[dix] Could not init font path element /usr/share/fonts/X11/cyrillic, removing from list!</strong></span></p>
<p>通过Google大神的帮助，解决办法很简单，安装这个需要的字体——<span style="color:#ff0000"><strong>sudo apt-get </strong></span><span style="color:#ff0000"><strong>install</strong></span><span style="color:#ff0000"><strong> xfonts-</strong></span><span style="color:#ff0000"><strong>cyrillic</strong></span></p></blockquote>
<p><span style="color:#0000ff"><strong>创建一份测试页面列表</strong></span></p>
<p>sudo gedit /var/www/URLs （在Apache下新建一个URLs列表，注意每一个链接为单独的一行)</p>
<p><span style="color:#0000ff"><strong>创建一份Firefox测试专用的Profiles</strong></span></p>
<p>/usr/lib/firefox-3.6.3/firefox -profilemanager</p>
<p>首先我们要修改Firefox的application.ini文件最后一段，避免Firefox崩溃后发送报告</p>
<blockquote>
<div>[Crash Reporter]</div>
<div>Enabled=<strong><span style="color:#ff0000">0</span></strong></div>
</blockquote>
<p>其次就是修改测试专用的Profiles的prefs.js，这个很关键，要设置一些Firefox属性才能让测试更好的进行下去，ShowSlow的论坛有推荐配置，（<a href="http://groups.google.com/group/showslow/browse_thread/thread/5a2b19db7dfbefb7">猛击这里</a>）</p>
<blockquote><p>gedit /home/eric/FFProfiles/prefs.js</p>
<p>## PREFS.JS RECOMMENDATIONS (AUTOMATION)</p>
<p><span style="color:#ff0000"><strong>#do not let automated firefox manipulate the profile extensions (auto </strong></span></p>
<p><span style="color:#ff0000"><strong>update) </strong></span></p>
<p>user_pref(“extensions.update.enabled”, false);</p>
<p>user_pref(“extensions.update.notifyUser”, false);</p>
<p><strong><span style="color:#ff0000">#disable session restore on crash (do not want stale/old tabs) </span></strong></p>
<p>user_pref(“browser.sessionstore.resume_from_crash”, false);</p>
<p><span style="color:#ff0000"><strong>#do not let javascript resize the window </strong></span></p>
<p>user_pref(“dom.disable_window_move_resize”, true);</p>
<p><span style="color:#ff0000"><strong>#do not let javascript manipulate context menus (automation) </strong></span></p>
<p>user_pref(“pref.advanced.javascript.disable_button.advanced”, false);</p>
<p><span style="color:#ff0000"><strong>#do not show me pop-up block messages (screenshot related) </strong></span></p>
<p>user_pref(“privacy.popups.showBrowserMessage”, false);</p>
<p><strong><span style="color:#ff0000">#do not warn for weak SSL or mixed SSL/HTTP content: </span></strong></p>
<p>user_pref(“security.warn_entering_weak”, false);</p>
<p>user_pref(“security.warn_viewing_mixed”, false);</p>
<p><span style="color:#ff0000"><strong>#firebug prefs </strong></span></p>
<p>user_pref(“extensions.firebug.allPagesActivation”, “on”);</p>
<p>user_pref(“extensions.firebug.net.enableSites”, true);</p>
<p>user_pref(“extensions.firebug.defaultPanelName”, “YSlow”);</p>
<p>user_pref(“extensions.firebug.previousPlacement”, 1);</p>
<p><span style="color:#ff0000"><strong>#yslow prefs </strong></span></p>
<p>user_pref(“extensions.yslow.autorun”, true);</p>
<p>user_pref(“extensions.yslow.beaconInfo”, “grade”);</p>
<p>user_pref(“extensions.yslow.beaconUrl”, “<strong><span style="color:#000000"><span style="font-weight:normal">http://localhost/showslow/beacon/yslow/</span></span><span style="color:#000000;font-weight:normal">“); </span></strong></p>
<p>user_pref(“extensions.yslow.optinBeacon”, true);</p></blockquote>
<p><span style="color:#0000ff"><strong>配置本地YSlow</strong></span></p>
<ol>
<li><span style="color:#0000ff"><span style="color:#000000">打开Firefox输入：about:config（我保证会很小心的）</span></span></li>
<li><span style="color:#0000ff"><span style="color:#000000">filter中输入：yslow</span></span></li>
<li><span style="color:#0000ff"><span style="color:#000000">修改以下三条数据</span></span></li>
<blockquote><p><span style="color:#ff0000"><span style="color:#000000">extensions.yslow.beaconUrl =</span> <strong>http://localhost/showslow/beacon/yslow/</strong></span></p>
<p><span style="color:#000000">如果测试和服务器不在同一机器上，请将<span style="color:#ff0000"><strong>localhost</strong></span>改成实际地址</span></p>
<p><strong><span style="color:#ff0000"><span style="color:#000000"><span style="font-weight:normal">extensions.yslow.beaconInfo =</span></span> grade</span></strong></p>
<p><span style="color:#ff0000"><strong><span style="color:#000000"><span style="font-weight:normal">extensions.yslow.optinBeacon =</span></span> true</strong></span></p></blockquote>
<li>重启Firefox，have fun  :)</li>
</ol>
<p>还等什么？开始你的测试之旅吧，查看测试报告的URL是：http://localhost/showslow/</p>
<blockquote><p><strong><span style="color:#ff0000">在这个地方遇到了一个问题就是所有配置都是正确的情况下，ShowSlow依然接收不到YSlow发送的测试数据，后来在RedHat的服务器上搭建环境同样遇到了这个问题，经过SA白非童鞋的帮助查看Apache的报错日志（/var/log/apache2/error.log）发现罪魁祸首是脚本中需要的几个模块必须是PHP5.2以上的版本，遂升级PHP至最新版本可解决问题。</span></strong></p></blockquote>
<p><span style="color:#0000ff"><strong>test_harness.pl</strong></span></p>
<p>这个脚本是用Perl语言写的，我也是刚接触这个语言，但是我还是推荐各位童鞋看看他的文件结构，老外写的代码阅读起来还是比较舒服，附带着大量注释方便我这样的小白也能轻松理解每个函数的意义。我们主要需要用到的是display，profile，source这三个属性（具体作用ReadMe有解释），可以参考下我运行这个脚本的方式：</p>
<p>perl test_harness.pl -display=:1 -source http://localhost/URLs -profile /home/eric/FFProfiles</p>
<blockquote><p>Can’t locate POE.pm in @INC (@INC contains: /etc/perl /usr/local/lib/perl/5.10.1 /usr/local/share/perl/5.10.1 /usr/lib/perl5 /usr/share/perl5 /usr/lib/perl/5.10 /usr/share/perl/5.10 /usr/local/lib/site_perl .) at test_harness.pl line 100.<br>
BEGIN failed–compilation aborted at test_harness.pl line 100.</p>
<p>这个问题纠结了我好几天，百思不得其解，关键还是我第一次使用Linux和Perl，对于很多报错信息都可以熟视无睹（请鄙视我），最后请教了Sergey童鞋，才恍然大悟原来是没有安装POE（请再次鄙视我），解决办法参考如下：</p>
<p><span style="color:#ff0000"><strong>sudo perl -MCPAN -e shell （sudo很关键啊，内牛满面）</strong></span></p>
<p><span style="color:#ff0000"><strong>cpan&gt; install POE</strong></span></p>
<p><span style="color:#ff0000"><strong>如果提示YAML没有安装 (类似于XML的数据描述语言)</strong></span></p>
<p><span style="color:#ff0000"><strong>cpan&gt;install YAML</strong></span></p>
<p><span style="color:#ff0000"><strong>cpan&gt; exit</strong></span></p></blockquote>
<p>这个时候自动化脚本已经开始运行了，我们可以通过外部机器访问虚拟机搭建的ShowSlow查看测试结果 (Ubuntu使用ifconfig查看本机IP地址，注意虚拟机网卡要设置成Bridged方式)</p>
<p><strong><span style="color:#0000ff">monitor.sh</span></strong></p>
<p>自动调用之前编写test_harness.pl脚本，当我们将调用test_harness的一些参数添加进monitor后使用Linux的Cron就可以实现自动化测试了：）</p>
<blockquote><p>注意将脚本文件夹的绝对路径赋值给Xvfb_PIDFILE，因为每次执行的时候系统会自动生成一个__xvfb.pid</p></blockquote>
<p>2010-7-1日更新</p>
<p>在Ubuntu和RedHat环境搭建下的Showslow平台点击测试URL进入Detail Page发现“Measurements over time”和“YSlow measurements history”两个区域的图形绘制不出，自己找了很久一直没有头绪，只好求助于ShowSlow的作者Sergey…<br>
1 确保config.php文件中的$showslow_base=http://localhost/showslow/<br>
2 查看phpinfo()确保Apache的模块mod_rewrite加载并启用了</p>
<blockquote><p>1 Enable mod_rewrite</p>
<p>sudo a2enmod rewrite ;</p>
<p>2 change all of  ”<span style="color:#ff0000"><strong>AllowOverride none</strong></span>” to “<span style="color:#ff0000"><strong>AllowOverride All</strong></span>”</p>
<p>sudo vim /etc/apache2/sites-enabled/000-default ;</p>
<p>3 Restart apache2</p>
<p>sudo /etc/init.d/apache2 restart;</p></blockquote>
<p>刷新一下页面图形是不是回来了：）</p>
<p>在实际工作中我们还遇到了一个不大不小的问题就是有些页面需要登录才能访问，而我们的测试环境又是全自动化的，所以在这里需要用到Firefox的一个插件叫做Greasemonkey，可以执行JS脚本，这样只需要我们编写一个自动登录的小JS脚本，然后在Greasemonkey中设置那几个URL需要使用这个脚本，这样每次访问需要登录的界面就会自动登录了。</p>
<p><strong>参考文档（部分需要翻墙）：</strong></p>
<p><a href="http://developer.yahoo.com/performance/rules.html">http://developer.yahoo.com/performance/rules.html</a></p>
<p><a href="http://en.wikipedia.org/wiki/Xvfb">http://en.wikipedia.org/wiki/Xvfb</a></p>
<p><a href="http://developer.yahoo.com/yslow/">http://developer.yahoo.com/yslow/</a></p>
<p><a href="http://www.showslow.com/">http://www.showslow.com/</a></p>
<p><a href="http://www.ubuntu.com/">http://www.ubuntu.com/</a></p>
<p><a href="http://www.sergeychernyshev.com/blog/automating-yslow-and-pagespeed-using-xvfb/">http://www.sergeychernyshev.com/blog/automating-yslow-and-pagespeed-using-xvfb/</a></p>
<p><a href="http://www.linkedin.com/in/xiphoidindustries">http://www.linkedin.com/in/xiphoidindustries</a></p>
<p><a href="http://groups.google.com/group/showslow/browse_thread/thread/5a2b19db7dfbefb7">http://groups.google.com/group/showslow/browse_thread/thread/5a2b19db7dfbefb7</a></p>
<p>–EOF–</p>