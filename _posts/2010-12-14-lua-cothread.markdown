---
layout: post
title:  "lua cothread"
date:   2010-12-14 08:05:14
author: 云风
categories: program
---

## lua cothread
### by 云风
### at 2010-12-14 08:05:14
### original <http://blog.codingnow.com/2010/12/lua_cothread.html>

<p>前段时间在玩 <a href="http://blog.codingnow.com/2010/11/go_prime.html">Go</a> ，非常喜欢 goroutine 的编程模型。采用 chan 进行 thread 间的通讯写起来很舒适。今天花了一个下午，为 lua 写了一个简单的库，模拟这种编程方式。暂且把这个东西叫作 lua cothread 。它基于 lua 的 coroutine ，只是写了个简单的调度器。</p>

<p>这个库有如下几个 api ：</p>

        <p>cothread.run(f,...)</p>

<p>启动一个函数 f ，放在调度器中。... 会被传入这个函数。这类似于 Go 的 go 关键字。</p>

<p>cothread.resume()</p>

<p>运行一个 tick 。这个库不同于 Go 的多线程模型。它是按 tick 一步步运行的。这适合于嵌入别的框架中。由框架调用一次 resume ，所有被 cothread 调度器管理的 thread 都运行一个 tick 。</p>

<p>resume 函数会返回当前活跃的 thread 数量。</p>

<p>cothread.sleep(ti)</p>

<p>当前 thread 休眠 ti 个 tick 。ti 可以为 0 ，但这可能会使得 resume 一直运行下去。</p>

<p>cothread.chan()</p>

<p>创建一个 chan （消息通道），不同于 Go ，这段玩具代码只有阻塞模式。当向 chan 写入时，thread 会阻塞到有另一个 thread 从这个 chan 中读取数据，或是把 chan 关闭。反之读操作亦然。</p>

<p>chan:read()</p>

<p>从 chan 中阻塞读取一个数据。如果 chan 被关闭，则返回 nil 。</p>

<p>chan:write()</p>

<p>向 chan 阻塞写一个数据，并返回 true 。如果 chan 被关闭，函数会返回 nil 。</p>

<p>cothread.select(tbl)</p>

<p>类似于 Go 的 select 。可以传入一张表。表中任何一项 chan 有数据可读时，便会触发对应的分支。表项可以填入 default 项，当没有任何备选数据时，将执行 default 项。若无 default 分支，select 将阻塞等待第一次的 chan 输入。具体使用见示例。</p>

<p><a href="http://blog.codingnow.com/cloud/LuaCothread">代码和示例可以看这里</a>。</p>