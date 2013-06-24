---
layout: post
title:  "完全模拟textarea的maxlength属性"
date:   2012-03-28 13:25:58
author: army
categories: program
---

## 完全模拟textarea的maxlength属性
### by army
### at 2012-03-28 13:25:58
### original <http://army8735.org/2012/03/28/1057.html>

<p>现代浏览器中，textarea的maxlength属性已经被浏览器内置支持，然而ie系列仍旧我行我素。</p>
<p>在低版本ie（ie8-）中，可以通过对propertychange的侦听来检测输入，从而模拟maxlength属性。然而ie9很奇怪，支持了input事件，但是删除、剪切、粘帖却不触发input！</p>
<p>幸运的是，ie9还可以侦听cut、paste这样的操作，于是大概模拟兼容主流浏览器的代码如下（使用jQuery）：</p>
<pre>define([&#39;jQuery&#39;, &#39;Event&#39;], function($, Event) {
	var ie9 = $.browser.msie &amp;&amp; $.browser.version == &#39;9.0&#39;;
	function virtualTextareaMaxlength(item) {
		var max = parseInt(item.attr(&#39;maxlength&#39;)),
			v = item.val();
		if(!isNaN(max) &amp;&amp; v.length &gt; max) {
			var i,
				bookmark,
				oS = document.selection.createRange(),
				oR = document.body.createTextRange();
			oR.moveToElementText(item[0]);
			bookmark = oS.getBookmark();
			for (i = 0; oR.compareEndPoints(&#39;StartToStart&#39;, oS) &lt; 0 &amp;&amp; oS.moveStart(&quot;character&quot;, -1) !== 0; i++)
				//ie的换行是\r\n，算2个字符长度
				if(v.charAt(i) == &#39;\n&#39;)
					i++;
			item.val(v.substr(0, Math.min(max, i - 1)) + v.substr(i, Math.min(max, v.length)));
			//模拟光标位置
			if(v.length != i) {
				var range = item[0].createTextRange();
				range.collapse(true);
				range.moveEnd(&#39;character&#39;, i - 1);
				range.moveStart(&#39;character&#39;, i - 1);
				range.select();
			}
		}
	}
	function cb(self, item) {
		var v = item.val();
		self.trigger(&#39;input&#39;, [v.length, parseInt(item.attr(&#39;maxlength&#39;))]);
	}
	return Event.extend(function(item) {
		Event.call(this);
		if($.type(item) == &#39;string&#39;)
			item = $(item);
		var self = this;
		if(window.addEventListener) {
			item.bind(&#39;input&#39;, function() {
				if(ie9)
					virtualTextareaMaxlength(item);
				cb(self, item);
			});
			//ie9对于delete、backspace、剪切、粘帖不支持，需hack
			if(ie9)
				item.bind(&#39;keydown cut paste&#39;, function(e) {
					switch(e.type) {
						case &#39;keydown&#39;:
							if(e.keyCode != 46 &amp;&amp; e.keyCode != <img src="http://army8735.org/wp-includes/images/smilies/icon_cool.gif" alt="8)">
								return;
						default:
							setTimeout(function() {
								virtualTextareaMaxlength(item);
								cb(self, item);
							}, 0);
					}
				});
		}
		else
			item.bind('propertychange', function() {
				virtualTextareaMaxlength(item);
				cb(self, item);
			});
	});
});</pre>
<p>依赖的Event模块：</p>
<pre>define('Event', ['jQuery', 'Class'], function($, Class) {
	var Klass = Class(function() {
		this.dispatcher1 = $({});
	}).methods({
		bind: function() {
			var self = this,
				args = Array.prototype.slice.call(arguments, 0),
				cb = args.pop();
				cb2 = function() {
					var as = Array.prototype.slice.call(arguments, 0);
					as.shift();
					cb.apply(self, as);
				};
				args.push(cb2);
			this.dispatcher1.bind.apply(this.dispatcher1, args);
			return self;
		}
	});
	['unbind', 'trigger'].forEach(function(k){
		Klass.prototype[k] = function() {
			this.dispatcher1[k].apply(this.dispatcher1, Array.prototype.slice.call(arguments, 0));
			return this;
		}
	});
	Klass.statics({
		mix: function() {
			Array.prototype.slice.call(arguments, 0).forEach(function(o) {
				var e = new Klass,
					mix = {};
				Object.keys(Klass.prototype).forEach(function(k) {
					mix[k] = function() {
						e[k].apply(e, Array.prototype.slice.call(arguments, 0));
					}
				});
				$.extend(o, mix);
			});
			return arguments[0];
		}
	});
	return Klass;
});</pre>
<p>依赖的Class模块：</p>
<pre>define('Class', function() {
	function inheritPrototype(subType, superType) {
		var prototype = Object.create(superType.prototype);
		prototype.constructor = subType;
		subType.prototype = prototype;
		//继承static变量
		Object.keys(superType).forEach(function(k) {
			if(!subType.hasOwnProperty(k))
				subType[k] = superType[k];
		});
		return subType;
	}
	function wrap(fn) {
		fn.extend = function(sub) {
			inheritPrototype(sub, fn);
			return wrap(sub);
		}
		fn.methods = function(o) {
			Object.keys(o).forEach(function(k) {
				fn.prototype[k] = o[k];
			});
			return fn;
		};
		fn.statics = function(o) {
			Object.keys(o).forEach(function(k) {
				fn[k] = o[k];
			});
			return fn;
		};
		return fn;
	}
	function klass(cons) {
		return wrap(cons);
	}
	klass.extend = inheritPrototype;
	return klass;
});</pre>