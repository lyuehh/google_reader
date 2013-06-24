---
layout: post
title:  "关于JavaScript 的事件[上]"
date:   2010-08-09 11:50:00
author: 钢钢
categories: program
---

## 关于JavaScript 的事件[上]
### by 钢钢
### at 2010-08-09 11:50:00
### original <http://www.cnblogs.com/xugang/archive/2010/08/09/1795623.html>

<p><a href="http://www.cnblogs.com/xugang/"><img src="http://pic.cnblogs.com/face/u24048.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/xugang/">钢钢</a> 发表于 2010-08-09 11:50 <a href="http://www.cnblogs.com/xugang/archive/2010/08/09/1795623.html">原文链接</a> 阅读: 707 评论: 6</p><p style="line-height:20px"> 
</p><p>
</p><p><font color="#0000ff" size="5"><strong>A  事件流（event  flow ）</strong></font></p>
<p><font color="#ff0000" size="4">事件模型分为两种：冒泡型事件、捕获型事件。</font></p>
<p style="line-height:20px"> 
</p><p>
</p><p><font size="4"><strong>冒泡型</strong>（dubbed  bubbling ）事件：指事件按照从最精确的对象到最不精确的对象的顺序逐一触发。</font></p>
<p><font size="4"><strong>捕获型</strong>（event  capturing ）事件：它与冒泡型事件相反，指事件按照从最不精确的对象到最精确的对象的顺序逐一触发。</font></p>
<p style="line-height:12px"> 
</p><p>
</p><p><a href="http://images.cnblogs.com/cnblogs_com/xugang/WindowsLiveWriter/JavaScript_9F4A/%C3%A5%C2%86%C2%92%C3%A6%C2%B3%C2%A1%C3%A4%C2%BA%C2%8B%C3%A4%C2%BB%C2%B6%C3%A5%C2%92%C2%8C%C3%A6%C2%8D%C2%95%C3%A8%C2%8E%C2%B7%C3%A4%C2%BA%C2%8B%C3%A4%C2%BB%C2%B6%C3%A7%C2%9A%C2%84%C3%A6%C2%89%C2%A7%C3%A8%C2%A1%C2%8C%C3%A9%C2%A1%C2%BA%C3%A5%C2%BA%C2%8F_2.png"><img title="冒泡事件和捕获事件的执行顺序" style="border:0px none;display:block;float:none;margin-left:auto;margin-right:auto" alt="冒泡事件和捕获事件的执行顺序" src="http://images.cnblogs.com/cnblogs_com/xugang/WindowsLiveWriter/JavaScript_9F4A/%E5%86%92%E6%B3%A1%E4%BA%8B%E4%BB%B6%E5%92%8C%E6%8D%95%E8%8E%B7%E4%BA%8B%E4%BB%B6%E7%9A%84%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F_thumb.png" width="443" border="0" height="344"></a></p>
<p style="line-height:12px"> 
</p><p>
</p><p><font color="#ff0000" size="4">捕获型事件也被称作自顶向下（DOM层次）的事件模型。</font></p>
<p><font color="#ff0000" size="4">由于IE 浏览器不支持捕获型事件，因此并没有被广泛应用。</font></p>
<p><font color="#ff0000" size="5"><strong></strong></font> </p>
<p><font color="#0000ff" size="5"><strong>B  事件监听</strong></font></p>
<p><font size="4"><strong>i &gt;  通用监听方法</strong></font></p>
<p>示例一：</p>
<div style="border:1px solid #999999;padding-left:18px;font-size:12px;width:700px;line-height:14px;font-family:courier new;background-color:#eeeeee">
<table style="border:0px none;padding:0px;width:100%" cellspacing="0">
    <tbody>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">p</span><span style="color:#ff0000"> onclick</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">alert('点击了');</span><span style="color:#0000ff">"</span><span style="color:#0000ff">&gt;</span><span style="color:#000000">Click</span><span style="color:#000000"> </span><span style="color:#000000">Me</span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/p</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
    </tbody>
</table>
</div>
<p style="line-height:14px"> 
</p><p>
</p><p>示例二：</p>
<div style="border:1px solid #999999;padding-left:18px;font-size:12px;width:700px;line-height:14px;font-family:courier new;background-color:#eeeeee">
<table style="border:0px none;padding:0px;width:100%" cellspacing="0">
    <tbody>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">html</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">head</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">title</span><span style="color:#0000ff">&gt;</span><span style="color:#000000"> </span><span style="color:#000000">demo</span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/title</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">meta</span><span style="color:#ff0000"> name</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">Author</span><span style="color:#0000ff">"</span><span style="color:#ff0000"> content</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">xugang</span><span style="color:#0000ff">"</span><span style="color:#8b0000"> </span><span style="color:#8b0000">/</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000;background-color:#ffff00">&lt;script type=&quot;text/javascript&quot;&gt;</span></td>
        </tr>
        <tr>
            <td> </td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">在onload</span><span style="color:#000000"> </span><span style="color:#000000">事件中添加所有标签的事件</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">window</span><span style="color:#000000">.</span><span style="color:#000000">onload</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000">()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">找到对象</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">var</span></strong><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">document</span><span style="color:#000000">.</span><strong><span style="color:#00008b">getElementById</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">myp</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">添加对象的onclick</span><span style="color:#000000"> </span><span style="color:#000000">事件</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><span style="color:#000000">onclick</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000">()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#00008b">alert</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">我被点击了</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000;background-color:#ffff00">&lt;/script&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/head</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">body</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">p</span><span style="color:#ff0000"> id</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">myp</span><span style="color:#0000ff">"</span><span style="color:#0000ff">&gt;</span><span style="color:#000000">Click</span><span style="color:#000000"> </span><span style="color:#000000">Me</span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/p</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/body</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/html</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
    </tbody>
</table>
</div>
<p>此代码实现了结构与行为的分离。</p>
<p><font color="#ff0000" size="5"></font> </p>
<p><font color="#ff0000" size="5">给浏览器添加监听方法，分为两种：IE 中的监听方法、标准DOM 的监听方法。</font></p>
<p style="line-height:12px"> 
</p><p>
</p><p><font size="4"><strong>ii &gt;  IE 中的监听方法</strong></font></p>
<p><font size="4">在IE 浏览器中，每个元素都有两个方法来处理事件的监听。分别是：<font color="#ff0000">attachEvent( )</font> 和 <font color="#ff0000">detachEvent( )</font> 。</font></p>
<p><font size="4">附加事件方法：[object].<font color="#ff0000">attachEvent(“事件名”，方法名);</font></font></p>
<p><font size="4">分离事件方法：[object].<font color="#ff0000">detachEvent(“事件名”，方法名);</font></font></p>
<p><font size="4">                    如：o_p.detachEvent(&quot;onclick&quot;,click_A);</font></p>
<p>示例：</p>
<div style="border:1px solid #999999;padding-left:18px;font-size:12px;width:700px;line-height:14px;font-family:courier new;background-color:#eeeeee">
<table style="border:0px none;padding:0px;width:100%" cellspacing="0">
    <tbody>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">html</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">head</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">title</span><span style="color:#0000ff">&gt;</span><span style="color:#000000"> </span><span style="color:#000000">demo</span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/title</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">meta</span><span style="color:#ff0000"> name</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">Author</span><span style="color:#0000ff">"</span><span style="color:#ff0000"> content</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">xugang</span><span style="color:#0000ff">"</span><span style="color:#8b0000"> </span><span style="color:#8b0000">/</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000;background-color:#ffff00">&lt;script type=&quot;text/javascript&quot;&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#008000">&lt;!--</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000"> </span><span style="color:#000000">click_A()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#00008b">alert</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">click_A</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000">删除监听函数</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><strong><span style="color:#00008b">detachEvent</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">onclick</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">,click_B)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td> </td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000"> </span><span style="color:#000000">click_B()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#00008b">alert</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">click_B<strong><span style="color:#008080">,</span></strong><strong><span style="color:#008080"> </span></strong><strong><span style="color:#008080">我只调用一次。</span></strong></span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td> </td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">var</span></strong><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">window</span><span style="color:#000000">.</span><span style="color:#000000">onload</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000">()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">document</span><span style="color:#000000">.</span><strong><span style="color:#00008b">getElementById</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">myP</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">添加监听函数</span><span style="color:#000000"> </span><span style="color:#000000">click_A</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><strong><span style="color:#00008b">attachEvent</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">onclick</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">,click_A)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">添加监听函数</span><span style="color:#000000"> </span><span style="color:#000000">click_B</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><strong><span style="color:#00008b">attachEvent</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">onclick</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">,click_B)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#008000">--&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000;background-color:#ffff00">&lt;/script&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/head</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">body</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">p</span><span style="color:#ff0000"> id</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">myP</span><span style="color:#0000ff">"</span><span style="color:#0000ff">&gt;</span><span style="color:#000000">Click</span><span style="color:#000000"> </span><span style="color:#000000">Me</span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/p</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/body</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/html</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
    </tbody>
</table>
</div>
<p style="line-height:15px"><font color="#ff0000">注意：</font></p>
<p style="line-height:15px"><font color="#ff0000">           ●  使用这种方式可以为同一元素添加多个监听函数；</font></p>
<p style="line-height:15px"><font color="#ff0000">           ●  在IE 浏览器中，函数的执行顺序与函数的添加顺序相反；</font></p>
<p style="line-height:15px"><font color="#ff0000">           ●  在IE 浏览器中，虽然函数有先后执行顺序，但都会<strong>同时调用</strong>；</font></p>
<p style="line-height:30px"> 
</p><p>
</p><p><font size="4"><strong>iii &gt;  标准DOM 的监听方法</strong></font></p>
<p><font size="4">在使用标准DOM 的浏览器中，每个元素也有两个方法来处理事件的监听。分别是：<font color="#ff0000">addEventListener( )</font> 和 <font color="#ff0000">removeEventListener( )</font> 。</font></p>
<p><font size="4">添加事件监听方法：[object].<font color="#ff0000"><font color="#ff0000">addEventListener</font>(“事件名”，方法名，事件模型 );</font></font></p>
<p><font size="4">移除事件监听方法：[object].<font color="#ff0000"><font color="#ff0000">removeEventListener</font>(“事件名”，方法名，事件模型 );</font></font></p>
<font color="#ff0000" size="4">注意：</font>
<p>  这里的“事件名”不能带 on ，如：click（如果是onclick 则错误！）</p>
<p>“事件模型”为boolean 类型，通常设置为 false ，即“冒泡型”事件。（如果是true 则为“捕获型”事件）</p>

<p>示例：</p>
<div style="border:1px solid #999999;padding-left:18px;font-size:12px;width:700px;line-height:14px;font-family:courier new;background-color:#eeeeee">
<table style="border:0px none;padding:0px;width:100%" cellspacing="0">
    <tbody>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">html</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">head</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">title</span><span style="color:#0000ff">&gt;</span><span style="color:#000000"> </span><span style="color:#000000">demo</span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/title</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">meta</span><span style="color:#ff0000"> name</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">Author</span><span style="color:#0000ff">"</span><span style="color:#ff0000"> content</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">xugang</span><span style="color:#0000ff">"</span><span style="color:#8b0000"> </span><span style="color:#8b0000">/</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000;background-color:#ffff00">&lt;script type=&quot;text/javascript&quot;&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#008000">&lt;!--</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000"> </span><span style="color:#000000">click_A()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#00008b">alert</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">click_A</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000">删除监听函数</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><span style="color:#000000">removeEventListener(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">click</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">,click_B,</span><strong><span style="color:#0000ff">false</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td> </td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000"> </span><span style="color:#000000">click_B()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#00008b">alert</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">被click_A删除, 一次都不能调用。</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td> </td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><strong><span style="color:#0000ff">var</span></strong><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">window</span><span style="color:#000000">.</span><span style="color:#000000">onload</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><strong><span style="color:#0000ff">function</span></strong><span style="color:#000000">()</span><span style="color:#000000">{</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000"> </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">document</span><span style="color:#000000">.</span><strong><span style="color:#00008b">getElementById</span></strong><span style="color:#000000">(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">myP</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">添加监听函数</span><span style="color:#000000"> </span><span style="color:#000000">click_A</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><span style="color:#000000">addEventListener(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">click</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">,click_A,</span><strong><span style="color:#0000ff">false</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#000000"> </span><span style="color:#000000">添加监听函数</span><span style="color:#000000"> </span><span style="color:#000000">click_B</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">o_p</span><span style="color:#000000">.</span><span style="color:#000000">addEventListener(</span><strong><span style="color:#008080">"</span></strong><strong><span style="color:#008080">click</span></strong><strong><span style="color:#008080">"</span></strong><span style="color:#000000">,click_B,</span><strong><span style="color:#0000ff">false</span></strong><span style="color:#000000">)</span><span style="color:#000000">;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000">    </span><span style="color:#000000"> </span><span style="color:#000000">}</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000">/</span><span style="color:#000000">/</span><span style="color:#008000">--&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#000000;background-color:#ffff00">&lt;/script&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/head</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">body</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#000000"> </span><span style="color:#000000"> </span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">p</span><span style="color:#ff0000"> id</span><span style="color:#8b0000">=</span><span style="color:#0000ff">"</span><span style="color:#0000ff">myP</span><span style="color:#0000ff">"</span><span style="color:#0000ff">&gt;</span><span style="color:#000000">Click</span><span style="color:#000000"> </span><span style="color:#000000">Me</span><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/p</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/body</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
        <tr>
            <td><span style="color:#0000ff">&lt;</span><span style="color:#8b0000">/html</span><span style="color:#0000ff">&gt;</span></td>
        </tr>
    </tbody>
</table>
</div>
<p>在Firefox 下运行通过，在IE 下报错。</p>
<p style="line-height:15px"><font color="#ff0000">注意：</font></p>
<p style="line-height:15px"><font color="#ff0000">           ●  使用这种方式同样可以为同一元素添加多个监听函数；</font></p>
<p style="line-height:15px"><font color="#ff0000">           ●  在Firefox 浏览器中，函数的执行顺序与函数的添加顺序一致（Firefox 与IE 正好相反）；</font></p>
<p style="line-height:15px"><font color="#ff0000">           ●  在Firefox 浏览器中，这种方式添加的函数是执行外一个再执行另一个（<strong>逐个执行</strong>）；</font></p>
<p style="line-height:36px"> </p>
<p style="font-size:20px;line-height:20px">下篇：<a title="关于JavaScript 的事件[下]" href="http://www.cnblogs.com/xugang/archive/2010/08/09/1795612.html">关于JavaScript 的事件[下]</a></p>
<p style="line-height:10px"> </p><img src="http://www.cnblogs.com/xugang/aggbug/1795623.html?type=1" width="1" height="1" alt=""><p>评论: 6　<a href="http://www.cnblogs.com/xugang/archive/2010/08/09/1795623.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/xugang/archive/2010/08/09/1795623.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/70373/">Android 3全速发力最早十月发布</a><span style="color:gray">(2010-08-09 20:38)</span><br>· <a href="http://news.cnblogs.com/n/70372/">甲骨文发布OEPE以支持Eclipse 3.6等工具</a><span style="color:gray">(2010-08-09 20:35)</span><br>· <a href="http://news.cnblogs.com/n/70371/">惠普临时CEO两大难题：业绩下滑 职位遭觊觎</a><span style="color:gray">(2010-08-09 20:32)</span><br>· <a href="http://news.cnblogs.com/n/70370/">云中谁寄锦书来, Bambook评测</a><span style="color:gray">(2010-08-09 20:14)</span><br>· <a href="http://news.cnblogs.com/n/70369/">Go语言近况</a><span style="color:gray">(2010-08-09 20:06)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/70369/">Go语言近况</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>