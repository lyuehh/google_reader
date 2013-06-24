---
layout: post
title:  "浅谈 HTML5 的 Drag and Drop"
date:   2012-09-06 02:06:51
author: ghSky
categories: program
---

## 浅谈 HTML5 的 Drag and Drop
### by ghSky
### at 2012-09-06 02:06:51
### original <http://ghsky.com/2012/09/talk-about-html5-drag-and-drop.html>

<h3>前言</h3>
<p>这里引用 PPK 大神一篇文章 <a href="http://www.quirksmode.org/blog/archives/2009/09/the_html5_drag.html">The HTML5 drag and drop disaster</a> 里面的一段话作为开始：</p>
<blockquote><p>
  After spending about a day and a half in testing I am forced to conclude that the <a href="http://dev.w3.org/html5/spec/editing.html#dnd">HTML5 drag and drop module</a> is not just a disaster, it’s a fucking disaster.
</p></blockquote>
<p>可想而知，这货是有多么坑爹啦？不过 PPK 的文章我确实也没仔细看，只是在翻阅资料的时候，偶尔看见了，然后第一句话就让我感同身受，好吧，那接下来就与 DND 战斗吧！</p>
<h3>需求</h3>
<p>大致说一下需求，这里需要完成一个拖拽上传的功能，相信这也是老生常谈了，2、3年前 Gmail 便实现了，国内各网站也有了不同程度的支持，<a href="http://www.diandian.com/">点点网</a> 一贯以追求良好的用户体验为楷模，所以今年年初他们也<a href="http://iam.diandian.com/post/2012-03-12/16197139">推出了这个功能</a>。但是总体来说，通过细致的体验后发现，细节方面做得还是不够完善。</p>
<p>做完整个功能之后，我总结一下我所希望考虑到的细节吧：</p>
<ol>
<li>有东西拖动进入页面，需要有反馈，知道这是一个拖动；</li>
<li>设定一个允许 drop 的区域，文件拖入此区域才算是一个正确的用户行为，并有相关反馈提示此处可以 drop，其他区域无法 drop，如果用户提前释放鼠标，则会有相关的 revert 的动画出现（貌似这个就 Mac 下会有，Windows 一切尽在不言中）；</li>
<li>drop 成功后，前端拿到适合的图片文件，直接通过 dataURI 进行展示，然后通过兼容之前 form 表单提交接口，后台完成图片数据的后端提交，基本做到平滑、用户无感；</li>
<li>考虑到浏览器其他插件会也会充分运用到 Drag and Drop 的因素，尽量做到不影响其他脚本功能。</li>
</ol>
<p>大致就那么多吧，前三点是开始完成需求时就一直要求完成的，也是其中最纠结的地方，充分让我体会到了 DND 事件和API是如何的 sucks!!!<br>
<span></span></p>
<h3>实现</h3>
<p>这次实现主要还是直接从实际功能出发，到最终代码完成，也没有重构独立成单独模块，这个是比较欠缺的地方，或许在模块的独立、重构中可以有所优化，不过这个也是后话了，之后遇到问题再做分享。</p>
<p>首先，通过 <a href="https://developer.mozilla.org/">MDN</a> 的文档了解一下 <a href="https://developer.mozilla.org/en-US/docs/DragDrop/Drag_and_Drop">Drag and drop</a> 提供的事件可真是丰富呀：<code>dragstart</code>, <code>dragenter</code>, <code>dragover</code>, <code>dragleave</code>, <code>drag</code>, <code>drop</code>, <code>dragend</code> 丰富到十分的迷茫。</p>
<p>用了半天 <code>dragstart</code>, <code>dragend</code>, <code>drag</code> 发现这仨货其实是只会在设置了 <code>draggable="true"</code> 的元素上触发，之前一直误用，不会触发，略显蛋疼～所以结合我们的需求，这仨东西跟我们没啥关系，忽略之～</p>
<p>剩下可用的 <code>dragenter</code>, <code>dragover</code>, <code>dragleave</code>, <code>drop</code> 四个事件可用，分别解释一下吧：</p>
<ul>
<li><code>dragenter</code> 当鼠标按下，开始拖拽，这时候的鼠标进入某一个元素便会触发 <code>dragenter</code> 事件，同时这个事件的监听函数一般需要判断是否允许在当前位置 drop 。</li>
<li><code>dragover</code> 这个事件和 <code>dragenter</code> 类似，大部分这两个事件的监听函数都可以公用一个。</li>
<li><code>dragleave</code> 顾名思义，与 <code>dragenter</code> 事件相反，当鼠标拖拽离开某元素时便会触发。</li>
<li><code>drop</code> 当拖拽完成，鼠标释放时，如果当前位置允许 drop 事件发生，则会触发其监听函数，感觉很废话的样子，但是就想之前 <code>dragenter</code>, <code>dragover</code> 这两个事件所述，其中的监听函数可以改变当前位置是否可以 drop 的特性，这个参数后面会提到。</li>
</ul>
<p>总体流程是这样的，在 <code>document</code> 上监听 <code>dragenter</code> 事件，当触发时，添加“拖拽到此处上传”的提示。当用户没有在上传区域 drop 或者鼠标拖拽移出浏览器区域时，上传提示消失。在上传提示显示时，如果用户在上传区域 drop 则会触发上传事件，如下图：</p>
<p>![dnd-workflow](http://ghsky.com/wp-content/uploads/2012/09/dnd-workflow.png "DND Workflow")</p>
<p>最简单的会在 <code>document</code> 上分别绑定 <code>dragenter</code>, <code>dragleave</code> 事件，用于处理上传提示显示的判断。但是……万事儿皆有但是啊……我在 <code>document</code> 上绑定的 <code>dragenter</code>, <code>dragleave</code> 最终都成为了一个事件代理的角色，类似 <code>mouseover</code>, <code>mouseout</code> 的意思。之前从命名上，你还以为功能和 <code>mouseenter</code>, <code>mouseleave</code> 其实你大错特错了！这就蛋疼了，子元素的 <code>dragenter</code>, <code>dragleave</code> 各种在 <code>document</code> 的侦听器上触发，而且更加坑爹的时，好歹我们对于 <code>mouseover</code>, <code>mouseout</code> 的处理用事件对象中的 <code>relatedTarget</code> 可以进行很方便的判断，但是在 <code>dargenter</code>, <code>dragleave</code> 我真的没有找到多浏览器兼容的判断属性（<code>relatedTarget</code> 在 Firefox 中工作很好，但是 Webkit 中就始终为 <code>null</code>）。怎么办呢？一种是计数的方法，理论上如果 enter 过 n 次，那么最终离开页面时也应该触发 n 次 leave，但是！！！有个奇特的问题，Firefox 下第一次进入的 <code>dragenter</code> 事件会被触发两次……这尼玛神马情况呀！肿么办？定义一个更加可靠的判断机制，对于每一个也没上的可见元素（注意，这里有个重大坑“可见”），每一次 <code>dragenter</code> 后必然在鼠标 drop 或者离开都会有一个相应的 <code>dragleave</code> 事件，所以这次的重点是元素的判断。在 jQuery 下面，直接用 <code>$()</code> 生成一个空的 jQuery collection，然后在 <code>dragenter</code> 的时候 <code>add</code> 事件对象的 <code>target</code> 到这个 collection 中，当 <code>dragleave</code> 的时候 <code>not</code> 对应的 <code>target</code> 这样，当在 <code>dragleave</code> 的时候，发现 colleciton 即鼠标要么 drop 掉了，这个方案是从 StackOverflow 上面看到的。这里不得不提一下刚刚说到的“可见”的那个大坑，曾经我在 <code>dragenter</code> 的时候通过 <code>class</code> 修改的方式，某一个元素成为了不可见元素，这时候就意味着该元素上永远不可能发生 <code>dragleave</code> 直到它再次可见未知，这样就会导致 collection 永远不为空，这是一个很惨痛的经历，debug 很久～解决办法则是对 css 进行调整，修改不可见元素的方式，这个需要不同情况不同处理，只是需要特别留意这个坑。</p>
<p>解决掉这个棘手的 <code>dragenter</code>, <code>dragleave</code> 事件后，你有没有觉得 DND 草案标准真的是十分的坑呀！接下来是一个比较隐蔽的坑，之前我的需求里面提到过，对于拖拽到非上传区域的东西，鼠标 drop 以后是需要告诉浏览器，这个东西我不要它，而不是默认阻止浏览器收下它的行为。这个点的理解可能会比较绕，我们这样想，如果你之前告诉浏览器这个地方不能 drop，那么当你 drop 以后，浏览器会自动放弃默认行为；而相反，浏览器不知道的话，则它可能执行一个不可预知的默认行为，对于从本地把图片拖拽到浏览器中，大部分浏览器的默认行为是打开它，而我这里明显需要限定在上传区域执行上传，否则阻止打开。这里的处理方式就会提现出一些细节的东西，比如在 Mac 下，我们到 Gmail 里面拖拽上传附件，拖拽的过程中，如果在适当区域 drop ，这时候你会发现刚刚拖拽过来的东西会有一个 revert 的动画，自动回到 Finder 里面去（好吧，Windows 用户你们可能就要失望了，YY去吧！），这个细节恰恰提现了，是浏览器自愿不接受 drop 呢，还是你强制它不接受的（主动与被动的问题，往往对判刑至关重要哟，亲～别想多了！）仔细查阅 <a href="https://developer.mozilla.org/en-US/docs/DragDrop/Drag_Operations#Drag_Effects">MDN 的文档(Drag Operations#Drag Effects)</a>，你终于会看见它说过：</p>
<blockquote>
<p>When dragging, there are several operations that may be performed. The copy operation is used to indicate that the data being dragged will be copied from its present location to the drop location. The move operation is used to indicate that the data being dragged will be moved, and the link operation is used to indicate that some form of relationship or connection will be created between the source and drop locations.<br>
  During a drag operation, a listener for the dragenter or dragover events can check the effectAllowed property to see which operations are permitted. A related property, dropEffect, should be set within one of these events to specify which single operation should be performed. Valid values for the dropEffect are none, copy, move or link. The combination values are not used for this property.</p>
</blockquote>
<p>说的主要就是对于拖拽过来的数据，在当前位置允许的 drop 效果，比如 <code>copy</code>, <code>move</code> 之类的，当然 <code>none</code> 这个可以有，这个就是我们想要设置的默认情况下不允许 drop 操作。这个需要设置的属性就是 <code>dropEffect</code>，通常在 <code>dragenter</code>, <code>dragover</code> 两个侦听器中进行修改。又找到一个关键点，一个坑啊！之前想都没想过，拖拽上传，竟然还能运行 <code>move</code>, <code>link</code>，我真是太年轻，太简单了……但是这么“丰富”的设置，真的是要搞死个人啊！好了，又一个目标基本达成。继续前进～</p>
<p>终于进入到比较关键的 drop 步骤，这个时候就要用到 <code>drop</code> 事件的侦听器了。对于刚刚提到的 <code>dropEffect</code> 一定要在恰当的位置设置正确，否则，你的 <code>drop</code> 事件永远不会触发。说到这里，我又不得不提我遭遇到的另一个坑了！我基本的处理逻辑是这样的，<code>dragenter</code> 在 <code>document</code> 中首次触发的时候，我会创建显示 drop 区域的那个元素出来，然后在这个元素上绑定了 <code>dragenter</code>, <code>dragover</code>, <code>dragover</code> 用于处理这种拖拽到上传元素上的视觉反馈，这里插一嘴，这种视觉反馈理论上说是可以用 css 的伪类完成，<a href="https://developer.mozilla.org/en-US/docs/DragDrop/Drag_Operations#Drop_Feedback">MDN 文档(Drag Operations#Drop Feedback)</a> 有说：</p>
<blockquote><p>
  However, you can also update the user interface with an insertion point or highlight as needed. For simple highlighting, you can use the -moz-drag-over CSS pseudoclass on a drop target.
</p></blockquote>
<p>只是我没有仔细去用，貌似有兼容性问题，我这个伪类上面加了一个 webkit 的兼容后就完全失效了，搞不懂，后续没仔细看，这个可以再关注。接着刚刚的说，视觉反馈是一个方面，另外一个更重要的方面就是修改 <code>dropEffect</code> 你可能已经猜到了，因为我在外层（也就是 <code>document</code>）上的 <code>dragenter</code>, <code>dragover</code> 中是让 <code>dropEffect = 'none'</code> 的，但是在上传区域显然需要改成 <code>copy</code> 那就改呗～～～但是！！！你想一想，我这里会事件冒泡的，好不好，到了上传区域我给改了，然后冒啊冒啊，最后到了 <code>document</code> 那里，它又给改回去了 =。=|| 显然有一种蛋蛋的忧伤～那么怎么办呢？第一个想法，阻止冒泡呗～～～好吧，试试！结果呢～～结果呢～发现，你阻止这里冒泡上去，很有可能就会影响在 <code>document</code> 上 <code>dragenter</code> 侦听器中对于 colleciton 的操作，导致的结果就是明明我拖拽了一个东西，但是由于阻止冒泡，添加到 colleciton 的操作没有进行，可能会导致潜在危险。好吧，第二个方法，我们可以尝试在冒泡到 <code>document</code> 的 <code>dragenter</code> 侦听器函数中，判断 <code>event.target</code> 是否是从上传区域过来的，如果是，则不再修改 <code>dropEffect</code>，这其实也是一种方式，只是我当时没有想到，没有采用过，理论上也能够实现，只是可能涉及到判断父容器的问题。我采用的方法，使用事件中的捕获阶段(Capture Phase)，由于我的上传区域是通过 <code>dragenter</code> 侦听器完成创建以及注册相关事件，那么不论在哪个元素上发生，我都应该首先完成上传区域的初始化，包括默认 <code>dropEffect</code> 的设定，然后呢，在冒泡阶段，真正的上传元素区域重置 <code>dropEffect</code> 时候不会再冒泡到 <code>document</code> 时使设置失效，感觉上这个方法有些 trick，但是结合当前代码是最合适的选择，我相信以后在一步步重构优化后，会更加清晰的理清这段逻辑，消除一些不必要的 trick，增强可维护性。</p>
<p>好了，终于到 <code>drop</code> 事件了。我们需要利用到事件对象上超强大的 <code>dataTransfer</code> 这个属性。这货老高级了，包括我之前弄过的一个从剪贴板获取数据也是需要用到和它类似的一个属性 <code>clipboardData</code>。他们应该都是一个“妈”派生的，所以会有很多共性，对于这个高级货的更多了解，自己 Google 之～这里在 <code>drop</code> 侦听器中只用到最简单的一个 <code>files</code> 属性，获取当前 <code>drop</code> 事件中包含的文件列表，可以从中拿到一个或多个文件，这里我们会拿第一个文件，而且会假设其为图片，然后需要解析得到这个图片的 <code>DataURI</code>，同时为了方面操作，我还需要拿到图片的长宽信息。从图片文件到 <code>DataURI</code> 的过程需要用到另外一个高级货 <code>FileReader</code>，这个也是 HTML5 标准的一部分，详情继续 Google 之～通过一个 <code>readAsDataURL</code> 的方法可以读取到图片文件的 <code>dataURI</code>， 然后通过 <code>new Image()</code> 可以创建一个图片对象，然后设置其 <code>src</code> 为 <code>dataURI</code> 即可完成本地图片的读取，同时可以在图片加载完成时拿到图片长宽以便一些操作，所以为了方便使用，我对其进行了一个简单的封装。</p>
<p>然后就要数到怎么样做到无缝接入后端。传统的后端图片上传，都是使用 <code>&lt;form enctype=&quot;multipart/form-data&quot;&gt;</code> 的方式编码文件为二进制串然后 post 出去。当初很傻很天真的我以为要做到无缝，还得自己去编码二进制，然后发现 <code>FileReader</code> 居然可以做到 <code>readAsBinaryString</code> 顿时哈哈仰天大笑，结果后面发现，人肉编码数据真心负责，还得去研读规范，好吧……这时，突然又发现了一个更高端的玩意儿 <code>FormData</code>， 不懂继续 Google 哟～顾名思义，用它可以直接构造表单数据，可以直接将 <code>file</code> 类型或者 <code>blob</code> 类型直接 <code>append</code> 到 <code>FormData</code> 实例中，然后调用 <code>XMLHttpRequest</code> 直接发送 <code>FormData</code> 实例，一切浏览器帮你搞定！好了，至此大致的开发流程已经走通，接下来就是各种重构和分模块，这个步骤我还在进行中，为了更好地复用，所以需要做很多的考虑和扩展，以后会对相关部分再做总结。</p>
<p>在上线后收到一些反馈，其中抱怨比较多的一个就是绑定在 <code>document</code> 上的 drag 一些列侦听器行为可能会影响到一些浏览器插件，甚至包括傲游3的超级拖拽（好吧，用户说的不爽，要改，他就是上帝啊～），肿么办？首先定位 <code>dragenter</code> 发现需要做一个类型检查的排除，如果用户拖拽的是文件，才会提示上传区域，否则一切忽略，随浏览器默认行为，这里又用到一段比较 trick 的代码：</p>
<pre><code>// if not drag file
if (evt.dataTransfer &amp;&amp; (types = evt.dataTransfer.types) &amp;&amp; (types = slice.call(types)) &amp;&amp;

    // &#39;Files&#39; keyword is not involed in file drag
    types.indexOf(&#39;Files&#39;) &lt; 0) {

  // not file drag, return directly in case other browser plugin behavior (e.g Maxthon 3 SuperDrag)
  return;

}
</code></pre>
<p>通过判断事件对象中的 <code>dataTransfer.types</code> 里是否包含 <code>'Files'</code> 关键字，这是测试下来最好的一个判断方法。</p>
<h3>总结</h3>
<p>代码我没有全部贴，因为大部分只是只言片语也不好贴，而且会涉及到一些文件依赖，如果有问题可以留言，代码我想可以总结会用更好的方式分享出来。</p>
<p>对于 DND 中的各种坑，算是见到了一部分吧，对于 HTML5 新兴技术在感慨方便的同时，也会遇到这种各样纠结的情况，DND 看起来已经是个很复杂的东西，很多事件，属性交织在一起，很是混乱。然后 <code>FileReader</code>, <code>FormData</code>, <code>dataTransfer</code>, <code>clipaboardData</code> 等等很高端的东西，会让你体验到很多原生应用的感觉，但是毕竟属于规范初期，很多兼容性问题和 BUG 也是没办法避免的，所以总结一下现在蛋蛋的状态就是“疼并爽YY”。</p><img src="http://www1.feedsky.com/t1/708404295/ghsky/feedsky/s.gif?r=http://ghsky.com/2012/09/talk-about-html5-drag-and-drop.html" border="0" height="0" width="0">