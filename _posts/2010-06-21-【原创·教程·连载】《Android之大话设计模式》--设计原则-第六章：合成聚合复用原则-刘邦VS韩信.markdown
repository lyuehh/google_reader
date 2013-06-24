---
layout: post
title:  "【原创·教程·连载】《Android之大话设计模式》--设计原则 第六章：合成聚合复用原则 刘邦VS韩信"
date:   2010-06-21 00:08:00
author: 国士工作室
categories: program
---

## 【原创·教程·连载】《Android之大话设计模式》--设计原则 第六章：合成聚合复用原则 刘邦VS韩信
### by 国士工作室
### at 2010-06-21 00:08:00
### original <http://www.cnblogs.com/guoshiandroid/archive/2010/06/21/1761626.html>

<p><a href="http://www.cnblogs.com/guoshiandroid/"><img src="http://pic.cnblogs.com/face/u137019.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/guoshiandroid/">国士工作室</a> 发表于 2010-06-21 00:08 <a href="http://www.cnblogs.com/guoshiandroid/archive/2010/06/21/1761626.html">原文链接</a> 阅读: 827 评论: 4</p><h1 align="center" style="font-size:2em">&lt;大话设计模式&gt;</h1>
<h2 align="center" style="font-size:1.5em">本教程说明及版权声明</h2>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px">l 该文档参考和使用了网络上的免费开放的图片和内容，并以免费开放的方式发布,希望为移动互联网和智能手机时代贡献绵薄之力！可以随意转载，但不得使用该文档谋利。</p>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px">l 如果对该文档有任何疑问或者建议，请进入官方博客</p>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px">   <a href="http://www.cnblogs.com/guoshiandroid/" style="color:#1a8bc8;text-decoration:none">http://www.cnblogs.com/guoshiandroid/</a>留言或者直接与国士工作室联系（后附联系方式），我们会慎重参考您的建议并根据需要对本文档进行修改，以造福更多开发者！</p>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px">l 《大话设计模式》的最新及完整内容会在国士工作室官方博客定期更新，请访问国士工作室博客</p>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px"><a href="http://www.cnblogs.com/guoshiandroid/" style="color:#1a8bc8;text-decoration:none">http://www.cnblogs.com/guoshiandroid/</a>获取更多更新内容。</p>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px;text-align:center"> <strong> <span style="line-height:24px;font-size:12pt">查





看其他部分：</span><a href="http://www.cnblogs.com/guoshiandroid/archive/2010/06/06/1752668.html" style="color:#1a8bc8;text-decoration:none"><span style="line-height:28px;color:#0000ff"><span style="line-height:24px;font-size:12pt">本教程整体说明及章节索引</span></span></a></strong></p>
<p style="margin-top:5px;margin-right:auto;margin-bottom:5px;margin-left:auto;text-indent:0px;line-height:19px;text-align:center"><span style="font-size:14pt"><a href="http://files.cnblogs.com/guoshiandroid/%e5%90%88%e6%88%90%e8%81%9a%e5%90%88%e5%a4%8d%e7%94%a8%e5%8e%9f%e5%88%99_%e5%88%98%e9%82%a6VS%e9%9f%a9%e4%bf%a1.pdf" style="color:#1a8bc8;text-decoration:none">本文PDF下载链接</a></span></p>
<p align="center"><strong>合成聚合复用原则</strong><strong> </strong><strong>刘邦</strong><strong>VS</strong><strong>韩信</strong><strong> </strong></p>
<p><strong>应用场景举例</strong>：</p>
<p align="center"><img src="http://pic002.cnblogs.com/img/guoshiandroid/201006/2010062023385937.png">VS<img src="http://pic002.cnblogs.com/img/guoshiandroid/201006/2010062023395967.png"></p>
<p> 一次，刘邦闲着没事，又想起了故人韩信，于是打开QQ想和韩信聊天，正巧韩信也在，开始寒暄了几句，就在不自觉中进入严肃的问题，刘邦问：“韩兄觉得如果是我带兵，最多能够带多少呢？”，韩信立即回复说：“十万”，刘邦心想本王竟然只能带十万，那你韩信又能够带多少呢，于是暂时按捺了心中的郁闷，很客气的问道：“那请问韩兄最多能够带多少呢”，韩信又立即回复道：“我啊，那自然是越多越好啦”，刘邦看到此言顿时大怒，而且语气还极其的傲慢，还“啊，啦”的，刘邦心想：“虽然你韩信带兵打仗有道，天下皆知，但是这样对本王说话也太过分了吧”，刘邦正要发飙，随即停了一下，深谙世道的刘邦问了一句：“将军神勇盖世，带兵百万，却为何会在我领导下呢？”，刘邦想：“好你个韩信，叫了你几声韩兄你就不知道自己是谁了，如果回答不上来，或是回答不好，看我如何收拾你！”，等了大约三秒钟，QQ闪了一下，只见上面赫然写道：“陛下虽不善统兵，却善御将”。刘邦大悦！</p>
<p><strong>定义：</strong><strong> </strong></p>
<p>合成聚合复用原则（Composite Aggregate Reuse Principle,简称为CARP）经常又被人们称为合成复用原则（Composite Reuse Principle,简称为CRP）.合成聚合复用原则是指在一个新的对象中使用原来已经存在的一些对象，是这些原来已经存在的对象称为新对象的一部分，新的对象通过向这些原来已经具有的对象委派相应的动作或者命令达到复用已有功能的目的。</p>
<p>合成复用原则跟简洁的表述是：要尽量使用合成和聚合，尽量不要使用继承。</p>
<p>聚合（Aggregation）是关联关系的一种，用来表示一种整体和部分的拥有关系。整体持有对部分的引用，可以调用部分的能够被访问的方法和属性等，当然这种访问往往是对接口和抽象类的访问。作为部分可以可以同时被多个新的对象引用，同时为多个新的对象提供服务。</p>
<p>合成（Composition）也是关联关系的一种，但合成是一种比聚合强得多的一种关联关系。在合成关系里面，部分和整体的生命周期是一样的。作为整体的新对象完全拥有对作为部分的支配权，包括负责和支配部分的创建和销毁等，即要负责作为部分的内存的分配和内存释放等。从这里也可以看出来，一个合成关系中的成员对象是不能喝另外的一个合成关系共享的。</p>
<p>为何“要尽量使用合成和聚合，尽量不要使用继承”呢？这是因为：第一，继承复用破坏包装，它把超类的实现细节直接暴露给了子类，这违背了信息隐藏的原则；第二：如果超类发生了改变，那么子类也要发生相应的改变，这就直接导致了类与类之间的高耦合，不利于类的扩展、复用、维护等，也带来了系统僵硬和脆弱的设计。而是用合成和聚合的时候新对象和已有对象的交互往往是通过接口或者抽象类进行的，就可以很好的避免上面的不足，而且这也可以让每一个新的类专注于实现自己的任务，符合单一职责原则。</p>
<p>聚合关系的示意图如下所示：</p>
<p> </p>
<p style="text-align:center"><img src="http://pic002.cnblogs.com/img/guoshiandroid/201006/2010062023421694.png"></p>
<p>聚合关系的示意图如下所示：</p>
<p> </p>
<p style="text-align:center"><img src="http://pic002.cnblogs.com/img/guoshiandroid/201006/2010062023433785.png"></p>
<p> </p>
<p><strong>故事分析：</strong><strong> </strong></p>
<p>“韩信带兵，多多益善”，韩信之所以有很大的影响力，一方面归功他建立了很大的功勋；另外一方面是因为他手握重兵。建立很大的功勋甚至是功高盖主，这就直接导致韩信在军队中深得人心，毕竟，作为士兵，很少有不愿意追随一个在战场上无往不胜的领袖的；手握重兵，有合成聚合的原则来说就是对自己统领的士兵将士保持有引用，就是聚合。韩信手中聚合有一大批能征善战的人，随时听从韩信的调遣，这是不能不让刘邦戒备的。而现在，又说了一个自己带兵多多益善的话，这怎能不让刘邦感觉生气呢？</p>
<p>而“普天之下，莫非王土；四海之内，莫非王臣”，韩信拥有士兵，刘邦却拥有天下，何况韩信也并非拥有全部士兵。“君叫臣死，臣不得不死”，刘邦拥有对天下苍生的生杀大权。韩信很清楚，“陛下虽不善统兵，却善御将”。刘邦是拥有一种比韩信更强的“拥有”关系，可以主宰生死，同时也可以主宰韩信的生死。这就相当于合成关系。更重要的是韩信也是合成关系的一个成员，是刘邦的将领。所以说刘邦比韩信更加强悍！</p>
<p>如下图所示：</p>
<p style="text-align:center"><img src="http://pic002.cnblogs.com/img/guoshiandroid/201006/2010062023452460.png"></p>
<p style="text-align:center"> </p>
<p><strong>Java</strong><strong>代码实现：</strong><strong> </strong></p>
<p>       新建大臣的接口：</p>
<p> </p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP;</p>
<p align="left">/*</p>
<p align="left"> * 大臣的接口</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>interface</strong> Minister {</p>
<p align="left">    /*</p>
<p align="left">     * 大臣能够执行的动作</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> duty();</p>
<p align="left">}</p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p>       士兵的接口</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP;</p>
<p align="left">/*</p>
<p align="left"> * 士兵的接口</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>interface</strong> Soldier {</p>
<p align="left">    /*</p>
<p align="left">     * 士兵能够执行的动作</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> duty();</p>
<p align="left">}</p>
<p align="left"> </p>
<p align="left"> </p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p>       韩信对大臣接口的实现</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP.impl;</p>
<p align="left"> </p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Minister;</p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Soldier;</p>
<p align="left">/*</p>
<p align="left"> * 韩信对大臣接口的实现</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>class</strong> Hanxin <strong>implements</strong> Minister {</p>
<p align="left">    //对士兵的聚合关系</p>
<p align="left">    Soldier[] soldiers;</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 无参构造方法</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> Hanxin() {}</p>
<p align="left">    /*</p>
<p align="left">     * 有士兵参数的构造方法</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> Hanxin(Soldier[] soldiers) {</p>
<p align="left">        <strong>super</strong>();</p>
<p align="left">        <strong>this</strong>.soldiers = soldiers;</p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 获取士兵的集合</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> Soldier[] getSoldiers() {</p>
<p align="left">        <strong>return</strong> soldiers;</p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 设置士兵的集合</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> setSoldiers(Soldier[]  soldiers) {</p>
<p align="left">        <strong>this</strong>.soldiers = soldiers;</p>
<p align="left">    }</p>
<p align="left">    /*</p>
<p align="left">     * 韩信的职能</p>
<p align="left">     *  @see com.diermeng.designPattern.CARP.Minister#duty()</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> duty() {</p>
<p align="left">        System.<i>out</i>.println("我是刘邦的大臣，永远忠实于刘邦");</p>
<p align="left"> </p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">}</p>
<p align="left"> </p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p>       士兵A对士兵接口的实现</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP.impl;</p>
<p align="left"> </p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Soldier;</p>
<p align="left">/*</p>
<p align="left"> * 士兵A</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>class</strong> SoldierA <strong>implements</strong> Soldier {</p>
<p align="left">    /*</p>
<p align="left">     * 士兵A的职责</p>
<p align="left">     *  @see com.diermeng.designPattern.CARP.Soldier#duty()</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> duty() {</p>
<p align="left">        System.<i>out</i>.println("我是韩信的士兵A");</p>
<p align="left"> </p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">}</p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p>       士兵B对士兵接口的实现</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP.impl;</p>
<p align="left"> </p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Soldier;</p>
<p align="left">/*</p>
<p align="left"> * 士兵B</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>class</strong> SoldierB  <strong>implements</strong> Soldier {</p>
<p align="left">    /*</p>
<p align="left">     * 士兵B的职责</p>
<p align="left">     *  @see com.diermeng.designPattern.CARP.Soldier#duty()</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> duty() {</p>
<p align="left">        System.<i>out</i>.println("我是韩信的士兵B");</p>
<p align="left"> </p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">}</p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p>       刘邦类</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP.impl;</p>
<p align="left"> </p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Minister;</p>
<p align="left"> </p>
<p align="left">/*</p>
<p align="left"> * 刘邦类</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>class</strong> Liubang {</p>
<p align="left">    //拥有大臣</p>
<p align="left">    Minister[] minister;</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 无参构造方法</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> Liubang() {}</p>
<p align="left">    /*</p>
<p align="left">     * 把大臣的数组作为参数传入构造方法</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> Liubang(Minister[] minister) {</p>
<p align="left">        <strong>super</strong>();</p>
<p align="left">        <strong>this</strong>.minister = minister;</p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 获取大臣的集合</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> Minister[] getMinister() {</p>
<p align="left">        <strong>return</strong> minister;</p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 设置大臣的集合</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> setMinister(Minister[]  minister) {</p>
<p align="left">        <strong>this</strong>.minister = minister;</p>
<p align="left">    }</p>
<p align="left"> </p>
<p align="left">    /*</p>
<p align="left">     * 刘邦的职能</p>
<p align="left">     */</p>
<p align="left">    <strong>public</strong> <strong>void</strong> duty()</p>
<p align="left">    {</p>
<p align="left">        System.<i>out</i>.println("我是皇帝，普天之下，莫非王土；四海之内，莫非王臣");</p>
<p align="left">    }</p>
<p align="left">}</p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p> </p>
<p>建立一个测试类，代码如下：</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left"><strong>package</strong> com.diermeng.designPattern.CARP.client;</p>
<p align="left"> </p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Minister;</p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.Soldier;</p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.impl.Hanxin;</p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.impl.Liubang;</p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.impl.SoldierA;</p>
<p align="left"><strong>import</strong> com.diermeng.designPattern.CARP.impl.SoldierB;</p>
<p align="left"> </p>
<p align="left">/*</p>
<p align="left"> * 测试类的客户端</p>
<p align="left"> */</p>
<p align="left"><strong>public</strong> <strong>class</strong> CARPClient {</p>
<p align="left">    <strong>public</strong> <strong>static</strong> <strong>void</strong> main(String[] args)</p>
<p align="left">    {</p>
<p align="left">        //声明并实例化士兵A</p>
<p align="left">        Soldier  soldierA = <strong>new</strong> SoldierA();</p>
<p align="left">        //声明并实例化士兵B</p>
<p align="left">        Soldier  soldierB = <strong>new</strong> SoldierB();</p>
<p align="left">        //构造士兵数组</p>
<p align="left">        Soldier[]  soldiers = {soldierA,soldierB};</p>
<p align="left"> </p>
<p align="left">        //声明并实例化韩信，同时传入士兵数组</p>
<p align="left">        Minister  hanxin = <strong>new</strong> Hanxin(soldiers);</p>
<p align="left"> </p>
<p align="left">        //构造大臣数组</p>
<p align="left">        Minister[]  minister = {hanxin};</p>
<p align="left">        //声明并实例化刘邦，同时传入大臣数组</p>
<p align="left">        Liubang  liubang = <strong>new</strong> Liubang(minister);</p>
<p align="left"> </p>
<p align="left">        liubang.duty();</p>
<p align="left"> </p>
<p align="left">        //循环输出大臣</p>
<p align="left">        <strong>for</strong>(Minister  aminister:liubang.getMinister()){</p>
<p align="left">            aminister.duty();</p>
<p align="left">        }</p>
<p align="left"> </p>
<p align="left">    }</p>
<p align="left">}</p>
<p> </p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p>程序运行结果如下：</p>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="568" valign="top">
<p align="left">我是皇帝，普天之下，莫非王土；四海之内，莫非王臣</p>
<p align="left">我是刘邦的大臣，永远忠实于刘邦</p>
<p align="left"> </p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p><strong>已有应用简介：</strong><strong> </strong></p>
<p>       对于面向对象的软件系统而言，如何提高软件的可维护性和可复用性始终是一个核心问题。合成聚合复用原则的合理而充分的使用时非常有利于构建可维护、可复用、可扩展和灵活性好的软件系统。合成聚合复用原则作为一种构造优质系统的手段几乎可以应用到任何环境中去。对于已有的应用这里就不在赘述啦。</p>
<p><strong>温馨提示：</strong><strong> </strong></p>
<p>       合成聚合复用原则虽然几乎可以应用到任何环境中去，但是这个原则也有自己的缺点。因为此原则鼓励使用已有的类和对象来构建新的类的对象，这就导致了系统中会有很多的类和对象需要管理和维护，从而增加系统的复杂性。</p>
<p>       同时，也不是说在任何环境下使用合成聚合复用原则就是最好的，如果两个类之间在符合分类学的前提下有明显的“IS-A”的关系，而且基类能够抽象出子类的共有的属性和方法，而此时子类有能通过增加父类的属性和方法来扩展基类，那么此时使用继承将是一种更好的选择。</p>
<p> </p>
<p><span style="color:#ff0000">注意：该文档参考和使用了网络上的免费开放的图片和内容，并以免费开放的方式发布,希望为移动互联网和智能手机时代贡献绵薄之力！可以随意转载，但不得使用该文档谋<span style="color:#ff0000">利</span></span><span style="color:#ff0000">。</span></p><img src="http://www.cnblogs.com/guoshiandroid/aggbug/1761626.html?type=1" width="1" height="1" alt=""><p>评论: 4　<a href="http://www.cnblogs.com/guoshiandroid/archive/2010/06/21/1761626.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/guoshiandroid/archive/2010/06/21/1761626.html#commentform">发表评论</a></p><p><a href="http://a4.yeshj.com/rd/35721/">众里寻你千百度，百度期待您的加盟</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/66685/">微软发布iPhone版Windows Live Messenger</a><span style="color:gray">(2010-06-21 16:57)</span><br>· <a href="http://news.cnblogs.com/n/66683/">上海网民每天平均上网3.63小时</a><span style="color:gray">(2010-06-21 16:53)</span><br>· <a href="http://news.cnblogs.com/n/66682/">门户世界杯广告数据爆出 腾讯网广告量质超群</a><span style="color:gray">(2010-06-21 16:50)</span><br>· <a href="http://news.cnblogs.com/n/66681/">meego也疯狂：MeeGo下试玩QQ</a><span style="color:gray">(2010-06-21 16:47)</span><br>· <a href="http://news.cnblogs.com/n/66680/">Google改进VP8编解码器</a><span style="color:gray">(2010-06-21 16:26)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/topic/53/">[热点话题]编程语言之争</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>