---
layout: post
title:  "Closure Compiler 高级模式及更多思考"
date:   2010-12-23 22:08:28
author: 法海
categories: program
---

## Closure Compiler 高级模式及更多思考
### by 法海
### at 2010-12-23 22:08:28
### original <http://ued.taobao.com/blog/2010/12/23/advanced-optimization-in-closure-compiler-and-more/>

<h3>前言</h3>
<p><a href="http://code.google.com/closure/compiler/">Google Closure Compiler</a> 是 <a href="http://code.google.com/closure/">Google Closure Tools</a> 的一员，在 2009 年底被 Google<br>
释出，早先，有 <a href="http://lifesinger.org/blog/">玉伯</a> 的 <a href="http://lifesinger.org/blog/2009/11/closure-compiler-vs-yuicompressor/" title="CC 和 YC 的比较">Closure Compiler vs. YUICompressor</a>，主要就 <em>压缩率</em><br>
上进行了对比，另外有 <a href="http://yiminghe.javaeye.com/blog/">承玉</a> 的 <a href="http://yiminghe.javaeye.com/blog/800881">应用 closure compiler 高级模式</a>，对 CC<br>
的高级模式做了些介绍</p>
<p>本文将详细介绍 CC 的高级模式部分，更重要的是，阐述 CC 高级模式背后的思考</p>
<p><span></span></p>
<h3>CC 是真正的编译器</h3>
<p>Closure Compiler 和 YUICompressor 并不是同类产品，虽然 CC 和 YC 同样产出压缩后的 JS 文件，<br>
但是 YC 只做了词法上的扫描，而 CC 并不只是一个 compressor 那么简单，器如其名，它是一个<br>
compiler</p>
<p>对于一个 compiler，一般地，它需要做到</p>
<ol>
<li>检查源文本中语法、语义、语用上的错误</li>
<li>根据分析产出物（符号表、语法树等）产出目标 / 中间代码</li>
<li>优化</li>
</ol>
<p>代码错误一般来自三个方面：</p>
<ol>
<li>
<p>语法（Syntax）</p>
<p>表示构成语言句子的各个记号之间的组合规律</p>
<p>大体上，parser / interpreter 在词法分析和语法分析阶段，产生符号表、语法树等分析产出物，<br>
具体见编译原理教科书……</p>
<p>语法上的错误，如：</p>
<pre><code>doSomething(;) // SyntaxError: Unexpected token ;
</code></pre>
<p>根据语法规则，在非 <code>for</code> 语句中的 <code>;</code> 意义是分隔符，而分隔符前的 <code>(</code> 并没有配对 <code>)</code>，<br>
因此报错</p>
</li>
<li>
<p>语义（Semantics）</p>
<p>表示各个记号的特定含义（各个记号和记号所表示的对象之间的关系）</p>
<p>compiler 需要根据语义分析产出中间代码，对于不产生中间代码的语言如 JS，则在运行时的<br>
解释期间指出错误</p>
<p>语义上的错误，如：</p>
<pre><code>0 = {}; // ReferenceError: Invalid left-hand side in assignment
</code></pre>
<p>根据赋值运算符 <code>=</code> 的意义，左操作数不能为字面量，所以虽然这个赋值语句包含了必需的<br>
左操作数、运算符、右操作数，仍然出错</p>
</li>
<li>
<p>语用（Pragmatics）</p>
<p>表示在各个记号所出现的行为中，它们的来源、使用和影响</p>
<p>语用上的错误，如：</p>
<pre><code>doSomething(); // ReferenceError: doSomething is not defined
</code></pre>
<p>在这里直接调用了一个未定义的函数，导致出错</p>
</li>
</ol>
<p>在一些其他场景中，虽然程序运行正确无误，但是仍然可以优化（这种优化并不是技巧上的），比如：</p>
<pre><code>function doSomethingElse() {}
(function() {
    return;
    doSomethingElse(); // No Exception but Redundant: Unreachable code
})();
</code></pre>
<p>在这里，doSomethingElse 函数之前由于有 return，因此这个函数调用将永远不能执行，这种冗余代码<br>
对整个程序来说毫无用处，可以去掉</p>
<p>对于 Closure Compiler 来说，它处理的对象是 js，不需要产生其他中间代码或汇编代码 / 机器码，<br>
因此输出的还是 js，但是是经过分析的、优化后的 js；另外，它也可以选择输出 parse tree（使用<br>
–print_tree 参数），所以，CC 的确完成了一个编译器需要实现的功能</p>
<h3>CC 功能概述</h3>
<p>在详细讨论 CC 的高级模式前，还是简明介绍一下功能体系</p>
<h4>编译级别</h4>
<p>CC 的 compilation_level 包括三个级别：</p>
<ol>
<li>
<p>WHITESPACE_ONLY</p>
<p>只删除空白、注释</p>
</li>
<li>
<p>SIMPLE_OPTIMIZATIONS</p>
<p>在 WHITESPACE_ONLY 基础上将局部变量和参数转成短名称</p>
</li>
<li>
<p>ADVANCED_OPTIMIZATIONS</p>
<p>更加激进的重命名、移除垃圾代码、内联函数</p>
</li>
</ol>
<p>可以看到，SIMPLE_OPTIMIZATIONS 级别的 CC，和 YC 无异，没做什么真正的编译工作，所以说，<br>
使用了高级模式的 CC 才是四肢健全的 CC =。=</p>
<h4>约束条件</h4>
<p>使用 CC 有一定约束条件，这影响到我们的编码风格：</p>
<ol>
<li>
<p>WHITESPACE_ONLY</p>
<ul>
<li>不认可 JS 1.5 以上版本的语言特性</li>
<li>不保留注释</li>
</ul>
</li>
<li>
<p>SIMPLE_OPTIMIZATIONS</p>
<ul>
<li>完全禁用 <code>with</code> 和 <code>eval</code></li>
<li>字符串中引用的函数名 / 参数名不会改动（CC 不改动所有字符串）</li>
</ul>
</li>
<li>
<p>ADVANCED_OPTIMIZATIONS 模式下的约束放到下文详述</p>
</li>
</ol>
<h4>注解</h4>
<p>Annotations 也是 CC 的重要组成部分，使用 JSDoc 风格，用以辅助高级模式下的编译，下文详述</p>
<h3>使用 CC 高级模式</h3>
<p>在 CC 下，启用高级模式的方法是加入参数 <code>--compilation_level ADVANCED_OPTIMIZATION</code></p>
<p>作为一个 compiler，CC 的高级模式下，额外的优化政策是</p>
<ol>
<li>更激进的重命名，如 obj.property 改为 a.b，将深度过高的命名空间平坦化等</li>
<li>移除垃圾代码，如删除未被调用的方法定义，警告逻辑死角（return 后的语句等）</li>
<li>将函数内联，如 a call b, b call c，a()，那么直接执行 c()</li>
</ol>
<p>要达到高级模式的预期优化效果，开发者必须对自己做一些约束，因为 js 是弱类型、动态性的。否则<br>
js 的这种灵活将使 compiler 无能为力</p>
<p>总体上，这种约束包括限定某些 js 编码风格，以及使用相应的 JSDoc 注解</p>
<p>以下详述具体的约束以及代码的检查 / 优化效果：</p>
<h4>强类型的模拟</h4>
<ul>
<li>
<p>@param 和 @type 中定义的类型会在编译期间得到检查，同样避免了在运行时检查，提高性能</p>
</li>
<li>
<p>@const 标记常量，当常量被写时会报错</p>
</li>
<li>
<p>模拟枚举，将同类可枚举常量定义为一个对象字面量，使用 @enum 标记：</p>
<pre><code>var STATUS = {
    LOADING: 3,
    COMPLETE: 4
};
</code></pre>
<p>编译结果中 <code>STATUS.LOADING</code> 会被直接替换为 3，其实完全模拟了 C 等语言中的枚举</p>
</li>
<li>
<p>使用 @constructor 标注函数为构造器，它仅能被实例化，而不可用作普通方法，<br>
<em>甚至是工厂方法</em>，CC 会确保构造器被合法使用，否则报错。这样确保开发者不必在 <em>运行时</em><br>
判断，构造器函数到底以怎样的形式被调用</p>
</li>
<li>
<p>在表达式中也可以使用 @type 来限定类型，这对于 JSON 特别有用，如</p>
<pre><code>var data = /** @type {UserModel} */({
    firstName : 'foo',
    lastName : 'bar'
});
</code></pre>
<p>在这里 UserModel 是个构造器，也可以使用 @typedef 来自定义复杂的数据类型</p>
</li>
</ul>
<h4>域可见性的模拟</h4>
<ul>
<li>
<p>使用 @private 标注私有域，私有域被外部引用会报错。开发者也可以按照“国际惯例”给私有域加上<br>
<code>_</code> 前缀或后缀，以提醒自己 / 协作者这是一个私有域，@private 注解用来告诉 CC；<br>
这样，开发者可以不必使用诸如老道的“模块模式”等技巧来真正地隐藏私有变量，将检查工作丢给<br>
CC，让开发尽可能朴实简单</p>
</li>
<li>
<p>类似有 @protect</p>
</li>
</ul>
<h4>类系统的模拟</h4>
<ul>
<li>
<p>使用 @extends 标注继承关系，继承体系会被优化</p>
</li>
<li>
<p>使用 @interface 标注接口，接口是类似 <code>function ThisIsAInterface(obj) {}</code><br>
的函数体为空的构造器定义，编译后将移除其相关代码。同时，标注 @implements 的构造器必须实现<br>
implemented 的接口的所有方法（正如其他 OO 语言一样），否则，CC 报错。这同样简化了接口 /<br>
实现的约束，靠 CC 来保证实现关系的可靠性</p>
</li>
</ul>
<h4>条件编译的模拟</h4>
<ul>
<li>使用 @define 标记状态开关，适用于调试 logger 等 开发 / 发布 状态需要分离的模式。<br>
可以在编译时指定参数来标识 define 参数的状态。这其实就是一个条件编译，真给力……</li>
</ul>
<h4>对象平坦化及属性名缩减</h4>
<ul>
<li>
<p>对象属性会被编译为单变量，比如 <code>foo.bar to foo$bar</code>，这种标记方法看起来很像 java<br>
中被编译出来的内部类~~之后 <code>foo$bar</code> 被进一步缩短。对象之所以能被平坦化是因为在<br>
js 中对象可以看做是一群引用 / 原始数据类型的容器</p>
</li>
<li>
<p>但是，js 对象实际上更复杂，所以被平坦化后会带来一些副作用，比如如果在对象（字面量）中使用<br>
this 指针，则编译后的结果会导致 this 指向错误。所以 Google 建议仅在 constructor 和<br>
prototype methods 中使用 this，这意味着，在所谓类单例（对象字面量）和类的静态方法（绑定到<br>
constructor 上的函数）中都避免使用 this 指针</p>
</li>
<li>
<p>在缩减对象属性 / 方法的名称长度时，有另外一个注意点，那就是必须始终使用 dot syntax（<code>.</code><br>
运算符），而不使用 quoted string（<code>[]</code> 运算符），除非索引名是一个变量。这是因为 CC<br>
<strong>始终不处理字符串中的内容</strong>，所以，<code>var o = { longName: 0 }; o["longName"]</code> 会被翻译为<br>
<code>var a = { b: 0 }; a["longName"]</code> 导致出错。实在想使用 quoted string，则在 <em>定义的时候</em><br>
也要使用 quoted string</p>
</li>
<li>
<p>对于全局变量，如果出现以 window.property 的形式引用的，必须始终定义为 window.porperty 形式：</p>
<pre><code>window.property = 1;
var property = 1; // wrong!
</code></pre>
<p>否则也会杯具，CC 可不会 window.property 翻译为 window.a</p>
</li>
</ul>
<h4>垃圾代码的移除</h4>
<ul>
<li>
<p>一个函数声明却未被调用时，默认地，声明体将被干掉</p>
</li>
<li>
<p>在这种机制下，如果一个方法是以 for in 的形式调用的，那么原方法也会被干掉，因为这种 <em>动态特征</em><br>
使得 CC 无法清楚方法是否确实在 for in 的时候被调用了</p>
</li>
<li>
<p>对于一些 unreachable 的代码，CC 将报警告</p>
</li>
<li>
<p>如果要产出一份被调用的公共接口，例如库，使用称作 <em>export</em> 的方法将函数导出，防止函数定义被<br>
CC 回收。具体的做法是将函数绑定到某个容器，比如：</p>
<pre><code>function displayNoteTitle(note) {
    alert(note['myTitle']);
}
// Store the function in a global property referenced by a string:
window['displayNoteTitle'] = displayNoteTitle;
</code></pre>
<p>对于需要 export 的函数，均使用 quoted string 风格</p>
</li>
</ul>
<h3>背后的思考</h3>
<p>根据以上高级模式优化的行为分析可知，CC 附加给开发者的约束主要有：</p>
<ol>
<li>
<p>强制以强类型的静态语言风格编写 js，将关注点从 <em>运行时的动态技巧</em> 转移到 组织代码、编写逻辑<br>
本身。</p>
<p>而可能由弱类型系统和动态特征产生的问题和风险则交给 CC，即通过开发者与 CC 达成一种<br>
编码约定 而规避掉</p>
</li>
<li>
<p>严格要求区分 <em>面向开发者的代码</em> 和 <em>面向机器的代码</em>。</p>
<p>虽然不像 C 等语言会编译产生目标代码，但是 CC 在一定程度上也生成了 <em>面向机器的 js</em>，<br>
包括压缩空白、缩减标识符、条件编译和冗余代码去除。</p>
<p>这和第一点其实是一脉相承的，同样要求开发者 <em>将关注点转移到开发本身</em></p>
</li>
<li>
<p>使用规范化的接口方式。</p>
<p>这不仅包括要求开发者使用恰当的 annotation（extend, interface, …），同时也给整个 OO-JS<br>
打下了一个框架，开发者必须使用同样的模式进行 OO 编码</p>
<p>另外，要求使用 export 技术统一导出公共接口更强化了这一点</p>
<p>总之，这一点进一步限定了开发者的编码风格，但是带来的好处是明显的：可读、可控、一致性</p>
</li>
</ol>
<p>曾经有读过 Closure Library 源码的同学评论道：</p>
<blockquote>
<p>  Google 根本不懂怎么写 javascript！代码里面各种冗余，并且充满了 java 的味道！</p>
</blockquote>
<p>当时确实也有这种感觉，比如 Google 把 <code>if(foo)</code> 写作 <code>if(foo != undefined)</code> 等等</p>
<p>Javascript 固然充满了丰富的动态特征，而且很多特性非常优雅，能够让代码简洁精悍，或者构造出一些<br>
令人惊叹 的技巧，但是也会产生一些副作用：</p>
<ul>
<li>
<p>首要的问题是可读性，静态的东西容易一目了然，动态的东西需要经过一番运算才能得出结论。<br>
比如 js 中的 极晚绑定，再比如 标识符运行时重写</p>
</li>
<li>
<p>其次的问题是执行性能。一个比较经典的众 js 工程师都在使用的技巧就是“<em>模拟函数重载</em>”——<br>
在函数体内判断 arguments 的特征，从而对应给出不同的逻辑。由于缺乏强类型，js 本身不能具备<br>
真正的重载，但是运行时的判断在带来灵活性的同时，必然会多出很多模拟重载的逻辑，降低性能</p>
</li>
</ul>
<p>在今年的 D2 大会上，<a href="http://hedgerwow.blogspot.com/">Hedger</a> 同学指出，大多数 js 开发者像是个 ninja（忍者），他们身怀绝技、<br>
神鬼莫测，单兵作战还可以，但是一旦碰到 army（军队，比如 Google 团队这样的 =，=）就是个悲剧</p>
<p>我比较欣赏这个比喻，大团队要良好地协作，必需遵循一定的规范和限制，优先保证可读性和一致性，与此同时<br>
失去的是奇技淫巧、自由灵活。所以采用何种编程风格、理念，需要具体问题具体分析…………</p>
<p>至少，目前 CC 提供了一个好的思路，它的高级模式推崇的编程风格也是很值得尝试、借鉴的</p>
<p>最后附上 CC 的常用命令选项……选项实在是有够多……</p>
<h3>CC 常用命令选项</h3>
<ul>
<li>–charset VAL 对所有文件定义的编码格式</li>
<li>–compilation<em>level [WHITESPACE</em>ONLY | SIMPLE<em>OPTIMIZATIONS | ADVANCED</em>OPTIMIZATIONS]<br>
设定编译级别</li>
<li>–debug 开启 debug 选项</li>
<li>–define (–D, -D) VAL 设定文件中使用 @define 标注的开关值，即条件编译</li>
<li>–externs VAL 编译代码需要调用未编译的代码时，使用它</li>
<li>–formatting [PRETTY<em>PRINT | PRINT</em>INPUT_DELIMITER] 格式化输出</li>
<li>–js VAL 输入文件，多指定多个，将会被合并</li>
<li>–js<em>output</em>file VAL 输出文件，如果不指定的话，直接输出到 standard output 流</li>
<li>–module VAL 定义模块</li>
<li>–output_manifest VAL 打印编译文件清单</li>
<li>–print_tree 打印语法分析树</li>
<li>–warning_level [QUIET | DEFAULT | VERBOSE] 设定报错模式</li>
</ul>