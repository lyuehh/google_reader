---
layout: post
title:  "作为笔记：Tomcat配置DataSource"
date:   2010-06-24 11:50:09
author: 
categories: program
---

## 作为笔记：Tomcat配置DataSource
### by 
### at 2010-06-24 11:50:09
### original <http://www.javaeye.com/topic/698236>

<p>修改Tomcat_Home/conf/server.xml，在GlobalNamingResources中加入：</p>
<p>
</p>
<pre name="code"> &lt;Resource name=&quot;jdbc/DataSource&quot; auth=&quot;Container&quot;
  type=&quot;javax.sql.DataSource&quot; driverClassName=&quot;oracle.jdbc.OracleDriver&quot;
  url=&quot;jdbc:oracle:thin:@127.0.0.1:1521:orcl&quot;
  username=&quot;user&quot; password=&quot;password&quot; maxActive=&quot;20&quot; maxIdle=&quot;10&quot;
  maxWait=&quot;-1&quot;/&gt; 	
</pre>

<p> 再在Context中加入引用：</p>
<p>
</p>
<pre name="code"> &lt;ResourceLink name=&quot;jdbc/DataSource&quot; global=&quot;jdbc/DataSource&quot;  type=&quot;javax.sql.DataSource&quot;/&gt;
</pre>

<p> 如果不加，则在使用的时候会出现：Cannot create JDBC driver of class '' for connect URL 'null' 错误</p>
<p> </p>
<p>在web.xml定义：</p>
<p>
</p>
<pre name="code">    &lt;resource-ref&gt;
        &lt;description&gt;popuserDataSource&lt;/description&gt;
        &lt;res-ref-name&gt;jdbc/DataSource&lt;/res-ref-name&gt;
        &lt;res-type&gt;javax.sql.DataSource&lt;/res-type&gt;
        &lt;res-auth&gt;Container&lt;/res-auth&gt;
    &lt;/resource-ref&gt;</pre>

<p> 在Spring中引用：</p>
<p>
</p>
<pre name="code">  &lt;bean id=&quot;DataSource&quot; class=&quot;org.springframework.jndi.JndiObjectFactoryBean&quot;&gt;
        &lt;property name=&quot;jndiName&quot; value=&quot;java:comp/env/jdbc/DataSource&quot;/&gt;
        &lt;property name=&quot;expectedType&quot; value=&quot;javax.sql.DataSource&quot;/&gt;
    &lt;/bean&gt;</pre>

<p> 完成。</p>
<p> </p>
<p>不知道为什么Tomcat非得要改web.xml指定引后才能引用<span style="white-space:pre">Resource <span style="white-space:normal">。真烦人。</span></span></p>
          
          <br><br>
          作者: <a href="http://netbus.javaeye.com">NetBus</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/698236" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li></ul>
<br><br><br>