---
layout: post
title:  "Spring3 MVC 笔记（二） —json+rest优化"
date:   2010-09-07 10:07:58
author: 
categories: program
---

## Spring3 MVC 笔记（二） —json+rest优化
### by 
### at 2010-09-07 10:07:58
### original <http://www.javaeye.com/topic/757229>

接上次的 spring mvc 注解的一些详细信息！
<br>                         其实也是一些个人的学习笔记  呵呵！
<br><a href="http://7454103.javaeye.com/blog/716975">http://7454103.javaeye.com/blog/716975</a>
<br>
<br>
<br>有了上面的基础！
<br>        一起来研究些其他的东西！
<br>前端时间写了个  struts2 集成 json 的帖子！回应还不错！ 呵呵！能帮助大家 或者一起讨论！我感觉是件很愉快的事情！ 但是由于工作的原因！每个月只能发一篇帖子 呵呵！！
<br>
<br><div>引用</div><div>&lt;一&gt; 
<br> 关于 spring3.03 前的版本的一个小改进
<br> 如果spring mvc sevlet 配置的 “/” 过滤任何请求 则css ，js 等无法访问到
<br> 解决办法：  1.0  &lt;servlet-mapping&gt;
<br>                   &lt;servlet-name&gt;default&lt;/servlet-name&gt;
<br>                    &lt;url-pattern&gt;*.css&lt;/url-pattern&gt;
<br>                  &lt;/servlet-mapping&gt;
<br>
<br>            2.0  urlrewrite 配置下页很方便！
<br>
<br>            3.0  spring 3.04 后  （已测试！）
<br>                    &lt;mvc:resources location=&quot;/css/&quot; mapping=&quot;/css/**&quot;/&gt; 
<br>                 &lt;mvc:resources location=&quot;/res/&quot; mapping=&quot;/res/**&quot;/&gt; </div>
<br>
<br><div>引用</div><div>&lt;二&gt;
<br> 关于spring的 annotation的 事物多说两句，
<br>              1.0 @Transactional 配置 只针对public 方法 其效果！ 非public 方法不会报错 单无事物支持！  如果写在类上面 默认对整个类的 所以 public 方法添加事物！所以一般不这么干！
<br>            2.0 spring mvc 配置文件 会覆盖事物配置！无论是 annotation 或者 XML 的都不行！上次问答比赛的时候 帮个兄弟整了好久才搞定！大家别犯同样的错误哈！ 解决办法 把 事物 在spring mvc配置文件 </div>
<br>&lt;三&gt;
<br>  那就来一起看看  json 咯！
<br>  添加jar 包：
<br>          jackson-core-asl-1.5.6.jar  jackson-core-lgpl-1.5.6.jar
<br>         jackson-mapper-asl-1.5.6.jar  jackson-mapper-lgpl-1.5.6.jar
<br>
<br>在 spring MVC 那个配置文件里面增加
<br><pre name="code">    &lt;bean class=&quot;org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter&quot;&gt;  
        &lt;property name=&quot;messageConverters&quot;&gt;  
            &lt;util:list id=&quot;beanList&quot;&gt;  
                &lt;ref bean=&quot;mappingJacksonHttpMessageConverter&quot; /&gt;  
            &lt;/util:list&gt;  
        &lt;/property&gt;  
    &lt;/bean&gt;  
  
    &lt;bean id=&quot;mappingJacksonHttpMessageConverter&quot;  
     class=&quot;org.springframework.http.converter.json.MappingJacksonHttpMessageConverter&quot; /&gt; </pre>
<br>
<br>就是注册个 json 解析器！ 
<br>使用实例：
<br>	@RequestMapping(value="view/{ids}") 
<br>	@ResponseBody
<br>	public Map&lt;String,GameClassEntity&gt; findByID(@PathVariable String ids, HttpServletRequest request,HttpServletResponse response) throws Exception {
<br>		 return map;
<br>	         }
<br>
<br>map 或者其他 String 上面都可以字段转换传 json 字符串了！ 当然也可以接受 json 类型的参数 解析！
<br>页面调用：
<br>
<br><pre name="code">&lt;script type=&quot;text/javascript&quot;&gt;
&lt;!--
    function  view(temp_id){
      if(typeof(temp_id)!=&quot;undefined&quot;){
           var url=&quot;../gameController/view/&quot;+temp_id;
           $.post(url,null,function(data){
              var ss= eval(&quot;(&quot;+data+&quot;)&quot;);
              alert(ss.gameList.game_name);
           });
        }
    }
 //--&gt;
&lt;/script&gt;</pre>
<br>
<br>以上是我的 只言片语！ 不足的地方欢迎各位多多指点！当然也可以直接交流！
<br>附上jar 包 供大家下载！  
<br>                                       我QQ：  252821719
          
  <br><br>
  <ul>
    本文附件下载:
    
      <li><a href="http://dl.javaeye.com/topics/download/9877a688-41dc-39b3-a1f8-e352601095f3">jackson-mapper-lgpl-1.5.6.jar</a> (479.7 KB)</li>
    
      <li><a href="http://dl.javaeye.com/topics/download/d7492e53-d900-3a71-a65b-71cdb048affa">jackson-mapper-asl-1.5.6.jar</a> (474.4 KB)</li>
    
      <li><a href="http://dl.javaeye.com/topics/download/06f84f46-3d47-3711-ae78-3ff3f434c647">jackson-core-lgpl-1.5.6.jar</a> (173.7 KB)</li>
    
      <li><a href="http://dl.javaeye.com/topics/download/fa51a60c-2e77-3407-8add-e04efd105e32">jackson-core-asl-1.5.6.jar</a> (168.4 KB)</li>
    
  </ul>

          <br><br>
          作者: <a href="http://7454103.javaeye.com">7454103</a> 
          <br>
          声明: 本文系JavaEye网站发布的原创文章，未经作者书面许可，严禁任何网站转载本文，否则必将追究法律责任！
          <br><br>
          <span style="color:red">
            <a href="http://www.javaeye.com/topic/757229" style="color:red">已有 <strong>2</strong> 人发表回复，猛击-&gt;&gt;<strong>这里</strong>&lt;&lt;-参与讨论</a>
          </span>
          <br><br><br>
<span style="color:#e28822">JavaEye推荐</span>
<br>
<ul><li><a href="http://www.iteye.com/clicks/269"><span style="color:red;font-weight:bold">北京：手机之家网站诚聘PHP程序员</span></a></li><li><a href="http://www.iteye.com/clicks/138"><span style="color:red;font-weight:bold">上海：高薪诚聘Python开发人员</span></a></li></ul>
<br><br><br>