---
layout: post
title:  "short：Node.js 与 MongoDB结合的开源短域名项目"
date:   2011-09-28 12:30:44
author: nosqlfan
categories: program
---

## short：Node.js 与 MongoDB结合的开源短域名项目
### by nosqlfan
### at 2011-09-28 12:30:44
### original <http://item.feedsky.com/~feedsky/nosqlfan/~8149226/561309000/6253001/1/item.html>

<p><a href="https://github.com/edwardhotchkiss/short">short</a>是一个开源的短域名服务，使用Node.js与<span><a href="http://blog.nosqlfan.com/tags/mongodb" title="查看 MongoDB 的全部文章">MongoDB</a></span>搭建，可以直接在你的Server程序中调用，也可以通过Node.js的http server模块以一个服务启动。</p>
<p>可以直接通过Node.js的<span><a href="http://blog.nosqlfan.com/tags/npm" title="查看 npm 的全部文章">npm</a></span>进行安装：</p>
<div>
<pre>$ npm install short</pre>
</div>
<p>你可以直接在你的Node.js项目中这样调用，生成长域名对应的短链接：</p>
<pre>var mongoose = require("mongoose");
var short = require("short");

mongoose.connect("mongodb://localhost/short");

var URL = "http://nodejs.org/";

short.make(URL, function(error, shortURL) {
    if (error) {
        console.error(error);
    } else {
        short.get(shortURL.hash, function(error, shortURLObject) {
            if (error) {
                console.error(error);
            } else {
                var URL = shortURLObject[0].URL
                var hash = shortURLObject[0].hash;
                console.log(URL, hash);
                process.exit(1);
            };
        });
    }
});</pre>
<p>下面代码用于搭建一个提供短域名跳转的HTTP 服务：</p>
<pre>var http = require(&quot;http&quot;);
var mongoose = require(&quot;mongoose&quot;);
var short = require(&quot;short&quot;);

mongoose.connect(&quot;mongodb://localhost/short&quot;);

var app = http.createServer(function(request, response) {
    var hash = request.url.slice(1);
    if (request.url === &quot;/&quot;) {
        response.writeHead(200, { &quot;Content-Type&quot; : &quot;text/html&quot; });
        response.write(&quot;URL not found!&quot;);
        response.end();
    } else {
        short.get(hash, function(error, shortURLObject) {
            if (error) {
                console.error(error);
            } else {
                if (shortURLObject) {
                    var URL = shortURLObject[0].URL;
                    response.writeHead(302, {
                        &quot;Location&quot; : URL
                    });
                    response.end();
                } else {
                    response.writeHead(200, { &quot;Content-Type&quot; : &quot;text/html&quot; });
                    response.write(&quot;URL not found!&quot;);
                    response.end();
                }
            };
        });
    }
});

app.listen(8080);
console.log(&quot;&gt; Open http://localhost:8080/kQ4c&quot;);</pre>
<p>项目地址：<a href="https://github.com/edwardhotchkiss/short">github.com</a></p>
<table cellspacing="0" cellpadding="3" border="0" style="clear:both">
    
    <tr>
        <td colspan="5"><b><font size="-1" style="display:block!important;padding:20px 0 5px!important">相关文章：</font></b></td>
    </tr>
    
        <tr>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important">
                    <a title="MongoDB GridFS介绍PPT两则" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3194.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3163.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB GridFS介绍PPT两则</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoSpy, MongoWatch及MongoDB数据压缩" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3205.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3163.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoSpy, MongoWatch及MongoDB数据压缩</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Reds：一个Redis加Node.js的全文搜索引擎" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2676.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3163.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/08/02/20527286.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Reds：一个Redis加Node.js的全文搜索引擎</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="Python调用MongoDB使用心得" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F2989.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3163.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://static.wumii.com/site_images/2011/09/07/28779696.png" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">Python调用MongoDB使用心得</font>
                    </a>
                </td>
                <td width="106" valign="top" style="padding:5px!important;margin:0!important;border-left:1px solid #dddddd!important">
                    <a title="MongoDB 开发版 1.5.3 发布" style="text-decoration:none!important" href="http://app.wumii.com/ext/redirect.htm?url=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F269.html&amp;from=http%3A%2F%2Fblog.nosqlfan.com%2Fhtml%2F3163.html">
                        <img style="margin:0!important;padding:2px!important;border:1px solid #dddddd!important;width:100px!important;height:100px!important" src="http://pic.yupoo.com/iammutex/B8sPBDdj/square.jpg" width="100px" height="100px"><br>
                        <font size="-1" color="#333333" style="display:block!important;line-height:15px!important;width:106px!important;font:12px/15px arial!important;height:60px!important;margin:3px 0 0 0!important;padding:0!important;overflow:hidden!important">MongoDB 开发版 1.5.3 发布</font>
                    </a>
                </td>
        </tr>
    
    <tr>
        <td colspan="5" align="right">
            <a style="text-decoration:none!important" href="http://www.wumii.com/widget/relatedItems.htm" title="无觅相关文章插件">
                <font size="-1" color="#bbbbbb" style="display:block!important;font-family:arial!important;padding:5px 0!important;font-size:12px!important;color:#bbb!important">无觅</font>
            </a>
        </td>
    </tr>
</table><img src="http://www1.feedsky.com/t1/561309000/nosqlfan/feedsky/s.gif?r=http://item.feedsky.com/~feedsky/nosqlfan/~8149226/561309000/6253001/1/item.html" border="0" height="0" width="0">