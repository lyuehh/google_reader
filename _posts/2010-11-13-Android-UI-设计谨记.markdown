---
layout: post
title:  "Android UI 设计谨记"
date:   2010-11-13 11:21:55
author: admin
categories: program
---

## Android UI 设计谨记
### by admin
### at 2010-11-13 11:21:55
### original <http://ucdchina.com/snap/8441>

<p><img title="1" src="http://img.ucdchina.com/upload/snap/2010-11/0816943d8657ca9e22eaf37d1496b37c.png" alt="" width="635" height="260"></p>
 
<p style="text-indent:.3in;line-height:20.0pt"><span>Android的官方开发者博客发了一份幻灯片，介绍了一些Android UI设计的小贴士，Roger在这里以看图说话的形式发出来，有兴趣的读者就继续往下翻吧。整个PPT共分5个部分，Part I 翻译的是前两个部分。</span></p>
 
<p style="text-indent:.3in;line-height:20.0pt"><span>作为开发者，为啥我们要关心UI，前面的一堆通通可以忽略掉，直接跳到最后一条。因为好的UI设计可以帮助我们卖出更多拷贝，赚到更多钱^_^</span></p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>Don’t:</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt;不要照搬你在其它平台的UI设计，应该让用户感觉是在真正使用一个 Android 软件，在你的商标显示和平台整体观感之间做好平衡</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt;不要过度使用模态对话框</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt;不要使用固定的绝对定位的布局</p>
 
<p style="text-indent:.3in;line-height:20.0pt">4&gt;不要使用px单位，使用dp或者为文本使用sp</p>
 
<p style="text-indent:.3in;line-height:20.0pt">5&gt; 不要使用太小的字体</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>Do:</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 为高分辨率的屏幕创建资源（缩小总比放大好）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 需要点击的元素要够大</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 图标设计遵循 Android 的准则</p>
 
<p style="text-indent:.3in;line-height:20.0pt">4&gt; 使用适当的间距（margins, padding）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">5&gt; 支持D-pad和trackball导航</p>
 
<p style="text-indent:.3in;line-height:20.0pt">6&gt; 正确管理活动（activity）堆栈</p>
 
<p style="text-indent:.3in;line-height:20.0pt">7&gt; 正确处理屏幕方向变化</p>
 
<p style="text-indent:.3in;line-height:20.0pt">8&gt; 使用主题/样式，尺寸和颜色资源来减少多余的值</p>
 
<p style="text-indent:.3in;line-height:20.0pt">9&gt;和视觉交互设计师合作!!!</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>设计哲学:</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 干净而不过于简单</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 关注内容而非修饰</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 保存一致，让用户容易投入其中，可附加少许变化</p>
 
<p style="text-indent:.3in;line-height:20.0pt">4&gt; 使用云端服务（存储和同步用户资料）来加强用户体验</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>优秀界面的设计准则:</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 关注用户</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 显示正确的内容</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 给予用户适当的回馈</p>
 
<p style="text-indent:.3in;line-height:20.0pt">4&gt; 有章可循的行为模式</p>
 
<p style="text-indent:.3in;line-height:20.0pt">5&gt; 容忍错误</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>关注用户：</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 了解你的用户（年龄，技能，文化，对你的应用的需求，使用的设备，何时何地如何使用设备）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; ‘用户优先’的设计心态 （用户通常是任务导向的行为模式）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 更早，更频繁的由真实用户来测试</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>显示正确的内容：</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 最常用的操作需要最快被用户看到并且可用</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 不太常用的功能可以放到菜单里面</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>给予用户适当的回馈：</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 交互式的UI元素最少需要反映出4种不同的状态 （default，disabled，focused，pressed）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 保证操作的结果是清晰可见的</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 多给予用户进度提示，但是不要干扰他们当前的操作</p>
 
<p style="text-indent:.3in;line-height:20.0pt">有章可循的行为模式：</p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 行为模式遵循用户的期望（正确的操作活动堆栈，显示用户期望看到的信息和动作）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 使用合适的方式来加强功能可见性（可点击的元素就应该看起来是可以点击的）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 如果用户完成一项任务需要复杂的操作，重新思考你的设计!!!</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>容忍错误：</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt; 只允许有意义的操作（适当禁用一些按钮）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt; 尽量减少不可回退的操作</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 允许回退（undo）比使用确定对话框更好（实际上，应该尽量少用确定对话框，它对用户是一种干扰）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">如果错误是可能发生的，那它就一定会发生。</p>
 
<p style="text-indent:.3in;line-height:20.0pt"><strong>设计考量：</strong></p>
 
<p style="text-indent:.3in;line-height:20.0pt">1&gt;屏幕的物理尺寸</p>
 
<p style="text-indent:.3in;line-height:20.0pt">2&gt;屏幕密度</p>
 
<p style="text-indent:.3in;line-height:20.0pt">3&gt; 屏幕的方向（竖向和横向）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">4&gt;主要的UI交互方式（触屏还是使用D-pad/trackball）</p>
 
<p style="text-indent:.3in;line-height:20.0pt">5&gt;软键盘还是物理键盘</p>
 
<p style="text-indent:.3in;line-height:20.0pt">6&gt;了解不同设备之间的相异之处是非常重要的!</p>
 
<p style="text-indent:.3in;line-height:20.0pt">7&gt;阅读CDD，学习设备可能差异的地方</p>
 
<p style="text-indent:.3in;line-height:20.0pt">8&gt;了解屏幕尺寸和密度分类</p><p>相关话题：<a href="http://ucdchina.com/topic/322">移动设备的交互和设计</a> 源地址：<a href="http://www.ucd-grow01.com/?p=115">http://www.ucd-grow01.com/?p=115</a></p>