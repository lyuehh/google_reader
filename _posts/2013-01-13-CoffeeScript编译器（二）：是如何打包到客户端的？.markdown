---
layout: post
title:  "CoffeeScript编译器（二）：是如何打包到客户端的？"
date:   2013-01-13 21:09:36
author: island205
categories: program
---

## CoffeeScript编译器（二）：是如何打包到客户端的？
### by island205
### at 2013-01-13 21:09:36
### original <http://island205.com/2013/01/13/how-to-package-to-browser/>

<p>CoffeeScript编译器中有很多细节可以学习，今天就来看看CoffeeScript是如何把Node模块打包到浏览器端的？</p>
<p>CNode上有篇帖子：<a href="http://cnodejs.org/topic/50964ec9c922cde41e275bd4">发现coffee-script在浏览器端的是自己实现的require，有谁研究过原理和实现么？</a></p>
<p><strong>这是一个什么问题？</strong></p>
<p>目前CoffeeScript的源码使用CoffeeScript写的，全都放在src下面，而且遵循的是Node的包规范，即使用require、exports、module这三个对象来组织代码。这些文件会被编译到lib/coffee-script下面，当然也都是一个个Node包文件。这些文件在Node环境中当然可以运行，但在浏览器环境中就不一样了，那如何能让这些代码在浏览器中运行呢？<span></span></p>
<p>说白了，就是如何能让require、exports、module在客户端也能转起来。</p>
<p><strong>可能的解决方案</strong></p>
<p>SeaJS提供了一种可能，我们可以使用CMD标准，把源码包起来，然后使用SeaJS加载到浏览器中运行：</p>
<pre><code>define(function (require, exports, module) {

    // coffee-script module code

})</code></pre>
<p>作为一个语言编译器为什么要依赖别的类库呢？因此CoffeeScript不是这样做的。</p>
<p><strong>CoffeeScript的做法</strong></p>
<p>你可以到<a href="https://github.com/jashkenas/coffee-script/blob/master/Cakefile#L97">这里</a>一探究竟。这是CoffeeScript项目的make文件，这个task就是打包成可运行在浏览器端的CoffeeScript。这个命令会把<code>['helpers', 'rewriter', 'lexer', 'parser', 'scope', 'nodes', 'coffee-script', 'browser']</code>里面的文件统统打包到一个文件中，打成如下的形式：</p>
<pre><code>(function (root) {
    var CoffeeScript = function () {
        function require(path) {
            return require[path];
        }

        require[&#39;./helper&#39;] = new function () {
            var exports = this;
            // helper code
        };

        require[&#39;./rewriter&#39;] = new function () {
            var exports = this;
            // rewriter code
        };

        // and so on

        require[&#39;./coffee-script&#39;] = new function () {
            var exports = this;

            // code

            Lexer = require(&#39;./lexer&#39;).Lexer;

            parser = require(&#39;./parser&#39;).parser;
            // code
        };

        return require[&#39;./coffee-script&#39;];
    }();

    if (typeof define === &#39;function&#39; &amp;&amp; define.amd) {
        define(function () {
            return CoffeeScript;
        });
    } else {
        root.CoffeeScript = CoffeeScript;
    }
}(this));</code></pre>
<p>这样看就明白了。</p>
<ol>
<li>声明一个大家都能用的<strong>require</strong>函数对象，把模块作为这个对象的属性挂在下面，通过require(‘module name’)获取依赖模块，这解决了require的问题；</li>
<li>使用<code><strong>new function () { var exports = this }</strong></code>，实例化一个匿名函数，将每个包中的对外接口通过exports，即实例化的对象也就是this，暴露出来；这解决了<strong>exports</strong>的问题；</li>
<li><strong>module</strong>的问题——对于CoffeeScript来说，并没有module的问题，因为在lib下面，没有一个文件使用了module来组织代码。</li>
</ol>
<p><strong>noloader</strong></p>
<p>在此基础上，我在github上放了个项目，<a href="https://github.com/island205/noloader">noloader</a>，希望能解决几个问题，欢迎围观，^_^。</p>
<p>CoffeeScript打包的方式是手动指明依赖的，如何能够支持更一般的情况呢，noloader使用uglify-js进行语法分析，找出每个包文件的依赖，自动的获取依赖树，然后根据依赖树打包。</p>
<p>CoffeeScript打包的方式并不支持<strong>module</strong>关键字，无法使用到其他项目中；noloader使用下面的代码模式来打包，能够支持module关键字：</p>
<pre><code>function require(id) {
    return require[id]
}

// util module
void(function () {
    var module = {
        exports: {}
    },
    exports = module.exports

    (function (require, exports, module) {
        require('./lang/lang')
        // code
        exports.util = util

    })(require, exports, module)
    require['./util'] = module.exports
})()</code></pre>
<p>当然，noloader目前还不是很完美，没有处理循环依赖的情况，require方面沿用CoffeeScript的方式，很粗糙，还有本质上的bug。不过noloader的期望是，能够提供一个自动化的express中间件，完全可以使用Node包模式开发客户端程序，可复用服务端的模块，根据浏览器中的引用请求，自动的按需打包。</p>