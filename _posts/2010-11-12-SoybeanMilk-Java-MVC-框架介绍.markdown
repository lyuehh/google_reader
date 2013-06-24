---
layout: post
title:  "SoybeanMilk Java MVC 框架介绍"
date:   2010-11-12 20:37:47
author: 
categories: program
---

## SoybeanMilk Java MVC 框架介绍
### by 
### at 2010-11-12 20:37:47
### original <http://www.javaeye.com/topic/810239>

<strong>SoybeanMilk</strong>是一个极其简易、友好、且零侵入的Java MVC实现框架：
<br>
<br><ul>
<li>它几乎没有学习成本，你只需要熟悉jsp和servlet技术
</li><li>也不需要你遵从任何代码编写模式
</li><li>你的代码中找不到任何这个框架的踪迹
</li></ul>
<br>
<br>使用它，您仅需要指定URL和参数值便可以执行任何Java对象的任何方法并控制和取得其返回结果。
<br>
<br>如果你是一个WEB开发者，并且已经有点厌烦现有的WEB开发框架所固有的开发方式（固定的代码编写模式、芝麻点的小功能也要写一堆接口），应该考虑了解下这个框架。
<br>
<br>还有，这个框架并不是仅能应用于WEB程序，你也可以在桌面程序中使用它。
<br>
<br>项目主页
<br><a href="http://code.google.com/p/soybeanmilk/">http://code.google.com/p/soybeanmilk/</a>
<br>
<br>这里有完整的项目包，其中也包括API、使用帮助、示例代码
<br><a href="http://code.google.com/p/soybeanmilk/downloads/list">http://code.google.com/p/soybeanmilk/downloads/list</a>
<br>
<br>---------------------------------------------------------------------------------------
<br>
<br>来看看使用<strong>SoybeanMilk</strong>时，你需要做些什么。
<br>
<br>首先，你需要编写你的Java业务类，如下所示：
<br><pre name="code">
package my.biz;
public class Calculator
{
        public int multiply(int a, int b)
        {
                return a*b;
        } 
}
</pre>
<br>
<br>然后，定义“/WEB-INF/soybean-milk.config.xml”配置文件：
<br><pre name="code">
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;soybean-milk&gt;
        &lt;resolvers&gt;
                &lt;resolver id=&quot;calculator&quot; class=&quot;my.biz.Calculator&quot; /&gt;
        &lt;/resolvers&gt;

        &lt;executables&gt;
                &lt;action name=&quot;/multiply.do&quot;&gt;
                        &lt;invoke method=&quot;multiply&quot; resolver=&quot;calculator&quot; result-key=&quot;multiplyResult&quot;&gt;
                                &lt;arg key=&quot;param.a&quot; /&gt;
                                &lt;arg value=&quot;10&quot; /&gt;
                        &lt;/invoke&gt;
                        
                        &lt;target url=&quot;/multiply_result.jsp&quot; /&gt;
                &lt;/action&gt;
        &lt;/executables&gt;
&lt;/soybean-milk&gt;
</pre>
<br>
<br>接下来，编写“/multiply_result.jsp”展示页面：
<br><pre name="code">
&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot; pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD HTML 4.01 Transitional//EN&quot; &quot;http://www.w3.org/TR/html4/loose.dtd&quot;&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta http-equiv=&quot;Content-Type&quot; content=&quot;text/html; charset=UTF-8&quot;&gt;
&lt;title&gt;计算结果&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
        计算结果：${multiplyResult}
&lt;/body&gt;
&lt;/html&gt;
</pre>
<br>
<br>配置web.xml，加入如下内容：
<br><pre name="code">
&lt;servlet&gt;
    &lt;servlet-name&gt;dispatchServlet&lt;/servlet-name&gt;
    &lt;servlet-class&gt;org.soybeanMilk.web.servlet.DispatchServlet&lt;/servlet-class&gt;
    &lt;init-param&gt;
      &lt;param-name&gt;encoding&lt;/param-name&gt;
      &lt;param-value&gt;UTF-8&lt;/param-value&gt;
    &lt;/init-param&gt;
    &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
  &lt;/servlet&gt;
  &lt;servlet-mapping&gt;
    &lt;servlet-name&gt;dispatchServlet&lt;/servlet-name&gt;
    &lt;url-pattern&gt;*.do&lt;/url-pattern&gt;
&lt;/servlet-mapping&gt;
</pre>
<br>
<br>并将<strong>SoybeanMilk</strong>库和其依赖库
<br>
<br>soybeanMilk-[<em>version</em>].jar
<br>commons-beanutils-1.8.2.jar
<br>commons-logging-1.0.4.jar
<br>commons-logging-api-1.1.jar
<br>log4j-1.2.14.jar（可选）
<br>
<br>放入/WEB-INF/lib目录下。
<br>
<br>最后，启动服务器，在浏览器中输入“ http://<em>youApp</em>/multiply.do?a=3 ”，完成！
<br>
          
          <br><br>
          作者: <a href="http://earthangry.javaeye.com">earthangry</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/810239" style="color:red">已有 <strong>13</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/433"><span style="color:red;font-weight:bold">—软件人才免语言低担保 赴美带薪读研！— </span></a></li></ul>
<br><br><br>