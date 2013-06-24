---
layout: post
title:  "剑走偏锋的 Native Client"
date:   2012-05-30 11:37:08
author: musiXboy
categories: program
---

## 剑走偏锋的 Native Client
### by musiXboy
### at 2012-05-30 11:37:08
### original <http://www.guao.hk/posts/cutting-edge-native-client.html>

<p>感谢读者 <a href="http://newhtml.net/">liuyanghejerry</a> 的投稿。</p>
<p>不知不觉，Google已经正式推出其Native Client (NaCl)过去约7个月之久。而目前国内似乎还没有多少关于NaCl的资料，所以在这里面向Web开发者做一下简单的介绍，希望能够起到一个抛砖引玉的效果。</p>
<p><em>本文的所有代码均来自于<a href="https://developers.google.com/native-client/devguide/tutorial">https://developers.google.com/native-client/devguide/tutorial</a>，如果您对其中的任何技术细节存在疑问，请以原文为准。</em></p>
<h4>何谓NaCl</h4>
<p>NaCl是一项能让C/C++代码运行在浏览器当中的技术。这是一个最通俗的说法，但不够精确。严格来说，NaCl技术在理论上能够实现任何编译型语言都在其之上运行。</p>
<p>但是目前由于技术上的原因，NaCl还做不到任何语言任何平台的编译型语言支持。</p>
<p>因为NaCl所憧憬的实际是LLVM技术，LLVM技术的要点即在于能将编译型语言转化为一个统一的中间语言，NaCl通过对这个中间语言的执行，即可达成任何编译型语言的运行。换句话说，NaCl实际上希望搭建一个虚拟机。</p>
<p>不过LLVM现在还不够成熟，NaCl不得不先使用GNU的编译套件，使用LLVM技术的版本被称为了PNaCl，目前还没有正式推出。同时也因为这个原因，ARM架构没能正式支持。</p>
<h4>为什么NaCl</h4>
<p>在开发层面上，NaCl希望解决一个问题：JavaScript的低效率。当然，从经营策略上来说，Google可能还希望籍此将桌面领域的成熟软件快速移植到其Chrome OS当中，不过这不是我们讨论的重点。</p>
<p>JavaScript毕竟是一门解释型语言，只有当浏览器执行到代码的时刻才能够看到代码，因此在执行优化上力度非常小。</p>
<p>但是Web应用发展至今，效率已经必须拿到桌面上考虑，如果你还想在浏览器里面看到更多优质的游戏的话。</p>
<h4>有关限制</h4>
<ul>
<li>缺乏能够切合的IDE</li>
<li>不支持硬件异常</li>
<li>不支持创建子进程</li>
<li>不支持原生TCP/UDP操作（但已提供了websocket支持）</li>
<li>不支持同步I/O</li>
<li>不支持内存剩余查询</li>
<li>内联汇编代码必须通过NaCl验证</li>
<li>NaCl的Pepper API必须通过主线程使用</li>
</ul>
<h4>如何使用</h4>
<p>上面那些对于行动主义来说其实都是P，真正需要聚焦的还是如何使用。</p>
<p>NaCl的典型项目由三个部分组成：</p>
<ul>
<li>网页（<code>*.html</code>）。这里所指的网页是一个泛指，它包括JS代码、CSS样式表已经HTML代码。</li>
<li>NaCl模块（<code>*.c;*.cc</code>）。这是C/C++代码的文件。</li>
<li>清单（<code>*.nmf</code>）。这份清单类似于Chrome Extension的清单，主要用于指明在不同架构的机器上调用什么模块。</li>
</ul>
<p>在真正开始之前，我们还需要安装一个NaCl的SDK。这个SDK当中主要包含了NaCl的模块编译工具链。大家可以从这里下载：<a href="https://developers.google.com/native-client/sdk/download">https://developers.google.com/native-client/sdk/download</a></p>
<p>安装之前，确保一下机器上有一个可用的Python 2.7，并加入到环境变量当中。</p>
<p>而SDK的安装则相当简单，只需要使用<code>naclsdk update</code>命令即可。</p>
<p>下面，创建一个名为hello_tutorial的目录，我们来搭建一个简单的demo。</p>
<p><span></span></p>
<h5>网页</h5>
<pre>&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;title&gt;hello_tutorial&lt;/title&gt;

  &lt;script type=&quot;text/javascript&quot;&gt;
    hello_tutorialModule = null;  // 模块的全局对象
    statusText = &#39;NO-STATUS&#39;;

    // 更新状态
    function moduleDidLoad() {
      hello_tutorialModule = document.getElementById(&#39;hello_tutorial&#39;);
      updateStatus(&#39;SUCCESS&#39;);
	  // 向模块发送消息
	  hello_tutorialModule.postMessage(&#39;hello&#39;);
    }

    // 消息句柄函数。句柄在NaCl模块发送相应消息时自动唤起。
	// 在C中是PPB_Messaging.PostMessage()，C++中则是pp::Instance.PostMessage()
	// 在这个demo当中，我们收到消息之后弹窗示意

    function handleMessage(message_event) {
      alert(message_event.data);
    }

    // 页面载入时很可能NaCl模块还没有载入，因此我们将状态写为正在读取；
	// 而如果模块已经载入，则什么都不做。
    function pageDidLoad() {
      if (hello_tutorialModule == null) {
        updateStatus(&#39;LOADING...&#39;);
      } else {
		// 事实上，NaCl模块的载入成功事件是不可能在页面载入成功事件之前就发生的，
		// 因此我们这里简单的认为页面载入之后所更新显示的消息仍旧是当前消息，而不是&#39;SUCCESS&#39;。
        updateStatus();
      }
    }

	// 设置状态。如果存在id为&#39;statusField&#39;的元素，那么将其设置为参数携带的状态
    function updateStatus(opt_message) {
      if (opt_message)
        statusText = opt_message;
      var statusField = document.getElementById(&#39;status_field&#39;);
      if (statusField) {
        statusField.innerHTML = statusText;
      }
    }
  &lt;/script&gt;
&lt;/head&gt;
&lt;body onload=&quot;pageDidLoad()&quot;&gt;

&lt;h1&gt;Native Client Module hello_tutorial&lt;/h1&gt;
&lt;p&gt;
  &lt;!-- 读取.nexe文件。通过.nmf，浏览器将结合目前所处的CPU架构来读取不同的模块文件。
  于此同时，&lt;embed&gt;元素之外的&lt;div&gt;还绑定着两个事件监听（&#39;load&#39;以及&#39;message&#39;），
  之所以绑定在div上，而不是embed之上，是为了确保模块在载入之前就可以将监听的绑定彻底完成，
  同时也确保了开发者可以在模块内部的初始化阶段调用API发送消息。
  --&gt;
  &lt;div id=&quot;listener&quot;&gt;
    &lt;script type=&quot;text/javascript&quot;&gt;
      var listener = document.getElementById(&#39;listener&#39;);
      listener.addEventListener(&#39;load&#39;, moduleDidLoad, true);
      listener.addEventListener(&#39;message&#39;, handleMessage, true);
    &lt;/script&gt;

    &lt;embed name=&quot;nacl_module&quot;
       id=&quot;hello_tutorial&quot;
       width=0 height=0
       src=&quot;hello_tutorial.nmf&quot;
       type=&quot;application/x-nacl&quot; /&gt;
  &lt;/div&gt;
&lt;/p&gt;

&lt;h2&gt;Status&lt;/h2&gt;
&lt;div id=&quot;status_field&quot;&gt;NO-STATUS&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
<h5>NaCl模块</h5>
<pre>/// @file hello_tutorial.cc
/// 载入NaCl模块时，浏览器首先将搜索CreateModule()方法，CreateModule()会返回一个对象实例，
/// 之后浏览器会调用该实例的CreateInstance()方法，这时浏览器每遇到一次相应的&lt;embed&gt;就会调用一次。
///
/// 浏览器通过Javascript的postMessage()函数与NaCl通信。
/// 当调用postMessage()时，浏览器会转而调用pp::Instance的HandleMessage()方法。
/// 而如果模块需要与外界主动通信，则是使用pp::Instance的postMessage()方法。
/// 注意，这两个postMessage()都是异步的，因此两者在调用之后迅速返回。

#include &lt;cstdio&gt;
#include &lt;string&gt;
#include &quot;ppapi/cpp/instance.h&quot;
#include &quot;ppapi/cpp/module.h&quot;
#include &quot;ppapi/cpp/var.h&quot;

namespace {
// 这个字符串用来判断消息是否是我们期望的内容
const char* const kHelloString = &quot;hello&quot;;
// 这个字符串用来向浏览器返回内容
const char* const kReplyString = &quot;hello from NaCl&quot;;
} // namespace

/// 每一个NaCl模块都将有一个相应的pp::Instance子类实例对应，
/// 为了与浏览器进行通信，你必须重载 HandleMessage()方法以及PostMessage()方法
class hello_tutorialInstance : public pp::Instance {
 public:

  explicit hello_tutorialInstance(PP_Instance instance) : pp::Instance(instance)
  {}
  virtual ~hello_tutorialInstance() {}

  /// HandleMessage() 负责接收浏览器中postMessage()发送的消息内容
  /// 其中的参数几乎可以表示任何东西，但通常都是JSON字符串，比如这样：
  ///   var json_message = JSON.stringify({ &quot;myMethod&quot; : &quot;3.14159&quot; });
  ///   nacl_module.postMessage(json_message);
  virtual void HandleMessage(const pp::Var&amp; var_message) {
    // 这里是处理消息的代码了
	if (!var_message.is_string())
		return;
	std::string message = var_message.AsString();
	pp::Var var_reply;
	if (message == kHelloString) {
		var_reply = pp::Var(kReplyString);
		PostMessage(var_reply);
	}
  }
};

class hello_tutorialModule : public pp::Module {
 public:
  hello_tutorialModule() : pp::Module() {}
  virtual ~hello_tutorialModule() {}

  virtual pp::Instance* CreateInstance(PP_Instance instance) {
    return new hello_tutorialInstance(instance);
  }
};

namespace pp {
Module* CreateModule() {
  return new hello_tutorialModule();
}
}  // namespace pp</pre>
<h5>Makefile</h5>
<p>Makefile是C\C++编译指令的存放文件，这份文件将指引编译器、链接器如何工作。因为我们现在所处情况特殊，所以Makefile需要自己编写。</p>
<pre># Copyright (c) 2012 The Native Client Authors. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

#
# GNU Make based build file.  For details on GNU Make see:
# http://www.gnu.org/software/make/manual/make.html
#

#
# Project information
#
# 这里的变量将指出每个项目的编译信息，如项目名、编译标志等等。
# 通常每个项目都将有所不同。这里只是作为一个demo，因此只有一些最简单的信息。
# 注意，编辑这里的内容需要你对编译器有足够的了解
#
PROJECT:=hello_tutorial
LDFLAGS:=-lppapi_cpp -lppapi
CXX_SOURCES:=$(PROJECT).cc

#
# 设置pepper目录
#
THIS_MAKEFILE:=$(abspath $(lastword $(MAKEFILE_LIST)))
NACL_SDK_ROOT?=$(abspath $(dir $(THIS_MAKEFILE))../..)

# Project Build flags
WARNINGS:=-Wno-long-long -Wall -Wswitch-enum -pedantic -Werror
CXXFLAGS:=-pthread -std=gnu++98 $(WARNINGS)

#
# 设置工具路径
#
#
OSNAME:=$(shell python $(NACL_SDK_ROOT)/tools/getos.py)
TC_PATH:=$(abspath $(NACL_SDK_ROOT)/toolchain/$(OSNAME)_x86_newlib)
CXX:=$(TC_PATH)/bin/i686-nacl-g++

#
# Cygwin专用设置
#
CYGWIN ?= nodosfilewarning
export CYGWIN

# Declare the ALL target first, to make the &#39;all&#39; target the default build
all: $(PROJECT)_x86_32.nexe $(PROJECT)_x86_64.nexe

# 32位文件生成规则
x86_32_OBJS:=$(patsubst %.cc,%_32.o,$(CXX_SOURCES))
$(x86_32_OBJS) : %_32.o : %.cc $(THIS_MAKE)
        $(CXX) -o $@ -c $&lt; -m32 -O0 -g $(CXXFLAGS)

$(PROJECT)_x86_32.nexe : $(x86_32_OBJS)
        $(CXX) -o $@ $^ -m32 -O0 -g $(CXXFLAGS) $(LDFLAGS)

# 64位文件生成规则
x86_64_OBJS:=$(patsubst %.cc,%_64.o,$(CXX_SOURCES))
$(x86_64_OBJS) : %_64.o : %.cc $(THIS_MAKE)
        $(CXX) -o $@ -c $&lt; -m64 -O0 -g $(CXXFLAGS)

$(PROJECT)_x86_64.nexe : $(x86_64_OBJS)
        $(CXX) -o $@ $^ -m64 -O0 -g $(CXXFLAGS) $(LDFLAGS)

# Define a phony rule so it always runs, to build nexe and start up server.
.PHONY: RUN
RUN: all
  python ../httpd.py</pre>
<h5>清单文件</h5>
<p>清单中指出了不同架构所使用的模块。</p>
<pre>{
  "program": {
    "x86-64": {"url": "hello_tutorial_x86_64.nexe"},
    "x86-32": {"url": "hello_tutorial_x86_32.nexe"}
  }
}</pre>
<p>所有这些文件齐全之后，只需要<code>make</code>即可完成自动编译。</p>
<h4>如何运行</h4>
<p>在编译之后，双击html页面打开其实并不会载入模块运行，这是由于浏览器的访问域规则不允许直接在用户的本地读取文件。因此，我们需要让本机成为一个服务器，以远程服务器的身份来读取模块最终传送给浏览器。</p>
<p>这很容易，SDK当中已经包含了提供http服务的python脚本：</p>
<pre>cd pepper_18/examples #注意，pepper的版本号应该依据你现有的情况来定
python httpd.py</pre>
<p>这样，<code>http://localhost:5103</code>就成为了我们的服务器地址。注意，http服务的根目录的位置位于examples目录。</p>
<p>现在，我们在浏览器的<code>chrome://flags</code>和<code>chrome://plugins</code>页面中打开NaCl的几个相关选项，就可以成功运行了。</p>
<p>Via <a href="http://newhtml.net/native-client/">NewHTML</a></p>
<hr>
<p><small>© musiXboy 发表于 <a href="http://www.guao.hk">谷奥——探寻谷歌的奥秘 ( http://www.guao.hk )</a>, 2012.  |
<a href="http://www.guao.hk/posts/cutting-edge-native-client.html#comments">4 条评论</a> |
<a href="http://www.guao.hk/posts/cutting-edge-native-client.html">永久链接</a> |
<a href="http://google.org.cn/about/">关于谷奥</a> |
<a href="http://google.org.cn/submit/">投稿/爆料</a><br>
Post tags: <a href="http://www.guao.hk/tag/chrome" rel="tag">Chrome</a>, <a href="http://www.guao.hk/tag/naci" rel="tag">NaCI</a>, <a href="http://www.guao.hk/tag/native-client" rel="tag">Native Client</a>
</small></p>
<img src="http://img.tongji.linezing.com/1105192/tongji.php" border="0" width="0" height="0">