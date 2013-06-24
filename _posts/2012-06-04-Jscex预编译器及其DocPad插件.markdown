---
layout: post
title:  "Jscex预编译器及其DocPad插件"
date:   2012-06-04 23:26:32
author: jeffz@live.com (老赵)
categories: program
---

## Jscex预编译器及其DocPad插件
### by jeffz@live.com (老赵)
### at 2012-06-04 23:26:32
### original <http://blog.zhaojie.me/2012/06/jscex-pre-complier-and-docpad-plugin.html>

<p>需求本身会是最好的动力。上个周末除了忙于构建<a href="http://jscex.info/">Jscex主站</a>以外，我还重新整理了Jscex的预编译器——或者说是AOT编译器。Jscex自带一个JIT编译器，配合eval可以在开发时避免额外的编译过程，这也可以说是Jscex的亮点之一。不过对于线上环境，一般都还是建议进行预编译，也就是将Jscex方法定义直接替换为目标代码。这么做的好处主要是为了降低部署时的脚本体积（摆脱对编译器的依赖所有代码加起来不到4KB），或是让异常情况下的错误定位变得容易（主要面向Node.js生产环境）。此外，为了便于编写文档，我还为DocPad开发了一个插件，用于对Jscex脚本进行预编译。</p>

<p>与其他Jscex的包不同，Jscex预编译器的定位不是一个Jscex模块，而是一个独立的工具。因此，它的第一种使用方式是作为一个命令：</p>

<pre>&gt; <strong>npm install jscexc</strong>
...

&gt; <strong>node node_modules/jscexc</strong>
Usage: node ./node_modules/jscexc --input &lt;input_file&gt; --output &lt;output_file&gt;

Options:
  --input, -i   The input file   [required]
  --output, -o  The output file  [required]

Missing required arguments: input, output</pre>

<p>但其实最好是将这个包构造为一个可以直接执行的命令，但我还不是还不是很清楚如何让它像mocha那样同时支持Windows和*nix，所以暂时还没去实现。等搞定这个问题之后，就可以像下面这样使用jscexc命令了：</p>

<pre>&gt; <strong>[sudo] npm -g install jscexc</strong>
...

&gt; <strong>jscexc --input test.js --output test.aot.js</strong></pre>

<p>于是这样便能将test.js的内容转化为test.aot.js，其中的Jscex方法定义都会被直接替换成目标代码。</p>

<p>这么做便达到了预编译的效果，但Jscex预编译器同样可以当作模块使用。例如，我在编写首页示例的时候，我就利用DocPad提供的插件机制，直接将*.jscex文件经过预编译，转化为最终的JavaScript代码：</p>

<pre><span style="color:green"># Export Plugin</span>
module.exports = (BasePlugin) -&gt;
    <span style="color:green"># Define Plugin</span>
    <span style="color:blue">class</span> JscexcPlugin <span style="color:blue">extends</span> BasePlugin
        name: <span style="color:darkred">&quot;jscexc&quot;</span>
        
        <span style="color:green"># Render some content</span>
        render: (opts, next) -&gt;
            <span style="color:green"># Prepare</span>
            {inExtension, outExtension, templateData, file} = opts

            <span style="color:green"># Check extensions</span>
            <span style="color:blue">if</span> inExtension <span style="color:blue">in</span> [<span style="color:darkred">&quot;jscex&quot;</span>] <span style="color:blue">and</span> outExtension <span style="color:blue">in</span> [<span style="color:darkred">&quot;js&quot;</span>]
                <span style="color:green"># Requires</span>
                jscexc = require(<span style="color:darkred">&quot;jscexc&quot;</span>)

                <span style="color:green"># Render</span>
                opts.content = jscexc.compile(opts.content)

            <span style="color:green"># Done, return back to DocPad</span>
            <span style="color:blue">return</span> next()</pre>

<p>虽然docpad-plugins-jscexc插件使用CoffeeScript编写，但看懂这段代码应该不成问题。总体而言，就是在需要的时候（这里是将*.jscex文件转化为*.js文件）把jscexc当作模块引入进来，再调用它的compile方法，任何Node.js文件都能这么使用jscexc包。现在，我在使用DocPad编写文档示例的时候，只要<a href="https://github.com/JeffreyZhao/jscex/blob/master/doc/src/documents/scripts/sorting-animations.js.jscex">直接编写Jscex代码</a>，DocPad会<a href="https://github.com/jscex/jscex.github.com/blob/master/scripts/sorting-animations.js">自动生成其目标内容</a>，而在页面上只需要引入<a href="https://github.com/jscex/jscex.github.com/blob/master/scripts/jscex-async.bundle.min.js">最基本的Jscex异步类库</a>即可，非常方便。</p>

<p>当然也有一些已知问题，例如输入文件的换行符必须是Unix下的\n而不能是Windows下的\n\r，这问题到不大，使用dos2unix或者随便哪个文本编辑器都能轻松搞定（当然我还是会修复这个问题的）。还有同学汇报说不能使用module语句——Jscex预编译器为了尽可能保留输入文件的内容，于是使用了能提供更多信息的<a href="https://github.com/mozilla/narcissus">Narcissus</a>分析器，可惜它实在太自作多情了一些，把module当作保留字了，所以我对Narcissus Parser做了简单的修补，也算是绕过了这个问题。</p>

<p>总有天我会换掉这个中看不中用的Narcissus分析器，例如<a href="http://esprima.org/">Esprima</a>似乎就挺不错的，而且实在不行就自己写一个。</p>