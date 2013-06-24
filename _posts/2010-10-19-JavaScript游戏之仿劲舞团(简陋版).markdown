---
layout: post
title:  "JavaScript游戏之仿劲舞团(简陋版)"
date:   2010-10-19 15:58:00
author: Floyd
categories: program
---

## JavaScript游戏之仿劲舞团(简陋版)
### by Floyd
### at 2010-10-19 15:58:00
### original <http://www.cnblogs.com/floyd/archive/2010/10/19/1855599.html>

<p><p>突然发觉自己好喜欢写一些js小游戏，觉得蛮有意思。。。又可以娱乐，又可以学到东西。。。哈哈</p>
<p>这次带来的是仿劲舞团的小游戏。。。就是上下左右。。按后按空格。。。不过，没有人跳舞，也没有背景音乐。。。简陋版嘛</p>
<p>先来个预览玩玩吧：</p>
<p>

</p>
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_up.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_down.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_left.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_right.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_upbingo.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_downbingo.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_leftbingo.gif" alt="">
<img src="http://images.cnblogs.com/cnblogs_com/floyd/266003/o_rightbingo.gif" alt="">
<div></div>
<div>
<div>
<div>连击：<span style="font-weight:bolder">0</span></div>
<div style="margin-left:30px">
<div>时间：</div>
<div><span></span></div>
</div>
</div>
</div>
<p> </p>
<p> 1：Direction类，方向类，就是显示上下左右那些，主要负责生成对应的dom元素。</p>
<div><img src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt=""><img src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif"><span>Direction类</span>
<div>
<div><span style="color:#008080"> 1</span> <span style="color:#008000">//</span><span style="color:#008000">方向类，生成上下左右的方向Dom</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 2</span> <span style="color:#008000">//</span><span style="color:#008000">direction:方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 3</span> <span style="color:#008000"></span><span style="color:#0000ff">var</span><span style="color:#000000"> Direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(direction){<br>
</span><span style="color:#008080"> 4</span> <span style="color:#000000">    <br>
</span><span style="color:#008080"> 5</span> <span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 6</span> <span style="color:#000000">    <br>
</span><span style="color:#008080"> 7</span> <span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.init(direction);<br>
</span><span style="color:#008080"> 8</span> <span style="color:#000000">    <br>
</span><span style="color:#008080"> 9</span> <span style="color:#000000">    </span><span style="color:#0000ff">return</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom;<br>
</span><span style="color:#008080">10</span> <span style="color:#000000">}<br>
</span><span style="color:#008080">11</span> <span style="color:#000000">Direction.prototype </span><span style="color:#000000">=</span><span style="color:#000000"> {<br>
</span><span style="color:#008080">12</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">方向对应的dom元素的类名</span><span style="color:#008000"><br>
</span><span style="color:#008080">13</span> <span style="color:#008000"></span><span style="color:#000000">    directionMap : {<br>
</span><span style="color:#008080">14</span> <span style="color:#000000">        </span><span style="color:#000000">"</span><span style="color:#000000">up</span><span style="color:#000000">"</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">up</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080">15</span> <span style="color:#000000">        </span><span style="color:#000000">"</span><span style="color:#000000">down</span><span style="color:#000000">"</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">down</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080">16</span> <span style="color:#000000">        </span><span style="color:#000000">"</span><span style="color:#000000">left</span><span style="color:#000000">"</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">left</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080">17</span> <span style="color:#000000">        </span><span style="color:#000000">"</span><span style="color:#000000">right</span><span style="color:#000000">"</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">right</span><span style="color:#000000">"</span><span style="color:#000000"><br>
</span><span style="color:#008080">18</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">19</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">初始化函数</span><span style="color:#008000"><br>
</span><span style="color:#008080">20</span> <span style="color:#008000"></span><span style="color:#000000">    init : </span><span style="color:#0000ff">function</span><span style="color:#000000">(direction){<br>
</span><span style="color:#008080">21</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> b </span><span style="color:#000000">=</span><span style="color:#000000"> document.createElement(</span><span style="color:#000000">'</span><span style="color:#000000">b</span><span style="color:#000000">'</span><span style="color:#000000">);<br>
</span><span style="color:#008080">22</span> <span style="color:#000000">        b.className </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.directionMap[direction];<br>
</span><span style="color:#008080">23</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">为方向dom元素扩展一个属性与方法</span><span style="color:#008000"><br>
</span><span style="color:#008080">24</span> <span style="color:#008000"></span><span style="color:#000000">        b.direction </span><span style="color:#000000">=</span><span style="color:#000000"> direction;<br>
</span><span style="color:#008080">25</span> <span style="color:#000000">        b.bingo </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.bingo;<br>
</span><span style="color:#008080">26</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">27</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom </span><span style="color:#000000">=</span><span style="color:#000000"> b;<br>
</span><span style="color:#008080">28</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">29</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">命中之后事件</span><span style="color:#008000"><br>
</span><span style="color:#008080">30</span> <span style="color:#008000"></span><span style="color:#000000">    bingo : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">31</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.className </span><span style="color:#000000">+=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">bingo</span><span style="color:#000000">'</span><span style="color:#000000">;<br>
</span><span style="color:#008080">32</span> <span style="color:#000000">    }<br>
</span><span style="color:#008080">33</span> <span style="color:#000000">}</span></div>
</div>
</div>
<p> </p>
<p> </p>
<p> 2：Panel类，背景类，就是游戏区，主要处理方向的生成与键盘事件，还有连击数。</p>
<div><img src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt=""><img src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif"><span>背景类</span>
<div>
<div><span style="color:#008080">  1</span> <span style="color:#008000">//</span><span style="color:#008000">背景类，处理方向的生成与键盘事件</span><span style="color:#008000"><br>
</span><span style="color:#008080">  2</span> <span style="color:#008000"></span><span style="color:#0000ff">var</span><span style="color:#000000"> Panel </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">  3</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">背景dom元素</span><span style="color:#008000"><br>
</span><span style="color:#008080">  4</span> <span style="color:#008000"></span><span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000">;<br>
</span><span style="color:#008080">  5</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">生成的方向类集合</span><span style="color:#008000"><br>
</span><span style="color:#008080">  6</span> <span style="color:#008000"></span><span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.directionList </span><span style="color:#000000">=</span><span style="color:#000000"> [];<br>
</span><span style="color:#008080">  7</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">目前输入到第几个方向</span><span style="color:#008000"><br>
</span><span style="color:#008080">  8</span> <span style="color:#008000"></span><span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000">;<br>
</span><span style="color:#008080">  9</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">连击数</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 10</span> <span style="color:#008000"></span><span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.actcount </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 11</span> <span style="color:#000000">    <br>
</span><span style="color:#008080"> 12</span> <span style="color:#000000">    </span><span style="color:#0000ff">this</span><span style="color:#000000">.init();<br>
</span><span style="color:#008080"> 13</span> <span style="color:#000000">}<br>
</span><span style="color:#008080"> 14</span> <span style="color:#000000">Panel.prototype </span><span style="color:#000000">=</span><span style="color:#000000"> {<br>
</span><span style="color:#008080"> 15</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">数字对应方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 16</span> <span style="color:#008000"></span><span style="color:#000000">    map : {<br>
</span><span style="color:#008080"> 17</span> <span style="color:#000000">        </span><span style="color:#000000">1</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">up</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 18</span> <span style="color:#000000">        </span><span style="color:#000000">2</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">down</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 19</span> <span style="color:#000000">        </span><span style="color:#000000">3</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">left</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 20</span> <span style="color:#000000">        </span><span style="color:#000000">4</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">right</span><span style="color:#000000">"</span><span style="color:#000000"><br>
</span><span style="color:#008080"> 21</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 22</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">每次出现多少个方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 23</span> <span style="color:#008000"></span><span style="color:#000000">    count : </span><span style="color:#000000">4</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 24</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">初始化</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 25</span> <span style="color:#008000"></span><span style="color:#000000">    init : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080"> 26</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 27</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> _this </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 28</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 29</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom </span><span style="color:#000000">=</span><span style="color:#000000"> document.getElementById(</span><span style="color:#000000">'</span><span style="color:#000000">panel</span><span style="color:#000000">'</span><span style="color:#000000">);<br>
</span><span style="color:#008080"> 30</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 31</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom.focus();<br>
</span><span style="color:#008080"> 32</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 33</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom.onkeydown </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(e){_this.keydown(e);};<br>
</span><span style="color:#008080"> 34</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 35</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">显示所有方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 36</span> <span style="color:#008000"></span><span style="color:#000000">    showDirection : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080"> 37</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 38</span> <span style="color:#000000">        </span><span style="color:#0000ff">for</span><span style="color:#000000">(</span><span style="color:#0000ff">var</span><span style="color:#000000"> i</span><span style="color:#000000">=</span><span style="color:#000000">1</span><span style="color:#000000">;i </span><span style="color:#000000">&lt;=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.count;i</span><span style="color:#000000">++</span><span style="color:#000000">){<br>
</span><span style="color:#008080"> 39</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.add();<br>
</span><span style="color:#008080"> 40</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080"> 41</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 42</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">清楚所有方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 43</span> <span style="color:#008000"></span><span style="color:#000000">    clearDirection : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080"> 44</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 45</span> <span style="color:#000000">        </span><span style="color:#0000ff">for</span><span style="color:#000000">(</span><span style="color:#0000ff">var</span><span style="color:#000000"> i</span><span style="color:#000000">=</span><span style="color:#000000">0</span><span style="color:#000000">,l</span><span style="color:#000000">=</span><span style="color:#0000ff">this</span><span style="color:#000000">.directionList.length;i </span><span style="color:#000000">&lt;</span><span style="color:#000000"> l;i</span><span style="color:#000000">++</span><span style="color:#000000">){<br>
</span><span style="color:#008080"> 46</span> <span style="color:#000000">            </span><span style="color:#0000ff">var</span><span style="color:#000000"> temp </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.directionList.pop();<br>
</span><span style="color:#008080"> 47</span> <span style="color:#000000">            <br>
</span><span style="color:#008080"> 48</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom.removeChild(temp);<br>
</span><span style="color:#008080"> 49</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080"> 50</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 51</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">添加一个方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 52</span> <span style="color:#008000"></span><span style="color:#000000">    add : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080"> 53</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 54</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> random </span><span style="color:#000000">=</span><span style="color:#000000"> parseInt(Math.random()</span><span style="color:#000000">*</span><span style="color:#000000">4</span><span style="color:#000000">+</span><span style="color:#000000">1</span><span style="color:#000000">,</span><span style="color:#000000">10</span><span style="color:#000000">);<br>
</span><span style="color:#008080"> 55</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 56</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> dir </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">new</span><span style="color:#000000"> Direction(</span><span style="color:#0000ff">this</span><span style="color:#000000">.map[random]);<br>
</span><span style="color:#008080"> 57</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 58</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.directionList.push(dir);<br>
</span><span style="color:#008080"> 59</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.dom.appendChild(dir);<br>
</span><span style="color:#008080"> 60</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 61</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">按键事件</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 62</span> <span style="color:#008000"></span><span style="color:#000000">    keydown : </span><span style="color:#0000ff">function</span><span style="color:#000000">(e){<br>
</span><span style="color:#008080"> 63</span> <span style="color:#000000">        e </span><span style="color:#000000">=</span><span style="color:#000000"> e</span><span style="color:#000000">||</span><span style="color:#000000"> window.event;<br>
</span><span style="color:#008080"> 64</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">阻止浏览器默认事件</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 65</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">if</span><span style="color:#000000">(e.keyCode </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#000000">32</span><span style="color:#000000"> </span><span style="color:#000000">||</span><span style="color:#000000"> e.keyCode </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#000000">37</span><span style="color:#000000"> </span><span style="color:#000000">||</span><span style="color:#000000"> e.keyCode </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#000000">38</span><span style="color:#000000"> </span><span style="color:#000000">||</span><span style="color:#000000"> e.keyCode </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#000000">39</span><span style="color:#000000"> </span><span style="color:#000000">||</span><span style="color:#000000"> e.keyCode </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#000000">40</span><span style="color:#000000">){<br>
</span><span style="color:#008080"> 66</span> <span style="color:#000000">            </span><span style="color:#0000ff">if</span><span style="color:#000000">(e.preventDefault)e.preventDefault();<br>
</span><span style="color:#008080"> 67</span> <span style="color:#000000">            </span><span style="color:#0000ff">else</span><span style="color:#000000"> e.returnValue </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">false</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 68</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080"> 69</span> <span style="color:#000000">        </span><span style="color:#0000ff">else</span><span style="color:#000000"> </span><span style="color:#0000ff">return</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 70</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 71</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> direction;<br>
</span><span style="color:#008080"> 72</span> <span style="color:#000000">        </span><span style="color:#0000ff">switch</span><span style="color:#000000">(e.keyCode){<br>
</span><span style="color:#008080"> 73</span> <span style="color:#000000">            </span><span style="color:#0000ff">case</span><span style="color:#000000"> </span><span style="color:#000000">32</span><span style="color:#000000">:direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">space</span><span style="color:#000000">'</span><span style="color:#000000">;</span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum</span><span style="color:#000000">--</span><span style="color:#000000">;</span><span style="color:#0000ff">break</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 74</span> <span style="color:#000000">            </span><span style="color:#0000ff">case</span><span style="color:#000000"> </span><span style="color:#000000">37</span><span style="color:#000000">:direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">left</span><span style="color:#000000">'</span><span style="color:#000000">;</span><span style="color:#0000ff">break</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 75</span> <span style="color:#000000">            </span><span style="color:#0000ff">case</span><span style="color:#000000"> </span><span style="color:#000000">38</span><span style="color:#000000">:direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">up</span><span style="color:#000000">'</span><span style="color:#000000">;</span><span style="color:#0000ff">break</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 76</span> <span style="color:#000000">            </span><span style="color:#0000ff">case</span><span style="color:#000000"> </span><span style="color:#000000">39</span><span style="color:#000000">:direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">right</span><span style="color:#000000">'</span><span style="color:#000000">;</span><span style="color:#0000ff">break</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 77</span> <span style="color:#000000">            </span><span style="color:#0000ff">case</span><span style="color:#000000"> </span><span style="color:#000000">40</span><span style="color:#000000">:direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">down</span><span style="color:#000000">'</span><span style="color:#000000">;</span><span style="color:#0000ff">break</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 78</span> <span style="color:#000000">            </span><span style="color:#0000ff">default</span><span style="color:#000000">:direction </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000">;</span><span style="color:#0000ff">break</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 79</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080"> 80</span> <span style="color:#000000">        <br>
</span><span style="color:#008080"> 81</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum</span><span style="color:#000000">++</span><span style="color:#000000">;<br>
</span><span style="color:#008080"> 82</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.checkInput(direction);<br>
</span><span style="color:#008080"> 83</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 84</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">检测用户输入方向</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 85</span> <span style="color:#008000"></span><span style="color:#000000">    checkInput : </span><span style="color:#0000ff">function</span><span style="color:#000000">(direction){<br>
</span><span style="color:#008080"> 86</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">检测是否输入完成</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 87</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">if</span><span style="color:#000000"> (direction </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">space</span><span style="color:#000000">'</span><span style="color:#000000"> </span><span style="color:#000000">&amp;&amp;</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum </span><span style="color:#000000">==</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.directionList.length) {<br>
</span><span style="color:#008080"> 88</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.finish();<br>
</span><span style="color:#008080"> 89</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080"> 90</span> <span style="color:#000000">        </span><span style="color:#0000ff">else</span><span style="color:#000000"> {<br>
</span><span style="color:#008080"> 91</span> <span style="color:#000000">            </span><span style="color:#0000ff">var</span><span style="color:#000000"> dir </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.directionList[</span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum </span><span style="color:#000000">-</span><span style="color:#000000"> </span><span style="color:#000000">1</span><span style="color:#000000">];<br>
</span><span style="color:#008080"> 92</span> <span style="color:#000000">            </span><span style="color:#008000">//</span><span style="color:#008000">检测是否按键正确</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 93</span> <span style="color:#008000"></span><span style="color:#000000">            </span><span style="color:#0000ff">if</span><span style="color:#000000"> (</span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum </span><span style="color:#000000">&gt;</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000"> </span><span style="color:#000000">&amp;&amp;</span><span style="color:#000000"> direction </span><span style="color:#000000">!=</span><span style="color:#000000"> </span><span style="color:#0000ff">null</span><span style="color:#000000"> </span><span style="color:#000000">&amp;&amp;</span><span style="color:#000000"> dir.direction </span><span style="color:#000000">==</span><span style="color:#000000"> direction) {<br>
</span><span style="color:#008080"> 94</span> <span style="color:#000000">                dir.bingo();<br>
</span><span style="color:#008080"> 95</span> <span style="color:#000000">            }<br>
</span><span style="color:#008080"> 96</span> <span style="color:#000000">            </span><span style="color:#0000ff">else</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.lose();<br>
</span><span style="color:#008080"> 97</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080"> 98</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080"> 99</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">完成一轮方向输入</span><span style="color:#008000"><br>
</span><span style="color:#008080">100</span> <span style="color:#008000"></span><span style="color:#000000">    finish : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">101</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">102</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.actcount </span><span style="color:#000000">+=</span><span style="color:#000000"> </span><span style="color:#000000">1</span><span style="color:#000000">;<br>
</span><span style="color:#008080">103</span> <span style="color:#000000">        document.getElementById(</span><span style="color:#000000">'</span><span style="color:#000000">actcount</span><span style="color:#000000">'</span><span style="color:#000000">).innerHTML </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.actcount;<br>
</span><span style="color:#008080">104</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.actEffor();<br>
</span><span style="color:#008080">105</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">随机下轮出现的方向数</span><span style="color:#008000"><br>
</span><span style="color:#008080">106</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.count </span><span style="color:#000000">=</span><span style="color:#000000"> parseInt(Math.random()</span><span style="color:#000000">*</span><span style="color:#000000">4</span><span style="color:#000000">+</span><span style="color:#000000">5</span><span style="color:#000000">,</span><span style="color:#000000">10</span><span style="color:#000000">);<br>
</span><span style="color:#008080">107</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">重置</span><span style="color:#008000"><br>
</span><span style="color:#008080">108</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.reset();<br>
</span><span style="color:#008080">109</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">显示方向</span><span style="color:#008000"><br>
</span><span style="color:#008080">110</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.showDirection();<br>
</span><span style="color:#008080">111</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">清理一些手尾</span><span style="color:#008000"><br>
</span><span style="color:#008080">112</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.onend();<br>
</span><span style="color:#008080">113</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">114</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">用户输入错误或者时间到</span><span style="color:#008000"><br>
</span><span style="color:#008080">115</span> <span style="color:#008000"></span><span style="color:#000000">    lose : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">116</span> <span style="color:#000000">        alert(</span><span style="color:#000000">'</span><span style="color:#000000">您输了，战绩为：</span><span style="color:#000000">'</span><span style="color:#000000">+</span><span style="color:#0000ff">this</span><span style="color:#000000">.actcount</span><span style="color:#000000">+</span><span style="color:#000000">"</span><span style="color:#000000">连击</span><span style="color:#000000">"</span><span style="color:#000000">);<br>
</span><span style="color:#008080">117</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">118</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.count </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">4</span><span style="color:#000000">;<br>
</span><span style="color:#008080">119</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.actcount </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000">;<br>
</span><span style="color:#008080">120</span> <span style="color:#000000">        document.getElementById(</span><span style="color:#000000">'</span><span style="color:#000000">actcount</span><span style="color:#000000">'</span><span style="color:#000000">).innerHTML </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.actcount;<br>
</span><span style="color:#008080">121</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.reset();<br>
</span><span style="color:#008080">122</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">123</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.onend();<br>
</span><span style="color:#008080">124</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.onlose();<br>
</span><span style="color:#008080">125</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">126</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">用户正确输入一轮方向后，连击数的效果</span><span style="color:#008000"><br>
</span><span style="color:#008080">127</span> <span style="color:#008000"></span><span style="color:#000000">    actEffor : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">128</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">129</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> flag </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000">,<br>
</span><span style="color:#008080">130</span> <span style="color:#000000">        colorMap </span><span style="color:#000000">=</span><span style="color:#000000"> {<br>
</span><span style="color:#008080">131</span> <span style="color:#000000">            </span><span style="color:#000000">0</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">Red</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080">132</span> <span style="color:#000000">            </span><span style="color:#000000">1</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">Blue</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080">133</span> <span style="color:#000000">            </span><span style="color:#000000">2</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">Orange</span><span style="color:#000000">"</span><span style="color:#000000">,<br>
</span><span style="color:#008080">134</span> <span style="color:#000000">            </span><span style="color:#000000">3</span><span style="color:#000000">:</span><span style="color:#000000">"</span><span style="color:#000000">White</span><span style="color:#000000">"</span><span style="color:#000000"><br>
</span><span style="color:#008080">135</span> <span style="color:#000000">        };<br>
</span><span style="color:#008080">136</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">137</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> process </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">138</span> <span style="color:#000000">            <br>
</span><span style="color:#008080">139</span> <span style="color:#000000">            document.getElementById(</span><span style="color:#000000">'</span><span style="color:#000000">actcount</span><span style="color:#000000">'</span><span style="color:#000000">).style.color </span><span style="color:#000000">=</span><span style="color:#000000"> colorMap[flag];<br>
</span><span style="color:#008080">140</span> <span style="color:#000000">            flag</span><span style="color:#000000">++</span><span style="color:#000000">;<br>
</span><span style="color:#008080">141</span> <span style="color:#000000">            <br>
</span><span style="color:#008080">142</span> <span style="color:#000000">            </span><span style="color:#0000ff">if</span><span style="color:#000000">(flag </span><span style="color:#000000">&lt;=</span><span style="color:#000000"> </span><span style="color:#000000">3</span><span style="color:#000000">){<br>
</span><span style="color:#008080">143</span> <span style="color:#000000">                setTimeout(process,</span><span style="color:#000000">100</span><span style="color:#000000">);<br>
</span><span style="color:#008080">144</span> <span style="color:#000000">            }<br>
</span><span style="color:#008080">145</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080">146</span> <span style="color:#000000">        process();<br>
</span><span style="color:#008080">147</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">148</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">重置</span><span style="color:#008000"><br>
</span><span style="color:#008080">149</span> <span style="color:#008000"></span><span style="color:#000000">    reset : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">150</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">151</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.clearDirection();<br>
</span><span style="color:#008080">152</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.inputNum </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000">;<br>
</span><span style="color:#008080">153</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">154</span> <span style="color:#000000">    onend : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){},<br>
</span><span style="color:#008080">155</span> <span style="color:#000000">    onlose: </span><span style="color:#0000ff">function</span><span style="color:#000000">(){}<br>
</span><span style="color:#008080">156</span> <span style="color:#000000">}</span></div>
</div>
</div>
<p> </p>
<p> </p>
<p> 3：Game类，游戏控制类，控制游戏的开始结束，以及倒计时等。 </p>
<div><img src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt=""><img src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif"><span>游戏控制类</span>
<div>
<div><span style="color:#008080"> 1</span> <span style="color:#008000">//</span><span style="color:#008000">游戏控制类</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 2</span> <span style="color:#008000"></span><span style="color:#0000ff">var</span><span style="color:#000000"> Game </span><span style="color:#000000">=</span><span style="color:#000000"> {<br>
</span><span style="color:#008080"> 3</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">背景类的dom</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 4</span> <span style="color:#008000"></span><span style="color:#000000">    panel : </span><span style="color:#0000ff">null</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 5</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">时间进度条dom</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 6</span> <span style="color:#008000"></span><span style="color:#000000">    range : </span><span style="color:#0000ff">null</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 7</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">每轮时间，毫秒</span><span style="color:#008000"><br>
</span><span style="color:#008080"> 8</span> <span style="color:#008000"></span><span style="color:#000000">    rangetime : </span><span style="color:#000000">3000</span><span style="color:#000000">,<br>
</span><span style="color:#008080"> 9</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">目前进度</span><span style="color:#008000"><br>
</span><span style="color:#008080">10</span> <span style="color:#008000"></span><span style="color:#000000">    nowrange : </span><span style="color:#000000">0</span><span style="color:#000000">,<br>
</span><span style="color:#008080">11</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">时间进度循环ID</span><span style="color:#008000"><br>
</span><span style="color:#008080">12</span> <span style="color:#008000"></span><span style="color:#000000">    rangid : </span><span style="color:#0000ff">null</span><span style="color:#000000">,<br>
</span><span style="color:#008080">13</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">开始按钮</span><span style="color:#008000"><br>
</span><span style="color:#008080">14</span> <span style="color:#008000"></span><span style="color:#000000">    startbtn : </span><span style="color:#0000ff">null</span><span style="color:#000000">,<br>
</span><span style="color:#008080">15</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">初始化</span><span style="color:#008000"><br>
</span><span style="color:#008080">16</span> <span style="color:#008000"></span><span style="color:#000000">    init : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">17</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">获取进度条</span><span style="color:#008000"><br>
</span><span style="color:#008080">18</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.range </span><span style="color:#000000">=</span><span style="color:#000000"> document.getElementById(</span><span style="color:#000000">'</span><span style="color:#000000">range</span><span style="color:#000000">'</span><span style="color:#000000">);<br>
</span><span style="color:#008080">19</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">新建游戏背景</span><span style="color:#008000"><br>
</span><span style="color:#008080">20</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.panel </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">new</span><span style="color:#000000"> Panel();<br>
</span><span style="color:#008080">21</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">清理手尾事件</span><span style="color:#008000"><br>
</span><span style="color:#008080">22</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.panel.onend </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">23</span> <span style="color:#000000">                    <br>
</span><span style="color:#008080">24</span> <span style="color:#000000">            Game.nowrange </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000">;<br>
</span><span style="color:#008080">25</span> <span style="color:#000000">            Game.range.style.width </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">0</span><span style="color:#000000"> </span><span style="color:#000000">+</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">px</span><span style="color:#000000">'</span><span style="color:#000000">;<br>
</span><span style="color:#008080">26</span> <span style="color:#000000">        };<br>
</span><span style="color:#008080">27</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">游戏输了触发事件</span><span style="color:#008000"><br>
</span><span style="color:#008080">28</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.panel.onlose </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">29</span> <span style="color:#000000">            clearInterval(Game.rangid);<br>
</span><span style="color:#008080">30</span> <span style="color:#000000">            <br>
</span><span style="color:#008080">31</span> <span style="color:#000000">            Game.startbtn.disabled </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">''</span><span style="color:#000000">;<br>
</span><span style="color:#008080">32</span> <span style="color:#000000">            Game.startbtn.focus();<br>
</span><span style="color:#008080">33</span> <span style="color:#000000">        };<br>
</span><span style="color:#008080">34</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">显示方向</span><span style="color:#008000"><br>
</span><span style="color:#008080">35</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.panel.showDirection();<br>
</span><span style="color:#008080">36</span> <span style="color:#000000">        </span><span style="color:#008000">//</span><span style="color:#008000">开始计时</span><span style="color:#008000"><br>
</span><span style="color:#008080">37</span> <span style="color:#008000"></span><span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.startTime();<br>
</span><span style="color:#008080">38</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">39</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">计时</span><span style="color:#008000"><br>
</span><span style="color:#008080">40</span> <span style="color:#008000"></span><span style="color:#000000">    startTime : </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">41</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">42</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> per </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">10</span><span style="color:#000000">,ftime </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">this</span><span style="color:#000000">.rangetime</span><span style="color:#000000">/</span><span style="color:#000000">20,_this = this;</span><span style="color:#000000"><br>
</span><span style="color:#008080">43</span> <span style="color:#000000"></span><span style="color:#000000">        <br>
</span><span style="color:#008080">44</span> <span style="color:#000000">        </span><span style="color:#0000ff">var</span><span style="color:#000000"> process </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#0000ff">function</span><span style="color:#000000">(){<br>
</span><span style="color:#008080">45</span> <span style="color:#000000">            <br>
</span><span style="color:#008080">46</span> <span style="color:#000000">            _this.nowrange </span><span style="color:#000000">+=</span><span style="color:#000000"> </span><span style="color:#000000">5</span><span style="color:#000000">;<br>
</span><span style="color:#008080">47</span> <span style="color:#000000">            </span><span style="color:#0000ff">if</span><span style="color:#000000">(_this.nowrange </span><span style="color:#000000">&lt;=</span><span style="color:#000000"> </span><span style="color:#000000">100</span><span style="color:#000000">){<br>
</span><span style="color:#008080">48</span> <span style="color:#000000">                _this.range.style.width </span><span style="color:#000000">=</span><span style="color:#000000"> _this.nowrange </span><span style="color:#000000">*</span><span style="color:#000000"> </span><span style="color:#000000">2</span><span style="color:#000000"> </span><span style="color:#000000">+</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">px</span><span style="color:#000000">'</span><span style="color:#000000">;<br>
</span><span style="color:#008080">49</span> <span style="color:#000000">            }<br>
</span><span style="color:#008080">50</span> <span style="color:#000000">            </span><span style="color:#0000ff">else</span><span style="color:#000000"> {<br>
</span><span style="color:#008080">51</span> <span style="color:#000000">                _this.panel.lose();<br>
</span><span style="color:#008080">52</span> <span style="color:#000000">            }<br>
</span><span style="color:#008080">53</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080">54</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.rangid </span><span style="color:#000000">=</span><span style="color:#000000"> setInterval(process,ftime);<br>
</span><span style="color:#008080">55</span> <span style="color:#000000">    },<br>
</span><span style="color:#008080">56</span> <span style="color:#000000">    </span><span style="color:#008000">//</span><span style="color:#008000">开始</span><span style="color:#008000"><br>
</span><span style="color:#008080">57</span> <span style="color:#008000"></span><span style="color:#000000">    start : </span><span style="color:#0000ff">function</span><span style="color:#000000">(obj){<br>
</span><span style="color:#008080">58</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">59</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.startbtn </span><span style="color:#000000">=</span><span style="color:#000000"> obj;<br>
</span><span style="color:#008080">60</span> <span style="color:#000000">        </span><span style="color:#0000ff">this</span><span style="color:#000000">.startbtn.disabled </span><span style="color:#000000">=</span><span style="color:#000000"> </span><span style="color:#000000">'</span><span style="color:#000000">true</span><span style="color:#000000">'</span><span style="color:#000000">;<br>
</span><span style="color:#008080">61</span> <span style="color:#000000">        <br>
</span><span style="color:#008080">62</span> <span style="color:#000000">        </span><span style="color:#0000ff">if</span><span style="color:#000000">(</span><span style="color:#000000">!</span><span style="color:#0000ff">this</span><span style="color:#000000">.panel){<br>
</span><span style="color:#008080">63</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.init();<br>
</span><span style="color:#008080">64</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080">65</span> <span style="color:#000000">        </span><span style="color:#0000ff">else</span><span style="color:#000000"> {<br>
</span><span style="color:#008080">66</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.startTime();<br>
</span><span style="color:#008080">67</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.panel.showDirection();<br>
</span><span style="color:#008080">68</span> <span style="color:#000000">            </span><span style="color:#0000ff">this</span><span style="color:#000000">.panel.dom.focus();<br>
</span><span style="color:#008080">69</span> <span style="color:#000000">        }<br>
</span><span style="color:#008080">70</span> <span style="color:#000000">    }<br>
</span><span style="color:#008080">71</span> <span style="color:#000000">}</span></div>
</div>
</div>
<p> </p>
<p> </p>
<p><span style="color:red"> </span><a href="http://files.cnblogs.com/floyd/DanceDirection.zip"><span style="color:red">完整源码地址&gt;&gt;</span></a></p>
<p>希望大家喜欢。。。有什么问题随时联系我。。。 </p>
<img src="http://www.cnblogs.com/floyd/aggbug/1855599.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/floyd/">Floyd</a> 发表于 2010-10-19 15:58 <a href="http://www.cnblogs.com/floyd/archive/2010/10/19/1855599.html">原文链接</a></p><p>评论: 13　<a href="http://www.cnblogs.com/floyd/archive/2010/10/19/1855599.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/floyd/archive/2010/10/19/1855599.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/77889/">微软发布测试版Office 365 正式版明年推出</a><span style="color:gray">(2010-10-20 07:51)</span><br>· <a href="http://news.cnblogs.com/n/77888/">新版Mac OS X在界面设计上将向iOS靠拢</a><span style="color:gray">(2010-10-20 07:49)</span><br>· <a href="http://news.cnblogs.com/n/77887/">为什么伟大的公司不能一直伟大</a><span style="color:gray">(2010-10-20 07:48)</span><br>· <a href="http://news.cnblogs.com/n/77886/">网络视频业首推人脸识别功能</a><span style="color:gray">(2010-10-20 07:35)</span><br>· <a href="http://news.cnblogs.com/n/77885/">Google TV整合YouTube高清播放功能</a><span style="color:gray">(2010-10-20 07:34)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/77829/">被绞杀的网景：互联网门口第一滴血</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>