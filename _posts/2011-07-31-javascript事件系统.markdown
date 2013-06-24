---
layout: post
title:  "javascript事件系统"
date:   2011-07-31 12:21:18
author: admin
categories: program
---

## javascript事件系统
### by admin
### at 2011-07-31 12:21:18
### original <http://stylechen.com/easyevent.html>

<p>2005年的时候，jQuery的作者John Resig提出了一个兼容IE和标准浏览器的addEvent的方案，在当时认为是最佳的方案，但是几天后<a href="http://dean.edwards.name/weblog/2005/10/add-event/">Dean Edwards</a>提出了更好的方案，John Resig之后把Dean Edwards的方法应用在了jQuery中(在jQuery的事件系统的注释代码中可以看到Dean Edwards的署名)。</p>
<p>jQuery历经了这么多的版本，在原来Dean Edwards的基础上，John Resig对于最初的addEvent的方法有了一定的改进。最近在深入学习javascript的事件系统时，看了不少的jQuery的代码，我自己也写了一个简单的javascript事件系统，也算是对于jQuery事件的一个精简。当然这里指得精简并不是说jQuery代码比较臃肿，而是jQuery作为一个类库，需要考虑的东西有很多，也紧密结合了选择器，所以代码量确实很庞大。我的javascript事件系统解决了几个最基本的常见问题：</p>
<p><strong>1. 尽量避免IE在绑定事件时出现的内存泄漏；</strong></p>
<p><strong>2. 修正了IE的this指向问题；</strong></p>
<p><strong>3. 让IE可以支持一些常见的事件标准方法；</strong></p>
<p><strong>4. 同类型的事件可以多次绑定并按顺序执行；</strong></p>
<pre>
(function( win, undefined ){
var easyEvent = function(){
// 用来存储数据的全局对象
var cacheData = {
	/*  1 : {
	 *		eclick : [ handler1, handler2, handler3 ];
	 *		clickHandler : function(){ //... };
	 *	} */
	},
	uuid = 1,
	expando = &#39;cache&#39; + ( +new Date() + &quot;&quot; ).slice( -8 );  // 生成随机数

var base = {

	// 设置、返回缓存的数据
	// 关于缓存系统详见：http://stylechen.com/cachedata.html
	data : function( elem, val, data ){
		var index = elem === win ? 0 :
				elem.nodeType === 9 ? 1 :
				elem[expando] ? elem[expando] :
				(elem[expando] = ++uuid),

			thisCache = cacheData[index] ?
			cacheData[index] :
			( cacheData[index] = {} );

		if( data !== undefined ){
			// 将数据存入缓存中
			thisCache[val] = data;
		}
		// 返回DOM元素存储的数据
		return thisCache[val];
	},

	// 删除缓存
	removeData : function( elem, val ){
		var index = elem === win ? 0 :
				elem.nodeType === 9 ? 1 :
				elem[expando];

		if( index === undefined ) return;

		// 检测对象是否为空
		var isEmptyObject = function( obj ) {
				var name;
				for ( name in obj ) {
					return false;
				}
				return true;
			},
			// 删除DOM元素所有的缓存数据
			delteProp = function(){
				delete cacheData[index];
				if( index &lt;= 1 ) return;
				try{
					// IE8及标准浏览器可以直接使用delete来删除属性
					delete elem[expando];
				}
				catch ( e ) {
					// IE6/IE7使用removeAttribute方法来删除属性(document会报错)
					elem.removeAttribute( expando );
				}
			};

		if( val ){
			// 只删除指定的数据
			delete cacheData[index][val];
			if( isEmptyObject( cacheData[index] ) ){
				delteProp();
			}
		}
		else{
			delteProp();
		}
	},

	// 绑定事件
	addEvent : function( elem, type, handler ){
		if( elem.addEventListener ){
			elem.addEventListener( type, handler, false );
		}
		else if( elem.attachEvent ){
			elem.attachEvent( &#39;on&#39; + type, handler );
		}
	},

	// 删除事件
	removeEvent : function( elem, type, handler ){
		if( elem.addEventListener ){
			elem.removeEventListener( type, handler, false );
		}
		else if( elem.attachEvent ){
			elem.detachEvent( &#39;on&#39; + type, handler );
		}
	},

	// 修复IE浏览器支持常见的标准事件的API
	fixEvent : function( e ){
		// 支持DOM 2级标准事件的浏览器无需做修复
		if ( e.target ){
			return e;
		}
		var event = {}, name;
		event.target = e.srcElement || document;
		event.preventDefault = function(){
			e.returnValue = false;
		};

		event.stopPropagation = function(){
			e.cancelBubble = true;
		};
		// IE6/7/8在原生的win.event中直接写入自定义属性
		// 会导致内存泄漏，所以采用复制的方式
		for( name in e ){
			event[name] = e[name];
		}
		return event;
	},

	// 依次执行事件绑定的函数
	eventHandler : function( elem ){
		return function( event ){
			event = $.fixEvent( event || win.event );
			var type = event.type,
				events = $.data( elem, &#39;e&#39; + type );

			for( var i = 0, handler; handler = events[i++]; ){
				if( handler.call(elem, event) === false ){
					event.preventDefault();
					event.stopPropagation();
				}
			}

			console.debug( event );
		}
	}

};

var $ = base;

var extend = {

	bind : function( elem, type, handler ){
		var events = $.data( elem, &#39;e&#39; + type ) || $.data( elem, &#39;e&#39; + type, [] );
		// 将事件函数添加到缓存中
		events.push( handler );
		// 同一事件类型只注册一次事件，防止重复注册
		if( events.length === 1 ){
			var eventHandler = $.eventHandler( elem );
			$.data( elem, type + &#39;Handler&#39;, eventHandler );
			$.addEvent( elem, type, eventHandler );
		}

		//console.debug( cacheData );
	},

	unbind : function( elem, type, handler ){
		var events = $.data( elem, &#39;e&#39; + type );
		if( !events ) return;

		// 如果没有传入要删除的函数则删除该事件类型的缓存
		if( !handler ){
			events = undefined;
		}
		// 如果有具体的函数则只删除一个
		else{
			for( var i = events.length - 1; i &gt;= 0; i-- ){
				var fn = events[i];
				if( fn === handler ){
					events.splice( i, 1 );
				}
			}
		}		

		// 删除事件和缓存
		if( !events || !events.length ){
			var eventHandler = $.data( elem, type + &#39;Handler&#39; );
			$.removeEvent( elem, type, eventHandler );
			$.removeData( elem, type + &#39;Handler&#39; );
			$.removeData( elem, &#39;e&#39; + type );
		}
	}
};

return extend;
};

win.easyEvent = easyEvent();

})( window, undefined );
</pre>
<p>调用：</p>
<pre>
<div>javascript事件系统</div>

&lt;script src=&quot;event.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot;&gt;
var box = document.getElementById( &#39;box&#39; ),
btn = document.getElementById( &#39;btn&#39; );

var getNodeName = function( e ){
alert( e.target.nodeName );  // DIV
};

easyEvent.bind( box, &#39;click&#39;, function(){
alert( this.innerHTML );  // javascript事件系统
});

easyEvent.bind( box, &#39;click&#39;, getNodeName );

// 只删除一个指定的事件函数
easyEvent.bind( btn, &#39;click&#39;, function(){
easyEvent.unbind( box, &#39;click&#39;, getNodeName );
});

// 删除所有该事件类型的事件函数
easyEvent.bind( btn, &#39;click&#39;, function(){
easyEvent.unbind( box, &#39;click&#39; );
});
&lt;/script&gt;
</pre>
<div>
原载于：雨夜带刀’s Blog<br>
本文链接：<a href="http://stylechen.com/easyevent.html">http://stylechen.com/easyevent.html</a><br>
如需转载请以链接形式注明原载或原文地址。
</div>