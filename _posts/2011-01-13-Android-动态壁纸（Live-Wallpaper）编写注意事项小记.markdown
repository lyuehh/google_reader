---
layout: post
title:  "Android 动态壁纸（Live Wallpaper）编写注意事项小记"
date:   2011-01-13 04:28:00
author: 斯克迪亚
categories: program
---

## Android 动态壁纸（Live Wallpaper）编写注意事项小记
### by 斯克迪亚
### at 2011-01-13 04:28:00
### original <http://www.cnblogs.com/SkyD/archive/2011/01/13/1934175.html>

<p><h2><a href="http://images.cnblogs.com/cnblogs_com/SkyD/201101/201101130427523530.png"><img style="background-image:none;border-bottom:0px;border-left:0px;margin:0px 0px 22px 22px;padding-left:0px;padding-right:0px;display:inline;float:right;border-top:0px;border-right:0px;padding-top:0px" title="image" border="0" alt="image" align="right" src="http://images.cnblogs.com/cnblogs_com/SkyD/201101/201101130427525789.png" width="72" height="120"></a>不要使用Thread</h2>
<p>应直接使用Handler与Runnable接口对象组合运行。</p>
<p>使用Thread会产生一些莫名其妙的问题，比如：壁纸预览时报错；壁纸设置时报错；壁纸设置后闪一下即消失变为默认静态壁纸；壁纸设置后停止运行但切换到待机状态再切回还是能运行的。</p>
<p>当壁纸设置后消失时，在DDMS中看到产生的异常信息类似：</p>
<p> </p>
<p>01-13 03:04:53.734: INFO/DEBUG(1856): *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***<br>
01-13 03:04:53.734: INFO/DEBUG(1856): Build fingerprint: 'hkcsl_cht/htc_bravo/bravo/bravo:2.2/FRF91/236241:user/release-keys'<br>
01-13 03:04:53.734: INFO/DEBUG(1856): pid: 2830, tid: 2867  &gt;&gt;&gt; com.skyd.mantrawheel &lt;&lt;&lt;<br>
01-13 03:04:53.734: INFO/DEBUG(1856): signal 11 (SIGSEGV), fault addr deadbaad<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  r0 00000000  r1 afd14699  r2 00000027  r3 00000074<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  r4 afd42328  r5 00000000  r6 00000000  r7 0000a000<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  r8 00000000  r9 48533900  10 485338d8  fp 000001e0<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  ip 00001730  sp 48533590  lr deadbaad  pc afd11cf0  cpsr 60000030<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d0  643a64696f72646e  d1  6472656767756265<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d2  062b818b0627c18a  d3  0633418d062f818c<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d4  0000018f0000018f  d5  0000018f0000018f<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d6  be6659913f797051  d7  0000000043c24000<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d8  000001e000000000  d9  40790000000000a0<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d10 3fd34413509f79fe  d11 bfe3441350ad386e<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d12 3ddb7cdfd9d7bdbb  d13 0000000000000000<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d14 0000000000000000  d15 0000000000000000<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d16 018e41d4018e7b6a  d17 018dcea8018e083e<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d18 018ed1b2018dd842  d19 0190c492018fcb22<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d20 0000000000000000  d21 0000000000000000<br>
01-13 03:04:53.734: INFO/DEBUG(1856):  d22 0000018f0000018f  d23 0000018f0000018f<br>
01-13 03:04:53.744: INFO/DEBUG(1856):  d24 0000018f0000018f  d25 0000018f0000018f<br>
01-13 03:04:53.744: INFO/DEBUG(1856):  d26 ffff19a8ffff19a8  d27 ffff19a8ffff19a8<br>
01-13 03:04:53.744: INFO/DEBUG(1856):  d28 0003e5c00003e5c0  d29 0003e5c00003e5c0<br>
01-13 03:04:53.744: INFO/DEBUG(1856):  d30 0001000000010000  d31 0001000000010000<br>
01-13 03:04:53.744: INFO/DEBUG(1856):  scr 60000012<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #00  pc 00011cf0  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #01  pc 0000be62  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #02  pc 0000cdc2  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #03  pc 000008d8  /system/lib/libstdc++.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #04  pc 0004d3f8  /system/lib/libskia.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #05  pc 0006ad50  /system/lib/libskia.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):          #06  pc 0006d5b0  /system/lib/libskia.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856): code around pc:<br>
01-13 03:04:53.814: INFO/DEBUG(1856): afd11cd0 2d00682d e029d1fb b12b68db c05cf8df <br>
01-13 03:04:53.814: INFO/DEBUG(1856): afd11ce0 f8442001 4798000c e054f8df 26002227 <br>
01-13 03:04:53.814: INFO/DEBUG(1856): afd11cf0 2000f88e eee4f7fb f7fd2106 f04fe802 <br>
01-13 03:04:53.814: INFO/DEBUG(1856): afd11d00 91035180 460aa901 96012006 f7fc9602 <br>
01-13 03:04:53.814: INFO/DEBUG(1856): afd11d10 a905eb88 20024632 eb92f7fc eed0f7fb <br>
01-13 03:04:53.814: INFO/DEBUG(1856): code around lr:<br>
01-13 03:04:53.814: INFO/DEBUG(1856): deadba8c ffffffff ffffffff ffffffff ffffffff <br>
01-13 03:04:53.814: INFO/DEBUG(1856): deadba9c ffffffff ffffffff ffffffff ffffffff <br>
01-13 03:04:53.814: INFO/DEBUG(1856): deadbaac ffffffff ffffffff ffffffff ffffffff <br>
01-13 03:04:53.814: INFO/DEBUG(1856): deadbabc ffffffff ffffffff ffffffff ffffffff <br>
01-13 03:04:53.814: INFO/DEBUG(1856): deadbacc ffffffff ffffffff ffffffff ffffffff <br>
01-13 03:04:53.814: INFO/DEBUG(1856): stack:<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533550  00000015  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533554  afd146c9  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533558  afd425a0  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     4853355c  afd4254c  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533560  00000000  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533564  afd156e3  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533568  afd14699  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     4853356c  afd14699  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533570  00000074  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533574  afd42328  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533578  00000000  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     4853357c  485335a4  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533580  0000a000  [heap]<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533584  afd1493b  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533588  df002777  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     4853358c  e3a070ad  <br>
01-13 03:04:53.814: INFO/DEBUG(1856): #00 48533590  afd438e4  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533594  afd1040c  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     48533598  afd42328  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     4853359c  48e40628  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335a0  48e40628  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335a4  fffffbdf  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335a8  afd42328  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335ac  afd4372c  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335b0  48e40628  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335b4  afd0be67  /system/lib/libc.so<br>
01-13 03:04:53.814: INFO/DEBUG(1856): #01 485335b8  48e40660  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335bc  00142180  [heap]<br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335c0  00001404  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335c4  485338b0  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335c8  00000000  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335cc  48e40628  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335d0  48e40628  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335d4  00000000  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335d8  485338b0  <br>
01-13 03:04:53.814: INFO/DEBUG(1856):     485335dc  afd0cdc5  /system/lib/libc.so<br>
</p>
<p><br>
</p>
<p>目前经过多次尝试仍未探知具体出错的地方。</p>
<p> </p>
<h2>可以指定一个Activity为动态壁纸设置界面</h2>
<p>需在壁纸设置文件中这样指定：</p>
<p>&lt;?xml  version=&quot;1.0&quot;  encoding=&quot;utf-8&quot;?&gt;<br>
&lt;wallpaper  xmlns:android=&quot;<a href="http://schemas.android.com/apk/res/android%22">http://schemas.android.com/apk/res/android"</a><br>
android:author="@+string/author"<br>
android:description="@string/description"<br>
android:thumbnail="@drawable/mani1"<br>
 <strong>android:settingsActivity="com.skyd.mantrawheel.Main"</strong><br>
/&gt;</p>
<p>并且该Activity必须在AndroidManifest.xml中这样注册：</p>
<p>&lt;activity android:name=&quot;.Main&quot; android:label=&quot;@string/app_name&quot; <strong>android:exported="true"</strong>&gt;&lt;/activity&gt;</p>
<p> </p>
<h2>最好指定uses-feature标记</h2>
<p>在AndroidManifest.xml中指定如下代码会使程序在市场中对不支持动态壁纸功能的用户隐藏： </p>
<p>&lt;uses-feature android:name=&quot;android.software.live_wallpaper&quot; /&gt;</p>
<p> </p>
<h2>必须在onCreate(SurfaceHolder surfaceHolder)中指定setTouchEventsEnabled(true);</h2>
<p>如果你放在onSurfaceCreated(SurfaceHolder holder)中指定，你会很郁闷地发现动态壁纸在2.1版本的系统中可以运行，2.2版本却报错。</p>
<p><a href="http://images.cnblogs.com/cnblogs_com/SkyD/201101/201101130427578269.png"><img style="background-image:none;border-bottom:0px;border-left:0px;padding-left:0px;padding-right:0px;display:inline;border-top:0px;border-right:0px;padding-top:0px" title="image" border="0" alt="image" src="http://images.cnblogs.com/cnblogs_com/SkyD/201101/201101130428064177.png" width="890" height="677"></a></p>
<p> </p>
<h2>参考资料</h2>
<p><a title="http://www.androiddevblog.net/android/creating-android-live-wallpaper#" href="http://www.androiddevblog.net/android/creating-android-live-wallpaper#">http://www.androiddevblog.net/android/creating-android-live-wallpaper#</a></p>
<p><a title="http://code.google.com/p/krvarma-android-samples/source/browse/trunk/patternwallpaper/?r=80" href="http://code.google.com/p/krvarma-android-samples/source/browse/trunk/patternwallpaper/?r=80">http://code.google.com/p/krvarma-android-samples/source/browse/trunk/patternwallpaper/?r=80</a></p><img src="http://www.cnblogs.com/SkyD/aggbug/1934175.html?type=1" width="1" height="1" alt=""><p>作者: <a href="http://www.cnblogs.com/SkyD/">斯克迪亚</a> 发表于 2011-01-13 04:28 <a href="http://www.cnblogs.com/SkyD/archive/2011/01/13/1934175.html">原文链接</a></p><p>评论: 1　<a href="http://www.cnblogs.com/SkyD/archive/2011/01/13/1934175.html#pagedcomment">查看评论</a>　<a href="http://www.cnblogs.com/SkyD/archive/2011/01/13/1934175.html#commentform">发表评论</a></p><hr><p>最新新闻：<br>· <a href="http://news.cnblogs.com/n/88214/">LivingSocial收购西班牙团购网站 发力海外市场</a><span style="color:gray">(2011-01-13 14:28)</span><br>· <a href="http://news.cnblogs.com/n/88213/">魏武挥：中国的Facebook？</a><span style="color:gray">(2011-01-13 14:05)</span><br>· <a href="http://news.cnblogs.com/n/88212/">苹果“有毒”</a><span style="color:gray">(2011-01-13 13:58)</span><br>· <a href="http://news.cnblogs.com/n/88211/">腾讯回应盛大麻球 称侵权游戏已下架</a><span style="color:gray">(2011-01-13 13:54)</span><br>· <a href="http://news.cnblogs.com/n/88210/">Android 2.4将与iPhone 5今夏同台竞技</a><span style="color:gray">(2011-01-13 13:43)</span><br></p><p>编辑推荐：<a href="http://news.cnblogs.com/n/88196/">编程的单纯心</a><br></p><p>网站导航：<a href="http://www.cnblogs.com">博客园首页</a>  <a href="http://home.cnblogs.com/">我的园子</a>  <a href="http://news.cnblogs.com">新闻</a>  <a href="http://home.cnblogs.com/ing/">闪存</a>  <a href="http://home.cnblogs.com/group/">小组</a>  <a href="http://space.cnblogs.com/q/">博问</a>  <a href="http://kb.cnblogs.com">知识库</a></p></p>