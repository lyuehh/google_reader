---
layout: post
title:  "使用Jscex改进Node Club（2）：引入Jscex类库"
date:   2012-02-20 21:57:45
author: jeffz@live.com (老赵)
categories: program
---

## 使用Jscex改进Node Club（2）：引入Jscex类库
### by jeffz@live.com (老赵)
### at 2012-02-20 21:57:45
### original <http://blog.zhaojie.me/2012/02/jscexify-nodeclub-2-import-jscex.html>

<p>之前我们已经将Node Club在本地运行起来了，接着我们便来引入<a href="https://github.com/JeffreyZhao/jscex">Jscex</a>类库，为常用异步方法扩展出Jscex版本，并试着编写一些最简单的Jscex代码。</p>

<h1>安装Jscex包</h1>

<p>为项目中安装Jscex十分容易，因为Jscex已经发布在官方NPM源中，我们只需<a href="https://github.com/JeffreyZhao/nodeclub/commit/81e04cea4c279efe275bb74660dfe41954a714e0">修改package.json文件</a>即可：</p>

<pre>{
    <span style="color:maroon">&quot;name&quot;</span>: <span style="color:maroon">&quot;NodeClub&quot;
  </span>, <span style="color:maroon">&quot;version&quot;</span>: <span style="color:maroon">&quot;0.0.1&quot;
  </span>, <span style="color:maroon">&quot;main&quot;</span>: <span style="color:maroon">&quot;./app.js&quot;
  </span>, <span style="color:maroon">&quot;private&quot;</span>: <span style="color:blue">true
  </span>, <span style="color:maroon">&quot;dependencies&quot;</span>: {
      <span style="color:maroon">&quot;express&quot;</span>: <span style="color:maroon">&quot;2.5.1&quot;</span>,
      <span style="color:maroon">&quot;ejs&quot;</span>: <span style="color:maroon">&quot;0.5.0&quot;</span>,
      <span style="color:maroon">&quot;eventproxy&quot;</span>: <span style="color:maroon">&quot;0.1.0&quot;</span>,
      <span style="color:maroon">&quot;mongoose&quot;</span>: <span style="color:maroon">&quot;2.4.1&quot;</span>,
      <span style="color:maroon">&quot;node-markdown&quot;</span>: <span style="color:maroon">&quot;0.1.0&quot;</span>,
      <span style="color:maroon">&quot;validator&quot;</span>: <span style="color:maroon">&quot;0.3.7&quot;</span>,
      <span style="color:maroon">&quot;nodemailer&quot;</span>: <span style="color:maroon">&quot;0.3.1&quot;</span>,
      <span style="color:maroon">&quot;jscex&quot;</span>: <span style="color:maroon">&quot;0.6.x&quot;</span>,
      <span style="color:maroon">&quot;jscex-jit&quot;</span>: <span style="color:maroon">&quot;0.6.x&quot;</span>,
      <span style="color:maroon">&quot;jscex-async&quot;</span>: <span style="color:maroon">&quot;0.6.x&quot;</span>,
      <span style="color:maroon">&quot;jscex-async-powerpack&quot;</span>: <span style="color:maroon">&quot;0.6.x&quot;
  </span>}
}</pre>

<p>在此我们引入四个和Jscex相关的包，并指定为0.6.x版本，以便与NPM上的Jscex同步更新。修改之后，便可以使用npm install安装新增的Jscex包：</p>

<pre>$ <strong>npm install</strong>
jscex-async@0.6.0 ./node_modules/jscex-async 
jscex-async-powerpack@0.6.0 ./node_modules/jscex-async-powerpack 
jscex@0.6.0 ./node_modules/jscex 
jscex-jit@0.6.0 ./node_modules/jscex-jit</pre>

<p>有了NPM之后，每次为项目添加依赖库也只需编辑package.json再npm install就行了。如果需要将本地版本与NPM源保持同步更新，也只需一句npm update命令。</p>

<h1>Node Club中的异步方法</h1>

<p>在JavaScript有各种各样的异步方法，要配合Jscex使用的话，则必须使用能与Jscex适配的异步方法，简称Jscex异步方法。在Node Club项目中出现最多的异步方法便是使用<a href="https://github.com/LearnBoost/mongoose">mongoose</a>访问MongoDB。mongoose不仅仅提供了MongoDB的访问能力，它还提供相当的ORM功能，可以让我们快速的定义模型，并与MongoDB数据库里的集合映射起来，例如：</p>

<pre><span style="color:blue">var </span>mongoose = require(<span style="color:maroon">&quot;mongoose&quot;</span>),
    Schema = mongoose.Schema,
    ObjectId = Schema.ObjectId;

<span style="color:blue">var </span>UserSchema = <span style="color:blue">new </span>Schema({
    name: String,
    password: String,
    createAt: Date
});

<span style="color:blue">var </span>User = mongoose.model(<span style="color:maroon">'User'</span>, UserSchema);</pre>

<p>此时User类型便已经和数据库建立了映射关系，例如我们可以操作数据：</p>

<pre><span style="color:blue">function </span>updatePassword(id, password, cb) {
    User.findOne({ _id: id }, <span style="color:blue">function </span>(error, user) {
        <span style="color:blue">if </span>(error) <span style="color:blue">return </span>cb(error);

        user.password = password;
        user.save(<span style="color:blue">function </span>(error) {
            <span style="color:blue">if </span>(error) <span style="color:blue">return </span>cb(error);

            cb(<span style="color:blue">null</span>);
        });
    });
}</pre>

<p>以上代码的作用是更新一个用户的密码，我们先使用User.findOne获得用户对象，修改后再使用save方法存回数据库。这里用到的都是一种“标准”的异步方法模式，即使用回调函数获得（或返回）结果，其第一个参数为错误对象，如果不为空，则表示出错了。因此每次调用一个异步方法的时候，我们都需要在回调方法里判断是否出错，这也是异步编程的麻烦之一。</p>

<h1>加载Jscex类库</h1>

<p>如果要在项目里加载Jscex，我会建议使用一个简单的模块来存放与Jscex初始化相关的代码，<a href="https://github.com/JeffreyZhao/nodeclub/commit/dc1d5d7fe5305d9cd0428e767745d500d5a70f3d">例如libs/jscex.js</a>：</p>

<pre><span style="color:blue">var </span>Jscex = require(<span style="color:maroon">&quot;jscex&quot;</span>);
require(<span style="color:maroon">&quot;jscex-jit&quot;</span>).init(Jscex);
require(<span style="color:maroon">&quot;jscex-async&quot;</span>).init(Jscex);
require(<span style="color:maroon">&quot;jscex-async-powerpack&quot;</span>).init(Jscex);

<span style="color:blue">var </span>Jscexify = Jscex.Async.Jscexify;

<span style="color:blue">var </span>mongoose = require(<span style="color:maroon">&quot;mongoose&quot;</span>);

<span style="color:blue">var </span>mp = mongoose.Model.prototype;
mp.saveAsync = Jscexify.fromStandard(mp.save);
mp.removeAsync = Jscexify.fromStandard(mp.remove);

<span style="color:blue">var </span>m = mongoose.Model;
m.findByIdAsync = Jscexify.fromStandard(m.findById);
m.findOneAsync = Jscexify.fromStandard(m.findOne);
m.findAsync = Jscexify.fromStandard(m.find);
m.countAsync = Jscexify.fromStandard(m.count);

exports.Jscex = Jscex;</pre>

<p>前几行代码是标准的Jscex引入方式。而从mongoose相关的代码开始，便是在为其扩展Jscex异步方法。mongoose对扩展十分友好，它把内部的元数据暴露出来，让我们进行统一扩展。例如为Model扩展一些静态或是实例方法，便相当于为所有的模型及其它们的实例添加了方法。这种做法充分利用了JavaScript的灵活性，假如它把所有的元数据都隐藏起来，那我们没法如此简单直接地引入Jscex扩展了——当然总是有办法的，只是麻烦一些。</p>

<p>这里我强烈建议每个JavaScript类库或框架的开发者都能实现这种方式，给使用者充分的扩展途径。以Jscex自身为例，它的每个异步任务对象都是Jscex.Async.Task类型的实例，这样jscex-async-powerpack模块便可以轻易补充各种强大的辅助方法。</p>

<p>此外，由于mongoose遵守异步方法“标准模式”（即使用回调函数传回结果，其第一个参数为错误对象），因此只要一个fromStandard辅助函数便可全部应对。在以后的代码中，我们会大量使用这些扩展后的Jscex异步方法。</p>

<h1>编写简单的Jscex异步函数</h1>

<p>现在我们就直接写几行Jscex代码吧。例如在controllers/user.js文件里定义了以下几个方法：</p>

<pre><span style="color:blue">function </span>get_user_by_id(id, cb) {
    User.findOne({ _id: id }, <span style="color:blue">function </span>(err, user) {
        <span style="color:blue">if </span>(err) <span style="color:blue">return </span>cb(err, <span style="color:blue">null</span>);
        <span style="color:blue">return </span>cb(err, user);
    });
}

<span style="color:blue">function </span>get_user_by_name(name, cb) {
    User.findOne({ name: name }, <span style="color:blue">function </span>(err, user) {
        <span style="color:blue">if </span>(err) <span style="color:blue">return </span>cb(err, <span style="color:blue">null</span>);
        <span style="color:blue">return </span>cb(err, user);
    });
}

<span style="color:blue">function </span>get_user_by_loginname(name, cb) {
    User.findOne({ loginname: name }, <span style="color:blue">function </span>(err, user) {
        <span style="color:blue">if </span>(err) <span style="color:blue">return </span>cb(err, <span style="color:blue">null</span>);
        <span style="color:blue">return </span>cb(err, user);
    });
}

<span style="color:blue">function </span>get_users_by_ids(ids, cb) {
    User.find({ <span style="color:maroon">'_id'</span>: { <span style="color:maroon">'$in'</span>: ids } }, <span style="color:blue">function </span>(err, users) {
        <span style="color:blue">if </span>(err) <span style="color:blue">return </span>cb(err, <span style="color:blue">null</span>);
        <span style="color:blue">return </span>cb(err, users);
    });
}

<span style="color:blue">function </span>get_users_by_query(query, opt, cb) {
    User.find(query, [], opt, <span style="color:blue">function </span>(err, users) {
        <span style="color:blue">if </span>(err) <span style="color:blue">return </span>cb(err, <span style="color:blue">null</span>);
        <span style="color:blue">return </span>cb(err, users);
    });
}</pre>

<p>以上几个方法都有相同的特征：它们都是简单的调用一个mongoose的异步方法，判断是否出错，并返回结果。正如之前提过的那样，编写异步代码的麻烦之一，便是在每次回调时都要判断是否出错，因此在实际项目中的异步代码都会比各种“演示”用的玩具代码更麻烦一些。不过我们可以<a href="https://github.com/JeffreyZhao/nodeclub/commit/9dcbb432bc2909e7e098a8eee3eba9981ca9da4c">使用Jscex来改写这些代码</a>：</p>

<pre><span style="color:blue">var </span>Jscex = require(<span style="color:maroon">&quot;../libs/jscex&quot;</span>).Jscex;

<span style="color:blue">var </span>get_user_by_id_async = eval(Jscex.compile(<span style="color:maroon">&quot;async&quot;</span>, <span style="color:blue">function </span>(id) {
    <span style="color:blue">return </span>$await(User.findOneAsync({ _id: id }));
}));

<span style="color:blue">var </span>get_user_by_name_async = eval(Jscex.compile(<span style="color:maroon">&quot;async&quot;</span>, <span style="color:blue">function </span>(name) {
    <span style="color:blue">return </span>$await(User.findOneAsync({ name: name }));
}));

<span style="color:blue">var </span>get_user_by_loginname_async = eval(Jscex.compile(<span style="color:maroon">&quot;async&quot;</span>, <span style="color:blue">function </span>(name) {
    <span style="color:blue">return </span>$await(User.findOneAsync({ loginname: name }));
}));

<span style="color:blue">var </span>get_users_by_ids_async = eval(Jscex.compile(<span style="color:maroon">&quot;async&quot;</span>, <span style="color:blue">function </span>(ids) {
    <span style="color:blue">return </span>$await(User.findAsync({ <span style="color:maroon">'_id'</span>: { <span style="color:maroon">'$in'</span>: ids } }));
}));

<span style="color:blue">var </span>get_users_by_query_async = eval(Jscex.compile(<span style="color:maroon">&quot;async&quot;</span>, <span style="color:blue">function </span>(query, opt) {
    <span style="color:blue">return </span>$await(User.findAsync(query, [], opt));
}));</pre>

<p>在编写Jscex方法中，我们无需操作回调函数，只要在异步点上使用$await进行“等待”即可。我们也无需显式地处理错误，因为一旦出现错误便会抛出异常，异常如果没有被某个try…catch捕获到，则会顺着调用路径一路向上传递，直到被我们的代码或是系统捕获为止。Jscex将简单易用的传统编程模式与实践重新带回异步编程中，做到“同步编写，异步执行”的效果。这就是Jscex诞生的意义。</p>

<p>从下一篇文章开始，我们将逐步改造Node Club网站中现有的代码。</p>

<h1>相关文章</h1>

<ul>
  <li><a href="http://blog.zhaojie.me/2012/02/jscexify-nodeclub-1-prepare-nodeclub-website.html">使用Jscex改进Node Club（1）：运行Node Club网站</a></li>

  <li>使用Jscex改进Node Club（2）：引入Jscex类库</li>
</ul>