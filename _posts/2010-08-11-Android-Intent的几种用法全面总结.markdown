---
layout: post
title:  "Android Intent的几种用法全面总结"
date:   2010-08-11 20:32:00
author: stulife
categories: program
---

## Android Intent的几种用法全面总结
### by stulife
### at 2010-08-11 20:32:00
### original <http://www.cnblogs.com/stulife/archive/2010/08/11/1797598.html>

<p><a href="http://www.cnblogs.com/stulife/"><img src="http://pic.cnblogs.com/face/u118120.jpg" alt="" border="0"></a><br>作者: <a href="http://www.cnblogs.com/stulife/">stulife</a> 发表于 2010-08-11 20:32 <a href="http://www.cnblogs.com/stulife/archive/2010/08/11/1797598.html">原文链接</a> 阅读: 2 评论: 0</p><div>
<table cellpadding="0" cellspacing="0">
<tbody>
<tr>
<td><span href="http://www.cnblogs.com/stulife/admin/tag.php?name=Intent">Intent</span>应该算是Android中特有的东西。你可以在Intent中指定<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E7%A8%8B%E5%BA%8F">程序</span>要执行的动作（比如：view,edit,dial），以及程序执行到该动作时所需要的<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E8%B5%84%E6%96%99">资料</span>。都指定好后，只要调用startActivity()，Android<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E7%B3%BB%E7%BB%9F">系统</span>会<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E8%87%AA%E5%8A%A8">自动</span>寻找最符合你指定要求的<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E5%BA%94%E7%94%A8">应用</span>程序，并执行该程序。<br><br>下面列出几种Intent的用法<br>显示网页:
<div>
<div><ol>
<li>Uri uri = Uri.parse("http://www.google.com");</li>
<li>Intent it  = new Intent(Intent.ACTION_VIEW,uri);</li>
<li>startActivity(it);</li>
</ol></div>
</div>
显示地图:
<div>
<div><ol>
<li>Uri uri = Uri.parse("geo:38.899533,-77.036476");</li>
<li>Intent it = new Intent(Intent.Action_VIEW,uri);</li>
<li>startActivity(it); </li>
</ol></div>
</div>
路径规划:
<div>
<div><ol>
<li>Uri uri = Uri.parse(&quot;http://maps.google.com/maps?f=d&amp;saddr=startLat%20startLng&amp;daddr=endLat%20endLng&amp;hl=en&quot;);</li>
<li>Intent it = new Intent(Intent.ACTION_VIEW,URI);</li>
<li>startActivity(it);</li>
</ol></div>
</div>
拨打电话:<br>调用拨号程序
<div>
<div><ol>
<li>Uri uri = Uri.parse("tel:xxxxxx");</li>
<li>Intent it = new Intent(Intent.ACTION_DIAL, uri);  </li>
<li>startActivity(it);  </li>
</ol></div>
</div>
<div>
<div><ol>
<li>Uri uri = Uri.parse("tel.xxxxxx");</li>
<li>Intent it =new Intent(Intent.ACTION_CALL,uri);</li>
<li>要使用这个必须在配置<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E6%96%87%E4%BB%B6">文件</span>中加入&lt;uses-permission id=&quot;<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=android">android</span>.permission.CALL_PHONE&quot; /&gt;</li>
</ol></div>
</div>
发送SMS/MMS<br>调用发送<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E7%9F%AD%E4%BF%A1">短信</span>的程序
<div>
<div><ol>
<li>Intent it = new Intent(Intent.ACTION_VIEW);   </li>
<li>it.putExtra(&quot;sms_body&quot;, &quot;The SMS text&quot;);   </li>
<li>it.setType(&quot;vnd.android-dir/mms-sms&quot;);   </li>
<li>startActivity(it);  </li>
</ol></div>
</div>
发送短信
<div>
<div><ol>
<li>Uri uri = Uri.parse(&quot;smsto:0800000123&quot;);   </li>
<li>Intent it = new Intent(Intent.ACTION_SENDTO, uri);   </li>
<li>it.putExtra(&quot;sms_body&quot;, &quot;The SMS text&quot;);   </li>
<li>startActivity(it);  </li>
</ol></div>
</div>
发送彩信
<div>
<div><ol>
<li>Uri uri = Uri.parse(&quot;content://media/external/images/media/23&quot;);   </li>
<li>Intent it = new Intent(Intent.ACTION_SEND);   </li>
<li>it.putExtra(&quot;sms_body&quot;, &quot;some text&quot;);   </li>
<li>it.putExtra(Intent.EXTRA_STREAM, uri);   </li>
<li>it.setType(&quot;image/png&quot;);   </li>
<li>startActivity(it); </li>
</ol></div>
</div>
发送Email
<div>
<div><ol>
<li></li>
<li>Uri uri = Uri.parse("mailto:xxx@abc.com");</li>
<li>Intent it = new Intent(Intent.ACTION_SENDTO, uri);</li>
<li>startActivity(it);</li>
</ol></div>
</div>
<div>
<div><ol>
<li>Intent it = new Intent(Intent.ACTION_SEND);   </li>
<li>it.putExtra(Intent.EXTRA_EMAIL, &quot;me@abc.com&quot;);   </li>
<li>it.putExtra(Intent.EXTRA_TEXT, &quot;The email body text&quot;);   </li>
<li>it.setType(&quot;text/plain&quot;);   </li>
<li>startActivity(Intent.createChooser(it, &quot;Choose Email Client&quot;));  </li>
</ol></div>
</div>
<div>
<div><ol>
<li>Intent it=new Intent(Intent.ACTION_SEND);     </li>
<li>String[] tos={&quot;me@abc.com&quot;};     </li>
<li>String[] ccs={&quot;you@abc.com&quot;};     </li>
<li>it.putExtra(Intent.EXTRA_EMAIL, tos);     </li>
<li>it.putExtra(Intent.EXTRA_CC, ccs);     </li>
<li>it.putExtra(Intent.EXTRA_TEXT, &quot;The email body text&quot;);     </li>
<li>it.putExtra(Intent.EXTRA_SUBJECT, &quot;The email subject text&quot;);     </li>
<li>it.setType(&quot;message/rfc822&quot;);     </li>
<li>startActivity(Intent.createChooser(it, &quot;Choose Email Client&quot;));   </li>
</ol></div>
</div>
添加附件
<div>
<div><ol>
<li>Intent it = new Intent(Intent.ACTION_SEND);   </li>
<li>it.putExtra(Intent.EXTRA_SUBJECT, &quot;The email subject text&quot;);   </li>
<li>it.putExtra(Intent.EXTRA_STREAM, &quot;file:///sdcard/mysong.mp3&quot;);   </li>
<li>sendIntent.setType(&quot;audio/mp3&quot;);   </li>
<li>startActivity(Intent.createChooser(it, "Choose Email Client"));</li>
</ol></div>
</div>
<span href="http://www.cnblogs.com/stulife/admin/tag.php?name=%E6%92%AD%E6%94%BE">播放</span>多媒体
<div>
<div><ol>
<li>  </li>
<li>Intent it = new Intent(Intent.ACTION_VIEW);</li>
<li>Uri uri = Uri.parse("file:///sdcard/song.mp3");</li>
<li>it.setDataAndType(uri, "audio/mp3");</li>
<li>startActivity(it);</li>
</ol></div>
</div>
<div>
<div><ol>
<li>Uri uri = Uri.withAppendedPath(MediaStore.Audio.Media.INTERNAL_CONTENT_URI, &quot;1&quot;);   </li>
<li>Intent it = new Intent(Intent.ACTION_VIEW, uri);   </li>
<li>startActivity(it);  </li>
</ol></div>
</div>
Uninstall 程序
<div>
<div><ol>
<li>Uri uri = Uri.fromParts(&quot;package&quot;, strPackageName, null);   </li>
<li>Intent it = new Intent(Intent.ACTION_DELETE, uri);   </li>
<li>startActivity(it);</li>
</ol></div>
</div>
</td>
</tr>
</tbody>
</table>
</div><img src="http://www.cnblogs.com/stulife/aggbug/1797598.html?type=1" width="1" height="1" alt=""><p>评论: 0　<a href="http://www.cnblogs.com/stulife/archive/2010/08/11/1797598.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/stulife/archive/2010/08/11/1797598.html#commentform">发表评论</a></p><p><a href="http://job.cnblogs.com/">程序员找工作，就在博客园</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/70580/">阿里巴巴CEO卫哲：将在美寻求更多并购机会</a><span style="color:gray">(2010-08-11 20:27)</span><br>· <a href="http://news.cnblogs.com/n/70579/">雅虎营销副总裁Pat McCarthy今日离职</a><span style="color:gray">(2010-08-11 20:26)</span><br>· <a href="http://news.cnblogs.com/n/70578/">苹果企业文化十大成因：只有乔布斯必不可少</a><span style="color:gray">(2010-08-11 20:23)</span><br>· <a href="http://news.cnblogs.com/n/70577/">谷歌CEO：挖掘内部产品效果不佳 将加快收购</a><span style="color:gray">(2010-08-11 20:21)</span><br>· <a href="http://news.cnblogs.com/n/70574/">Groovy创始人：Java面临终结 Scala将取而代之</a><span style="color:gray">(2010-08-11 18:01)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/70537/">用电脑说爱你！看看程序员如何告白</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">个人主页</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://space.cnblogs.com">社区</a>  <a href="http://kb.cnblogs.com">知识库</a></p>