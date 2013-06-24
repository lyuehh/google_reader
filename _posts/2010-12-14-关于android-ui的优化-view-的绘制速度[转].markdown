---
layout: post
title:  "关于android ui的优化 view 的绘制速度[转]"
date:   2010-12-14 18:22:50
author: 
categories: program
---

## 关于android ui的优化 view 的绘制速度[转]
### by 
### at 2010-12-14 18:22:50
### original <http://www.javaeye.com/topic/842116>

关于如何优化activity的启动速度， view 的绘制速度， 可参考这个sdk里的文档。 android-sdk-windows-1.5_r1/docs/resources/articles/window-bg-speed.html。
<br>
<br>
<br>
<br>看完后你就知道 android:windowBackground 太重要了，影响到绘制效率。
<br>
<br>
<br>
<br>这里要说的是另外一点， 不是这个windowBackground 。
<br>
<br>
<br>
<br>
<br>
<br>android 为了提高滚动等各方面的绘制速度，可以为每一个view建立一个缓存，使用 View.buildDrawingCache为自己的view 建立相应的缓存，
<br>
<br>这个所谓的缓存，实际上就是一个Bitmap对象。只是 这个 bitmap 对象可以有多种格式而已，如
<br>
<br>
<br>
<br>     Bitmap.Config.ARGB_8888;
<br>
<br>     Bitmap.Config.ARGB_4444;
<br>
<br>     Bitmap.Config.ARGB_8888;
<br>
<br>     Bitmap.Config.ARGB_8888;
<br>
<br>     Bitmap.Config.RGB_565；
<br>
<br>
<br>
<br>
<br>
<br>   默认的格式是Bitmap.Config.ARGB_8888.，但大多数嵌入式设备使用的显示格式都是Bitmap.Config.RGB_565. 对于后者， 并没有
<br>
<br>alpha 值，所以绘制的时候不需要计算alpha合成，速递当让快些。其次，RGB_565可以直接使用优化了的memcopy函数，效率相对高出许多。
<br>
<br>
<br>
<br>  所以， 在用buildDrawingCache建立缓存时， 可以使用RGB_565格式。但是如何制定这个格式呢 ？buildDrawingCache有两个版本，     buildDrawingCache(boolean) 和 buildDrawingCache()。并没有任何参数可以设置rgb格式，看看源码先：
<br>
<br> public void buildDrawingCache(boolean autoScale) {
<br>
<br>        if ((mPrivateFlags &amp; DRAWING_CACHE_VALID) == 0 || (autoScale ?
<br>
<br>                (mDrawingCache == null || mDrawingCache.get() == null) :
<br>
<br>                (mUnscaledDrawingCache == null || mUnscaledDrawingCache.get() == null))) {
<br>
<br>            if (ViewDebug.TRACE_HIERARCHY) {
<br>
<br>                ViewDebug.trace(this, ViewDebug.HierarchyTraceType.BUILD_CACHE);
<br>
<br>            }
<br>
<br>            if (Config.DEBUG &amp;&amp; ViewDebug.profileDrawing) {
<br>
<br>                EventLog.writeEvent(60002, hashCode());
<br>
<br>            }
<br>
<br>            int width = mRight - mLeft;
<br>
<br>            int height = mBottom - mTop;
<br>
<br>            final AttachInfo attachInfo = mAttachInfo;
<br>
<br>            final boolean scalingRequired = attachInfo != null &amp;&amp; attachInfo.mScalingRequired;
<br>
<br>            if (autoScale &amp;&amp; scalingRequired) {
<br>
<br>                width = (int) ((width * attachInfo.mApplicationScale) + 0.5f);
<br>
<br>                height = (int) ((height * attachInfo.mApplicationScale) + 0.5f);
<br>
<br>            }
<br>
<br>            final int drawingCacheBackgroundColor = mDrawingCacheBackgroundColor;
<br>
<br>            final boolean opaque = drawingCacheBackgroundColor != 0 ||
<br>
<br>                (mBGDrawable != null &amp;&amp; mBGDrawable.getOpacity() == PixelFormat.OPAQUE);
<br>
<br>            if (width &lt;= 0 || height &lt;= 0 ||
<br>
<br>                    (width * height * (opaque ? 2 : 4) &gt; // Projected bitmap size in bytes
<br>
<br>                            ViewConfiguration.get(mContext).getScaledMaximumDrawingCacheSize())) {
<br>
<br>                destroyDrawingCache();
<br>
<br>                return;
<br>
<br>            }
<br>
<br>            boolean clear = true;
<br>
<br>            Bitmap bitmap = autoScale ? (mDrawingCache == null ? null : mDrawingCache.get()) :
<br>
<br>                    (mUnscaledDrawingCache == null ? null : mUnscaledDrawingCache.get());
<br>
<br>            if (bitmap == null || bitmap.getWidth() != width || bitmap.getHeight() != height) {
<br>
<br>                Bitmap.Config quality;
<br>
<br>                if (!opaque) {
<br>
<br>                    switch (mViewFlags &amp; DRAWING_CACHE_QUALITY_MASK) {
<br>
<br>                        case DRAWING_CACHE_QUALITY_AUTO:
<br>
<br>                            quality = Bitmap.Config.ARGB_8888;
<br>
<br>                            break;
<br>
<br>                        case DRAWING_CACHE_QUALITY_LOW:
<br>
<br>                            quality = Bitmap.Config.ARGB_4444;
<br>
<br>                            break;
<br>
<br>                        case DRAWING_CACHE_QUALITY_HIGH:
<br>
<br>                            quality = Bitmap.Config.ARGB_8888;
<br>
<br>                            break;
<br>
<br>                        default:
<br>
<br>                            quality = Bitmap.Config.ARGB_8888;
<br>
<br>                            break;
<br>
<br>                    }
<br>
<br>                } else {
<br>
<br>                    quality = Bitmap.Config.RGB_565;
<br>
<br>                }
<br>
<br>                // Try to cleanup memory
<br>
<br>                if (bitmap != null) bitmap.recycle();
<br>
<br>                try {
<br>
<br>                    bitmap = Bitmap.createBitmap(width, height, quality);
<br>
<br>                    bitmap.setDensity(getResources().getDisplayMetrics().densityDpi);
<br>
<br>                    if (autoScale) {
<br>
<br>                        mDrawingCache = new SoftReference&lt;Bitmap&gt;(bitmap);
<br>
<br>                    } else {
<br>
<br>                        mUnscaledDrawingCache = new SoftReference&lt;Bitmap&gt;(bitmap);
<br>
<br>                    }
<br>
<br>                } catch (OutOfMemoryError e) {
<br>
<br>                    // If there is not enough memory to create the bitmap cache, just
<br>
<br>                    // ignore the issue as bitmap caches are not required to draw the
<br>
<br>                    // view hierarchy
<br>
<br>                    if (autoScale) {
<br>
<br>                        mDrawingCache = null;
<br>
<br>                    } else {
<br>
<br>                        mUnscaledDrawingCache = null;
<br>
<br>                    }
<br>
<br>                    return;
<br>
<br>                }
<br>
<br>                clear = drawingCacheBackgroundColor != 0;
<br>
<br>            }
<br>
<br>            Canvas canvas;
<br>
<br>            if (attachInfo != null) {
<br>
<br>                canvas = attachInfo.mCanvas;
<br>
<br>                if (canvas == null) {
<br>
<br>                    canvas = new Canvas();
<br>
<br>                }
<br>
<br>                canvas.setBitmap(bitmap);
<br>
<br>                // Temporarily clobber the cached Canvas in case one of our children
<br>
<br>                // is also using a drawing cache. Without this, the children would
<br>
<br>                // steal the canvas by attaching their own bitmap to it and bad, bad
<br>
<br>                // thing would happen (invisible views, corrupted drawings, etc.)
<br>
<br>                attachInfo.mCanvas = null;
<br>
<br>            } else {
<br>
<br>                // This case should hopefully never or seldom happen
<br>
<br>                canvas = new Canvas(bitmap);
<br>
<br>            }
<br>
<br>            if (clear) {
<br>
<br>                bitmap.eraseColor(drawingCacheBackgroundColor);
<br>
<br>            }
<br>
<br>            computeScroll();
<br>
<br>            final int restoreCount = canvas.save();
<br>
<br>            if (autoScale &amp;&amp; scalingRequired) {
<br>
<br>                final float scale = attachInfo.mApplicationScale;
<br>
<br>                canvas.scale(scale, scale);
<br>
<br>            }
<br>
<br>            canvas.translate(-mScrollX, -mScrollY);
<br>
<br>            mPrivateFlags |= DRAWN;
<br>
<br>            // Fast path for layouts with no backgrounds
<br>
<br>            if ((mPrivateFlags &amp; SKIP_DRAW) == SKIP_DRAW) {
<br>
<br>                if (ViewDebug.TRACE_HIERARCHY) {
<br>
<br>                    ViewDebug.trace(this, ViewDebug.HierarchyTraceType.DRAW);
<br>
<br>                }
<br>
<br>                mPrivateFlags &amp;= ~DIRTY_MASK;
<br>
<br>                dispatchDraw(canvas);
<br>
<br>            } else {
<br>
<br>                draw(canvas);
<br>
<br>            }
<br>
<br>            canvas.restoreToCount(restoreCount);
<br>
<br>            if (attachInfo != null) {
<br>
<br>                // Restore the cached Canvas for our siblings
<br>
<br>                attachInfo.mCanvas = canvas;
<br>
<br>            }
<br>
<br>            mPrivateFlags |= DRAWING_CACHE_VALID;
<br>
<br>        }
<br>
<br>    }
<br>
<br>看完后明白了，至少跟两个因素有关 drawingCacheBackgroundColor 和 mBGDrawable.
<br>
<br>用setDrawingCacheBackgroundColor(0xffff0000)设置为 非默认颜色后，建立的缓存就是rgb565了，可以用下列方法验证一下:
<br>
<br> final Bitmap cache = mContent.getDrawingCache();
<br>
<br>            if (cache != null) {
<br>
<br>            	Config cfg = cache.getConfig();
<br>
<br>            	Log.d(TAG, &quot;----------------------- cache.getConfig() = &quot; + cfg);
<br>
<br>        }
          
          <br><br>
          作者: <a href="http://w11h22j33.javaeye.com">w11h22j33</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/842116" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>