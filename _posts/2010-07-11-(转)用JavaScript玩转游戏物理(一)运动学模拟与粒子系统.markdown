---
layout: post
title:  "(转)用JavaScript玩转游戏物理(一)运动学模拟与粒子系统"
date:   2010-07-11 00:43:00
author: 完美世界
categories: program
---

## (转)用JavaScript玩转游戏物理(一)运动学模拟与粒子系统
### by 完美世界
### at 2010-07-11 00:43:00
### original <http://www.cnblogs.com/qingxin/archive/2010/07/11/1775069.html>

<p>作者: <a href="http://www.cnblogs.com/qingxin/">完美世界</a> 发表于 2010-07-11 00:43 <a href="http://www.cnblogs.com/qingxin/archive/2010/07/11/1775069.html">原文链接</a> 阅读: 131 评论: 3</p><div><a href="http://www.cnblogs.com/miloyip/archive/2010/06/14/Kinematics_ParticleSystem.html">用JavaScript玩转游戏物理(一)运动学模拟与粒子系统</a>
<div>


<img height="512" width="512" src="http://images.cnblogs.com/cnblogs_com/miloyip/250166/r_particlesystem1.jpg">
<h2>系列简介</h2>
<p>也许，三百年前的艾萨克·牛顿爵士(Sir Issac Newton, 1643-1727)并没幻想过，物理学广泛地应用在今天许多游戏、动画中。为什么在这些应用中要使用物理学？笔者认为，自我们出生以来，一直感受着物理世界的规律，意识到物体在这世界是如何&quot;正常移动&quot;，例如射球时球为抛物线(自旋的球可能会做成弧线球) 、石子系在一根线的末端会以固定频率摆动等等。要让游戏或动画中的物体有真实感，其移动方式就要符合我们对&quot;正常移动&quot;的预期。</p>
<p>今天的游戏动画应用了多种物理模拟技术，例如运动学模拟(kinematics simulation)、刚体动力学模拟(rigid body dynamics simulation)、绳子/布料模拟(string/cloth simulation)、柔体动力学模拟(soft body dynamics simulation)、流体动力学模拟(fluid dynamics simulation)等等。另外碰撞侦测(collision detection)是许多模拟系统里所需的。</p>
<p>本系列希望能介绍一些这方面最基础的知识，继续使用JavaScript做例子，以即时互动方式体验。</p>
<h2>本文简介</h2>
<p>作为系列第一篇，本文介绍最简单的运动学模拟，只有两条非常简单的公式。运动学模拟可以用来模拟很多物体运动(例如马里奥的跳跃、炮弹等)，本文将会配合粒子系统做出一些视觉特效(粒子系统其实也可以用来做游戏的玩法，而不单是视觉特效)。</p>
<h2>运动学模拟</h2>
<p>运动学(kinematics)研究物体的移动，和动力学(dynamics)不同之处，在于运动学不考虑物体的质量(mass)/转动惯量(moment of inertia)，以及不考虑加之于物体的力(force )和力矩(torque)。</p>
<p>我们先回忆牛顿第一运动定律:</p>
<blockquote>当物体不受外力作用，或所受合力为零时，原先静止者恒静止，原先运动者恒沿着直线作等速度运动。该定律又称为「惯性定律」。</blockquote>
<p>此定律指出，每个物体除了其位置(position)外，还有一个线性速度(linear velocity)的状态。然而，只模拟不受力影响的物体并不有趣。撇开力的概念，我们可以用线性加速度(linear acceleration)去影响物体的运动。例如，要计算一个自由落体在任意时间t的y轴座标，可以使用以下的分析解(analytical solution):</p>
<div>y(t)=\frac{1}{2}gt^2+v_0t+y_0</div>
<p>当中，<span>y_0</span>和<span>v_0</span>分别是t=0时的y轴起始座标和速度，而g则是重力加速度(gravitational acceleration)。</p>
<p>这分析解虽然简单，但是有一些缺点，例如g是常数，在模拟过程中不能改变；另外，当物体遇到障碍物，产生碰撞时，这公式也很难处理这种不连续性(discontinuity) 。</p>
<p>在计算机模拟中，通常需要计算连续的物体状态。用游戏的用语，就是计算第一帧的状态、第二帧的状态等等。设物体在任意时间t的状态：位置矢量为<span>\mathbf{r}(t)</span>、速度矢量为<span>\mathbf{v}(t)</span>、加速度矢量为<span>\mathbf{a}(t)</span>。我们希望从时间<span>t</span>的状态，计算下一个模拟时间<span>t+\Delta t</span>的状态。最简单的方法，是采用欧拉方法(Euler method)作数值积分(numerical integration):</p>
<div>\begin{align*} \mathbf{v}(t+\Delta t) &amp;= \mathbf{v}(t)+\mathbf{a}(t)\Delta t \\ \mathbf{r}(t+\Delta t) &amp;= \mathbf{r}(t)+\mathbf{v}(t)\Delta t \end{align*} </div>
<p>欧拉方法非常简单，但有准确度和稳定性问题，本文会先忽略这些问题。本文的例子采用二维空间，我们先实现一个JavaScript二维矢量类:</p>
<pre>// Vector2.js
Vector2 = function(x, y) { this.x = x; this.y = y; };
 
Vector2.prototype = {
    copy : function() { return new Vector2(this.x, this.y); },
    length : function() { return Math.sqrt(this.x * this.x + this.y * this.y); },
    sqrLength : function() { return this.x * this.x + this.y * this.y; },
    normalize : function() { var inv = 1/this.length(); return new Vector2(this.x * inv, this.y * inv); },
    negate : function() { return new Vector2(-this.x, -this.y); },
    add : function(v) { return new Vector2(this.x + v.x, this.y + v.y); },
    subtract : function(v) { return new Vector2(this.x - v.x, this.y - v.y); },
    multiply : function(f) { return new Vector2(this.x * f, this.y * f); },
    divide : function(f) { var invf = 1/f; return new Vector2(this.x * invf, this.y * invf); },
    dot : function(v) { return this.x * v.x + this.y * v.y; }
};
 
Vector2.zero = new Vector2(0, 0);
</pre>
<p>然后，就可以用HTML5 Canvas去描绘模拟的过程:</p>
var position = new Vector2(10, 200);
var velocity = new Vector2(50, -50);
var acceleration = new Vector2(0, 10);
var dt = 0.1;
 
function step() {
    position = position.add(velocity.multiply(dt));
    velocity = velocity.add(acceleration.multiply(dt));
 
    ctx.strokeStyle = "#000000";
    ctx.fillStyle = "#FFFFFF";
    ctx.beginPath();
    ctx.arc(position.x, position.y, 5, 0, Math.PI*2, true); 
    ctx.closePath();
    ctx.fill();
    ctx.stroke();
}
    
start("kinematicsCancas", step);
 <br>RunStopClear <br>
<table border="0" style="width:100%">
<tbody>
<tr>
<td></td>
<td width="10"> </td>
<td width="100%" valign="top">
<h4>修改代码试试看</h4>
<ol>
<li>改变起始位置</li>
<li>改变起始速度(包括方向) </li>
<li>改变加速度</li>
</ol></td>
</tr>
</tbody>
</table>
<p>这程序的核心就是step()函数头两行代码。很简单吧？</p>
<h2>粒子系统</h2>
<p>粒子系统(particle system)是图形里常用的特效。粒子系统可应用运动学模拟来做到很多不同的效果。粒子系统在游戏和动画中，常常会用来做雨点、火花、烟、爆炸等等不同的视觉效果。有时候，也会做出一些游戏性相关的功能，例如敌人被打败后会发出一些闪光，主角可以把它们吸收。</p>
<h3>粒子的定义</h3>
<p>粒子系统模拟大量的粒子，并通常用某些方法把粒子渲染。粒子通常有以下特性:</p>
<ol>
<li>粒子是独立的，粒子之间互不影响(不碰撞、没有力) </li>
<li>粒子有生命周期，生命结束后会消失</li>
<li>粒子可以理解为空间的一个点，有时候也可以设定半径作为球体和环境碰撞</li>
<li>粒子带有运动状态，也有其他外观状态(例如颜色、影像等) </li>
<li>粒子可以只有线性运动，而不考虑旋转运动(也有例外) </li>
</ol>
<p>以下是本文例子里实现的粒子类:</p>
<pre>// Particle.js
Particle = function(position, velocity, life, color, size) {
    this.position = position;
    this.velocity = velocity;
    this.acceleration = Vector2.zero;
    this.age = 0;
    this.life = life;
    this.color = color;
    this.size = size;
};
</pre>
<h3>游戏循环</h3>
<p>粒子系统通常可分为三个周期:</p>
<ol>
<li>发射粒子</li>
<li>模拟粒子(粒子老化、碰撞、运动学模拟等等) </li>
<li>渲染粒子</li>
</ol>
<p>在游戏循环(game loop)中，需要对每个粒子系统执行以上的三个步骤。</p>
<h3>生与死</h3>
<p>在本文的例子里，用一个JavaScript数组particles储存所有活的粒子。产生一个粒子只是把它加到数组末端。代码片段如下:</p>
<pre>//ParticleSystem.js
function ParticleSystem() {
    // Private fields
    var that = this;
    var particles = new Array();
 
    // Public fields
    this.gravity = new Vector2(0, 100);
    this.effectors = new Array();
 
    // Public methods
        
    this.emit = function(particle) {
        particles.push(particle);
    };
 
    // ...
}
</pre>
<p>粒子在初始化时，年龄(age)设为零，生命(life)则是固定的。年龄和生命的单位都是秒。每个模拟步，都会把粒子老化，即是把年龄增加<span>\Delta t</span>，年龄超过生命，就会死亡。代码片段如下:</p>
<pre>function ParticleSystem() {
    // ... 
    this.simulate = function(dt) {
        aging(dt);
        applyGravity();
        applyEffectors();
        kinematics(dt);
    };
    
    // ...
 
    // Private methods
    
    function aging(dt) {
        for (var i = 0; i &lt; particles.length; ) {
            var p = particles[i];
            p.age += dt;
            if (p.age &gt;= p.life)
                kill(i);
            else
                i++;
        }
    }
 
    function kill(index) {
        if (particles.length &gt; 1)
            particles[index] = particles[particles.length - 1];
        particles.pop();
    }
    // ...
}
</pre>
<p>在函数kill()里，用了一个技巧。因为粒子在数组里的次序并不重要，要删除中间一个粒子，只需要复制最末的粒子到那个元素，并用pop()移除最末的粒子就可以。这通常比直接删除数组中间的元素快(在C++中使用数组或std::vector亦是)。</p>
<h3>运动学模拟</h3>
<p>把本文最重要的两句运动学模拟代码套用至所有粒子就可以。另外，每次模拟会先把引力加速度写入粒子的加速度。这样做是为了将来可以每次改变加速度(续篇会谈这方面)。</p>
<pre>function ParticleSystem() {
    // ...
    function applyGravity() {
        for (var i in particles)
            particles[i].acceleration = that.gravity;
    }
 
    function kinematics(dt) {
        for (var i in particles) {
            var p = particles[i];
            p.position = p.position.add(p.velocity.multiply(dt));
            p.velocity = p.velocity.add(p.acceleration.multiply(dt));
        }
    }
    // ...
}
</pre>
<h3>渲染</h3>
<p>粒子可以用很多不同方式渲染，例如用圆形、线段(当前位置和之前位置)、影像、精灵等等。本文采用圆形，并按年龄生命比来控制圆形的透明度，代码片段如下:</p>
<pre>function ParticleSystem() {
    // ...
    this.render = function(ctx) {
        for (var i in particles) {
            var p = particles[i];
            var alpha = 1 - p.age / p.life;
            ctx.fillStyle = "rgba("
                + Math.floor(p.color.r * 255) + ","
                + Math.floor(p.color.g * 255) + ","
                + Math.floor(p.color.b * 255) + ","
                + alpha.toFixed(2) + ")";
            ctx.beginPath();
            ctx.arc(p.position.x, p.position.y, p.size, 0, Math.PI * 2, true);
            ctx.closePath();
            ctx.fill();
        }
    }
    // ...
}
</pre>
<h3>基本粒子系统完成</h3>
<p>以下的例子里，每帧会发射一个粒子，其位置在画布中间(200,200)，发射方向是360度，速率为100，生命为1秒，红色、半径为5象素。</p>
var ps = new ParticleSystem();
var dt = 0.01;
 
function sampleDirection() {
    var theta = Math.random() * 2 * Math.PI;
    return new Vector2(Math.cos(theta), Math.sin(theta));
}
 
function step() {
    ps.emit(new Particle(new Vector2(200, 200), sampleDirection().multiply(100), 1, Color.red, 5));
    ps.simulate(dt);
 
    clearCanvas();
    ps.render(ctx);
}
 
start("basicParticleSystemCanvas", step);
 <br>RunStop <br>
<table border="0" style="width:100%">
<tbody>
<tr>
<td></td>
<td width="10"> </td>
<td width="100%" valign="top">
<h4>修改代码试试看</h4>
<ol>
<li>改变发射位置</li>
<li>向上发射，发射范围在90度内</li>
<li>改变生命</li>
<li>改变半径</li>
<li>每帧发射5个粒子</li>
</ol></td>
</tr>
</tbody>
</table>
<h2>简单碰撞</h2>
<p>为了说明用数值积分相对于分析解的优点，本文在粒子系统上加简单的碰撞。我们想加入一个需求，当粒子碰到长方形室(可设为整个Canvas大小)的内壁，就会碰撞反弹，碰撞是完全弹性的(perfectly elastic collision)。</p>
<p>在程序设计上，我把这功能用回调方式进行。 ParticleSystem类有一个effectors数组，在进行运动学模拟之前，先执行每个effectors对象的apply()函数:</p>
<p>而长方形室就这样实现:</p>
<pre>// ChamberBox.js
function ChamberBox(x1, y1, x2, y2) {
    this.apply = function(particle) {
        if (particle.position.x - particle.size &lt; x1 || particle.position.x + particle.size &gt; x2)
            particle.velocity.x = -particle.velocity.x;
 
        if (particle.position.y - particle.size &lt; y1 || particle.position.y + particle.size &gt; y2)
            particle.velocity.y = -particle.velocity.y;
    };
}
</pre>
<p>这其实就是当侦测到粒子超出内壁的范围，就反转该方向的速度分量。</p>
<p>此外，这例子的主循环不再每次把整个Canvas清空，而是每帧画一个半透明的黑色长方形，就可以模拟动态模糊(motion blur)的效果。粒子的颜色也是随机从两个颜色中取样。</p>
var ps = new ParticleSystem();
ps.effectors.push(new ChamberBox(0, 0, 400, 400)); // 最重要是多了这语句
var dt = 0.01;
 
function sampleDirection(angle1, angle2) {
    var t = Math.random();
    var theta = angle1 * t + angle2 * (1 - t);
    return new Vector2(Math.cos(theta), Math.sin(theta));
}
 
function sampleColor(color1, color2) {
    var t = Math.random();
    return color1.multiply(t).add(color2.multiply(1 - t));
}
 
function step() {
    ps.emit(new Particle(new Vector2(200, 200), sampleDirection(Math.PI * 1.75, Math.PI * 2).multiply(250), 3, sampleColor(Color.blue, Color.purple), 5));
    ps.simulate(dt);
 
    ctx.fillStyle="rgba(0, 0, 0, 0.1)";
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ps.render(ctx);
}
 
start("collisionChamberCanvas", step);
 <br>RunStop <br>
<h2>互动发射</h2>
<p>最后一个例子加入互动功能，在鼠标位置发射粒子，粒子方向是按鼠标移动速度再加上一点噪音(noise)。粒子的大小和生命都加入了随机性。</p>
var ps = new ParticleSystem();
ps.effectors.push(new ChamberBox(0, 0, 400, 400));
var dt = 0.01;
var oldMousePosition = Vector2.zero, newMousePosition = Vector2.zero;
 
function sampleDirection(angle1, angle2) {
    var t = Math.random();
    var theta = angle1 * t + angle2 * (1 - t);
    return new Vector2(Math.cos(theta), Math.sin(theta));
}
 
function sampleColor(color1, color2) {
    var t = Math.random();
    return color1.multiply(t).add(color2.multiply(1 - t));
}
 
function sampleNumber(value1, value2) {
    var t = Math.random();
    return value1 * t + value2 * (1 - t);
}
 
function step() {
    var velocity = newMousePosition.subtract(oldMousePosition).multiply(10);
    velocity = velocity.add(sampleDirection(0, Math.PI * 2).multiply(20));    
    var color = sampleColor(Color.red, Color.yellow);
    var life = sampleNumber(1, 2);    
    var size = sampleNumber(2, 4);
    ps.emit(new Particle(newMousePosition, velocity, life, color, size));
    oldMousePosition = newMousePosition;
    
    ps.simulate(dt);
 
    ctx.fillStyle="rgba(0, 0, 0, 0.1)";
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ps.render(ctx);
}
 
start("interactiveEmitCanvas", step);
 
canvas.onmousemove = function(e) {
    if (e.layerX || e.layerX == 0) { // Firefox 
        e.target.style.position='relative';
        newMousePosition = new Vector2(e.layerX, e.layerY);
    }
    else
        newMousePosition = new Vector2(e.offsetX, e.offsetY);
};
 <br>RunStop <br>
<h2>总结</h2>
<p>本文介绍了最简单的运动学模拟，使用欧拉方法作数值积分，并以此法去实现一个有简单碰撞的粒子系统。本文的精华其实只有两条简单公式(只有两个加数和两个乘数)，希望让读者明白，其实物理模拟可以很简单。虽然本文的例子是在二维空间，但这例子能扩展至三维空间，只须把Vector2换成Vector3。本文完整源代码可<a href="http://files.cnblogs.com/miloyip/particle20100614.zip">下载</a>。</p>
<p>续篇会谈及在此基础上加入其他物理现象，有机会再加入其他物理模拟课题。希望各位支持，并给本人更多意见。</p>
<p> </p>
<p>本文摘自:</p>
<div></div>

<div>
<div>
<div><a href="http://home.cnblogs.com/miloyip/"><img src="http://pic.cnblogs.com/face/u113403.jpg"></a>
<div><a href="http://home.cnblogs.com/miloyip/">Milo Yip</a> 的园子,再次感谢原文作者为大家提供新的技术 原文地址:<a href="http://www.cnblogs.com/miloyip/archive/2010/06/14/1758272.html">http://www.cnblogs.com/miloyip/archive/2010/06/14/1758272.html</a></div>
<div></div>
</div>
</div>
</div>
</div>
</div>


<div></div><img src="http://www.cnblogs.com/qingxin/aggbug/1775069.html?type=1" width="1" height="1" alt=""><p>评论: 3　<a href="http://www.cnblogs.com/qingxin/archive/2010/07/11/1775069.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/qingxin/archive/2010/07/11/1775069.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/enterprise/2453/">百度期待您的加盟</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/68009/">《经济观察报》：腾讯的风险</a><span style="color:gray">(2010-07-10 23:01)</span><br>· <a href="http://news.cnblogs.com/n/68008/">谷歌力推低价Android手机的野心</a><span style="color:gray">(2010-07-10 22:58)</span><br>· <a href="http://news.cnblogs.com/n/68007/">Google 创始人 Larry Page 指责 Steve Jobs 说话 “太唐骏”</a><span style="color:gray">(2010-07-10 22:55)</span><br>· <a href="http://news.cnblogs.com/n/68006/">Chrome 扩展曝严重安全漏洞，可获取登陆用户名与密码</a><span style="color:gray">(2010-07-10 22:54)</span><br>· <a href="http://news.cnblogs.com/n/68004/">乔布斯缺席太阳谷峰会 不失要人地位难被忘记</a><span style="color:gray">(2010-07-10 19:14)</span><br></p><p>编辑推荐：<a href="http://www.cnblogs.com/firelong/archive/2010/07/09/1774490.html">关于近期C#大论战的回应</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>