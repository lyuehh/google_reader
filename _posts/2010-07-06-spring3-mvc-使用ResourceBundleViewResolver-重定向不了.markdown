---
layout: post
title:  "spring3 mvc 使用ResourceBundleViewResolver 重定向不了"
date:   2010-07-06 15:00:11
author: 
categories: program
---

## spring3 mvc 使用ResourceBundleViewResolver 重定向不了
### by 
### at 2010-07-06 15:00:11
### original <http://www.javaeye.com/topic/706605>

相关配置如下： 
<br><pre name="code">
&lt;bean id=&quot;viewResolver&quot;  class=&quot;org.springframework.web.servlet.view.ResourceBundleViewResolver&quot;&gt;  
        &lt;property name=&quot;basename&quot; value=&quot;views&quot; &gt;  
        &lt;/property&gt;  
    &lt;/bean&gt;  
</pre> 
<br>
<br>
<br>
<br>views.properties: 
<br><pre name="code">
UserListView.url=/WEB-INF/jsp/auth/user/list.jsp  
UserListView.(class)=org.springframework.web.servlet.view.JstlView  
  
UserCreateView.(class)=org.springframework.web.servlet.view.JstlView  
UserCreateView.url=/WEB-INF/jsp/auth/user/create.jsp  
  
UserEditView.(class)=org.springframework.web.servlet.view.JstlView  
UserEditView.url=/WEB-INF/jsp/auth/user/edit.jsp  
  
UserShowView.(class)=org.springframework.web.servlet.view.JstlView  
UserShowView.url=/WEB-INF/jsp/auth/user/show.jsp  
  
UserSuccessView.(class)=org.springframework.web.servlet.view.RedirectView  
UserSuccessView.url=list  

</pre>
<br>
<br>
<br>
<br>本来spring以前版本是这样写的： UserSuccessView.url=redirect:/auth/user/list 
<br>
<br>在spring3中好像不行了，试了几次，发现写成UserSuccessView.url=list 
<br>这样，编辑，点击保存 ，后能转发到list页面。 
<br>
<br>但是，随后的问题是，在删除时，回不到list页面。 
<br>
<br>请教。 
          
          <br><br>
          作者: <a href="http://kehl.javaeye.com">kevincollins</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/706605" style="color:red">已有 <strong>0</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>