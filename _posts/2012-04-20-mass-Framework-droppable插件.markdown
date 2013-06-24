---
layout: post
title:  "mass Framework droppable插件"
date:   2012-04-20 10:02:00
author: 司徒正美
categories: program
---

## mass Framework droppable插件
### by 司徒正美
### at 2012-04-20 10:02:00
### original <http://www.cnblogs.com/rubylouvre/archive/2012/04/20/2458630.html>

<p>mass Framework拖放组件的第二弹，八大行为组件之一，droppable终于完成了。它是专门用于处理拖放块与放置对象之间的关系。放置对象在我的框架有个更好的名字叫，靶场。一个拖放块相当于导弹，其活动范围就是其射程，那么放置对象就是它的靶场。</p>
<br>
<p>在HTML5原生拖放API中，当一个元素成为靶场，它可以绑定以下四个事件：</p>
<ul>
     <li><em>dragenter</em>：当光标进入靶场时，执行此回调。</li>
     <li><em>dragover</em>：当光标进入靶场后，执行此回调。</li>
     <li><em>dragleave</em>：当光标进入靶场时，执行此回调。</li>
     <li><em>drop</em>：当光标进入靶场后，留在其上移动时放开鼠标时，执行此回调。</li>
</ul>
<br>
<p>从上面的描述也可知，原生放置API只能处理光标与放置的关系，与mouseenter, mouseleave, mousemove如此一辙，如果我想更换其他触发规则就歇菜了，比如比如我只想拖动块有一半面积进入了靶场才触发dragenter，或者当它完全位于靶场内才触发，或者只要有相交就触发，因此仅靠这些原生事件是远远不够了。因此mass Framework的droppable参数中有一个叫tolerance，用于自定义触发模式。默认是pointer模式，也即W3C的模式,当鼠标进入靶场才触发dragenter回调,离开则触发dragleave回调。intersect模式，当拖动块与靶场存在重叠部分，就触发dragenter回调。middle模式，只有当拖动块有一半的面积进入靶场，才触发dragenter回调。fit模式，只有当拖动块完全位于靶场之内，才触发dragenter回调。</p>
<br>
<p>说实话，mass Framework的droppable在诸多地方借鉴script.aculo.us，这是一个伟大的库，史上最强的动画库，至今还没有人能敌。jQuery ui也一样，也是在script.aculo.us的基础上进行改进。但script.aculo.us也好，jQuery ui也好，droppable与dragable是两个不同的体系，事件分绑在两个元素上。drag, dragstart, dragend归draggable管，drop, dragenter, dragmove, dragleave归droppabke管，这严重违悖了其内在的实现机理。我们在拖动时，会产生一个数据体，它包含自身方位大小与各种事件回调，此外还有一个数组，里面存在着它可接受的靶场。这个jquery ui有accept这个参数来处理，通过选择器引擎等到一组放置对象，再转换为包含方位大小与事件回调的数据体。script.aculo.us则通过Droppables.add来添加数据库。droppable的数据体肯定要包含于draggable的数据体中，要不就无法判定重叠相交等关系。既然如此，一开始就应该把参数全部定义在一块。加之，mass Framewor是k取droppable其投掷之意，而不是放置之意，认为droppable是draggable的一种增强，先有draggable才有droppable，因此我在这里就不照搬script.aculo.us的接口定义。将droppable离经叛道地整成draggable，也算一“超创新”吧（比“微创新”强N倍之意！）</p>
<pre>
$.define("droppable","more/draggable",function(Draggable){

    Draggable.implement({
        //有关droppable的增强功能
    })

    $.fn.droppable = function( hash ){
        return this.draggable( hash )
    }
});
</pre>

<p>下面是例子，更多例子与<a href="https://github.com/RubyLouvre">文档见github</a>。</p>
<pre>
            $.require(&quot;ready,more/droppable&quot;,function(){
                $(&#39;.drag&#39;).droppable({
                    range:&quot;.drop&quot;,//指定靶场的CSS表达式
                    hoverClass: &quot;active&quot;,//当拖动块进入靶场添加的类名
                    tolerance:  function( event, drg, drp ){//指定触发规则
                        var r = drp.width / 2, x = drp.left + r, y = drp.top + r,
                        h = Math.min( Math.abs( x - drg.left ), Math.abs( x - drg.right ) ),
                        v = Math.min( Math.abs( y - drg.top ), Math.abs( y - drg.bottom ) );
                        if ( drg.top &lt; y &amp;&amp; drg.bottom &gt; y )
                            return h &lt;= r ? 1 : 0;
                        else if ( drg.left &lt; x &amp;&amp; drg.right &gt; x )
                            return v &lt;= r ? 1 : 0;
                        else
                            return Math.sqrt( h * h + v * v ) &lt;= r ? 1 : 0;
                    } ,
                    drop: function(event, dd){//在靶场放下时添加的类名
                        dd.dropper.toggleClass(&quot;dropped&quot;);
                    },
                    dragleave:function(event, dd){
                        dd.dropper.removeClass(&quot;dropped&quot;);
                    }
                });
            });
</pre>
<p>请等博客左上角的FLASH时钟动画出现后才进行拖动。</p>

     
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <img src="http://www.cnblogs.com/rubylouvre/aggbug/2458630.html?type=1" width="1" height="1" alt=""><p><a href="http://www.cnblogs.com/rubylouvre/archive/2012/04/20/2458630.html">本文链接</a></p>