---
layout: post
title:  "使用Jscex改进Node Club（1）：运行Node Club网站"
date:   2012-02-20 20:49:15
author: jeffz@live.com (老赵)
categories: program
---

## 使用Jscex改进Node Club（1）：运行Node Club网站
### by jeffz@live.com (老赵)
### at 2012-02-20 20:49:15
### original <http://blog.zhaojie.me/2012/02/jscexify-nodeclub-1-prepare-nodeclub-website.html>

<p>一直想做个相对完整的项目来演示下<a href="https://github.com/JeffreyZhao/jscex">Jscex</a>的使用，可惜缺少创意和精力，一直没能实现。前几天看到<a href="http://club.cnodejs.org/">Node Club</a>将其网站开源了，不禁让我十分欢喜。Node Club网站是个真实案例，复杂度适中，既是Jscex的典型使用场景，又能避开我不擅长的网页样式设计和制作，简直是一个再合适不过的基础样板了。周末我大致看了下代码，也试着将几个部分使用Jscex改进了一下，效果也十分显著，于是打算写作一个系列指引，希望可以对Jscex类库的推广有所帮助。在此第一篇，自然是最基本的环境建设开始说起。</p>

<h1>配置系统环境</h1>

<p>首先，我强烈建议您可以在GitHub上Fork一下<a href="https://github.com/muyuan/nodeclub">Node Club网站的项目</a>，有个版本管理环境做后盾，做什么都会放心许多。我也将所有修改存放在GitHub上，其<a href="https://github.com/JeffreyZhao/nodeclub">master分支</a>是我个人不断改进的版本，您可以时刻关注其最新发展。除此之外，我还创建了一个<a href="https://github.com/JeffreyZhao/nodeclub/tree/tutorial">tutorial分支</a>，专门为这个系列文章存放代码，保持两者进度一致，同时尽量将修改过程和版本提交对应起来。</p>

<p>要运行Node Code，首先您得安装必要的环境：</p>

<ul>
  <li><a href="http://nodejs.org/">Node.js</a> </li>

  <li><a href="http://npmjs.org/">Node Package Manager</a> </li>

  <li><a href="http://mongodb.org/">MongoDB</a> </li>
</ul>

<p>无论您使用的是Windows，Linux还是OS X，以上都有对应的安装方式，在此就不详细描述了。文章中会出现一些控制台脚本，是我在OS X上运行的命令和结果。如果没有特殊说明，则路径是nodeclub项目的根目录。如果你使用的是Windows，则需要进行一些修改，其实最常见的修改就是分割路径的斜线还有可执行文件的使用。</p>

<p>这里假设您对以上三个工具，最好还包括Git有了一定了解。其实目前也无需十分熟悉，看看官方文档，搜几篇文章瞅瞅，就应该差不多了。</p>

<h1>配置Node Club环境</h1>

<p>假设您已经下载了Node Club代码（当然最好您是git clone一份自己fork出来项目），并将其解压缩至nodeclub目录中。现在您应该可以使用npm install命令安装所有依赖的包：</p>

<pre>$ <strong>npm install</strong>
eventproxy@0.1.0 ./node_modules/eventproxy 
ejs@0.5.0 ./node_modules/ejs 
validator@0.3.7 ./node_modules/validator 
node-markdown@0.1.0 ./node_modules/node-markdown 
mongoose@2.4.1 ./node_modules/mongoose 
├── colors@0.5.1
├── hooks@0.1.9
└── mongodb@0.9.7-1.4
express@2.5.1 ./node_modules/express 
├── mime@1.2.5
├── qs@0.4.2
├── mkdirp@0.0.7
└── connect@1.8.5
nodemailer@0.3.1 ./node_modules/nodemailer 
├── mailcomposer@0.1.4 (mimelib-noiconv@0.1.6)
└── simplesmtp@0.1.12</pre>

<p>要运行网站，则还需要准备一份配置文件。您可以将config.default.js复制一份至config.js文件，并建议修改一下一些配置：</p>

<pre>exports.config = {
    <span style="color:#006400">// 网站端口号，默认为80，可能会有冲突，建议改成其他值
    </span>port: 8080,

    <span style="color:#006400">// 发系统邮件时使用的用户名
    </span>mail_user: <span style="color:maroon">'xxxxx@gmail.com'</span>,
    <span style="color:#006400">// 发系统邮件时使用的密码
    </span>mail_pass: <span style="color:maroon">'xxxxx'</span>,
    <span style="color:#006400">// SMTP服务器地址
    </span>mail_host: <span style="color:maroon">'smtp.gmail.com'</span>,
    <span style="color:#006400">// 系统邮件发信人
    </span>mail_sender: <span style="color:maroon">'xxxxx@gmail.com'</span>,
    <span style="color:#006400">// 根据需求配置是否验证
    </span>mail_use_authentication: <span style="color:blue">true</span>,
};</pre>

<p>其中大部分的设置是在配置系统邮件的SMTP服务器，您可以像我一样使用Gmail，或是跟Node Club原项目一样使用126的邮箱。</p>

<h1>运行Node Club网站</h1>

<p>要运行Node Club网站，则需要启动MongoDB数据库，例如：</p>

<pre>mongodb-osx-x86_64-2.0.2/bin$ <strong>mkdir data</strong>
mongodb-osx-x86_64-2.0.2/bin$ <strong>./mongodb --dbpath data</strong></pre>

<p>此时您就在本地启动了一个MongoDB进程，使用默认端口27017，这与网站的默认配置相符。此时您就可以执行app.js来启动网站：</p>

<pre>$ <strong>node app.js</strong>
NodeClub listening on port 8080 in development mode
God bless love....</pre>

<p>在浏览器里访问http://127.0.0.1:8080，您应该就能看到一个空白的Node Club站点：</p>
<a href="http://img.zhaojie.me/blog/jscexify-nodeclub/1.png"><img alt="Node Club空白首页" src="http://img.zhaojie.me/blog/jscexify-nodeclub/1.png" width="450"></a> 

<p>此时您可以点击右上角的“注册”链接，注册一个名为admin的用户，部分操作（例如标签管理）需要使用该账号才能进行。注册时会要求填一个邮箱，提交后会收到一封激活邮件，但其中的链接可能不能直接访问（遗漏了端口号，应该是个bug），您可以将其地址复制到浏览器里修改并访问。激活成功后便可登录，登陆后会进入空白的后台页面：</p>
<a href="http://img.zhaojie.me/blog/jscexify-nodeclub/2.png"><img alt="Node Club空白后台" src="http://img.zhaojie.me/blog/jscexify-nodeclub/2.png" width="450"></a> 

<p>至此万事俱备，任何时候您想重启Node Club网站，只需ctrl+c中断node app.js命令再重新运行即可。从下一篇文章开始，我们将正式开始Jscex之旅。</p>

<h1>相关文章</h1>

<ul>
  <li>使用Jscex改进Node Club（1）：运行Node Club网站</li>
</ul>