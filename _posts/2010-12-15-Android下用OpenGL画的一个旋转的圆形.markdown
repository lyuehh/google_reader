---
layout: post
title:  "Android下用OpenGL画的一个旋转的圆形"
date:   2010-12-15 17:02:32
author: 
categories: program
---

## Android下用OpenGL画的一个旋转的圆形
### by 
### at 2010-12-15 17:02:32
### original <http://www.javaeye.com/topic/843106>

这是我在android下用OGL ES画的第一个图形，Render的override部分引用了其它同学的一些代码
<br>怎么上截图？
<br>
<br><pre name="code">public class GLRender implements Renderer{

	
	float rotateAngle;
	
	//顶点数组,GL ES只能用这个办法画圆吗？
	private float[] vertices = new float[720];

	//度到弧度的转换
    public float DegToRad(float deg)
    {
    	return (float) (3.14159265358979323846 * deg / 180.0);
    }

	
    @Override
    public void onDrawFrame(GL10 gl) {
        // TODO Auto-generated method stub
    	
        // 进入这个函数第一件要做的事就是清除屏幕和深度缓存
        gl.glClear(GL10.GL_COLOR_BUFFER_BIT | GL10.GL_DEPTH_BUFFER_BIT);

        //画圆形
    	drawCircle(gl);
    }
    
    public void drawCircle(GL10 gl)
    {
        //重置投影矩阵
        gl.glLoadIdentity();
        // 移动操作，移入屏幕(Z轴)5个像素, x, y , z
        gl.glTranslatef(0.0f, 0.0f, -5.0f);
        
        //旋转, angle, x, y , z
        gl.glRotatef(rotateAngle, 1.0f, 0.0f, 0.0f);

        // 设置当前色为红色, R, G, B, Alpha
        gl.glColor4f(1.0f, 0.1f, 0.1f, 1.0f);
        
        //设置圆形顶点数据，这个是在创建时生成
    	FloatBuffer verBuffer = FloatBuffer.wrap(vertices);

    	//设置顶点类型为浮点坐标(GL_FLOAT)，不设置或者设置错误类型将导致图形不能显示或者显示错误
    	gl.glVertexPointer(2, GL10.GL_FLOAT, 0, verBuffer);

        //打开顶点数组
        gl.glEnableClientState(GL10.GL_VERTEX_ARRAY);
        
    	//向OGL发送实际画图指令
    	gl.glDrawArrays(GL10.GL_TRIANGLE_FAN, 0, 360);
    	
        //关闭顶点数组功能
        gl.glDisableClientState(GL10.GL_VERTEX_ARRAY);

        //画图结束
        gl.glFinish();
        
        //更改旋转角度
        rotateAngle += 0.5;
    }
    

    @Override
    public void onSurfaceChanged(GL10 gl, int width, int height) {
        // TODO Auto-generated method stub
        
        float ratio = (float) width / height;
        //设置OpenGL场景的大小
        gl.glViewport(0, 0, width, height);
        //设置投影矩阵
        gl.glMatrixMode(GL10.GL_PROJECTION);
        //重置投影矩阵
        gl.glLoadIdentity();
        // 设置视口的大小
        gl.glFrustumf(-ratio, ratio, -1, 1, 1, 10);
        // 选择模型观察矩阵
        gl.glMatrixMode(GL10.GL_MODELVIEW);    
        // 重置模型观察矩阵
        gl.glLoadIdentity();    
        
    }

    @Override
    public void onSurfaceCreated(GL10 gl, EGLConfig config) {
        // TODO Auto-generated method stub
        // 启用阴影平滑
        gl.glShadeModel(GL10.GL_SMOOTH);
        // 黑色背景
        gl.glClearColor(0, 0, 0, 0);
        // 设置深度缓存
        gl.glClearDepthf(1.0f);                            
        // 启用深度测试
        gl.glEnable(GL10.GL_DEPTH_TEST);                        
        // 所作深度测试的类型
        gl.glDepthFunc(GL10.GL_LEQUAL);                            
        
        // 告诉系统对透视进行修正
        gl.glHint(GL10.GL_PERSPECTIVE_CORRECTION_HINT, GL10.GL_FASTEST);
        
        
        //初始化圆形数据
    	for (int i = 0; i &lt; 720; i += 2) {
    	    // x value
    	    vertices[i]   = (float) (Math.cos(DegToRad(i)) * 1);
    	    // y value
    	    vertices[i+1] = (float) (Math.sin(DegToRad(i)) * 1);
    	}
    }
}</pre>
<br>
<br>完整程序见附件
          
  <br><br>
  <ul>
    本文附件下载:
    
      <li><a href="http://dl.javaeye.com/topics/download/3f20ad00-0257-37ea-8e0a-9690ff535827">FirstOpenGlPrj.rar</a> (41.8 KB)</li>
    
  </ul>

          <br><br>
          作者: <a href="http://dyingearth.javaeye.com">dyingearth</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/843106" style="color:red">已有 <strong>1</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>