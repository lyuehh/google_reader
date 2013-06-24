---
layout: post
title:  "【jQuery插件】用jQuery Masonry快速构建一个pinterest网站布局"
date:   2011-03-21 22:53:38
author: 愚人码头
categories: program
---

## 【jQuery插件】用jQuery Masonry快速构建一个pinterest网站布局
### by 愚人码头
### at 2011-03-21 22:53:38
### original <http://www.css88.com/archives/3321>

前段时间领导给我看了一个网站：http://pinterest.com/， Pinterest 这个名字还算容易理解：「Pin」就是用图钉往墙上钉东西，「interest」就是趣味。 通过 Pinterest ，你可以： 1) 发现有趣的图片——设计、摄影、产品、新闻——并刺激灵感； 2) 将有趣的图片归类——tag、board、category、source——并分享给好友。 一打开Pinterest你会被那些漂亮的图片和网站的杂志风格所吸引。非常强的视觉冲击。 当然今天不是讨论产品的东西，Pinterest的布局非常有意思，按照列排，宽度自适应。正好知道jquery有个叫Masonry的插件,这个插件非常合适做Pinterest类似的布局，而且非常简单。我做了一个简单的类似Pinterest布局的demo，html和css是Pinterest的。 查看demo：http://www.css88.com/demo/jquery-masonery/； jQuery Masonry插件的参数也很简单： $(&#39;#wrapper&#39;).masonry({ singleMode: false, // 禁用测量每个浮动元素的宽度。 // 如果浮动元素具有相同的宽度，设置为true。 // 默认： false columnWidth: 240, // 1列网格的宽度，单位为像素（px）。 // 默认： 第一个浮动元素外宽度。 itemSelector: &#39;.box:visible&#39;, // 附加选择器，用来指定哪些元素包裹的元素会重新排列。 resizeable: true, // 绑定一个 Masonry 访问 用来 窗口 resize时布局平滑流动. // 默认：true animate: true, // 布局重排动画。 // 默认：false animationOptions: {}, [...]