---
layout: post
title:  "Call/CC与Node.js"
date:   2011-10-31 20:22:20
author: 
categories: program
---

## Call/CC与Node.js
### by 
### at 2011-10-31 20:22:20
### original <http://blog.kghost.info/index.php/2011/10/callcc-and-node-js/>

<em>本文介绍一个使用大概10行代码, 实现一个类似老赵JSCEX<sup>[<a href="http://blog.kghost.info/#jscex">1</a>]</sup>的工具</em><br>
<br>
首先来看一下下面这段Node.js代码:<br>
<br>
<pre>var copy = function(src, dst) {
	var fd_src = fs.open(src, &quot;r&quot;);
	var fd_dst = fs.open(dst, &quot;w&quot;);
	while (true) {
		var buffer_size = 4096;
		var buffer = new Buffer(buffer_size);
		var bytes_read = fs.read(fd_src, buffer, 0, buffer_size);
		if (bytes_read &gt; 0) {
			var write_at = 0;
			while (write_at &lt; bytes_read) {
				write_at += fs.write(fd_dst, buffer, write_at, bytes_read - write_at);
			}
		} else {
			break;
		}
	}
};
</pre>
<br>
这段看起来像是一个文件拷贝的函数, 但是这个函数在Node里面是无法运行的, 因为Node的所有函数调用都必须是异步的, 这里用到的fs.open, fs.read, fs.write都必须修改为异步调用, 修改后可运行的代码如下:<br>
<br>
<pre>var copy = function(src, dst) {
	fs.open(src, &quot;r&quot;, undefined, function(err, fd_src) {
		fs.open(dst, &quot;w&quot;, undefined, function(err, fd_dst) {
			var copy_rec = function(position) {
				var buffer_size = 4096;
				var buffer = new Buffer(4096);
				fs.read(fd_src, buffer, 0, buffer_size, position, function(err, bytes_read) {
					if (bytes_read &gt; 0) {
						var write_rec = function(write_at) {
							if (write_at &lt; bytes_read) {
								fs.write(fd_dst, buffer, 0, bytes_read, position, function(err, written) {
									if (written &gt; 0) {
										write_rec(write_at + written);
									}
								});
							} else {
								copy_rec(position + bytes_read);
							}
						};
						write_rec(0);
					}
				});
			};
			copy_rec(0);
		});
	});
};
</pre>
<br>
看到这里, 我想第一次接触Node的同学就已经要崩溃了, 一个简单的文件拷贝函数竟然需要嵌套4~5层函数回调, 那么更复杂的比如web servlet岂不是要嵌套10多层函数回调了?<br>
<br>
难道没有方法让最上面的简洁明了的代码运行起来么? 下面就介绍一种方法, <b>不修改最开始那个简洁明了的拷贝函数, 让它直接跑起来, 并且不会阻塞整个JS引擎</b><br>
<br>
首先分析一下同步调用与异步调用的区别<br>
<pre>// sync
var fd = fs.read (name, mode);
// use fd


// async
fs.read (name, mode, function (fd) {
	// use fd
});
</pre>
<br>
仔细观察这两个调用, 会发现其实这就是一个CPS变换<sup>[<a href="http://blog.kghost.info/#callcc-and-node-js-n-2">2</a>]</sup>, 所以, 我们只需要做一个CPS变换的包装就可以实现同步的文件操作, 具体实现如下:<br>
<br>
<pre>var efs = {
	open: function (name, mode) {
		return call_cc(function (continuation) {
			fs.open(name, mode, undefined, function(err, fd_src) {
				continuation(fd_src);
			});
		});
	},
	read: function (fd, buffer, offset, length) {
		return call_cc(function (continuation) {
			fs.read(fd, buffer, offset, length, null, function(err, bytes_read) {
				continuation(bytes_read);
			});
		});
	},
	write: function (fd, buffer, offset, length) {
		return call_cc(function (continuation) {
			fs.write(fd, buffer, offset, length, null, function(err, written) {
				continuation(written);
			});
		});
	}
};
</pre>
<br>
这个实现的核心就是call/cc函数<sup>[<a href="http://blog.kghost.info/#callcc-and-node-js-n-3">3</a>]</sup>, 如果了解lisp或者scheme的同学可能会非常熟悉这个函数, 这个函数作用是把当前正在运行的线程挂起, 然后执行call/cc本体, call/cc函数<b>不会返回</b>, 直到主动调用参数传进来的那个回调函数(本例中continuation变量绑定的函数), call/cc函数的返回值就是传给回调函数的参数. call/cc函数确实很难理解, 下面举例说明一下:<br>
<br>
<pre>util.log("before call/cc");
var result = call_cc(function (continuation) {
	util.log("in call/cc before set timer");
	setTimeout(function () {
		util.log("before callback");
		continuation("hello world!");
		util.log("after callback");
	}, 1000);
	util.log("in call/cc after set timer");
});
util.log("after call/cc " + result);
</pre>
<br>
这段代码的输出为:<br>
<br>
<pre>31 Oct 13:01:33 - before call/cc
// 运行call/cc函数主体
31 Oct 13:01:33 - in call/cc before set timer
31 Oct 13:01:33 - in call/cc after set timer
// 休息1秒, 进入setTimeout的回调
31 Oct 13:01:34 - before callback
// 进入continuation函数, 调用continuation函数就相当于call/cc函数返回了"hello world!", 从call/cc返回开始继续执行
31 Oct 13:01:34 - after call/cc hello world!
// 执行setTimeout回调剩下部分
31 Oct 13:01:34 - after callback
</pre>
<br>
现在我们已经使用CPS转换得到了"同步"的文件处理函数, 虽然说看起来是同步的, 但是内部使用的是call/cc实现, 实际上是不会阻塞JS引擎的, 其他回调仍然能正常执行. 有了这些函数的支持, 本文开头给出的那个文件拷贝函数就能够<b>不作任何修改正常执行</b>, 现在一切问题都已经解决了, 唯一剩下的问题就是call/cc函数的实现, 标准的node.js引擎是没有call/cc函数的, 这里我们使用node-fibers<sup>[<a href="http://blog.kghost.info/#fiber">4</a>]</sup>实现call/cc函数.<br>
<br>
<pre>var call_cc = function(cont) {
	var fiber = Fiber.current;
	cont(function (a) {
		fiber.run(a);
	});
	return Fiber["yield"]();
};
</pre>
<br>
由于node-fiber的限制, 这个copy函数必须运行在fiber环境中, 完整代码如下:<br>
<br>
<pre>var fs = require(&quot;fs&quot;);
require(&quot;fibers&quot;);

var call_cc = function(cont) {
	var fiber = Fiber.current;
	cont(function (a) {
		fiber.run(a);
	});
	return Fiber[&quot;yield&quot;]();
};

var efs = {
	open: function (name, mode) {
		return call_cc(function (continuation) {
			fs.open(name, mode, undefined, function(err, fd_src) {
				continuation(fd_src);
			});
		});
	},
	read: function (fd, buffer, offset, length) {
		return call_cc(function (continuation) {
			fs.read(fd, buffer, offset, length, null, function(err, bytes_read) {
				continuation(bytes_read);
			});
		});
	},
	write: function (fd, buffer, offset, length) {
		return call_cc(function (continuation) {
			fs.write(fd, buffer, offset, length, null, function(err, written) {
				continuation(written);
			});
		});
	}
};

var copy = function(src, dst) {
	var fd_src = efs.open(src, &quot;r&quot;);
	var fd_dst = efs.open(dst, &quot;w&quot;);
	while (true) {
		var buffer_size = 4096;
		var buffer = new Buffer(buffer_size);
		var bytes_read = efs.read(fd_src, buffer, 0, buffer_size);
		if (bytes_read &gt; 0) {
			var write_at = 0;
			while (write_at &lt; bytes_read) {
				write_at += efs.write(fd_dst, buffer, write_at, bytes_read - write_at);
			}
		} else {
			break;
		}
	}
};

Fiber(function () {
	copy(&quot;src&quot;, &quot;dst&quot;);
}).run();
</pre>
<br>
<br>
最后一点空间, 我想简单评论一下老赵的JSCEX<sup>[<a href="http://blog.kghost.info/#jscex">1</a>]</sup>, 简单来说, 就是一点: <b>简直就是在重复发明轮子</b>.<br>
有关函数式编程, 在lisp界已经有40~50年的研究, 各个方面都已经有非常完善的体系, 当然这种异步同步函数之间互相转换早已经有体系化, 可证明的研究<sup>[<a href="http://blog.kghost.info/#callcc-and-node-js-n-5">5</a>]</sup>, 实现这样一个异步类库, 最好是从底层做起, 首先实现fiber<sup>[<a href="http://blog.kghost.info/#fiber">4</a>]</sup>或者shift/reset<sup>[<a href="http://blog.kghost.info/#callcc-and-node-js-n-6">6</a>]</sup>这样的底层函数, 然后再其之上, 构建一个异步框架. 像老赵这样, 一上来就搞JS再编译, 那是绕了十万八千里的远路, 不仅本身会引入bug, 也会给用户的调试造成很大困扰, 性能问题也无法保证.<br>
<br>
<ol>
	<li><strong><sup>[1]</sup></strong> JavaScript版本的AsyncEnumerator <a href="http://blog.zhaojie.me/2010/11/asynciterator-the-asyncenumerator-in-javascript.html">http://blog.zhaojie.me/2010/11/asynciterator-the-asyncenumerator-in-javascript.html</a> <a href="http://blog.kghost.info/#to-jscex">↩</a></li>
	<li><strong><sup>[2]</sup></strong> Continuation-passing style <a href="http://en.wikipedia.org/wiki/Continuation-passing_style">http://en.wikipedia.org/wiki/Continuation-passing_style</a> <a href="http://blog.kghost.info/#to-callcc-and-node-js-n-2">↩</a></li>
	<li><strong><sup>[3]</sup></strong> Call-with-current-continuation <a href="http://en.wikipedia.org/wiki/Call-with-current-continuation">http://en.wikipedia.org/wiki/Call-with-current-continuation</a> <a href="http://blog.kghost.info/#to-callcc-and-node-js-n-3">↩</a></li>
	<li><strong><sup>[4]</sup></strong> Fiber support for v8 and Node <a href="https://github.com/laverdet/node-fibers">https://github.com/laverdet/node-fibers</a> <a href="http://blog.kghost.info/#to-fiber">↩</a></li>
	<li><strong><sup>[5]</sup></strong> Olivier Danvy and Andrzej Filinski. 1990. Abstracting control. In Proceedings of the 1990 ACM conference on LISP and functional programming (LFP '90). ACM, New York, NY, USA, 151-160. DOI=10.1145/91556.91622 <a href="http://doi.acm.org/10.1145/91556.91622">http://doi.acm.org/10.1145/91556.91622</a> <a href="http://blog.kghost.info/#to-callcc-and-node-js-n-5">↩</a></li>
	<li><strong><sup>[6]</sup></strong> Tiark Rompf, Ingo Maier, and Martin Odersky. 2009. Implementing first-class polymorphic delimited continuations by a type-directed selective CPS-transform. In Proceedings of the 14th ACM SIGPLAN international conference on Functional programming (ICFP '09). ACM, New York, NY, USA, 317-328. DOI=10.1145/1596550.1596596 <a href="http://doi.acm.org/10.1145/1596550.1596596">http://doi.acm.org/10.1145/1596550.1596596</a> <a href="http://blog.kghost.info/#to-callcc-and-node-js-n-6">↩</a></li></ol>