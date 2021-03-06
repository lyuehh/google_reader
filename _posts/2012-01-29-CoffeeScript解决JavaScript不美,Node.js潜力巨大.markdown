---
layout: post
title:  "CoffeeScript解决JavaScript不美,Node.js潜力巨大"
date:   2012-01-29 06:57:06
author: iceskysl
categories: program
---

## CoffeeScript解决JavaScript不美,Node.js潜力巨大
### by iceskysl
### at 2012-01-29 06:57:06
### original <http://www.iceskysl.com/?p=1635>

<p>如果你对JavaScript感兴趣，但是不喜欢写一大堆一大堆的JavaScript代码，那真的不是你的错，因为很多人都说JavaScript不美，究其原因，可以归纳为：<strong><em>JavaScript的诞生是个悲剧. 它是函数式+动态语言的优秀内核, 却硬被绑上了C/Java的语法。</em></strong></p>
<p>如果你真的喜欢JavaScript，那么你可以尝试了解更多～比如CoffeeScript，再比如Node.js，再比如Connect,再比如Express，再再比如jade，再再再比如npm等等，于是我们从安装npm（a package manager for node）开始～</p>
<p><strong>#装npm</strong><br>
ice@mac:~ &gt; curl http://npmjs.org/install.sh | sh<br>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current<br>
                                 Dload  Upload   Total   Spent    Left  Speed<br>
100  7881  100  7881    0     0   3664      0  0:00:02  0:00:02 –:–:–  8584<br>
tar=/usr/bin/tar<br>
version:<br>
bsdtar 2.8.3 – libarchive 2.8.3<br>
install npm@1.1<br>
fetching: http://registry.npmjs.org/npm/-/npm-1.1.0-3.tgz<br>
0.6.2<br>
1.1.0-3<br>
cleanup prefix=/usr/local</p>
<p>All clean!<br>
/usr/local/bin/npm -&gt; /usr/local/lib/node_modules/npm/bin/npm-cli.js<br>
npm@1.1.0-3 /usr/local/lib/node_modules/npm<br>
It worked<br>
<span></span><br>
#看看怎么用<br>
ice@mac:~ &gt; npm help</p>
<p>Usage: npm </p>
<p>where  is one of:<br>
    adduser, apihelp, author, bin, bugs, c, cache, completion,<br>
    config, deprecate, docs, edit, explore, faq, find, get,<br>
    help, help-search, home, i, info, init, install, la, link,<br>
    list, ll, ln, login, ls, outdated, owner, pack, prefix,<br>
    prune, publish, r, rb, rebuild, remove, restart, rm, root,<br>
    run-script, s, se, search, set, show, star, start, stop,<br>
    submodule, tag, test, un, uninstall, unlink, unpublish,<br>
    unstar, up, update, version, view, whoami</p>
<p>npm  -h     quick help on <br>
npm -l           display full usage info<br>
npm faq          commonly asked questions<br>
npm help   search for help on <br>
npm help npm     involved overview</p>
<p>Specify configs in the ini-formatted file:<br>
    /Users/ice/.npmrc<br>
or on the command line via: npm  –key value<br>
Config info can be viewed via: npm help config</p>
<p>npm@1.1.0-3 /usr/local/lib/node_modules/npm</p>
<p><strong>#用npm装connect</strong><br>
ice@mac:~ &gt; npm install connect<br>
npm http GET https://registry.npmjs.org/connect<br>
npm http 200 https://registry.npmjs.org/connect<br>
npm http GET https://registry.npmjs.org/connect/-/connect-1.8.5.tgz<br>
npm http 200 https://registry.npmjs.org/connect/-/connect-1.8.5.tgz<br>
npm http GET https://registry.npmjs.org/qs<br>
npm http GET https://registry.npmjs.org/mime<br>
npm http GET https://registry.npmjs.org/formidable<br>
npm http 200 https://registry.npmjs.org/formidable<br>
npm http 200 https://registry.npmjs.org/mime<br>
npm http 200 https://registry.npmjs.org/qs<br>
npm http GET https://registry.npmjs.org/formidable/-/formidable-1.0.8.tgz<br>
npm http GET https://registry.npmjs.org/mime/-/mime-1.2.4.tgz<br>
npm http GET https://registry.npmjs.org/qs/-/qs-0.4.1.tgz<br>
npm http 200 https://registry.npmjs.org/mime/-/mime-1.2.4.tgz<br>
npm http 200 https://registry.npmjs.org/formidable/-/formidable-1.0.8.tgz<br>
npm http 200 https://registry.npmjs.org/qs/-/qs-0.4.1.tgz<br>
connect@1.8.5 ./node_modules/connect<br>
├── mime@1.2.4<br>
├── qs@0.4.1<br>
└── formidable@1.0.8</p>
<p><strong>#用npm装express</strong><br>
ice@mac:~ &gt;  npm install express<br>
npm http GET https://registry.npmjs.org/express<br>
npm http 200 https://registry.npmjs.org/express<br>
npm http GET https://registry.npmjs.org/express/-/express-2.5.6.tgz<br>
npm http 200 https://registry.npmjs.org/express/-/express-2.5.6.tgz<br>
npm http GET https://registry.npmjs.org/mime<br>
npm http GET https://registry.npmjs.org/qs<br>
npm http GET https://registry.npmjs.org/mkdirp/0.0.7<br>
npm http 304 https://registry.npmjs.org/qs<br>
npm http 304 https://registry.npmjs.org/mime<br>
npm http 200 https://registry.npmjs.org/mkdirp/0.0.7<br>
npm http GET https://registry.npmjs.org/mkdirp/-/mkdirp-0.0.7.tgz<br>
npm http 200 https://registry.npmjs.org/mkdirp/-/mkdirp-0.0.7.tgz<br>
express@2.5.6 ./node_modules/express<br>
├── mkdirp@0.0.7<br>
├── mime@1.2.4<br>
└── qs@0.4.1</p>
<p><strong>#用npm装jade</strong><br>
ice@mac:~ &gt; npm install jade<br>
npm http GET https://registry.npmjs.org/jade<br>
npm http 200 https://registry.npmjs.org/jade<br>
npm http GET https://registry.npmjs.org/jade/-/jade-0.20.0.tgz<br>
npm http 200 https://registry.npmjs.org/jade/-/jade-0.20.0.tgz<br>
npm http GET https://registry.npmjs.org/commander<br>
npm http GET https://registry.npmjs.org/mkdirp<br>
npm http 200 https://registry.npmjs.org/mkdirp<br>
npm http GET https://registry.npmjs.org/mkdirp/-/mkdirp-0.3.0.tgz<br>
npm http 200 https://registry.npmjs.org/commander<br>
npm http GET https://registry.npmjs.org/commander/-/commander-0.2.1.tgz<br>
npm http 200 https://registry.npmjs.org/mkdirp/-/mkdirp-0.3.0.tgz<br>
npm http 200 https://registry.npmjs.org/commander/-/commander-0.2.1.tgz<br>
jade@0.20.0 ./node_modules/jade<br>
├── commander@0.2.1<br>
└── mkdirp@0.3.0</p>
<p>好了，你入门了，更多资料，请自行查看如下链接的资料吧～</p>
<p><strong>相关资料</strong></p>
<p>CoffeeScript: CoffeeScript is a little language that compiles into JavaScript</p>
<p>http://coffeescript.org/</p>
<p>为什么CoffeeScript这么美?</p>
<p>http://cnodejs.org/blog/?p=1584</p>
<p>CoffeeScript: The beautiful way to write JavaScript</p>
<p>http://amix.dk/blog/post/19612</p>
<p>10个让朋友对你刮目相看的CoffeeScript单行代码绝技</p>
<p>http://heikezhi.com/2011/06/08/10-coffeescript-one-liners-to-impress-your-friends/</p>
<p>npm: a package manager for node</p>
<p>https://github.com/isaacs/npm</p>
<p>Express：http://expressjs.com/<br>
High performance, high class web development for Node.js</p>
<p>Connect: Connect is a middleware layer for Node.js</p>
<p>http://www.senchalabs.org/connect/</p>
<p>jade: http://jade-lang.com/<br>
Node Template Engine</p>
<p>coffee-box: Blog engine for fashionable developers. Built upon Node.js, Express, MongoDB and CoffeeScript.</p>
<p>https://github.com/qiao/coffee-box</p>